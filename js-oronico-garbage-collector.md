# Jank Busters Part One

Jank, or in other words visible stutters, can be noticed when Chrome fails to render a frame within 16.66 ms (disrupting 60 frames per second motion). As of today most of the V8 garbage collection work is performed on the main rendering thread, c.f. Figure 1, often resulting in jank when too many objects need to be maintained. Eliminating jank has always been a high priority for the V8 team ([1](https://blog.chromium.org/2011/11/game-changer-for-interactive.html), [2](https://www.youtube.com/watch?v=3vPOlGRH6zk), [3](https://v8.dev/blog/free-garbage-collection)). This article discusses a few optimizations that were implemented between Chrome 41 and Chrome 46 which significantly reduce garbage collection pauses resulting in better user experience.

![img](https://v8.dev/_img/jank-busters/gc-main-thread.png)Figure 1: Garbage collection performed on the main thread

A major source of jank during garbage collection is processing various bookkeeping data structures. Many of these data structures enable optimizations that are unrelated to garbage collection. Two examples are the list of all ArrayBuffers, and each ArrayBuffer’s list of views. These lists allow for an efficient implementation of the DetachArrayBuffer operation without imposing any performance hit on access to an ArrayBuffer view. In situations, however, where a web page creates millions of ArrayBuffers, (e.g., WebGL-based games), updating those lists during garbage collection causes significant jank. In Chrome 46, we removed these lists and instead detect detached buffers by inserting checks before every load and store to ArrayBuffers. This amortizes the cost of walking the big bookkeeping list during GC by spreading it throughout program execution resulting in less jank. Although the per-access checks can theoretically slow down the throughput of programs that heavily use ArrayBuffers, in practice V8's optimizing compiler can often remove redundant checks and hoist remaining checks out of loops, resulting in a much smoother execution profile with little or no overall performance penalty.

Another source of jank is the bookkeeping associated with tracking the lifetimes of objects shared between Chrome and V8. Although the Chrome and V8 memory heaps are distinct, they must be synchronized for certain objects, like DOM nodes, that are implemented in Chrome’s C++ code but accessible from JavaScript. V8 creates an opaque data type called a handle that allows Chrome to manipulate a V8 heap object without knowing any of the details of the implementation. The object’s lifetime is bound to the handle: as long as Chrome keeps the handle around, V8’s garbage collector won’t throw away the object. V8 creates an internal data structure called a global reference for each handle it passes back out to Chrome through the V8 API, and these global references are what tell V8’s garbage collector that the object is still alive. For WebGL games, Chrome may create millions of such handles, and V8, in turn, needs to create the corresponding global references to manage their lifecycle. Processing these huge amounts of global references in the main garbage collection pause is observable as jank. Fortunately, objects communicated to WebGL are often just passed along and never actually modified, enabling simple static [escape analysis](https://en.wikipedia.org/wiki/Escape_analysis). In essence, for WebGL functions that are known to usually take small arrays as parameters the underlying data is copied on the stack, making a global reference obsolete. The result of such a mixed approach is a reduction of pause time by up to 50% for rendering-heavy WebGL games.

Most of V8’s garbage collection is performed on the main rendering thread. Moving garbage collection operations to concurrent threads reduces the waiting time for the garbage collector and further reduces jank. This is an inherently complicated task since the main JavaScript application and the garbage collector may simultaneous observe and modify the same objects. Until now, concurrency was limited to sweeping the old generation of the regular object JS heap. Recently, we also implemented concurrent sweeping of the code and map space of the V8 heap. Additionally, we implemented concurrent unmapping of unused pages to reduce the work that has to be performed on the main thread, c.f. Figure 2.

![img](https://v8.dev/_img/jank-busters/gc-concurrent-threads.png)Figure 2: Some garbage collection operations performed on the concurrent garbage collection threads.

# Jank Busters Part Two: Orinoco

In a [previous blog post](https://v8.dev/blog/jank-busters), we introduced the problem of jank caused by garbage collection interrupting a smooth browsing experience. In this blog post we introduce three optimizations that lay the groundwork for a new garbage collector in V8, codenamed *Orinoco*. Orinoco is based on the idea that implementing a mostly parallel and concurrent garbage collector without strict generational boundaries will reduce garbage collection jank and memory consumption while providing high throughput. Instead of implementing Orinoco behind a flag as a separate garbage collector, we decided to ship features of Orinoco incrementally on V8 tip of tree to benefit users immediately. The three features discussed in this post are parallel compaction, parallel remembered set processing, and black allocation.

V8 implements a [generational garbage collector](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)#Generational) where objects may move within the young generation, from the young to the old generation, and within the old generation. Moving objects is expensive since the underlying memory of objects needs to be copied to new locations and the pointers to those objects are also subject to updating. Figure 1 shows the phases and how they were executed before Orinoco. Essentially, objects were moved first and then pointers between those objects were updated afterwards, all in sequential order, resulting in observable jank.

![img](https://v8.dev/_img/orinoco/sequential.png)Figure 1: Sequential moving of objects and updating pointers

V8 partitions its heap memory into fixed-size chunks, called pages, that are assigned to either young or old generation space. Objects are initially allocated in the young generation. Upon garbage collection, live objects are moved within the young generation once. Objects that survive another garbage collection are promoted to the old generation. For both phases, which we call collectively young generation evacuation, we parallelize the copying of memory based on pages. Within the young generation, moving objects always involves allocating memory on fresh pages (and releasing the old pages), leaving behind a compact memory layout. In the old generation this process happens in a slightly different manner, since dead memory leaves behind unusable holes (or fragmentation). Some of these holes can be reused via free lists, but others are left behind, requiring compaction to move live objects to a better packed (potentially new) page. Similar to the young generation this process is parallelized on page-level.

Since there are no dependencies between young generation evacuation and old generation compaction, Orinoco now performs these phases in parallel, as shown in Figure 2. The result of these improvements is a reduction of compaction time of 75% from ~7ms to under 2ms on average.

![img](https://v8.dev/_img/orinoco/parallel.png)Figure 2: Parallel moving of objects and updating pointers

The second optimization introduced by Orinoco improves how garbage collection tracks pointers. When an object moves location on the heap, the garbage collector has to find all pointers that contain the old location of the moved object and update them with the new location. Since iterating through the heap to find the pointers would be very slow, V8 uses a data structure called a *remembered* *set* to keep track of all the interesting pointers on the heap. A pointer is interesting if it points to an object that may move during garbage collection. For example, all pointers from the old generation to the new generation are interesting because new generation objects move on every garbage collection. Pointers to objects in heavily fragmented pages are also interesting because these objects will move to other pages during compaction.

Previously, V8 implemented remembered sets as arrays of pointer addresses, or *store buffers*. There was one store buffer for the young generation and one for each of the fragmented old generation pages. The store buffer of a page contains addresses of all incoming pointers as shown in Figure 3. Entries are appended to a store buffer in a *write barrier*, which guards write operations in JavaScript code. This may result in duplicate entries since a store buffer may include a pointer multiple times and two different store buffers may include the same pointer. Duplicate entries make parallelization of the pointer update phase difficult because of the data race caused by two threads trying to update the same pointer.

![img](https://v8.dev/_img/orinoco/old-remembered-set.png)Figure 3: Old remembered set

Orinoco removes this complexity by reorganizing the remembered set to simplify parallelization and make sure that threads get disjoint sets of pointers to update. Instead of storing incoming interesting pointers in an array, each page now stores the offsets of interesting pointers originating from that page in buckets of bitmaps as shown in Figure 4. Each bucket is either empty or points to a bitmap of a fixed length. A bit in the bitmap corresponds to a pointer offset in the page. If a bit is set then the pointer is interesting and is in the remembered set. Using this data-structure we can parallelize pointer updates based on pages. The absence of duplicate entries and the dense representation of pointers also allowed us to remove complex code for handling remembered set overflow. In our long running Gmail benchmark, this change [reduced](https://drive.google.com/file/d/0BxRQ51WfVicyMk9nYUk5YVY1VjQ/view) the maximum pause time of compacting garbage collection by 45% from 42ms to 23 ms.

![img](https://v8.dev/_img/orinoco/new-remembered-set.png)Figure 4: New remembered set

The third optimization that Orinoco introduces is *black allocation*, an improvement to the marking phase of the garbage collector. Black allocation (shipped in V8 5.1) is a garbage collection technique in which all objects allocated in the old generation (e.g. [pre-tenured allocations](http://research.google.com/pubs/pub43823.html) or promoted objects by the garbage collector) are marked black immediately in order to designate them as "live". The intuition behind black allocation is that objects allocated in the old generation are likely long living. Therefore, objects that were recently allocated in the old generation should at least survive the next old generation garbage collection, otherwise they were falsely promoted. After coloring newly allocated objects black the garbage collector will not visit them. We speed up coloring of black objects by allocating them on black pages where all objects are black by default. Another benefit of black pages is that they do not have to be swept, since all objects allocated on them are (by definition) live. Black allocation speeds up incremental marking progress since marking work does not increase with new allocations. The impact of black allocation is clearly visible on the Octane Splay benchmark where the throughput and latency score improved by about 30% while using about 20% less memory due to faster marking progress and less garbage collection work overall.



# Trash talk: the Orinoco garbage collector

Over the past years the V8 garbage collector (GC) has changed a lot. The Orinoco project has taken a sequential, stop-the-world garbage collector and transformed it into a mostly parallel and concurrent collector with incremental fallback.

Any garbage collector has a few essential tasks that it has to do periodically:

1. Identify live/dead objects
2. Recycle/reuse the memory occupied by dead objects
3. Compact/defragment memory (optional)

These tasks can be performed in sequence or can be arbitrarily interleaved. A straight-forward approach is to pause JavaScript execution and perform each of these tasks in sequence on the main thread. This can cause jank and latency issues on the main thread, which we’ve talked about in [previous](https://v8.dev/blog/jank-busters) [blog posts](https://v8.dev/blog/orinoco), as well as reduced program throughput.

## Major GC (Full Mark-Compact)

The major GC collects garbage from the entire heap.

![img](https://v8.dev/_img/trash-talk/01.svg)Major GC happens in three phases: marking, sweeping and compacting.

### Marking

Figuring out which objects can be collected is an essential part of garbage collection. Garbage collectors do this by using reachability as a proxy for ‘liveness’. This means that any object currently reachable within the runtime must be kept, and any unreachable objects may be collected.

Marking is the process by which reachable objects are found. The GC starts at a set of known objects pointers, called the root set. This includes the execution stack and the global object. It then follows each pointer to a JavaScript object, and marks that object as reachable. The GC follows every pointer in that object, and continues this process recursively, until every object that is reachable in the runtime has been found and marked.

### Sweeping

Sweeping is a process where gaps in memory left by dead objects are added to a data structure called a free-list. Once marking has completed, the GC finds contiguous gaps left by unreachable objects and adds them to the appropriate free-list. Free-lists are separated by the size of the memory chunk for quick lookup. In the future when we want to allocate memory, we just look at the free-list and find an appropriately sized chunk of memory.

### Compaction

The major GC also chooses to evacuate/compact some pages, based on a fragmentation heuristic. You can think of compaction sort of like hard-disk defragmentation on an old PC. We copy surviving objects into other pages that are not currently being compacted (using the free-list for that page). This way, we can make use of the small and scattered gaps within the memory left behind by dead objects.

One potential weakness of a garbage collector which copies surviving objects is that when we allocate a lot of long-living objects, we pay a high cost to copy these objects. This is why we choose to compact only some highly fragmented pages, and just perform sweeping on others, which does not copy surviving objects.

## Generational layout

The heap in V8 is split into different regions called [generations](https://v8.dev/blog/orinoco-parallel-scavenger). There is a young generation (split further into ‘nursery’ and ‘intermediate’ sub-generations), and an old generation. Objects are first allocated into the nursery. If they survive the next GC, they remain in the young generation but are considered ‘intermediate’. If they survive yet another GC, they are moved into the old generation.

![img](https://v8.dev/_img/trash-talk/02.svg)The V8 heap is split into generations. Objects are moved through generations when they survive a GC.

In garbage collection there is an important term: “The Generational Hypothesis”. This basically states that most objects die young. In other words, most objects are allocated and then almost immediately become unreachable, from the perspective of the GC. This holds not only for V8 or JavaScript, but for most dynamic languages.

V8’s generational heap layout is designed to exploit this fact about object lifetimes. The GC is a compacting/moving GC, which means that it copies objects which survive garbage collection. This seems counterintuitive: copying objects is expensive at GC time. But we know that only a very small percentage of objects actually survive a garbage collection, according to the generational hypothesis. By moving only the objects which survive, every other allocation becomes ‘implicit’ garbage. This means that we only pay a cost (for copying) proportional to the number of surviving objects, not the number of allocations.

## Minor GC (Scavenger)

There are two garbage collectors in V8. The [**Major GC (Mark-Compact)**](https://v8.dev/blog/trash-talk#major-gc) collects garbage from the whole heap. The **Minor GC (Scavenger)** collects garbage in the young generation. The major GC is effective at collecting garbage from the whole heap, but the generational hypothesis tells us that newly allocated objects are very likely to need garbage collection.

In the Scavenger, which only collects within the young generation, surviving objects are always evacuated to a new page. V8 uses a ‘semi-space’ design for the young generation. This means that half of the total space is always empty, to allow for this evacuation step. During a scavenge, this initially-empty area is called ‘To-Space’. The area we copy from is called ‘From-Space’. In the worst case, every object could survive the scavenge and we would need to copy every object.

For scavenging, we have an additional set of roots which are the old-to-new references. These are pointers in old-space that refer to objects in the young generation. Rather than tracing the entire heap graph for every scavenge, we use [write barriers](https://www.memorymanagement.org/glossary/w.html#term-write-barrier) to maintain a list of old-to-new references. When combined with the stack and globals, we know every reference into the young generation, without the need to trace through the entire old generation.

The evacuation step moves all surviving objects to a contiguous chunk of memory (within a page). This has the advantage of completing removing fragmentation - gaps left by dead objects. We then switch around the two spaces i.e. To-Space becomes From-Space and vice-versa. Once GC is completed, new allocations happen at the next free address in the From-Space.

![img](https://v8.dev/_img/trash-talk/03.svg)The scavenger evacuates live objects to a fresh page.

We quickly run out of space in the young generation with this strategy alone. Objects that survive a second GC are evacuated into the old generation, rather than To-Space.

The final step of scavenging is to update the pointers that reference the original objects, which have been moved. Every copied object leaves a forwarding-address which is used to update the original pointer to point to the new location.

![img](https://v8.dev/_img/trash-talk/04.svg)The scavenger evacuates ‘intermediate’ objects to the old generation, and ‘nursery’ objects to a fresh page.

In scavenging we actually do these three steps — marking, evacuating, and pointer-updating — all interleaved, rather than in distinct phases.

## Orinoco

Most of these algorithms and optimizations are common in garbage collection literature and can be found in many garbage collected languages. But state-of-the-art garbage collection has come a long way. One important metric for measuring the time spent in garbage collection is the amount of time that the main thread spends paused while GC is performed. For traditional ‘stop-the-world’ garbage collectors, this time can really add up, and this time spent doing GC directly detracts from the user experience in the form of janky pages and poor rendering and latency.

![img](https://v8.dev/_img/v8-orinoco.svg)Logo for Orinoco, V8’s garbage collector

Orinoco is the codename of the GC project to make use of the latest and greatest parallel, incremental and concurrent techniques for garbage collection, in order to free the main thread. There are some terms here that have a specific meaning in the GC context, and it’s worth defining them in detail.

### Parallel

Parallel is where the main thread and helper threads do a roughly equal amount of work at the same time. This is still a ‘stop-the-world’ approach, but the total pause time is now divided by the number of threads participating (plus some overhead for synchronization). This is the easiest of the three techniques. The JavaScript heap is paused as there is no JavaScript running, so each helper thread just needs to make sure it synchronizes access to any objects that another helper might also want to access.

![img](https://v8.dev/_img/trash-talk/05.svg)The main thread and helper threads work on the same task at the same time.

### Incremental

Incremental is where the main thread does a small amount of work intermittently. We don’t do an entire GC in an incremental pause, just a small slice of the total work required for the GC. This is more difficult, because JavaScript executes between each incremental work segment, meaning that the state of the heap has changed, which might invalidate previous work that was done incrementally. As you can see from the diagram, this does not reduce the amount of time spent on the main thread (in fact, it usually increases it slightly), it just spreads it out over time. This is still a good technique for solving one of our original problems: main thread latency. By allowing JavaScript to run intermittently, but also continue garbage collection tasks, the application can still respond to user input and make progress on animation.

![img](https://v8.dev/_img/trash-talk/06.svg)Small chunks of the GC task are interleaved into the main thread execution.

### Concurrent

Concurrent is when the main thread executes JavaScript constantly, and helper threads do GC work totally in the background. This is the most difficult of the three techniques: anything on the JavaScript heap can change at any time, invalidating work we have done previously. On top of that, there are now read/write races to worry about as helper threads and the main thread simultaneously read or modify the same objects. The advantage here is that the main thread is totally free to execute JavaScript — although there is minor overhead due to some synchronization with helper threads.

![img](https://v8.dev/_img/trash-talk/07.svg)GC tasks happen entirely in the background. The main thread is free to run JavaScript.

## State of GC in V8

### Scavenging

Today, V8 uses parallel scavenging to distribute work across helper threads during the young generation GC. Each thread receives a number of pointers, which it follows, eagerly evacuating any live objects into To-Space. The scavenging tasks have to synchronize via atomic read/write/compare-and-swap operations when trying to evacuate an object; another scavenging task may have found the same object via a different path and also try to move it. Whichever helper moved the object successfully then goes back and updates the pointer. It leaves a forwarding pointer so that other workers which reach the object can update other pointers as they find them. For fast synchronization-free allocation of surviving objects, the scavenging tasks use thread-local allocation buffers.

![img](https://v8.dev/_img/trash-talk/08.svg)Parallel scavenging distributes scavenging work across multiple helper threads and the main thread.

### Major GC

Major GC in V8 starts with concurrent marking. As the heap approaches a dynamically computed limit, concurrent marking tasks are started. The helpers are each given a number of pointers to follow, and they mark each object they find as they follow all references from discovered objects. Concurrent marking happens entirely in the background while JavaScript is executing on the main thread. [Write barriers](https://dl.acm.org/citation.cfm?id=2025255) are used to keep track of new references between objects that JavaScript creates while the helpers are marking concurrently.

![img](https://v8.dev/_img/trash-talk/09.svg)The major GC uses concurrent marking and sweeping, and parallel compaction and pointer updating.

When the concurrent marking is finished, or we reach the dynamic allocation limit, the main thread performs a quick marking finalization step. The main thread pause begins during this phase. This represents the total pause time of the major GC. The main thread scans the roots once again, to ensure that all live objects are marked, and then along with a number of helpers, starts parallel compaction and pointer updating. Not all pages in old-space are eligible for compaction — those that aren’t will be swept using the free-lists mentioned earlier. The main thread starts concurrent sweeping tasks during the pause. These run concurrently to the parallel compaction tasks and to the main thread itself — they can continue even when JavaScript is running on the main thread.

## Idle-time GC

Users of JavaScript don’t have direct access to the garbage collector; it is totally implementation-defined. V8 does however provide a mechanism for the embedder to trigger garbage collection, even if the JavaScript program itself can’t. The GC can post ‘Idle Tasks’ which are optional work that would eventually be triggered anyway. Embedders like Chrome might have some notion of free or idle time. For example in Chrome, at 60 frames per second, the browser has approximately 16.6 ms to render each frame of an animation. If the animation work is completed early, Chrome can choose to run some of these idle tasks that the GC has created in the spare time before the next frame.

![img](https://v8.dev/_img/trash-talk/10.svg)Idle GC makes use of free time on the main thread to perform GC work proactively.

For more details, refer to [our in-depth publication on idle-time GC](https://queue.acm.org/detail.cfm?id=2977741).

## Takeaways

The garbage collector in V8 has come a long way since its inception. Adding parallel, incremental and concurrent techniques to the existing GC was a multi-year effort, but has paid off, moving a lot of work to background tasks. It has drastically improved pause times, latency, and page load, making animation, scrolling, and user interaction much smoother. The [parallel Scavenger](https://v8.dev/blog/orinoco-parallel-scavenger) has reduced the main thread young generation garbage collection total time by about 20%–50%, depending on the workload. [Idle-time GC](https://v8.dev/blog/free-garbage-collection) can reduce Gmail’s JavaScript heap memory by 45% when it is idle. [Concurrent marking and sweeping](https://v8.dev/blog/jank-busters) has reduced pause times in heavy WebGL games by up to 50%.

But the work here is not finished. Reducing garbage collection pause times is still important for giving users the best experience on the web, and we are looking into even more advanced techniques. On top of that, Blink (the renderer in Chrome) also has a garbage collector (called Oilpan), and we are doing work to improve [cooperation](https://dl.acm.org/citation.cfm?doid=3288538.3276521) between the two collectors and to port some of the new techniques from Orinoco to Oilpan.

Most developers don’t need to think about the GC when developing JavaScript programs, but understanding some of the internals can help you to think about memory usage and helpful programming patterns. For example, with the generational structure of the V8 heap, short-lived objects are actually very cheap from the garbage collector’s perspective, as we only pay for objects that survive the collection. These sorts of patterns work well for many garbage-collected languages, not just JavaScript.

# Concurrent marking in V8

This post describes the garbage collection technique called *concurrent marking*. The optimization allows a JavaScript application to continue execution while the garbage collector scans the heap to find and mark live objects. Our benchmarks show that concurrent marking reduces the time spent marking on the main thread by 60%–70%. Concurrent marking is the last puzzle piece of the [Orinoco project](https://v8.dev/blog/orinoco) — the project to incrementally replace the old garbage collector with the new mostly concurrent and parallel garbage collector. Concurrent marking is enabled by default in Chrome 64 and Node.js v10.

## Background

Marking is a phase of V8’s [Mark-Compact](https://en.wikipedia.org/wiki/Tracing_garbage_collection) garbage collector. During this phase the collector discovers and marks all live objects. Marking starts from the set of known live objects such as the global object and the currently active functions — the so-called roots. The collector marks the roots as live and follows the pointers in them to discover more live objects. The collector continues marking the newly discovered objects and following pointers until there are no more objects to mark. At the end of marking, all unmarked objects on the heap are unreachable from the application and can be safely reclaimed.

We can think of marking as a [graph traversal](https://en.wikipedia.org/wiki/Graph_traversal). The objects on the heap are nodes of the graph. Pointers from one object to another are edges of the graph. Given a node in the graph we can find all out-going edges of that node using the [hidden class](https://v8.dev/blog/fast-properties) of the object.

![img](https://v8.dev/_img/concurrent-marking/00.svg)Figure 1. Object graph

V8 implements marking using two mark-bits per object and a marking worklist. Two mark-bits encode three colors: white (`00`), grey (`10`), and black (`11`). Initially all objects are white, which means that the collector has not discovered them yet. A white object becomes grey when the collector discovers it and pushes it onto the marking worklist. A grey object becomes black when the collector pops it from the marking worklist and visits all its fields. This scheme is called tri-color marking. Marking finishes when there are no more grey objects. All the remaining white objects are unreachable and can be safely reclaimed.

![img](https://v8.dev/_img/concurrent-marking/01.svg)Figure 2. Marking starts from the roots

![img](https://v8.dev/_img/concurrent-marking/02.svg)Figure 3. The collector turns a grey object into black by processing its pointers

![img](https://v8.dev/_img/concurrent-marking/03.svg)Figure 4. The final state after marking is finished

Note that the marking algorithm described above works only if the application is paused while marking is in progress. If we allow the application to run during marking, then the application can change the graph and eventually trick the collector into freeing live objects.

## Reducing marking pause

Marking performed all at once can take several hundred milliseconds for large heaps.

![img](https://v8.dev/_img/concurrent-marking/04.svg)

Such long pauses can make applications unresponsive and result in poor user experience. In 2011 V8 switched from the stop-the-world marking to incremental marking. During incremental marking the garbage collector splits up the marking work into smaller chunks and allows the application to run between the chunks:

![img](https://v8.dev/_img/concurrent-marking/05.svg)

The garbage collector chooses how much incremental marking work to perform in each chunk to match the rate of allocations by the application. In common cases this greatly improves the responsiveness of the application. For large heaps under memory pressure there can still be long pauses as the collector tries to keep up with the allocations.

Incremental marking does not come for free. The application has to notify the garbage collector about all operations that change the object graph. V8 implements the notification using a Dijkstra-style write-barrier. After each write operation of the form `object.field = value` in JavaScript, V8 inserts the write-barrier code:

```cpp
// Called after `object.field = value`.
write_barrier(object, field_offset, value) {
  if (color(object) == black && color(value) == white) {
    set_color(value, grey);
    marking_worklist.push(value);
  }
}
```

The write-barrier enforces the invariant that no black object points to a white object. This is also known as the strong tri-color invariant and guarantees that the application cannot hide a live object from the garbage collector, so all white objects at the end of marking are truly unreachable for the application and can be safely freed.

Incremental marking integrates nicely with idle time garbage collection scheduling as described in an [earlier blog post](https://v8.dev/blog/free-garbage-collection). Chrome’s Blink task scheduler can schedule small incremental marking steps during idle time on the main thread without causing jank. This optimization works really well if idle time is available.

Because of the write-barrier cost, incremental marking may reduce throughput of the application. It is possible to improve both throughput and pause times by making use of additional worker threads. There are two ways to do marking on worker threads: parallel marking and concurrent marking.

**Parallel** marking happens on the main thread and the worker threads. The application is paused throughout the parallel marking phase. It is the multi-threaded version of the stop-the-world marking.

![img](https://v8.dev/_img/concurrent-marking/06.svg)

**Concurrent** marking happens mostly on the worker threads. The application can continue running while concurrent marking is in progress.

![img](https://v8.dev/_img/concurrent-marking/07.svg)

The following two sections describe how we added support for parallel and concurrent marking in V8.

## Parallel marking

During parallel marking we can assume that the application is not running concurrently. This substantially simplifies the implementation because we can assume that the object graph is static and does not change. In order to mark the object graph in parallel, we need to make the garbage collector data structures thread-safe and find a way to efficiently share marking work between threads. The following diagram shows the data-structures involved in parallel marking. The arrows indicate the direction of data flow. For simplicity, the diagram omits data-structures that are needed for heap defragmentation.

![img](https://v8.dev/_img/concurrent-marking/08.svg)Figure 5. Data structures for parallel marking

Note that the threads only read from the object graph and never change it. The mark-bits of the objects and the marking worklist have to support read and write accesses.

## Marking worklist and work stealing

The implementation of the marking worklist is critical for performance and balances fast thread-local performance with how much work can be distributed to other threads in case they run out of work to do.

The extreme sides in that trade-off space are (a) using a completely concurrent data structure for best sharing as all objects can potentially be shared and (b) using a completely thread-local data structure where no objects can be shared, optimizing for thread-local throughput. Figure 6 shows how V8 balances these needs by using a marking worklist that is based on segments for thread-local insertion and removal. Once a segment becomes full it is published to a shared global pool where it is available for stealing. This way V8 allows marking threads to operate locally without any synchronization as long as possible and still handle cases where a single thread reaches a new sub-graph of objects while another thread starves as it completely drained its local segments.

![img](https://v8.dev/_img/concurrent-marking/09.svg)Figure 6. Marking worklist

## Concurrent marking

Concurrent marking allows JavaScript to run on the main thread while worker threads are visiting objects on the heap. This opens the door for many potential data races. For example, JavaScript may be writing to an object field at the same time as a worker thread is reading the field. The data races may confuse the garbage collector to free a live object or to mix up primitive values with pointers.

Each operation on the main thread that changes the object graph is a potential source of a data race. Since V8 is a high-performance engine with many object layout optimizations, the list of potential data race sources is rather long. Here is a high-level breakdown:

- Object allocation.
- Write to an object field.
- Object layout changes.
- Deserialization from the snapshot.
- Materialization during deoptimization of a function.
- Evacuation during young generation garbage collection.
- Code patching.

The main thread needs to synchronize with the worker threads on these operations. The cost and complexity of synchronization depends on the operation. Most operations allow lightweight synchronization with atomic memory accesses, but a few operations require exclusive access to the object. In the following subsections we highlight some of the interesting cases.

### Write barrier

The data race caused by a write to an object field is resolved by turning the write operation into a [relaxed atomic write](https://en.cppreference.com/w/cpp/atomic/memory_order#Relaxed_ordering) and tweaking the write barrier:

```cpp
// Called after atomic_relaxed_write(&object.field, value);
write_barrier(object, field_offset, value) {
  if (color(value) == white && atomic_color_transition(value, white, grey)) {
    marking_worklist.push(value);
  }
}
```

Compare it with the previously-used write barrier:

```cpp
// Called after `object.field = value`.
write_barrier(object, field_offset, value) {
  if (color(object) == black && color(value) == white) {
    set_color(value, grey);
    marking_worklist.push(value);
  }
}
```

There are two changes:

1. The color check of the source object (`color(object) == black`) is gone.
2. The color transition of the `value` from white to grey happens atomically.

Without the source object color check the write barrier becomes more conservative, i.e. it may mark objects as live even if those objects are not really reachable. We removed the check to avoid an expensive memory fence that would be needed between the write operation and the write barrier:

```cpp
atomic_relaxed_write(&object.field, value);
memory_fence();
write_barrier(object, field_offset, value);
```

Without the memory fence the object color load operation can be reordered before the write operation. If we don’t prevent the reordering, then the write barrier may observe grey object color and bail out, while a worker thread marks the object without seeing the new value. The original write barrier proposed by Dijkstra et al. also does not check the object color. They did it for simplicity, but we need it for correctness.

### Bailout worklist

Some operations, for example code patching, require exclusive access to the object. Early on we decided to avoid per-object locks because they can lead to the priority inversion problem, where the main thread has to wait for a worker thread that is descheduled while holding an object lock. Instead of locking an object, we allow the worker thread to bailout from visiting the object. The worker thread does that by pushing the object into the bailout worklist, which is processed only by the main thread:

![img](https://v8.dev/_img/concurrent-marking/10.svg)Figure 7. The bailout worklist

Worker threads bail out on optimized code objects, hidden classes and weak collections because visiting them would require locking or expensive synchronization protocol.

In retrospect, the bailout worklist turned out to be great for incremental development. We started implementation with worker threads bailing out on all object types and added concurrency one by one.

### Object layout changes

A field of an object can store three kinds of values: a tagged pointer, a tagged small integer (also known as a Smi), or an untagged value like an unboxed floating-point number. [Pointer tagging](https://en.wikipedia.org/wiki/Tagged_pointer) is a well-known technique that allows efficient representation of unboxed integers. In V8 the least significant bit of a tagged value indicates whether it is a pointer or an integer. This relies on the fact that pointers are word-aligned. The information about whether a field is tagged or untagged is stored in the hidden class of the object.

Some operations in V8 change an object field from tagged to untagged (or vice versa) by transitioning the object to another hidden class. Such an object layout change is unsafe for concurrent marking. If the change happens while a worker thread is visiting the object concurrently using the old hidden class, then two kinds of bugs are possible. First, the worker may miss a pointer thinking that it is an untagged value. The write barrier protects against this kind of bug. Second, the worker may treat an untagged value as a pointer and dereference it, which would result in an invalid memory access typically followed by a program crash. In order to handle this case we use a snapshotting protocol that synchronizes on the mark-bit of the object. The protocol involves two parties: the main thread changing an object field from tagged to untagged and the worker thread visiting the object. Before changing the field, the main thread ensures that the object is marked as black and pushes it into the bailout worklist for visiting later on:

```cpp
atomic_color_transition(object, white, grey);
if (atomic_color_transition(object, grey, black)) {
  // The object will be revisited on the main thread during draining
  // of the bailout worklist.
  bailout_worklist.push(object);
}
unsafe_object_layout_change(object);
```

As shown in the code snippet below, the worker thread first loads the hidden class of the object and snapshots all the pointer fields of the object specified by the hidden class using [atomic relaxed load operations](https://en.cppreference.com/w/cpp/atomic/memory_order#Relaxed_ordering). Then it tries to mark the object black using an atomic compare and swap operation. If marking succeeded then this means that the snapshot must be consistent with the hidden class because the main thread marks the object black before changing its layout.

```cpp
snapshot = [];
hidden_class = atomic_relaxed_load(&object.hidden_class);
for (field_offset in pointer_field_offsets(hidden_class)) {
  pointer = atomic_relaxed_load(object + field_offset);
  snapshot.add(field_offset, pointer);
}
if (atomic_color_transition(object, grey, black)) {
  visit_pointers(snapshot);
}
```

Note that a white object that undergoes an unsafe layout change has to be marked on the main thread. Unsafe layout changes are relatively rare, so this does not have a big impact on performance of real world applications.

## Putting it all together

We integrated concurrent marking into the existing incremental marking infrastructure. The main thread initiates marking by scanning the roots and filling the marking worklist. After that it posts concurrent marking tasks on the worker threads. The worker threads help the main thread to make faster marking progress by cooperatively draining the marking worklist. Once in a while the main thread participates in marking by processing the bailout worklist and the marking worklist. Once the marking worklists become empty, the main thread finalizes garbage collection. During finalization the main thread re-scans the roots and may discover more white objects. Those objects are marked in parallel with the help of worker threads.

![img](https://v8.dev/_img/concurrent-marking/11.svg)

## Results

Our [real-world benchmarking framework](https://v8.dev/blog/real-world-performance) shows about 65% and 70% reduction in main thread marking time per garbage collection cycle on mobile and desktop respectively.

![img](https://v8.dev/_img/concurrent-marking/12.svg)Time spent in marking on the main thread (lower is better)

Concurrent marking also reduces garbage collection jank in Node.js. This is particularly important since Node.js never implemented idle time garbage collection scheduling and therefore was never able to hide marking time in non-jank-critical phases. Concurrent marking shipped in Node.js v10.

# Tracing from JS to the DOM and back again

Debugging memory leaks in Chrome 66 just became much easier. Chrome’s DevTools can now trace and snapshot C++ DOM objects and display all reachable DOM objects from JavaScript with their references. This feature is one of the benefits of the new C++ tracing mechanism of the V8 garbage collector.

## Background

A memory leak in a garbage collection system occurs when an unused object is not freed due to unintentional references from other objects. Memory leaks in web pages often involve interaction between JavaScript objects and DOM elements.

The following [toy example](https://ulan.github.io/misc/leak.html) shows a memory leak that happens when a programmer forgets to unregister an event listener. None of the objects referenced by the event listener can be garbage collected. In particular, the iframe window leaks together with the event listener.

```js
// Main window:
const iframe = document.createElement('iframe');
iframe.src = 'iframe.html';
document.body.appendChild(iframe);
iframe.addEventListener('load', function() {
  const localVariable = iframe.contentWindow;
  function leakingListener() {
    // Do something with `localVariable`.
    if (localVariable) {}
  }
  document.body.addEventListener('my-debug-event', leakingListener);
  document.body.removeChild(iframe);
  // BUG: forgot to unregister `leakingListener`.
});
```

The leaking iframe window also keeps all its JavaScript objects alive.

```js
// iframe.html:
class Leak {};
window.globalVariable = new Leak();
```

It is important to understand the notion of retaining paths to find the root cause of a memory leak. A retaining path is a chain of objects that prevents garbage collection of the leaking object. The chain starts at a root object such as the global object of the main window. The chain ends at the leaking object. Each intermediate object in the chain has a direct reference to the next object in the chain. For example, the retaining path of the `Leak` object in the iframe looks as follows:

![img](https://v8.dev/_img/tracing-js-dom/retaining-path.svg)Figure 1: Retaining path of an object leaked via `iframe` and event listener

Note that the retaining path crosses the JavaScript / DOM boundary (highlighted in green/red, respectively) two times. The JavaScript objects live in the V8 heap, while DOM objects are C++ objects in Chrome.

## DevTools heap snapshot

We can inspect the retaining path of any object by taking a heap snapshot in DevTools. The heap snapshot precisely captures all objects on the V8 heap. Up until recently it had only approximate information about the C++ DOM objects. For instance, Chrome 65 shows an incomplete retaining path for the `Leak` object from the toy example:

![img](https://v8.dev/_img/tracing-js-dom/chrome-65.png)Figure 2: Retaining path in Chrome 65

Only the first row is precise: the `Leak` object is indeed stored in the `global_variable` of the iframe’s window object. Subsequent rows approximate the real retaining path and make debugging of the memory leak hard.

As of Chrome 66, DevTools traces through C++ DOM objects and precisely captures the objects and references between them. This is based on the powerful C++ object tracing mechanism that was introduced for cross-component garbage collection earlier. As a result, [the retaining path in DevTools](https://www.youtube.com/watch?v=ixadA7DFCx8) is actually correct now:



## Under the hood: cross-component tracing

DOM objects are managed by Blink — the rendering engine of Chrome, which is responsible for translating the DOM into actual text and images on the screen. Blink and its representation of the DOM are written in C++ which means that the DOM cannot be directly exposed to JavaScript. Instead, objects in the DOM come in two halves: a V8 wrapper object available to JavaScript and a C++ object representing the node in the DOM. These objects have direct references to each other. Determining liveness and ownership of objects across multiple components, such as Blink and V8, is difficult because all involved parties need to agree on which objects are still alive and which ones can be reclaimed.

In Chrome 56 and older versions (i.e. until Mar 2017), Chrome used a mechanism called *object grouping* to determine liveness. Objects were assigned groups based on containment in documents. A group with all of its containing objects was kept alive as long as a single object was kept alive through some other retaining path. This made sense in the context of DOM nodes that always refer to their containing document, forming so-called DOM trees. However, this abstraction removed all of the actual retaining paths which made it hard to use for debugging as shown in Figure 2. In the case of objects that did not fit this scenario, e.g. JavaScript closures used as event listeners, this approach also became cumbersome and led to various bugs where JavaScript wrapper objects would prematurely get collected, which resulted in them being replaced by empty JS wrappers that would lose all their properties.

Starting from Chrome 57, this approach was replaced by cross-component tracing, which is a mechanism that determines liveness by tracing from JavaScript to the C++ implementation of the DOM and back. We implemented incremental tracing on the C++ side with write barriers to avoid any stop-the-world tracing jank we’ve been talking about in [previous blog posts](https://v8.dev/blog/orinoco-parallel-scavenger). Cross-component tracing does not only provide better latency but also approximates liveness of objects across component boundaries better and fixes several [scenarios](https://bugs.chromium.org/p/chromium/issues/detail?id=501866) that used to cause leaks. On top of that, it allows DevTools to provide a snapshot that actually represents the DOM, as shown in Figure 3.

# High-performance garbage collection for C++

In the past we have [already](https://v8.dev/blog/trash-talk) [been](https://v8.dev/blog/concurrent-marking) [writing](https://v8.dev/blog/tracing-js-dom) about garbage collection for JavaScript, the document object model (DOM), and how all of this is implemented and optimized in V8. Not everything in Chromium is JavaScript though, as most of the browser and its Blink rendering engine where V8 is embedded are written in C++. JavaScript can be used to interact with the DOM that is then processed by the rendering pipeline.

Because the C++ object graph around the DOM is heavily tangled with Javascript objects, the Chromium team switched a couple of years ago to a garbage collector, called [Oilpan](https://www.youtube.com/watch?v=_uxmEyd6uxo), for managing this kind of memory. Oilpan is a garbage collector written in C++ for managing C++ memory that can be connected to V8 using [cross-component tracing](https://research.google/pubs/pub47359/) that treats the tangled C++/JavaScript object graph as one heap.

This post is the first in a series of Oilpan blog posts which will provide an overview of the core principles of Oilpan and its C++ APIs. For this post we will cover some of the supported features, explain how they interact with various subsystems of the garbage collector, and do a deep dive into concurrently reclaiming objects in the sweeper.

Most excitingly, Oilpan is currently implemented in Blink but moving to V8 in the form of a [garbage collection library](https://chromium.googlesource.com/v8/v8.git/+/HEAD/include/cppgc/). The goal is to make C++ garbage collection easily available for all V8 embedders and more C++ developers in general.

## Background

Oilpan implements a [Mark-Sweep](https://en.wikipedia.org/wiki/Tracing_garbage_collection) garbage collector where garbage collection is split among two phases: *marking* where the managed heap is scanned for live objects, and *sweeping* where dead objects on the managed heap are reclaimed.

We’ve covered the basics of marking already when introducing [concurrent marking in V8](https://v8.dev/blog/concurrent-marking). To recap, scanning all objects for live ones can be seen as graph traversal where objects are nodes and pointers between objects are edges. Traversal starts at roots which are registers, native execution stack (which we will call stack from now on), and other globals, as described [here](https://v8.dev/blog/concurrent-marking#background).

C++ is not different to JavaScript in that aspect. In contrast to JavaScript though, C++ objects are statically typed and thus cannot change their representation at runtime. C++ objects managed using Oilpan leverage this fact and provide a description of pointers to other objects (edges in the graph) via visitor pattern. The basic pattern for describing Oilpan objects is the following:

```cpp
class LinkedNode final : public GarbageCollected<LinkedNode> {
 public:
  LinkedNode(LinkedNode* next, int value) : next_(next), value_(value) {}
  void Trace(Visitor* visitor) const {
    visitor->Trace(next_);
  }
 private:
  Member<LinkedNode> next_;
  int value_;
};

LinkedNode* CreateNodes() {
  LinkedNode* first_node = MakeGarbageCollected<LinkedNode>(nullptr, 1);
  LinkedNode* second_node = MakeGarbageCollected<LinkedNode>(first_node, 2);
  return second_node;
}
```

In the example above, `LinkedNode` is managed by Oilpan as indicated by inheriting from `GarbageCollected<LinkedNode>`. When the garbage collector processes an object it discovers outgoing pointers by invoking the `Trace` method of the object. The type `Member` is a smart pointer that is syntactically similar to e.g. `std::shared_ptr`, which is provided by Oilpan and used to maintain a consistent state while traversing the graph during marking. All of this allows Oilpan to precisely know where pointers reside in its managed objects.

Avid readers probably noticed and may be scared that `first_node` and `second_node` are kept as raw C++ pointers on the stack in the example above. Oilpan does not add abstractions for working with the stack, relying solely on conservative stack scanning to find pointers into its managed heap when processing roots. This works by iterating the stack word-by-word and interpreting those words as pointers into the managed heap. This means that Oilpan does not impose a performance penalty for accessing stack-allocated objects. Instead, it moves the cost to the garbage collection time where it scans the stack conservatively. Oilpan as integrated in the renderer tries to delay garbage collection until it reaches a state where it’s guaranteed to have no interesting stack. Since the web is event based and execution is driven by processing tasks in event loops, such opportunities are plentiful.

Oilpan is used in Blink which is a large C++ codebase with lots of mature code and thus also supports:

- Multiple inheritance through mixins and references to such mixins (interior pointers).
- Triggering garbage collection during executing constructors.
- Keeping objects alive from non-managed memory through `Persistent` smart pointers which are treated as roots.
- Collections covering sequential (e.g. vector) and associative (e.g. set and map) containers with compaction of collection backings.
- Weak references, weak callbacks, and [ephemerons](https://en.wikipedia.org/wiki/Ephemeron).
- Finalizer callbacks that are executed before reclaiming individual objects.

## Sweeping for C++

Stay tuned for a separate blog post on how marking in Oilpan works in detail. For this article we assume marking is done and Oilpan has discovered all reachable objects with the help of their `Trace` methods. After marking all reachable objects have their mark bit set.

Sweeping is now the phase where dead objects (those unreachable during marking) are reclaimed and their underlying memory is either returned to the operating system or made available for subsequent allocations. In the following we show how Oilpan’s sweeper works, both from a usage and constraint perspective, but also how it achieves high reclamation throughput.

The sweeper finds dead objects by iterating the heap memory and checking the mark bits. In order to preserve the C++ semantics, the sweeper has to invoke the destructor of each dead object before freeing its memory. Non-trivial destructors are implemented as finalizers.

From the programmer’s perspective, there is no defined order in which destructors are executed, as the iteration used by the sweeper does not consider construction order. This imposes a restriction that finalizers are not allowed to touch other on-heap objects. This is a common challenge for writing user-code that requires finalization order as managed languages generally do not support order in their finalization semantics (e.g. Java). Oilpan uses a Clang plugin that statically verifies, among many other things, that no heap objects are accessed during destruction of an object:

```cpp
class GCed : public GarbageCollected<GCed> {
 public:
  void DoSomething();
  void Trace(Visitor* visitor) {
    visitor->Trace(other_);
  }
  ~GCed() {
    other_->DoSomething();  // error: Finalizer '~GCed' accesses
                            // potentially finalized field 'other_'.
  }
 private:
  Member<GCed> other_;
};
```

For the curious: Oilpan provides pre-finalization callbacks for complex use cases that require access to the heap before objects are destroyed. Such callbacks impose more overhead than destructors on each garbage collection cycle though and are only used sparingly in Blink.

## Incremental and concurrent sweeping

Now that we have covered the restrictions of destructors in a managed C++ environment, it is time to look at how Oilpan implements and optimizes the sweeping phase in more detail.

Before diving into details it is important to recall how programs in general are executed on the web. Any execution, e.g., JavaScript programs but also garbage collection, is driven from the main thread by dispatching tasks in an [event loop](https://en.wikipedia.org/wiki/Event_loop). The renderer, much like other application environments, supports background tasks that run concurrently to the main thread to aid processing any main-thread work.

Starting out simple, Oilpan originally implemented stop-the-world sweeping which ran as a part of the garbage collection finalization pause interrupting executing of the application on the main thread:

![img](https://v8.dev/_img/high-performance-cpp-gc/stop-the-world-sweeping.svg)Stop-the-world sweeping

For applications with soft real-time constraints the determining factor when dealing with garbage collection is latency. Stop-the-world sweeping may induce a significant pause time resulting in user-visible application latency. As the next step to reduce latency, sweeping was made incremental:

![img](https://v8.dev/_img/high-performance-cpp-gc/incremental-sweeping.svg)Incremental sweeping

With the incremental approach, sweeping is split up and delegated to additional main-thread tasks. In the best case, such tasks are executed completely in [idle time](https://research.google/pubs/pub45361/), avoiding interfering with any regular application execution. Internally, the sweeper divides work into smaller units based on a notion of pages. Pages can be in two interesting states: *to-be-swept* pages that the sweeper still needs to process, and *already-swept* pages that the sweeper already processed. Allocation only considers already-swept pages and will refill local allocation buffers (LABs) from free lists that maintain a list of available memory chunks. For getting memory from a free list the application will first try to find memory in already-swept pages, then try to help processing to-be-swept pages by inlining the sweeping algorithm into allocation, and only request new memory from the OS in case there is none.

Oilpan has used incremental sweeping for years but as applications and their resulting object graphs grew bigger and bigger, sweeping started to impact application performance. To improve over incremental sweeping we started leveraging background tasks for concurrent reclamation of memory. There are two basic invariants used to rule out any data races between background tasks executing the sweeper and the application allocating new objects:

- The sweeper only processes dead memory which is by definition not reachable by the application.
- The application only allocates on already-swept pages which are by definition not being processed by the sweeper anymore.

Both invariants ensure that there should be no contender for the object and its memory. Unfortunately, C++ heavily relies on destructors which are implemented as finalizers. Oilpan enforces finalizers to run on the main thread to assist developers and rule out data races within the application code itself. To solve this issue, Oilpan defers object finalization to the main thread. More concretely, whenever the concurrent sweeper runs into an object that has a finalizer (destructor), it pushes it onto a finalization queue that will be processed in a separate finalization phase, which is always executed on the main thread also running the application. The overall workflow with concurrent sweeping looks like this:

![img](https://v8.dev/_img/high-performance-cpp-gc/concurrent-sweeping.svg)Concurrent sweeping using background tasks

Since finalizers may require accessing all of the object’s payload, adding the corresponding memory to the free list is delayed till after executing the finalizer. If no finalizers are executed, the sweeper running on the background thread immediately adds the reclaimed memory to the free list.

# Results

Background sweeping has shipped in Chrome M78. Our [real-world benchmarking framework](https://v8.dev/blog/real-world-performance) shows a reduction of main thread sweeping time by 25%-50% (42% on average). See a selected set of line items below.

![img](https://v8.dev/_img/high-performance-cpp-gc/results.svg)Main thread sweeping time in milliseconds

The remainder of time spent on the main thread is for executing finalizers. There’s ongoing work on reducing finalizers for heavily-instantiated object types in Blink. The exciting part here is that all these optimizations are done in application code as sweeping will automatically adjust in the absence of finalizers.

Stay tuned for more posts on C++ garbage collection in general and Oilpan library updates specifically as we move closer to a release that can be used by all users of V8.
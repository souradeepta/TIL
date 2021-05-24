# Understanding memory reordering

...and why it matters when writing lock-free multithreading code.



In the previous article of this series, [Lock-free multithreading with atomic operations](https://www.internalpointers.com/post/lock-free-multithreading-atomic-operations), I introduced **lock-free multithreading**: a low-level strategy for synchronizing threads in concurrent software.

Based upon **atomic operations** — machine instructions performed directly by the CPU that can't be broken into smaller steps, lock-free multithreading provides a faster and more fine-tuned synchronization mechanism if compared to traditional primitives like [mutexes and semaphores](https://internalpointers.com/post/introduction-thread-synchronization).

As always, with great power comes great responsibility. In lock-free programming you get closer to the metal, so it is always a good idea to understand how the machine works and some of its quirks.

In this article I want to show you one of the most important side effects that hardware (and software too) might cause on your lock-free code. This is also a great opportunity to marvel at the complexity of the miniaturized world inside your computer.



## Memory reordering, or the unpleasant surprise

The first thing any programming course out there will teach you is how instructions written in the source code are executed *sequentially* by your computer. A program is just a list of operations laid down in a text file that the processor performs from top to bottom.

Surprisingly, this is often a lie: your machine has the ability to change the order of *some* low-level operations according to its needs, especially when reading from and writing to memory. This weird modification, called **memory reordering**, occurs both hardware and software wise and it is mostly due to performance reasons.

Memory reordering is a paradigm developed to make use of instruction cycles that would otherwise be wasted. This trick dramatically improves the speed of your programs; on the other hand it might wreak havoc over lock-free multithreading. We will see why in a minute.

Let's first take a closer look at the reasons why something this unpredictable would happen.



## Memory reordering in a nutshell

Programs are loaded in the **main memory** in order to be executed. The CPU task is to run instructions stored there, along with reading and writing data when necessary.

Over time this type of memory has become damn slow if compared to the processor. For example, a modern CPU is capable of executing ten instructions per nanosecond, but will require many tens of nanoseconds to fetch some data from memory! Engineers don't like such waste of time, so they equip the CPU with a small yet extremely fast chunk of special memory called **cache**.

The cache is where the processor stores its most frequently used data, in order to avoid lethargic interactions with the main memory. When the processor needs to read from or write to main memory, it first checks whether a copy of that data is available in its own cache. If so, the processor reads from or writes to the cache directly instead of waiting for the slower main memory response.

Modern CPUs are made of multiple **cores** — the component that performs actual computations, and each core has its own chunk of cache that is in turn connected to the main memory, as pictured in the following image:

![CPU, cache and main memory](https://raw.githubusercontent.com/monocasual/internalpointers-files/master/2019/12/cpu-cache-main-memory.png)

\1. A simplified model of multiple cores connected to the main memory through cache. This is known as a **shared memory system**, because the main memory is accessed by multiple entities.

All in all, cache makes computers run faster. Or better, it helps the processor not to waste precious time waiting for the main memory response by keeping it always busy and productive.

### Memory reordering as an optimization trick

Clearly such caching mechanism increases the system complexity in a multi-core scenario. Now you will need detailed rules to determine how data flows across different caches, and to make sure each core has the most up-to-date version of it. Known as **cache coherence protocols**, they could potentially trigger huge performance penalties. So engineers conceived the memory reordering trick (and many others!) to get the best out of each core.

There are several reasons why a memory reordering might take place. For example, consider two cores instructed to access the same chunk of data in memory. Core A reads from memory, core B writes to it. A cache coherence protocol might force core A to wait while core B writes its local cached data back to the main memory, so that core A can read the most up-to-date information. The waiting core might choose to run other memory instructions in advance, instead of wasting precious cycles doing nothing. Even if such decision would depart from what you have explicitly written in your program.

Compilers and virtual machines too take the liberty of reordering instructions when certain optimizations are enabled. These changes happen at compile time and can be easily spotted by looking at the assembly code or byte code. Software memory reordering exists to take advantage of any feature the underlying hardware may offer, just to make your code run faster.



## A concrete example of hardware memory reordering

Consider the following example, written in hardware pseudo-code. Each step of the program corresponds to an individual processor instruction:

```
x = 0
v = false

thread_one():
    while load(v) == false: 
        continue
    print(load(x))

thread_two():
    store(x, 1)
    store(v, true)
```

In the snippet above two threads are running in parallel *on two different cores*. The first thread waits until `v` is set to `true` by the other one. Let's also assume that `store()` and `load()` are atomic CPU instructions that write and read memory.

What would you expect to see printed on screen by thread one? If it starts before thread two (and [it is not always the case](https://internalpointers.com/post/gentle-introduction-multithreading#race-conditions)), there is no right answer. You might see `1` if no reordering takes place. However, it is possible for `v` to be updated before `x`, and the print statement might then print `0` if the store instructions are reordered in the second thread. Similarly, a memory reordering might take place in the first thread and it is possible for `x` to be loaded before `v` is checked.



## The impact of memory reordering on multithreading

Hardware memory reordering is not an issue on single-core machines, where threads are a software construct ruled by the operating system. The CPU just receives a continuous stream of memory instructions. They still can be reordered, yet according to a fundamental rule: *memory accesses by a given core will appear to that core to have occurred as written in your program*. So memory reordering might take place, but only if it doesn't screw up the final outcome.

This rule still applies to each core in a multi-core scenario, but nothing takes care of the whole picture where different operations are simultaneously performed on separate hardware components ([true parallelism](https://internalpointers.com/post/gentle-introduction-multithreading#what-threads-are-used-for)). Make your threads run on two physical cores and you will experience any kind of weird surprise as seen in the example above. Not to mention the reordering performed by compilers and virtual machines!

The usual [locking synchronization mechanisms](https://internalpointers.com/post/introduction-thread-synchronization) such as mutexes and semaphores are designed to take care of the memory reordering problem for you, both hardware and software wise. They are high level tools after all.

A multithreaded program that follows a lock-free approach is way closer to the metal instead: it exploits `store`s and `load`s atomic instructions to synchronize its threads, as seen in [the previous episode](https://internalpointers.com/post/lock-free-multithreading-atomic-operations). Funnily enough these are the operations that might get reordered, destroying all your careful plans.



## How to solve the memory reordering problem

You definitely don't want to build your synchronization mechanism upon something that might change randomly. This problem can be fixed in a practical way by triggering a **memory barrier**, a CPU instruction that forces the processor to execute memory operations in a predictable way. A memory barrier works like a barricade: operations issued prior to the barrier are guaranteed to be performed before operations issued after it.

A memory barrier is a hardware thing: you have to talk directly to the CPU. This makes it a low-level solution which hurts the portability of your programs. The best way to tackle the problem is to step up the software hierarchy and make use of the tools that operating systems, compilers and virtual machines provide.

Software gadgets are only the halfway stage, though. Let's first take a high-level look at all the memory scenarios that might take place in a system, whether hardware or software, in order to build a clear mental map of the issue. The so-called **memory model** will help in the process.



## The memory model

A memory model is an abstract way to describe what a system may and may not do when it comes to accessing and reordering memory. Processors and programming languages implement one, especially if they make use of multithreading: a memory model applies both to hardware and software.

When a system is very cautious about changing the order of memory operations is said to be following a **strong memory model**. Conversely, in a **weak memory model** you can expect all sorts of crazy reorderings. For example, processors in the x86 family belong to the former category, while ARM and PowerPC processors belong to the latter. What about software instead?

### The benefits of a software memory model

While hardware memory models are set in stone for obvious reasons, the software counterpart lets you choose how memory accesses can be ordered, according to your needs. This is an interesting property that will help a lot while writing lock-free multithreading code.

For example, a compiler can be instructed to produce machine code that follows a strong memory model, in order to avoid unwelcome reorderings around atomic operations used as synchronization mechanism. The compiler will do its best to provide the memory model you have requested by issuing the correct memory barriers, in case the underlying hardware implements a weak model. It also takes care of any memory reordering operation that might occur on the software side. Working with a software memory model abstracts away the hardware details.

Basically all programming languages implement a memory model, in the sense that they follow specific rules for handling the memory internally. Some of them just stop there, as they don't deal with multithreading directly. Some others, [Java](https://docs.oracle.com/javase/9/docs/api/java/lang/invoke/VarHandle.html), [Rust](https://doc.rust-lang.org/nomicon/atomics.html) and [C++](https://en.cppreference.com/w/cpp/atomic/memory_order) to name a few, also provide tools to control the memory reordering behavior as described above.



## Fine-tuning the memory model

Strong vs. weak memory model is a theoretical classification of how memory operations get reordered. When it comes to actual coding, most programming languages that support atomic operations give you three ways to control memory reordering. Let's take a closer look.

### 1) Sequential consistency

The less intrusive way of memory reordering is no reordering at all. This is a form of strong memory model and is called **sequential consistency**: exactly what you need to solve all lock-free multithreading problems mentioned above. Disabling reordering makes your multithreaded program easy to follow: the source code is executed in the same order you have written it.

Sequential consistency adds another important feature to parallel execution: if forces a **total order** of all atomic memory operations in all threads. To better understand this statement consider the following example, in hardware pseudocode:

```
x = 0
y = 0

thread_A:
    store(x, 1)

thread_B: 
    store(y, 1)

thread_C:
    assert(load(x) == 1 && load(y) == 0)

thread_D:
    assert(load(x) == 0 && load(y) == 1)
```

Forget memory reordering inside specific threads for a second and just look at the big picture. If threads run, say, in order A - C - B - D, thread C sees `x == 1` and `y == 0` because it has been executed right in the middle between A and B, so its assertion won't fail. Here's the catch: the total order imposed by sequential consistency forces thread D to see the same events as thread C, so its assert will fail. It's impossible for thread D to see the two `store`s ordered in a different way than what thread C has perceived. In other words, with sequential consistency all threads will see the same story line.

As I said before this is a very intuitive and natural way of thinking about multithreaded execution. However, sequential consistency disables any potential hardware or software optimization a memory reorder could offer: a recipe for serious performance bottlenecks. Sequential consistency is a necessary evil sometimes, for example in multiple producer-multiple consumer situations where all consumers must observe the actions of all producers occurring in the same order.

### 2) Acquire-release ordering

**Acquire-release** is halfway between a strong and a weak memory model. First of all, acquire-release works like sequential consistency except that you don't have a total order of execution. Let's go back to the previous example for a moment: with acquire-release, thread D is allowed to see a different story line than thread C, so that its assert too may pass.

The absence of a total order is just a side effect here. Acquire-release provides synchronization around *a specific* shared atomic variable *across multiple threads*. That is, you can for example synchronize thread A and thread C around the shared variable `x` so that thread C loads the value only when thread A has finished writing. In this case `y` is not taken into account, so you can expect any kind of reordering around it.

Concretely, programming languages that support this ordering allow you to tag memory accesses as `acquire` or `release`. An atomic store marked as `release` on a shared variable in thread A guarantees that thread B will see the full and not reordered sequence of memory events performed by thread A, once thread B triggers an atomic load marked as `acquire` on the same variable. I know it's brain-melting, but this is the foundation that mutexes stand on: critical sections and their *protected area* are built with this stuff (*acquire-release* name comes from the mutex jargon, where you acquire and release locks on it).

Acquire-release allows for more optimization opportunities as only some memory reorderings are prevented. On the other hand your code becomes more difficult to reason about.

### 3) Relaxed ordering

This is a form of weak memory model. With **relaxed ordering** you are telling your program that you don't care about memory reordering at all. The compiler and the processor are free to do whathever they want in order to optimize the execution. What remains is of course the atomic nature of the memory operation: this is useful for example for incrementing shared counters, where you just want the operation to be atomic so that other threads don't see the operation half complete.

Relaxed ordering doesn't guarantee a specific memory ordering, so it's not a tool you can safely use for thread synchronization. On the other hand, allowing any kind of memory tricks increases the performance of your multithreaded application.



## What's next?

In this article I wanted to take a bird's eye view on the memory reordering problem, the reasons behind its existence and its impact on lock-free multithreading. In the next episode I'll get my hands dirty by playing with some C++ code that makes use of atomic operations.

Why C++? The language has recently introduced [a very detailed memory model](https://en.cppreference.com/w/cpp/language/memormodel), which allows you to fine-tune how memory operations can be ordered around the C++ atomic objects. I believe it's a good way to see how sequential consistency, acquire-release and relaxed ordering play together in some real world scenarios. Wish me good luck :)



## Sources

AA.VV. - [A Primer on Memory Consistency and Cache Coherence](https://www.morganclaypool.com/doi/abs/10.2200/S00346ED1V01Y201104CAC016)
AA. VV. - [C++ Reactive Programming](https://books.google.it/books?id=MMNiDwAAQBAJ)
Paul E. McKenney - [Memory Barriers: a Hardware View for Software Hackers](http://www.puppetmastertrading.com/images/hwViewForSwHackers.pdf)
cppreference.com - [std::memory_order](https://en.cppreference.com/w/cpp/atomic/memory_order)
GCC Wiki - [Memory model synchronization modes](https://gcc.gnu.org/wiki/Atomic/GCCMM/AtomicSync)
doc.rust-lang.org - [Atomics](https://doc.rust-lang.org/nomicon/atomics.html)
Herb Sutter - [Atomic Weapons 1 of 2](https://youtu.be/A8eCGOqgvH4?t=3419)
The ryg blog - [Cache coherency primer](https://fgiesen.wordpress.com/2014/07/07/cache-coherency/)
The ryg blog - [Atomic operations and contention](https://fgiesen.wordpress.com/2014/08/18/atomics-and-contention/)
Bartosz Milewski - [C++ atomics and memory ordering](https://bartoszmilewski.com/2008/12/01/c-atomics-and-memory-ordering/)
Bartosz Milewski - [Who ordered sequential consistency?](https://bartoszmilewski.com/2008/11/11/who-ordered-sequential-consistency/)
StackOverflow - [Memory barriers force cache coherency?](https://stackoverflow.com/questions/30958375/memory-barriers-force-cache-coherency)
StackOverflow - [How does the cache coherency protocol enforce atomicity?](https://stackoverflow.com/questions/25345440/how-does-the-cache-coherency-protocol-enforce-atomicity)
StackOverflow - [Does a memory barrier ensure that the cache coherence has been completed?](https://stackoverflow.com/questions/42746793/does-a-memory-barrier-ensure-that-the-cache-coherence-has-been-completed)
StackOverflow - [C++11 introduced a standardized memory model. What does it mean? And how is it going to affect C++ programming?](https://stackoverflow.com/questions/6319146/c11-introduced-a-standardized-memory-model-what-does-it-mean-and-how-is-it-g)
StackOverflow - [How do memory_order_seq_cst and memory_order_acq_rel differ?](https://stackoverflow.com/questions/12340773/how-do-memory-order-seq-cst-and-memory-order-acq-rel-differ)
StackOverflow - [What do each memory_order mean?](https://stackoverflow.com/questions/12346487/what-do-each-memory-order-mean)
Just Software Solutions - [Memory Models and Synchronization](https://www.justsoftwaresolutions.co.uk/threading/memory_models_and_synchronization.html)
Wikipedia - [CPU cache](https://en.wikipedia.org/wiki/CPU_cache)
Wikipedia - [Cache coherence](https://en.wikipedia.org/wiki/Cache_coherence)
Wikipedia - [Memory barrier](https://en.wikipedia.org/wiki/Memory_barrier)
Wikipedia - [Memory ordering](https://en.wikipedia.org/wiki/Memory_ordering)
James Bornholt - [Memory Consistency Models: A Tutorial](https://www.cs.utexas.edu/~bornholt/post/memory-models.html)
Preshing on Programming - [Memory Ordering at Compile Time](https://preshing.com/20120625/memory-ordering-at-compile-time/)
Preshing on Programming - [Memory Barriers Are Like Source Control Operations](https://preshing.com/20120710/memory-barriers-are-like-source-control-operations/)
Linux Journal - [Memory Ordering in Modern Microprocessors, Part I](https://www.linuxjournal.com/article/8211)
Doug Lea - [Using JDK 9 Memory Order Modes](http://gee.cs.oswego.edu/dl/html/j9mm.html)
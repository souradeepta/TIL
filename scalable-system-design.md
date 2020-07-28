# Scalable System Design

[ref]: https://horicky.blogspot.com/2008/02/scalable-system-design.html

Building scalable system is becoming a hotter and hotter topic.  Mainly  because more and more people are using computer these days, both the  transaction volume and their performance expectation has grown  tremendously.

This one covers general considerations.  I have another blogs with more specific coverage on [DB scalability](http://horicky.blogspot.com/2008/03/database-scalability.html) as well as [Web site scalability](http://horicky.blogspot.com/2008/03/web-site-scalability.html).

**General Principles**

**"Scalability" is not equivalent to "Raw Performance"**

- Scalability is about reducing the adverse impact due to growth on performance, cost, maintainability and many other aspects
- e.g. Running every components in one box will have higher performance when  the load is small.  But it is not scalable because performance drops  drastically when the load is increased beyond the machine's capacity

**Understand environmental workload conditions that the system is design for**

- Dimension of growth and growth rate:  e.g. Number of users, Transaction volume, Data volume
- Measurement and their target: e.g. Response time, Throughput

**Understand who is your priority customers**

- Rank the importance of traffic so you know what to sacrifice in case you cannot handle all of them

**Scale out and Not scale up**

- Scale the system horizontally (adding more cheap machine), but not vertically (upgrade to a more powerful machine)

**Keep your code modular and simple**

- The ability to swap out old code and search with new code without worries  of breaking other parts of the system allows you to experiment different ways of optimization quickly
- Never sacrifice code modularity for any (including performance-related) reasons

**Don't guess the bottleneck, Measure it**

- Bottlenecks are slow code which are frequently executed.  Don't optimize slow code if they are rarely executed
- Write performance unit test so you can collect fine grain performance data at the component level
- Setup a performance lab so you can conduct end-to-end performance improvement measurement easily

**Plan for growth**

- Do regular capacity planning.  Collect usage statistics, predict the growth rate



## Common Techniques

**Server Farm (real time access)**

- If there is a large number of independent (potentially concurrent)  request, then you can use a server farm which is basically a set of  identicay configured machine, frontend by a load balancer.
- The application itself need to be stateless so the request can be  dispatched purely based on load conditions and not other factors.
- Incoming requests will be dispatched by the load balancer to different machines  and hence the workload is spread and shared across the servers in the  farm.  
- The architecture allows horizontal growth so when the workload increases, you can just add more server instances into the farm.
- This strategy is even more effective when combining with Cloud computing as  adding more VM instances into the farm is just an API call.

**Data Partitioning**

- Spread your data into multiple DB so that data access workload can be distributed across multiple servers
- By nature, data is stateful.  So there must be a deterministic mechanism  to dispatch data request to the server that host the data
- Data  partitioning mechanism also need to take into considerations the data  access pattern.  Data that need to be accessed together should be  staying in the same server.  A more sophisticated approach can migrate  data continuously according to data access pattern shift.
- Most distributed key/value store do this

**Map / Reduce (Batch Parallel Processing)**

- The algorithm itself need to be parallelizable.  This usually mean the  steps of execution should be relatively independent of each other.
- Google's [Map/Reduce](http://labs.google.com/papers/mapreduce.html) is a good framework for this model.  There is also an open source Java framework [Hadoop](http://hadoop.apache.org/core/) as well.

**Content Delivery Network (Static Cache)**

- This is common for static media content.  The idea is to create many copies  of contents that are distributed geographically across servers.
- User request will be routed to the server replica with close proxmity

**Cache Engine** **(Dynamic Cache)**

- This is a time vs space tradeoff.  Some executions may use the same set of  input parameters  over and over again.  Therefore, instead of redo the  same  execution for same input parameters, we can remember the previous  execution's result.
- This is typically implemented as a lookup cache.
- [Memcached](http://www.danga.com/memcached/) and [EHCache](http://ehcache.sourceforge.net/) are some of the popular caching packages

**Resources Pool**

- DBSession and TCP connection are expensive to create, so reuse them across multiple requests 

**Calculate an approximate result**

- Instead of calculate an accurate answer, see if you can tradeoff some accuracy for speed.
- If real life, usually some degree of inaccuracy is tolerable

**Filtering at the source**

- Try to do more processing upstream (where data get generated) than  downstream because it reduce the amount of data being propagated

**Asynchronous Processing**

- You make a call which returns a result.  But you don't need to use the  result until at a much later stage of your process.  Therefore, you  don't need to wait immediately after making the call., instead you can  proceed to do other things until you reach the point where you need to  use the result.
- In additional, the waiting thread is idle but  consume system resources.  For high transaction volume, the number of  idle threads is (arrival_rate * processing_time) which can be a very big number if the arrival_rate is high.  The system is running under a very ineffective mode
- The service call in this example is better handled using an asynchronous processing model.  This is typically done in 2 ways:  Callback and Polling
- In callback mode, the caller  need to provide a response handler when making the call.  The call  itself will return immediately before the actually work is done at the  server side.  When the work is done later, response will be coming back  as a separate thread which will execute the previous registered response handler.  Some kind of co-ordination may be required between the  calling thread and the callback thread.
- In polling mode, the  call itself will return a "future" handle immediately.  The caller can  go off doing other things and later poll the "future" handle to see if  the response if ready.  In this model, there is no extra thread being  created so no extra thread co-ordination is needed.

**Implementation design considerations**

- Use efficient algorithms and data structure.  Analyze the time (CPU) and  space (memory) complexity for logic that are execute frequently (ie: hot spots).  For example, carefully decide if hash table or binary tree  should be use for lookup.
- Analyze your concurrent access  scenarios when multiple threads accessing shared data.  Carefully  analyze the synchronization scenario and make sure the locking is  fine-grain enough.  Also watch for any possibility of deadlock situation and how you detect or prevent them.  A wrong concurrent access model  can have huge impact in your system's scalability.  Also consider using  Lock-Free data structure (e.g. Java's Concurrent Package have a couple  of them)
- Analyze the memory usage patterns in your logic.   Determine where new objects are created and where they are eligible for  garbage collection.  Be aware of the creation of a lot of short-lived  temporary objects as they will put a high load on the Garbage Collector.
- However, never trade off code readability for performance.  (e.g. Don't try to  bundle too much logic into a single method).  Let the VM handle this  execution for you.

# Scalable System Design Patterns

[ref]: https://horicky.blogspot.com/2010/10/scalable-system-design-patterns.html

**Load Balancer**

In this model, there is a dispatcher that determines which worker instance will handle the request based on different policies.  The application  should best be "stateless" so any worker instance can handle the  request.

This pattern is deployed in almost every medium to large web site setup.

[![img](https://1.bp.blogspot.com/_j6mB7TMmJJY/TLnj_mWL50I/AAAAAAAAAgg/JFPsfGcAenI/s400/p1.png)](https://1.bp.blogspot.com/_j6mB7TMmJJY/TLnj_mWL50I/AAAAAAAAAgg/JFPsfGcAenI/s1600/p1.png)

**Scatter and Gather**

In this model, the dispatcher multicast the request to all workers of the  pool.  Each worker will compute a local result and send it back to the  dispatcher, who will consolidate them into a single response and then  send back to the client.

This pattern is used in Search engines like Yahoo, Google to handle user's keyword search request ... etc.

[![img](https://2.bp.blogspot.com/_j6mB7TMmJJY/TLlDyOK60HI/AAAAAAAAAfI/JreI7fqvohA/s400/P2.png)](https://2.bp.blogspot.com/_j6mB7TMmJJY/TLlDyOK60HI/AAAAAAAAAfI/JreI7fqvohA/s1600/P2.png)

**Result Cache**

In this model, the dispatcher will first lookup if the request has been  made before and try to find the previous result to return, in order to  save the actual execution.

This pattern is commonly used in large enterprise application.  Memcached is a very commonly deployed cache server.

[![img](https://4.bp.blogspot.com/_j6mB7TMmJJY/TLlEpBawVMI/AAAAAAAAAfQ/Jp8vbVYnF0s/s400/P3.png)](https://4.bp.blogspot.com/_j6mB7TMmJJY/TLlEpBawVMI/AAAAAAAAAfQ/Jp8vbVYnF0s/s1600/P3.png)

**Shared Space**

This model also known as "Blackboard"; all workers monitors information from the shared space and contributes partial knowledge back to the  blackboard.  The information is continuously enriched until a solution  is reached.

This pattern is used in JavaSpace and also commercial product GigaSpace.

[![img](https://3.bp.blogspot.com/_j6mB7TMmJJY/TLlFf-b8lPI/AAAAAAAAAfY/Poy8V0eH1gA/s400/P4.png)](https://3.bp.blogspot.com/_j6mB7TMmJJY/TLlFf-b8lPI/AAAAAAAAAfY/Poy8V0eH1gA/s1600/P4.png)

**Pipe and Filter**

This model is also known as "Data Flow Programming"; all workers connected by pipes where data is flow across.

[This pattern](http://www.eaipatterns.com/PipesAndFilters.html) is a very common EAI pattern.

[![img](https://4.bp.blogspot.com/_j6mB7TMmJJY/TLlGIM4IDiI/AAAAAAAAAfg/nQgVADmUl5w/s400/P5.png)](https://4.bp.blogspot.com/_j6mB7TMmJJY/TLlGIM4IDiI/AAAAAAAAAfg/nQgVADmUl5w/s1600/P5.png)

**Map Reduce**  The model is targeting batch jobs where disk I/O is the major bottleneck.   It use a distributed file system so that disk I/O can be done in  parallel.

This pattern is used in many of Google's internal application, as well as implemented in open source [Hadoop ](http://hadoop.apache.org/)parallel processing framework.  I also find this pattern [can be used in many many application design scenarios](http://horicky.blogspot.com/2010/08/designing-algorithmis-for-map-reduce.html).

[![img](https://3.bp.blogspot.com/_j6mB7TMmJJY/TLlHPyMkTII/AAAAAAAAAf4/McnK_GGkYpw/s400/P7.png)](https://3.bp.blogspot.com/_j6mB7TMmJJY/TLlHPyMkTII/AAAAAAAAAf4/McnK_GGkYpw/s1600/P7.png)

**Bulk Synchronous Parellel**

This model is based on lock-step execution across all workers, coordinated  by a master.  Each worker repeat the following steps until the exit  condition is reached, when there is no more active workers.

1. Each worker read data from input queue
2. Each worker perform local processing based on the read data
3. Each worker push local result along its direct connection

This pattern has been used in Google's [Pregel graph processing model](http://horicky.blogspot.com/2010/07/google-pregel-graph-processing.html) as well as the [Apache Hama](http://incubator.apache.org/hama/) project.

[![img](https://4.bp.blogspot.com/_j6mB7TMmJJY/TLnhYZH7PTI/AAAAAAAAAgY/YHy5K8H6hZA/s400/P8.png)](https://4.bp.blogspot.com/_j6mB7TMmJJY/TLnhYZH7PTI/AAAAAAAAAgY/YHy5K8H6hZA/s1600/P8.png)

**Execution Orchestrator**

This  model is based on an intelligent scheduler / orchestrator to schedule  ready-to-run tasks (based on a dependency graph) across a clusters of   dumb workers.

This pattern is used in [Microsoft's Dryad project](http://research.microsoft.com/en-us/projects/dryad/)

[![img](https://3.bp.blogspot.com/_j6mB7TMmJJY/TLlH_a9WOMI/AAAAAAAAAgI/41l0bvV3fkE/s400/P8.png)](https://3.bp.blogspot.com/_j6mB7TMmJJY/TLlH_a9WOMI/AAAAAAAAAgI/41l0bvV3fkE/s1600/P8.png)



# Between Elasticity and Scalability

Follow up on an interesting question being asked in the Cloud Computing Forum.

**Definition of Scalability**

"Scalability" is the ability to meet an increasing workload requirement by  incrementally adding a proportional amount of resources capacity.   Therefore, Scalability is quantified by a measure of "linearity", which  maintains a constant ratio between resource : workload.  This linearity  allows the enterprise to plan for its resource budget according to their foreseeable workload growth.

In reality, the range of linearity  is not infinite.  It is important to understand where the linearity  reaches its limit and whether the workload will growth beyond such  limit.

A "scalable" design can push the limit of such linearity  very far.  Far enough that you don't need to worry about the workload  ever get beyond that.  Scalability architecture is typically built on a  repeatable basic infrastructure with minimal coupling such that growth  can be achieved by simply repeatedly buying the same set of basic  hardware.

Traditionally, people design for scalability to make  sure that their operation cost can grow linearly with respect to the  workload.  They usually don't worry about "removing" resources, nor it  worries about whether the resource is fully utilized, because the  purchased resources is already a sunk cost.  At least for now,  "Scalability" is not about managing fluctuating workload and also  doesn't have a time dimension.  But I expect such mindset will change as people move to Cloud computing when operation cost depends on usage.   There are financial motivations to decommission resource once they are  no longer used.

Many of existing distributed applications have a  very scalable architecture, but very few of them are design with extreme elasticity in mind.  Usually they expect the underlying infrastructure  are relatively stable with low churn rate (machine crashes are the only  cause of infrastructure changes, which is relatively infrequent),  therefore they are willing to pay a higher degradation when the churn  happens.


**Definition of Elasticity**

On the other hand, "elasticity" is the ability to instantly commission and decommission large amount of resource capacity on the fly, and then  charge purely based on the actual resource usage.  "Elasticity" is  quantitatively measured by

- Speed of commissioning / decommissioning
- Max amount of resource can be brought in
- Granularity of usage accounting

It is important to understand that even you get "elasticity" automatically when your application is deployed in the cloud, you DO NOT get  "scalability" (which is a characteristic of your application  architecture).  A non-scalable in-house application deployed into the  cloud won't magically becomes scalable.  Instead, you will just receive a huge bill from your cloud provider at the month end.  "Elasticity" is a double edge sword, it lets bad things happen fast.

"Elasticity"  is a pretty new feature to our industry and we are in the process of  discovering how we can fully utilize it.  Most of existing distributed  applications are built with scalability but not extreme elasticity in  mind.  I think an ideal app should have both "scalability" and  "elasticity" built in, but I haven't seen much yet.  Therefore, although most of existing applications can run in the cloud without  modification, I don't think they have fully exploit the benefits of the  cloud yet.

"Statefulness" and "Elasticity" are fundamentally in conflict with each other.  Data transfer rate has put a limit of the speed of commissioning/decommissioning, which is a measure of elasticity.  In other words,   I believe smart data  distribution algorithm can reduce this friction by reducing the amount  of data transfer during commissioning.  This is an area of research that I'd love to see.

**New Design Paradigm for Cloud Applications**

In order to fully exploit the benefits of the cloud, it is necessary to  have a new set of design considerations, with both scalability and  elasticity in mind.  There are many coverages in how to do a scalable  design, which I am not trying to repeat here.  Lets focus more on  "design for elasticity".



1. Make your application as stateless as possible (but don't just dump all state somewhere else)
2. Use  a smart data migration / balancing algorithm which minimize data movement when the  infrastructure grow and shrink elastically.  A typical way to do this is to use asynchronous data transfer so that your application can serve  immediately (even before the completion of data transfer)
3. Use a smart scheduler that control the growth/shrink of the environment.  Such scheduling algorithm takes the usage charge and performance impact during the dust  settlement period into consideration to decide whether a change of the  environment should be initiated.  Underlying, there is also a monitoring mechanism in place that collects the necessary metrics, perform the  analytics and feedback the result to the scheduler to fine tune its  earlier policy decision.
# Cloud Computing patterns

[ref]: https://horicky.blogspot.com/2009/11/cloud-computing-patterns.html

**1.   Use Cloud for Scaling**
 The key idea is to spin up and down machine resources according to  workload so the user only pay for the actual usage.  There is two types  of access patterns: passive listener model and active worker model.

**Passive listener model** uses a  synchronous communication pattern where the client pushes request to the server and synchronously wait for the processing result.
[![img](https://4.bp.blogspot.com/_j6mB7TMmJJY/SwYh8WQq4nI/AAAAAAAAAT8/1iVfT0Bu2Cg/s320/p1.png)](https://4.bp.blogspot.com/_j6mB7TMmJJY/SwYh8WQq4nI/AAAAAAAAAT8/1iVfT0Bu2Cg/s1600/p1.png)In the passive listener model, machine instances are typically sit behind a load balancer. To scale the resource according to the work load, we can use a monitor service that send NULL client request and use the  measured response time to spin up and down the size of the machine  resources.

 On the other hand, **Active worker model** uses an asynchronous communication patterns where the client put the  request to a queue, which will be periodically polled by the server.   After queuing the request, the client will do some other work and come  back later to pickup the result.  The client can also provide a callback address where the server can push the result into after the processing  is done.
[![img](https://4.bp.blogspot.com/_j6mB7TMmJJY/SwYiBY4GmII/AAAAAAAAAUE/HBkz9z8M-io/s320/P2.png)](https://4.bp.blogspot.com/_j6mB7TMmJJY/SwYiBY4GmII/AAAAAAAAAUE/HBkz9z8M-io/s1600/P2.png)In the active worker model, the monitor can measure the number of requests sitting in the queue and use that to determine whether machine  instances (at the consuming end) need to be spin up or down.


**2.  Use Cloud for Multi-tenancy**
[Multi-tenancy](http://horicky.blogspot.com/2009/08/multi-tenancy-in-cloud-computing.html) is more a SaaS provider (rather than an enterprise) usage scenario.   The key idea is to use the same set of code / software to host the  application for different customers (tenants)   who may have slightly  different requirement in

- UI branding
- Business rules for decision criteria
- Data schema

The approach is to provide sufficient "customization" capability for  their customer.  The most challenging part is to determine which aspects should be opened for customization and which shouldn't.  After  identifying these configurable parameters, it is straightforward to  define configuration metadata to capture that.

**3.  Use Cloud for Batch processing**This is about running things like statistics computation, report generation, machine learning, analytics ... etc.  These task is done in batch mode  and so it is more economical to use the "pay as you go" model.  On the  other hand, batch processing has very high tolerance in latency and so  is a perfect candidate of running in the cloud.
[![img](https://4.bp.blogspot.com/_j6mB7TMmJJY/SwYiOU8WuPI/AAAAAAAAAUU/yc_ZTspSGZ8/s400/P3.png)](https://4.bp.blogspot.com/_j6mB7TMmJJY/SwYiOU8WuPI/AAAAAAAAAUU/yc_ZTspSGZ8/s1600/P3.png)Here is an example of how to run [Map/Reduce framework](http://horicky.blogspot.com/2008/11/hadoop-mapreduce-implementation.html) in the cloud.  Microsoft hasn't provided a Map/Reduce solution at this  moment but Simon mentioned that Dryad in Microsoft research may be a  future Microsoft solution.  Interestingly, Simon also recommended  Hadoop.

 Of course, one challenge is how to move the data from the cloud in the first place.  In my earlier blog, I have describe [some best practices ](http://horicky.blogspot.com/2009/08/skinny-straw-in-cloud-shake.html)on this.

**4.  Use Cloud for Storage**
 The idea of storing data into the cloud and no need to worry about DBA  tasks.  Most cloud vendor provide large scale key/value store as well as RDBMS services.  Their data storage services will also take care of  data partitioning, replication ... etc.  Building [cloud storage is a big topic](http://horicky.blogspot.com/2009/11/nosql-patterns.html) involving many distributed computing concepts and techniques, I have covered it in a [separate blog](http://horicky.blogspot.com/2009/11/nosql-patterns.html).

**5.  Use Cloud for Communication**
 A **queue** (or mailbox) service provide a mechanism for different machines to communicate in an asynchronous manner via message passing.
[![img](https://4.bp.blogspot.com/_j6mB7TMmJJY/SwYsvJbTyOI/AAAAAAAAAUk/cFQGNjplDlM/s320/P5.png)](https://4.bp.blogspot.com/_j6mB7TMmJJY/SwYsvJbTyOI/AAAAAAAAAUk/cFQGNjplDlM/s1600/P5.png)
 Azure also provide a **relay service** in the cloud which is quite useful for machines behind different  firewall to communicate.  In a typical firewall setup, incoming  connection is not allowed so these machine cannot directly establish a  socket to each other.  In order for them to communicate, each need to  open an on-going socket connection to the cloud relay, which will route  traffic between these connections.

[![img](https://3.bp.blogspot.com/_j6mB7TMmJJY/SwYtJzwTCEI/AAAAAAAAAU0/L_i0ubRjVks/s400/P6.png)](https://3.bp.blogspot.com/_j6mB7TMmJJY/SwYtJzwTCEI/AAAAAAAAAU0/L_i0ubRjVks/s1600/P6.png)I have used the same technique in a previous P2P project where user's PC  behind their firewall need to communicate, and I know this relay  approach works very well.

# Multi-tenancy in cloud computing

Followup on an interesting discussion in Cloud Computing discussion  group.  What is a tenant ?  Is multi-tenancy an important feature of  cloud ?  Who are the participants and their roles in the cloud ecosystem ?

**Participants in the cloud**
In my model, a "**SaaS provider**" is the organization that provides a domain specific SaaS App to its  users (e.g. SmugMug for photo sharing).  In this case, the **SaaS consumer** is just any individual who has a SmugMug account.  The SaaS provider may choose an **infrastructure provider** (e.g. Amazon) to host its SaaS App.  In this example, SmugMug is a SaaS provider and **Infrastructure consumer** at the same time.


**Definition of a Tenant**
Now, who is the "tenant" in this picture.  I think Amazon will consider  SmugMug as a tenant.  But I doubt SmugMug will consider its individual  user a tenant.

But what if SmugMug offer a services to car  manufacturers so they can store, organize and image process their  photos, which will show up in the car manufacturer's website.  Will  SmugMug consider BMW a tenant ?  I think the answer is "yes".  So maybe  the definition of a tenant is "**my user who has her own users**".

You can see there can be a value chain built up.  So except the start and  end point of this value chain, everyone is a "tenant" to its service  provider.

**Multi-tenancy**
After we defne what a "tenant" is, what does "multi-tenancy" mean ?  In my  opinion, "multi-tenancy" is for the benefit of the service provider so  they can manage the resource ultization more efficiently, but  multi-tenancy is not to the tenant's advantage at all.  In the fake  example I gave above, would BMW prefers a multi-tenancy environment from SmugMug ?  My guess is that BMW would in fact worry if their data is  sitting together with their competitors in a shared infrastructure.  I  bet they would prefer an environment which is isolated as much as  possible.

While "multi-tenancy" indicates that some  infrastructure is shared, at what layers are things being shared can  make a big difference.  For example, Amazon AWS is multi-tenant at the  hardware level in that its users may be sharing a physical machine.  On  the other hand, Force.com is multi-tenant at the DB level in that its  users are sharing data in the same DB tables.  And Amazon is relying on  the hypervisor to provide the isolation between tenants while Force.com  is relying on a query rewriter to do the same.

While  "multi-tenancy" at the highest layer basically advocates a shared-DB  approach, does it enables better collaboration or sharing between  tenants ?  I don't think so.  I think all we need is to have an  authentication model such that spontaneous workgroup can be formed and  membership can be identified.  Then it is just a matter of a requesting  tenant to presents his membership to another tenant when making a SaaS  service call.  What I mean is they are using an SOA approach to access  data, rather than directly access a shared-DB.

# Skinny Straw in the Cloud Shake

There is recently[ an article by Bernard Golden](http://tinyurl.com/lrvctl) talking about network constraint (bandwidth and latency) as well as the associated bandwidth usage cost continues to become one main obstacle  in cloud computing.

There are two concerns here.  One is about  not meeting the application's performance goal (throughput and response  time).  The other is about the cost of running in the cloud.  (receive a large phone bill from your cloud provider)

The goal is to reduce the total amount of data transfer.  A number of cloud app design patterns can be used ...

How do you put the code and data together before the processing can start ?

**Try to be as stateless as possible**
There is zero data data transfer to be transferred if your component is  stateless by nature.  Following techniques are assuming that there are  some unavoidable stateful components involved.

**Move your data creation process into the cloud first
**Instead of uploading huge volume of data from your data center into the cloud  so processing can be started, can you move the data creation process  into the cloud ?  Of course, you need to carefully evaluate the security implications here.

**Distribute the architecture of your data creation**
If the subsequent processing is based on a parallel execution  architecture, why not distribute the data creation processing also.   This will save a data repartition step.

**Move the code to the data**
Code usually has a much smaller footprint than the data it processes.   Therefore it is more economical to move processing logic to the data  rather than downloading the data to process.  Of course, we need to  check to make sure the machine hosting the data has enough CPU power to  execute the processing logic.

**Do as much as possible along current partition**
A typically parallel processing architecture partitions data along some  dimensions, conduct the processing in parallel, and then repartition  data along other dimensions, conduct the next stage of processing, and  so on ...

See if you can rearrange the order of processing such  that you can do as much as possible within the current partition.  The  goal is to minimize the number of repartitions where a lot of data  transfer is needed.

**Minimize data redistribution at grow/shrink
**How do you redistribute data to newly joined VM such that the overall data  transfer can be minimized ?  For example, "consistent hashing" algorithm can be used such that data redistribution only happens within the  neighbor of newly joined VM rather than every other existing VMs.

**Conduct data redistribution in the background**
Data redistribution should have an impact on performance but not accuracy.   In other words, the newly joined VMs should be able to serve immediately while doing data redistribution in the background.  The data  redistribution algorithm (which may take a longer time to finish) also  need to adapt to continuous joining VMs.  In other words, data  redistribution can be just an ongoing performance improvement process in a highly dynamic workload environment.

**Place component with bandwidth cost in mind**
Other than the amount of data being transferred (which should be minimized  anyway), it is equally important to look into bandwidth cost.  Typically the cloud provider will charge a substantial amount in bandwidth usage  across the cloud boundary.  Therefore, it is important to place the  components such that if data transfer do need to occur, it will occur  within the cloud rather than across the cloud boundary.  This requires a careful analysis of the communication pattern among application  components and group frequently communicating components so they will be deployed within the same cloud.

**Migrate data as communication pattern changes**
Communication pattern may change after the system is deployed.  It is important to  continuously monitor the actual communication patterns and determine if a migration is needed to minimize the bandwidth cost.  It is important to consider the gain versus the cost of migration.  Gain is estimated by  multiplying the communication frequency with the time that the new  communication pattern is going to persist.  Cost is estimated by the  total among of data redistribution traffic caused by component  migration.  And only when the migration cost is smaller than the gain  will the migration take place.

**Exploit Caching**
Use a local cache to reduce the need of data access, especially if the data is relatively static.

**Allow direct access to data
**This is against the philosophy of SOA where the internal state should be  encapsulated behind an API interface.  In this model, when a client want to extract the data, it need to first make a request to the owning  application, which then make a request to the DB, get the data, encode  that into the web service response, and pass the result back to the  client.  Is network bandwidth is costly, it will be much more efficient  if the client can have direct access to the DB.

**Expose latency information to the application**
Provide latency map so application can dynamically adjust their communication partners who they want to communicate with.
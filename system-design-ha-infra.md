# An introduction to High Availability Architecture

In the real world, there can be situations when a dip in performance of your servers might occur from events ranging from a sudden spike in traffic can lead to a sudden power outage. It can be much worse and your servers can be crippled- irrespective of whether your applications are hosted in the cloud or a physical machine. Such situations are unavoidable. However, rather than hoping that it doesn’t occur, what you should actually do is to gear up so that your systems don’t encounter failure.

The answer to the problem is the use of High Availability (HA) configuration or architecture. High availability architecture is an approach of defining the components, modules or implementation of services of a system which ensures optimal operational performance, even at times of high loads. Although there are no fixed rules of implementing HA systems, there are generally a few good practices that one must follow so that you gain the most out of the least resources.

## Why do you need it?

Let us define downtime before we move further. Downtime is the period of time when your system (or network) is not available for use, or is unresponsive. Downtime can cause a company huge losses, as all their services are put on hold when their systems are down. In August 2013, [Amazon went down for 15 minutes](http://smallbiztrends.com/2013/08/amazon-down-custom-error-page.html) (both web and mobile services), and ended up losing over $66000 per minute. Those are huge numbers, even for a company of the size of Amazon.

There are two types of downtimes- scheduled and unscheduled. A scheduled downtime is a result of maintenance, which is unavoidable. This includes applying patches, updating software or even changes in the database schema. An unscheduled downtime is, however, caused by some unforeseen event, like hardware or software failure. This can happen due to power outages or failure of a component. Scheduled downtimes are generally excluded from performance calculations.

The prime objective of implementing High Availability architecture is to make sure you system or application is configured to handle different loads and different failures with minimal or no downtime. The are multiple components that help you in achieving this, and we will be discussing them briefly.

## How is availability measured?

Organizations who plan to fully utilize a cloud infrastructure must also be capable of meeting demands for 24/7 availability. Availability can be measured as the percentage of time that systems are available.

> ## *x = (n – y) \* 100/n*

Where n is the total number of minutes in a calendar month and y is the total number of minutes that service is unavailable in the given calendar month. **High availability** simply refers to a component or system that is continuously operational for a desirably long period of time. The widely-held but almost impossible to achieve standard of availability for a product or system is referred to as ‘five 9s’ (99.999 percent) availability. High availability is a requirement for any enterprise that hopes to protect their business against the risks brought about by a system outage. These risks, can lead to millions of dollars in revenue loss.

## Is it really worth the money?

The fact that going for high availability architecture gives you higher performance is all right, but it comes at a big cost too. You must ask yourself if you think the decision is justified from the point of view of finance.

A decision must be made on whether the extra uptime is truly worth the amount of money that has to go into it. You must ask yourself how damaging potential downtimes can be for your company and how important your services are in running your business.

## How do we achieve it?

Now that you have decided to go with it, let’s discuss the ways to implement it. Non-intuitively, adding more components to a system doesn’t help in making it more stable and achieve high availability. It can actually lead to the opposite as more components increases the probability of failures. Modern designs allow for distribution of the workloads across multiple instances such as a network or a cluster, which helps in optimizing resource use, maximizing output, minimizing response times and avoiding overburden of any system in the process known as load balancing. It also involves switching to a standby resource like a server, component or network in the case of failure of an active one, known as Failover systems.

### 1. Use of Multiple Application Servers:

Imagine that you have a single server to render your services and a sudden spike in traffic leads to its failure (or crashes it). In such a situation, until your server is restarted, no more requests can be served, which leads to a downtime.

The obvious solution here is to deploy your application over multiple servers. You need to distribute the load among all these, so that none of them are overburdened and the output is optimum. You can also deploy parts of your application on different servers. For instance, there could be a separate server for handling mails or a separate one for processing static files like images (like a Content Delivery Network).

### 2. Scaling Databases:

Databases are the most popular and perhaps one of the most conceptually simple ways to save user data. One must remember that databases are equally important to your services as your application servers. Databases run on separate servers (like the [Amazon RDS](http://aws.amazon.com/rds/)) and are prone to crashes as well. What’s worse is that databases crashes can lead to a loss of user data, which can prove to be costly.

Redundancy is a process which creates systems with high levels of availability by achieving failure detectability and avoiding common cause failures. This can be achieved by maintaining slaves, which can step in if the main server crashes. Another interesting concept of scaling databases is sharding. A shard is a horizontal partition in a database, where rows of the same table  which is then run on a separate server.

### 3. Diversified Geographical Locations:

Scaling up your applications and then your databases is a really big step ahead, but what if all the servers are at the same physical location and something terrible like a natural disaster affects the data center at which your servers are located? This can lead to potentially huge downtimes.

It is, therefore, imperative that you keep your servers in different locations. Most modern web services allow you to select the geographical location of your servers. You should choose wisely to make sure your servers are distributed all over the world and not localized in an area.

Within this post, I have tried to touch upon the basic ideas that form the idea of high availability architecture. In the final analysis, it is evident that no single system can solve all the problems. Hence, you need to assess your situation carefully and decide on what options suit them best. Here’s hoping that this introduced you to the world of high availability architecture and helped you decide how to go about achieving this for yourself.

## What are the best practices?

In order to curb system failures and keep both planned and unplanned downtimes at bay, the use of a High Availability (HA) architecture is highly recommended, especially for mission-critical applications. Availability experts insist that for any system to be highly available, its parts should be well designed and rigorously tested. The design and subsequent implementation of a high availability architecture can be difficult given the vast range of software, hardware and deployment options. However, a successful effort typically starts with distinctly defined and comprehensively understood business requirements. The chosen architecture should be able to meet the desired levels of security, scalability, performance and **availability**.

The only way to guarantee compute environments have a desirable level of operational continuity during production hours is by designing them with high availability. In addition to properly designing the architecture, enterprises can keep crucial applications online by observing the recommended best practices for high availability.

1. ***Data Backups, Recovery and Replication***

The hallmark of a good data protection plan that protects against system failure is a sound backup and recovery strategy. Valuable data should never be stored without proper backups, replication or the ability to recreate the data. Every data center should plan for data loss or corruption in advance. Data errors may create customer authentication issues, damage financial accounts and subsequently business community credibility. The recommended strategy for maintaining data integrity is creating a full backup of the primary database then incrementally testing the source server for data corruptions. Creating full backups is at the forefront of recovering from catastrophic system failure.

1. ***Clustering***

Even with the highest quality of software engineering, all application services are bound to fail at some point. High availability is all about delivering application services regardless of failures. Clustering can provide instant failover application services in the event of a fault. An application service that is ‘cluster aware’ is capable of calling resources from multiple servers; it falls back to a secondary server if the main server goes offline. A High Availability cluster includes multiple nodes that share information via shared data memory grids. This means that any node can be disconnected or shutdown from the network and the rest of the cluster will continue to operate normally, as long as at least a single node is fully functional. Each node can be upgraded individually and rejoined while the cluster operates. The high cost of purchasing additional hardware to implement a cluster can be mitigated by setting up a virtualized cluster that utilizes the available hardware resources.

1. ***Network Load Balancing***

Load balancing is an effective way of increasing the availability of critical web-based applications. When server failure instances are detected, they are seamlessly replaced when the traffic is automatically redistributed to servers that are still running. Not only does load balancing lead to high availability it also facilitates incremental scalability. Network load balancing can be accomplished via either a ‘pull’ or a ‘push’ model. It facilitates higher levels of fault tolerance within service applications.

1. ***Fail Over Solutions***

High availability architecture traditionally consists of a set of loosely coupled servers which have failover capabilities. Failover is basically a backup operational mode in which the functions of a system component are assumed by a secondary system in the event that the primary one goes offline, either due to failure or planned down time. A *‘cold failover’* occurs when the secondary server is only started after the primary one has been completely shut down. A ‘*hot failover*’ occurs when all the servers are running simultaneously, and the load is directed entirely towards a single server at any given time. In both scenarios, tasks are automatically offloaded to a standby system component so that the process remains as seamless as possible to the end user. Failover can be managed via DNS, in an well-controlled environment.

1. ***Geographic redundancy***

Geo-redundancy is the only line of defense when it comes to preventing service failure in the face of catastrophic events such as natural disasters that cause system outages. Like in the case of geo-replication, multiple servers are deployed at geographical distinct sites. The locations should be globally distributed and not localized in a specific area. It is crucial to run independent application stacks in each of the locations, so that in case there is a failure in one location, the other can continue running. Ideally, these locations should be completely independent of each other.

1. ***Plan for failure***

Despite the fact that applying the best practices for high availability is essentially planning for failure; there are other actions an organization can take to increase their preparedness in the event of a system failure leading to downtime. Organizations should keep failure or resource consumption data that can be used to isolate problems and analyze trends. This data can only be gathered through continuous monitoring of operational workload. A recovery help desk can be put in place to gather problem information, establish problem history, and begin immediate problem resolutions. A recovery plan should not only be well documented but also tested regularly to ensure its practicality when dealing with unplanned interrupts. Staff training on availability engineering will improve their skills in designing, deploying, and maintaining high availability architectures. Security policies should also be put in place to curb incidences of system outages due to security breaches.

 
**Example: FileCloud High Availability Architecture**
Following diagram explains how FileCloud servers can be configured for High Availability to improve service reliability and reduce downtime. [Click here for more details.](https://www.getfilecloud.com/supportdocs/display/cloud/FileCloud+High+Availability)

![High Availability Architecture Example](https://www.getfilecloud.com/supportdocs/download/attachments/8160924/Capture.JPG?version=1&modificationDate=1421171438000&api=v2)

--------------------

# What is High Availability?

### Introduction

With an increased demand for reliable and performant infrastructures designed to serve critical systems, the terms scalability and high availability couldn’t be more popular. While handling increased system load is a common concern, decreasing downtime and eliminating single points of failure are just as important. High availability is a quality of infrastructure design at scale that addresses these latter considerations.

In this guide, we will discuss what exactly high availability means and how it can improve your infrastructure’s reliability.



## What Is High Availability?

In computing, the term availability is used to describe the period of time when a service is available, as well as the time required by a system to respond to a request made by a user. High availability is a quality of a system or component that assures a high level of operational performance for a given period of time.

### Measuring Availability

Availability is often expressed as a percentage indicating how much uptime is expected from a particular system or component in a given period of time, where a value of 100% would indicate that the system never fails. For instance, a system that guarantees 99% of availability in a period of one year can have up to 3.65 days of downtime (1%).

These values are calculated based on several factors, including both scheduled and unscheduled maintenance periods, as well as the time to recover from a possible system failure.



## How Does High Availability Work ?

High availability functions as a failure response mechanism for infrastructure. The way that it works is quite simple conceptually but typically requires some specialized software and configuration.



## When Is High Availability Important ?

When setting up robust production systems, minimizing downtime and service interruptions is often a high priority. Regardless of how reliable your systems and software are, problems can occur that can bring down your applications or your servers.
​
Implementing high availability for your infrastructure is a useful strategy to reduce the impact of these types of events. Highly available systems can recover from server or component failure automatically.



## What Makes a System Highly Available?

One of the goals of high availability is to eliminate single points of failure in your infrastructure. A single point of failure is a component of your technology stack that would cause a service interruption if it became unavailable. As such, any component that is a requisite for the proper functionality of your application that does not have redundancy is considered to be a single point of failure.
​
To eliminate single points of failure, each layer of your stack must be prepared for redundancy. For instance, imagine you have an infrastructure consisting of two identical, redundant web servers behind a load balancer. The traffic coming from clients will be equally distributed between the web servers, but if one of the servers goes down, the load balancer will redirect all traffic to the remaining online server.

The web server layer in this scenario is not a single point of failure because:

- redundant components for the same task are in place
- the mechanism on top of this layer (the load balancer) is able to detect failures in the components and adapt its behavior for a timely recovery

But what happens if the load balancer goes offline?

With the described scenario, which is not uncommon in real life, the load balancing layer itself remains a single point of failure. Eliminating this remaining single point of failure, however, can be challenging; even though you can easily configure an additional load balancer to achieve redundancy, there isn’t an obvious point above the load balancers to implement failure detection and recovery.

Redundancy alone cannot guarantee high availability. A mechanism must be in place for detecting failures and taking action when one of the components of your stack becomes unavailable.

Failure detection and recovery for redundant systems can be implemented using a top-to-bottom approach: the layer on top becomes responsible for monitoring the layer immediately beneath it for failures. In our previous example scenario, the load balancer is the top layer. If one of the web servers (bottom layer) becomes unavailable, the load balancer will stop redirecting requests for that specific server.

![Diagram 01: Load Balancers / Top-to-bottom](https://assets.digitalocean.com/articles/high-availability/Diagram_2.png)

This approach tends to be simpler, but it has limitations: there will be a point in your infrastructure where a top layer is either nonexistent or out of reach, which is the case with the load balancer layer. Creating a failure detection service for the load balancer in an external server would simply create a new single point of failure.

With such a scenario, a distributed approach is necessary. Multiple redundant nodes must be connected together as a cluster where each node should be equally capable of failure detection and recovery.

![Diagram 02: Cluster / Distributed](https://assets.digitalocean.com/articles/high-availability/Diagram_1.png)

For the load balancer case, however, there’s an additional complication, due to the way nameservers work. Recovering from a load balancer failure typically means a failover to a redundant load balancer, which implies that a DNS change must be made in order to point a domain name to the redundant load balancer’s IP address. A change like this can take a considerable amount of time to be propagated on the Internet, which would cause a serious downtime to this system.

A possible solution is to use [DNS round-robin load balancing](https://www.digitalocean.com/community/tutorials/how-to-configure-dns-round-robin-load-balancing-for-high-availability). However, this approach is not reliable as it leaves failover the client-side application.

A more robust and reliable solution is to use systems that allow for flexible IP address remapping, such as [floating IPs](https://www.digitalocean.com/community/tutorials/how-to-use-floating-ips-on-digitalocean). On demand IP address remapping eliminates the propagation and caching issues inherent in DNS changes by providing a static IP address that can be easily remapped when needed. The domain name can remain associated with the same IP address, while the IP address itself is moved between servers.

This is how a highly available infrastructure using Floating IPs looks like:

![Diagram 03: Floating IPs](https://assets.digitalocean.com/articles/high_availability/ha-diagram-animated.gif)



## What System Components Are Required for High Availability?

There are several components that must be carefully taken into consideration for implementing high availability in practice. Much more than a software implementation, high availability depends on factors such as:

- **Environment:** if all your servers are located in the same geographical area, an environmental condition such as an earthquake or flooding could take your whole system down. Having redundant servers in different datacenters and geographical areas will increase reliability.
- **Hardware:** highly available servers should be resilient to power outages and hardware failures, including hard disks and network interfaces.
- **Software:** the whole software stack, including the operating system and the application itself, must be prepared for handling unexpected failure that could potentially require a system restart, for instance.
- **Data:** data loss and inconsistency can be caused by several factors, and it’s not restricted to hard disk failures. Highly available systems must account for data safety in the event of a failure.
- **Network:** unplanned network outages represent another possible point of failure for highly available systems. It is important that a redundant network strategy is in place for possible failures.



## What Software Can Be Used to Configure High Availability?

Each layer of a highly available system will have different needs in terms of software and configuration. However, at the application level, load balancers represent an essential piece of software for creating any high availability setup.

[HAProxy](https://www.digitalocean.com/community/tutorials/an-introduction-to-haproxy-and-load-balancing-concepts) (High Availability Proxy) is a common choice for load balancing, as it can handle load balancing at multiple layers, and for different kinds of servers, including [database servers](https://www.digitalocean.com/community/tutorials/how-to-use-haproxy-to-set-up-mysql-load-balancing--3).

Moving up in the system stack, it is important to implement a reliable redundant solution for your application entry point, normally the load balancer. To remove this single point of failure, as mentioned before, we need to implement a cluster of load balancers behind a Floating IP. Corosync and Pacemaker are popular choices for creating such a setup, on both [Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-create-a-high-availability-setup-with-corosync-pacemaker-and-floating-ips-on-ubuntu-14-04) and [CentOS](https://www.digitalocean.com/community/tutorials/how-to-create-a-high-availability-setup-with-pacemaker-corosync-and-floating-ips-on-centos-7) servers.



## Conclusion

High availability is an important subset of reliability engineering, focused towards assuring that a system or component has a high level of operational performance in a given period of time. At a first glance, its implementation might seem quite complex; however, it can bring tremendous benefits for systems that require increased reliability.
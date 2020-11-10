# **How to build a multi-region active-active architecture on AWS**

[ref: https://read.acloud.guru/why-and-how-do-we-build-a-multi-region-active-active-architecture-6d81acb7d208]

This is the second part of my series on building a multi-region, active-active architectures. In the previous post, we talked about the [**Quest for Availability**](https://read.acloud.guru/the-quest-for-availability-771fa8a94a7c) since it seats at the heart of such design.

In this post, I will discuss the **why** and the **how** of designing multi-region, active-active architectures.

Bear in mind, that building and successfully running multi-region, active-active architecture is hard, so this post does not pretend to cover everything involved with doing that. Instead, it should be treated as an introduction to this art.

## Why bother with multi-region architectures?

Good question and glad you asked! There are basically three reasons why you would want to have a multi-region architecture.

1. **Improve latency for end-users**
2. **Disaster recovery**
3. **Business requirements**

## **1. Improve latency for end-users**

The idea is very simple and is related to the speed of light, which no one has yet managed to crack. The closer your backend origin is to end-users, the better the experience.

Content Delivery Networks (CDN) like [Amazon CloudFront](https://aws.amazon.com/cloudfront/) have successfully been used to speed up the delivery of content, especially static one (e.g., images, videos, JavaScript libraries, etc.) to end-users across the globe. Using a globally-distributed network of caching servers, static content is served as if it was local to consumers, thus improving the delivery of that static content.

However, even if CloudFront solves the problem for much of your content, some more dynamic calls still need to be done on the backend, and it could be far away, adding precious milliseconds to the request.

![Image for post](https://miro.medium.com/max/60/1*-rGhWmNF23-Hfx4uQWR5LA.png?q=20)

![Image for post](https://miro.medium.com/max/1015/1*-rGhWmNF23-Hfx4uQWR5LA.png)

For example, if you have users in Europe but your backend is in US or Australia, the added latency is respectively approximately 140ms and 300ms. Those delays would be unacceptable to start with for many popular games, banking requirements or interactive applications.

Indeed, latency plays a huge part in customer’s perception of a high-quality experience and was proved to impact the behaviour of users to some noticeable extent: with [lower latency generating more user engagements](https://dl.acm.org/citation.cfm?id=2609627).

This observation has also been confirmed several times by few large companies:

- **Amazon**: 100 ms of extra load time caused a 1% drop in sales (Greg Linden, source [here](http://stanforddatamining.2006-11-29.ppt/)).
- **Google**: 500 ms of extra load time caused 20% fewer searches (Marissa Mayer, source [here](https://books.google.fi/books?id=UPY7Tt_nlBAC&lpg=PA105&ots=_w_03082nU&dq=Google 500 ms of extra load time 20% fewer searches Marrissa Mayer&pg=PA105#v=onepage&q=Google 500 ms of extra load time 20% fewer searches Marrissa Mayer&f=false)).
- **Yahoo!**: 400 ms of extra load time caused a 5–9% increase in the number of people who clicked “back” before the page even loaded (Nicole Sullivan, source [here](https://books.google.fi/books?id=UPY7Tt_nlBAC&lpg=PA105&ots=_w_03082sS&dq=400 ms of extra load time caused a 5–9% increase in the number of people who clicked “back”&pg=PA105#v=onepage&q=400 ms of extra load time caused a 5–9% increase in the number of people who clicked “back”&f=false)).

As technology improves and especially with the advent of AR, VR and MR, requiring even more immersive and lifelike experiences, developers now need to produce software systems with stringent latency requirements. Therefore, having locally available applications and content is becoming more and more important.

## **2. Disaster recovery**

On Christmas Eve 2012, [Netflix streaming service experienced an outage](https://medium.com/netflix-techblog/a-closer-look-at-the-christmas-eve-outage-d7b409a529ee) caused by the AWS [ELB control plane](https://aws.amazon.com/message/680587/). The ELB failure was outside of Netflix’s ability to correct.

What followed was, in my opinion one of the most significant and inspiring engineering work done by any AWS customer— achieving **regional resiliency**.

![Image for post](https://miro.medium.com/max/60/1*e1Xht1m5hZF5S07my4gbZg.png?q=20)

![Image for post](https://miro.medium.com/max/1734/1*e1Xht1m5hZF5S07my4gbZg.png)

Regional Resiliency at work.

> Netflix is designed to handle failure of all or part of a single availability zone in a region as we run across three zones and operate with no loss of functionality on two. We are working on ways of **extending our resiliency to handle partial or complete regional outages**. Adrian Cockcroft. ([link](https://medium.com/netflix-techblog/a-closer-look-at-the-christmas-eve-outage-d7b409a529ee))

Indeed, if your application, like Netflix, is composed of multiple different services and if one of these services, critical to your application, experiences issues, you might want to shift the traffic to a healthy region to prevent angry customers.

> It is not possible to prevent threats to availability, but it is possible to mitigate them.

Netflix was not the only one to learn from this ELB failure of 2012— AWS too has learned from that failure and has [taken steps](https://aws.amazon.com/message/680587/) into reducing the blast radius of potential failures and today’s implementation of the control plane is more robust.

Failures always happen and when they do, it is important to work both on reducing the occurrence of problems and also to work on mitigating the severity of impact of problems.

<iframe src="https://cdn.embedly.com/widgets/media.html?type=text%2Fhtml&amp;key=a19fcc184b9711e1b4764040d3dc5c07&amp;schema=twitter&amp;url=https%3A//twitter.com/werner/status/1062807131&amp;image=https%3A//i.embed.ly/1/image%3Furl%3Dhttps%253A%252F%252Fpbs.twimg.com%252Fprofile_images%252F563188960394891264%252Fz3-Rh11q_400x400.jpeg%26key%3Da19fcc184b9711e1b4764040d3dc5c07" allowfullscreen="" frameborder="0" height="224" width="680" title="Werner Vogels on Twitter" class="t u v ic aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 224px;"></iframe>

## **3. Business requirements**

Finally, some customers may have business requirements to store data in distinct regions, separated by several hundreds of kilometres. Therefore, those customers have to store data in multiple regions. This is becoming more and more common since [AWS has now 18 regions globally](https://aws.amazon.com/about-aws/global-infrastructure/), spread between the Americas, Asia Pacific, Europe, Middle East and Africa.

## How to build multi-region active-active architecture

Simply put, a multi-region, active-active architecture gets all the services on the client request path deployed across multiple AWS Regions. In order to do so, several requirements have to be fulfilled.

1. Data replication between regions must be **fast** **and** **reliable**.
2. Having a **global** **network infrastructure** to connect your different regions.
3. Services should not have local state — they must be **stateless**, and state should be shared between regions.
4. Synchronous cross-regional calls should be avoided when possible. Applications should **use regional resources**.
5. **DNS routing** should be used to permit for different scenarios.

Let’s take a closer look at these requirements.

## 1 — Reliable Data Replication

Let’s talk a little bit about the [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem). The CAP theorem states that it is impossible for a distributed system to simultaneously provide more than two out of the following three guarantees: [**Consistency**](https://en.wikipedia.org/wiki/Consistency_model)**,** [**Availability**](https://en.wikipedia.org/wiki/Availability) and [**Partition Tolerance**](https://en.wikipedia.org/wiki/Network_partitioning)**.** But especially that in the presence of a network partition, one has to choose between consistency and availability.

This means that we have two choices: giving up consistency will allow the system to remain highly available, prioritising on consistency means that the system might not always be available.

Since we are in building a multi-region architecture and are optimising for availability, we have to give up consistency — by design; This also means we need to **embrace asynchronous systems and replication**.

![Image for post](https://miro.medium.com/max/60/1*ETiKPw0DVSPcNiJOIGo4Dw.png?q=20)

![Image for post](https://miro.medium.com/max/1326/1*ETiKPw0DVSPcNiJOIGo4Dw.png)

Synchronous vs Asynchronous pattern

For distributed data stores, asynchronous replication decouples the primary node from its replicas at the expense of introducing replication lag or latency.

This means that changes performed on the primary node are not immediately reflected on its replicas — the result of this lag created what is often referred to as [eventual consistency](https://en.wikipedia.org/wiki/Eventual_consistency). When a system achieves eventual consistency, it is said to have converged, or achieved replica convergence.

To achieve replica convergence, a system must reconcile differences between multiple copies of distributed data. It can do so by doing the following reconciliations:

- Comparing versions of the data.
- “Smart” comparison of the data itself.
- Choosing an arbitrary final state.

The most common approach to reconciliation, and also the one used in most systems, including DynamoDB Global Tables, is called the *“last writer wins”*.

The effect of **asynchronous replication** must be taken into consideration when designing applications since besides having architectural consequences, it also has some implications for the client user-interface design and experience.

Such implications are that interfaces should be completely non-blocking. User interactions and actions should resolve instantly without the need to wait for any backend response — everything should resolve itself in the background, asynchronously and transparently to the user.

No loading messages or spinners staying forever on the screen. Requests to the server should be entirely decoupled from the user interface. This “trick” will also make users believe the application is fast, even if in reality, it isn’t — hiding network latency and even full-service failure.

This is often referred to as **graceful degradation** and it also used by Netflix to mitigate certain failures.

> “We might fail a service that generates the personalized list of movies that are shown to the user, which is determined based on their viewing history. When this service fails, the system should return a default (i.e., nonpersonalized) list of movies.” [Netflix — Chaos Engineering](http://www.oreilly.com/webops-perf/free/files/chaos-engineering.pdf)

## 2 — Global Network Infrastructure

A few years ago, when deploying multi-regions architecture, it was standard practice to setup secured VPN connections between regions in order to replicate the data asynchronously.

While deploying and managing those connections has become easier, the main problem was that they went over the Internet, therefore were subject to sudden change in routing and especially latency — making it difficult to maintain a consistently good replication.

![Image for post](https://miro.medium.com/max/60/1*_HlloqwPf-F9f3WWcAtGtA.png?q=20)

![Image for post](https://miro.medium.com/max/829/1*_HlloqwPf-F9f3WWcAtGtA.png)

James Hamilton — AWS re:Invent 2016

To overcome that problem, James Hamilton, Vice President & Distinguished Engineer at AWS, [announced](https://www.youtube.com/watch?v=uj7Ting6Ckk) that AWS was now providing a high bandwidth, global network infrastructure powered by redundant 100GbE links circling the globe.

This means that AWS Regions are now connected to a private global network backbone, which provides lower cost and more consistent cross-region network latency when compared with the public internet — and the benefits are clear:

1. Improved latency, packet loss, & overall quality.
2. Avoid network interconnect capacity conflicts.
3. Greater operational control.

> “If you’ve got a packet, the more people that touch it, the less likely it is to get delivered.” James Hamilton, re:Invent 2016.

## 3 — Stateless Applications

I previously wrote about the [local state being a cloud anti-pattern](https://hackernoon.com/10-lessons-from-10-years-of-aws-part-1-258b56703fcf). This is even more true for multi-region architecture. When clients interact with an application they do so in a series of interactions called a session.

In a stateless architecture, the server must treat all client requests independently of prior requests or sessions and should not stores any session information locally. So given the same input, a stateless application should provide the same response to any end-user.

Stateless applications can scale horizontally since any request can be handled by any available computing resources (e.g., instances, containers or functions).

![Image for post](https://miro.medium.com/max/538/1*HbJKa9tYyaDHWxEo_UNMTg.png)

Stateful Applications

Sharing state with **any** instances, containers or functions is possible by using in-memory object caching systems like [Memcached](http://memcached.org/), [Redis](https://redis.io/), [EVCache](https://github.com/netflix/evcache/wiki), or distributed databases like [Cassandra](http://cassandra.apache.org/) or [DynamoDB](https://aws.amazon.com/dynamodb/) (*more later*) depending on the structure of your object and your requirements in terms of performances.

Netflix, already in 2013, famously [talked](https://www.youtube.com/watch?v=hAyA86QGRnI) and [wrote](https://medium.com/netflix-techblog/active-active-for-multi-regional-resiliency-c47719f6685b) about testing Cassandra in multi-region setup; writing 1 million records in one region of a multi-region cluster, followed by a read, 500ms later, in another region, while keeping a production level of load on the cluster — all that without any data loss.

## 4 — Use local resources and avoid cross-regional calls

As mentioned previously, preventing increased latency is critical for applications. Therefore, it is important to avoid synchronous cross-region calls and always make sure resources are locally available for the application to use, thus optimising latency.

For example, objects stored into an Amazon S3 bucket should be replicated in multiple regions to allow for local access from any region. Luckily, Amazon has recently implemented the feature called [cross-region replication](https://docs.aws.amazon.com/AmazonS3/latest/dev/crr.html) for Amazon S3. Cross-region replication is a bucket-level configuration that enables automatic, asynchronous copying of objects across buckets in different AWS Regions.

![Image for post](https://miro.medium.com/max/60/1*SMawCtVcSkQ6ZaQMZ0Vd7Q.png?q=20)

![Image for post](https://miro.medium.com/max/1251/1*SMawCtVcSkQ6ZaQMZ0Vd7Q.png)

S3 Cross Region replication

This local access of resources also applies for databases. To support this scenario, AWS launched, already in 2012, [Cross-Region Read Replicas for Amazon RDS for MySQL followed by MariaDB, PostgreSQL](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReadRepl.html) and eventually [Amazon Aurora](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/AuroraMySQL.Replication.CrossRegion.html).

![Image for post](https://miro.medium.com/max/60/1*lA2vZQAnJxvIqmeTex-O3A.png?q=20)

![Image for post](https://miro.medium.com/max/1408/1*lA2vZQAnJxvIqmeTex-O3A.png)

Cross-Region Read Replicas for Amazon RDS MySQL, MariaDB, PostgreSQL and Amazon Aurora.

Separating the writes from the reads across multiple regions will improve your disaster recovery capabilities, but it will also let you scale read operations into a region that is closer to your users — and make it easier to migrate from one region to another.

The main restriction with this pattern is that all critical writes traffic must go to one single master, in the region of origin.

![Image for post](https://miro.medium.com/max/60/1*Zkulfkw-4jq07IEEQUh1bw.png?q=20)

![Image for post](https://miro.medium.com/max/1248/1*Zkulfkw-4jq07IEEQUh1bw.png)

Reads (green) and Writes (red) patterns.

Please remember that in order to work with cross-region read replicas, you must embrace eventual consistency as discussed above — due to the replication of data being asynchronous.

*Note: Using RDS, you can monitor this replication lag by using* [*Amazon CloudWatch*](http://aws.amazon.com/cloudwatch/) *and raise an alert if it reaches a level that is unacceptably high for your application.*

To prevent having cross-region writes on the database, AWS recently announced at re:Invent 2017 the launch of a new feature: [multi-master capability to Amazon Aurora](https://aws.amazon.com/about-aws/whats-new/2017/11/sign-up-for-the-preview-of-amazon-aurora-multi-master/), first within a single region but multi-region by the end of 2018.

![Image for post](https://miro.medium.com/max/60/1*SqUwu8tKcgrMnl_2wDj38g.png?q=20)

![Image for post](https://miro.medium.com/max/1500/1*SqUwu8tKcgrMnl_2wDj38g.png)

Andy Jassy, CEO of AWS, announcing Aurora Multi-Master at re:Invent 2017.

Multi-Master clusters improve Aurora’s already high availability. If one of your master instances fail, the other instances in the cluster will take over immediately, maintaining read and write availability through instance failures or even complete AZ failures, with zero application downtime.

*Note: Amazon Aurora Multi-Master is now available in Preview for the MySQL-compatible edition of Amazon Aurora. You can* [*sign up*](https://pages.awscloud.com/amazon-aurora-multimaster-preview.html) *to request participation.*

But that’s not all, something more was announced at re:Invent 2017 — [**DynamoDB Global Table**](https://aws.amazon.com/dynamodb/global-tables/) **—** a fully managed, **multi-region, and multi-master** database, allowing for the first time to build globally distributed applications.

![Image for post](https://miro.medium.com/max/60/1*n9f2wYeTZrhPxcX-yfzeig.png?q=20)

![Image for post](https://miro.medium.com/max/813/1*n9f2wYeTZrhPxcX-yfzeig.png)

Global applications powered by DynamoDB Global Tables.

A DynamoDB global table consists of multiple replica tables, one per region of choice *(currently 5 regions are supported)*, that DynamoDB treats as a single unit. Every replica has the same table name and the same primary key schema.

Applications can write data to any of the replica tables; DynamoDB automatically propagates the write to the other replica tables in the other AWS regions.

*Note: DynamoDB is the same database that is used at* ***Amazon.com\*** *during* [*Prime Days*](https://aws.amazon.com/blogs/aws/prime-day-2017-powered-by-aws/)*, and let me give you some numbers:*

> [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) requests from Alexa, the Amazon.com sites, and the Amazon fulfillment centers totalled **3.34 trillion**, peaking at **12.9 million per second**. ([source](https://aws.amazon.com/blogs/aws/prime-day-2017-powered-by-aws/))

## 5 — DNS routing

In order to route traffic between regions, we need to use a Domain Name System (DNS) which support configurable routing policies.

[Amazon Route 53](https://aws.amazon.com/route53/) provides highly available and scalable Domain Name System (DNS), domain name registration, and health-checking web services. But most importantly for our use case, it supports [traffic flow](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/traffic-flow.html) through a variety of [routing policies](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html), all of which can be combined with DNS failover.

1. **Geolocation routing policy** — Use when you want to route traffic based on the location of your users.
2. **Geoproximity routing policy** — Use when you want to route traffic based on the location of your resources and, optionally, shift traffic from resources in one location to resources in another.
3. **Latency routing policy** — Use when you have resources in multiple locations and you want to route traffic to the resource that provides the best latency.
4. **Multi-value answer routing policy** — Use when you want Route 53 to respond to DNS queries with up to eight healthy records selected at random.
5. **Weighted routing policy** — Use to route traffic to multiple resources in proportions that you specify.

![Image for post](https://miro.medium.com/max/60/1*x-mNZiERa1ORmoEwCFg_tw.png?q=20)

![Image for post](https://miro.medium.com/max/1020/1*x-mNZiERa1ORmoEwCFg_tw.png)

![Image for post](https://miro.medium.com/max/60/1*e8ycgIp31VfvaURWB6sijA.png?q=20)

![Image for post](https://miro.medium.com/max/1104/1*e8ycgIp31VfvaURWB6sijA.png)

Routing policies (Geoproximity and latency) with Route53.

## The Run-Down

In the post, we learned that in order to build a multi-region, active-active architecture, all the services on the client request path must be deployed across multiple AWS Regions, that we must embrace **asynchronous** **designs and architectures** and that we must build applications that are **fully** **stateless**.

Of course, we should leverage services like Amazon S3 or DynamoDB that are highly available and that benefit from the global network build by Amazon around the globe to have **reliable replication of data**. Finally, we also discussed the use of traffic flow in order to support different routing policies between AWS Regions.
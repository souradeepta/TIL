# What is a CDN? | How do CDNs work?

Explore how a CDN works under the hood to deliver fast, efficient and secure delivery of content to websites and Internet services.

## What is a CDN

A content delivery network (CDN) refers to a geographically distributed group of servers which work together to provide fast delivery of Internet content.

A CDN allows for the quick transfer of assets needed for loading Internet content including HTML pages, javascript files, stylesheets, images, and videos. The popularity of CDN services continues to grow, and today the majority of web traffic is served through CDNs, including traffic from major sites like Facebook, Netflix, and Amazon.

A properly configured CDN may also help protect websites against some common malicious attacks, such as [Distributed Denial of Service (DDOS) attacks](https://www.cloudflare.com/learning/ddos/what-is-a-ddos-attack/).

## Is a CDN the same as a web host?

While a CDN does not host content and can’t replace the need for proper web hosting, it does help [cache](https://www.cloudflare.com/learning/cdn/what-is-caching/) content at the [network edge](https://www.cloudflare.com/learning/serverless/glossary/what-is-edge-computing/), which improves website performance. Many websites struggle to have their [performance](https://www.cloudflare.com/learning/cdn/performance/) needs met by traditional hosting services, which is why they opt for CDNs.

By utilizing caching to reduce hosting bandwidth, [helping to prevent interruptions in service](https://www.cloudflare.com/learning/cdn/cdn-load-balance-reliability/), and [improving security](https://www.cloudflare.com/learning/cdn/cdn-ssl-tls-security/), CDNs are a popular choice to relieve some of the major pain points that come with traditional web hosting.

## What are the benefits of using a CDN?

Although the benefits of using a CDN vary depending on the size and needs of an Internet property, the primary benefits for most users can be broken down into 4 different components:

1. **Improving website load times** - By distributing content closer to website visitors by using a nearby CDN server (among other optimizations), visitors experience faster page loading times. As visitors are more inclined to click away from a slow-loading site, a CDN can reduce bounce rates and increase the amount of time that people spend on the site. In other words, a faster a website means more visitors will stay and stick around longer.
2. **Reducing bandwidth costs** - Bandwidth consumption costs for website hosting is a primary expense for websites. Through caching and other optimizations, CDNs are able to reduce the amount of data an origin server must provide, thus reducing hosting costs for website owners.
3. **Increasing content availability and redundancy** - Large amounts of traffic or hardware failures can interrupt normal website function. Thanks to their distributed nature, a CDN can handle more traffic and withstand hardware failure better than many origin servers.
4. **Improving website security** - A CDN may improve security by providing [DDoS mitigation](https://www.cloudflare.com/learning/ddos/ddos-mitigation/), improvements to security certificates, and other optimizations.

## How does a CDN work?

At its core, a CDN is a network of servers linked together with the goal of delivering content as quickly, cheaply, reliably, and securely as possible. In order to improve speed and connectivity, a CDN will place servers at the exchange points between different networks.

These [Internet exchange points (IXPs)](https://www.cloudflare.com/learning/cdn/glossary/internet-exchange-point-ixp/) are the primary locations where different Internet providers connect in order to provide each other access to traffic originating on their different networks. By having a connection to these high speed and highly interconnected locations, a CDN provider is able to reduce costs and transit times in high speed data delivery.

![What is a CDN](https://cloudflare.com/img/learning/cdn/what-is-a-cdn/what-is-a-cdn.png)

Beyond placement of servers in IXPs, a CDN makes a number of optimizations on standard client/server data transfers. CDNs place Data Centers at strategic locations across the globe, enhance security, and are designed to survive various types of failures and Internet congestion.

## Latency - How does a CDN improve website load times?

When it comes to websites loading content, users drop off quickly as a site slows down. CDN services can help to reduce load times in the following ways:

- The globally distributed nature of a CDN means reduce distance between users and website resources. Instead of having to connect to wherever a website’s [origin server](https://www.cloudflare.com/learning/cdn/glossary/origin-server/) may live, a CDN lets users connect to a geographically closer [data center](https://www.cloudflare.com/learning/cdn/glossary/internet-exchange-point-ixp/). Less travel time means faster service.
- Hardware and software optimizations such as efficient load balancing and solid-state hard drives can help data reach the user faster.
- CDNs can reduce the amount of data that’s transferred by reducing file sizes using tactics such as [minification](https://www.cloudflare.com/learning/performance/why-minify-javascript-code/) and file compression. Smaller file sizes mean quicker load times.
- CDNs can also speed up sites which use [TLS](https://www.cloudflare.com/learning/security/glossary/transport-layer-security-tls/)/[SSL](https://www.cloudflare.com/learning/security/glossary/what-is-ssl/) certificates by optimizing connection reuse and enabling TLS false start.

[Explore all the ways a CDN helps websites load faster](https://www.cloudflare.com/learning/cdn/performance/)

## Reliability and Redundancy - How does a CDN keep a website always online?

Uptime is a critical component for anyone with an Internet property. Hardware failures and spikes in traffic, as a result of either malicious attacks or just a boost in popularity, have the potential to bring down a web server and prevent users from accessing a site or service. A well-rounded CDN has several features that will minimize downtime:

- Load balancing distributes network traffic evenly across several servers, making it easier to scale rapid boosts in traffic.
- Intelligent failover provides uninterrupted service even if one or more of the CDN servers go offline due to hardware malfunction; the failover can redistribute the traffic to the other operational servers.
- In the event that an entire data center is having technical issues, [Anycast](https://www.cloudflare.com/learning/cdn/glossary/anycast-network/) routing transfers the traffic to another available data center, ensuring that no users lose access to the website.

[Learn more about how a CDN helps keep websites online](https://www.cloudflare.com/learning/cdn/cdn-load-balance-reliability/)

## Data Security - How does a CDN protect data?

Information security is an integral part of a CDN. a CDN can keep a site secured with fresh [TLS/SSL certificates](https://www.cloudflare.com/learning/ssl/what-is-an-ssl-certificate/) which will ensure a high standard of authentication, [encryption](https://www.cloudflare.com/learning/ssl/what-is-encryption/), and integrity. Investigate the security concerns surrounding CDNs, and explore what can be done to securely deliver content. [Learn about CDN SSL/TLS security](https://www.cloudflare.com/learning/cdn/cdn-ssl-tls-security/)

## Bandwidth Expense - How does a CDN reduce bandwidth costs?

Every time an origin server responds to a request, bandwidth is consumed. See how a CDN, like the [Cloudflare CDN](https://www.cloudflare.com/cdn), cuts down on origin requests. (coming soon)

# CDN Performance

A primary benefit of a CDN is its ability to deliver content quickly and efficiently. CDN performance optimizations can be broken into three categories. Explore the CDN Guide.

## How does a CDN improve load times?

Virtually everyone on the Internet has experienced the benefits of a Content Distribution Network ([CDN](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/)). The majority of technology companies including companies like Google, Apple and Microsoft use CDNs to reduce latency in loading web page content.

A CDN will typically place servers at the exchange points between different networks. These [internet exchange points (IXPs)](https://www.cloudflare.com/learning/cdn/glossary/internet-exchange-point-ixp/) are the primary locations where different internet providers link to each other in order to provide each other access to resources on their different networks. In addition to IXPs, a CDN will place servers in [data centers](https://www.cloudflare.com/learning/cdn/glossary/data-center/) in locations across the globe in high traffic areas and strategic locations to be able to move traffic as quickly as possible.

A primary benefit of a CDN is its ability to deliver content quickly and efficiently. CDN performance optimizations can be broken into three categories:

1. **Distance reduction** – reduce the physical distance between a client and the requested data
2. **Hardware/software optimizations** – improve performance of [server-side infrastructure](https://www.cloudflare.com/learning/serverless/glossary/client-side-vs-server-side/), such as by using solid-state hard drives and efficient load balancing
3. **Reduced data transfer** – employ techniques to reduce file sizes so that initial page loads occur quickly

In order to understand the benefits of using a CDN, let’s explore what a normal client/server data transfer looks like without a CDN in place.

## What is the difference in load times with and without a CDN?

Let's imagine that someone in New York needs to access a website hosted on a server in Singapore. The physical separation between these locations is substantial, with a physical distance of about 9,520 miles.

![Distance Without CDN](https://www.cloudflare.com/img/learning/cdn/performance/cdn-distance-1.png)

If a server hosting website content (the [origin server](https://www.cloudflare.com/learning/cdn/glossary/origin-server/)) is located in Singapore, each request for each webpage asset must travel from New York to Singapore and back again. Much like taking an international flight with many connections along the way, each request must travel through a series of routers along its distant travel from point A to point B.

If you want to see a real example of how many different connections (hops) it takes your computer to reach a particular web service from your present location, explore the traceroute utility using a desktop computer.

![CDN Transit Time Improvements](https://www.cloudflare.com/img/learning/cdn/performance/cdn-distance-2.png)

Because the request from New York to Singapore needs to pass through each of the router locations along the way, the amount of time (latency) is increased both by the total distance and the time it takes each router to process the request. Once the origin server processes the request and responds to the client making the request, it then sends information back through a similar sequence of routers before it returns to New York. The measurement of this total round trip is referred to in telecommunications as [RTT for “round trip time.”](https://www.cloudflare.com/learning/cdn/glossary/round-trip-time-rtt/) Ignoring for the moment available bandwidth and potential network congestion, let’s walk through an example of the latency factors.

For the sake of illustration, let's say:

- It takes 250ms for a request to go from New York to Singapore.
- Establishing a [TCP/IP connection](https://www.cloudflare.com/learning/ddos/glossary/tcp-ip/) will add 3 instances of 250ms of latency.
- The webpage requires 5 unique assets consisting of images, JavaScript files and the webpage itself.

Let's see roughly how long it will take this webpage to load:

- 750ms: The TCP/IP connection is made between the client in New York and the origin server in Singapore.
- 250ms: The [HTTP](https://www.cloudflare.com/learning/ddos/glossary/hypertext-transfer-protocol-http) request for the webpage travels from New York to Singapore.
- 250ms: The requester in New York receives a response from the origin server in Singapore with a 200 status code and the webpage including all the additional assets needed.
- 250ms: Each of the 5 assets are requested by the client in New York.
- 1500ms: The five assets are delivered asynchronously to the client from the origin server in Singapore.

In this simple example, the total transit time for this webpage to load is about **3000ms**.

As you can see, each time a request is made and a response is sent, the entire path between the client in New York and the origin in Singapore is traversed. As websites becomes larger and require a greater number of assets, the latency between point A and point B continues to increase.

Let's revisit the example of content hosted in Singapore served to a web client in New York, but now the Singapore site is using a CDN with a server in Atlanta that contains a cached copy of the static website:

- It takes 50ms for a request to go from New York to Atlanta.
- Establishing a TCP/IP connection will add 3 instances of 50ms of latency
- The webpage requires 5 unique assets consisting of images, JavaScript files and the webpage itself.

Let's see roughly how long it will take this webpage to load using the CDN:

- 150ms: The TCP/IP connection is made between the client in New York and the edge server in Atlanta.
- 50ms: The HTTP GET request for the webpage travels from the client to the edge server.
- 50ms: The client receives a response from the edge server cache with the webpage including a list of all the additional assets still required.
- 50ms: Each of the 5 assets are requested by the client.
- 800ms: The five assets are delivered asynchronously to the client from the edge server.

The total transit time for this webpage to load is about **1100ms**.

![CDN Distance Optimized](https://www.cloudflare.com/img/learning/cdn/performance/cdn-distance-3.png)

In this example, the reduction in distance between the client and the content creates a 1900ms improvement in latency for static content, representing a nearly 2 second improvement in load time.

![CDN Latency Improvement](https://www.cloudflare.com/img/learning/cdn/performance/cdn-latency-graph.png)

By reducing the total distance all the necessary traffic needs to traverse, each user to the website is saving an amount of load time. Because users start to leave the site (bounce) very quickly as wait times increase, this improvement represents both a better user experience and higher user time on page.

## How does a CDN load content? What is caching?

As mentioned earlier, normally when a client requests a file from an origin server, the request needs to go roundtrip to that server and back again. A CDN improves the latency by pulling static content files from the origin server into the distributed CDN network in a process called caching. Some CDNs will have advanced features that allow for the selective caching of dynamic content as well. Once the data is cached, the CDN serves the content to the client from the closest CDN data center.

![Request Without CDN Caching](https://www.cloudflare.com/img/learning/cdn/performance/cdn-origin-client.png)

![Request With CDN Caching](https://www.cloudflare.com/img/learning/cdn/performance/cdn-caching.png)

After a [TCP handshake](https://www.cloudflare.com/learning/ddos/glossary/tcp-ip/) has been made, the client machine makes a HTTP request to the CDN’s network. If the content has not yet been cached, the CDN will first download content from the origin by making an extra request between the origin server and the CDN’s edge server.

Here are the 4 steps during a typical CDN caching:

1. When the user requests a webpage, the user's request is routed to the CDN’s nearest [edge server](https://www.cloudflare.com/learning/cdn/glossary/edge-server/).
2. The edge server then makes a request to the origin server for the content the user requested.
3. The origin responds to the edge server’s request.
4. Finally the edge server responds to the client.

![CDN Caching Request](https://www.cloudflare.com/img/learning/cdn/performance/cdn-caching-first-request.png)

The value of a CDN’s proximity to the client occurs after the initial request to the origin server has already been made. Once the data has been cached from the origin server onto the CDN’s network, each subsequent request from the client only needs to go as far as the nearest edge server. This means that if the nearest edge server is closer than the origin server, latency can be reduced and content can be served much faster.

![cached CDN edge response](https://www.cloudflare.com/img/learning/cdn/performance/cdn-caching-second-request.png)

It's important to keep in mind that the amount of time needed to download assets and to process requests and responses is not presently being included; so far only the transit time needed to transfer information between these two locations is being calculated. Other important latency factors that we will be exploring include data reduction, hard disk speed and network congestion.

## How does a CDN reduce file sizes to increase speeds?

In order improve page load times, CDNs reduce overall data transfer amounts between the CDN's cache servers and the client. Both the latency and the required bandwidth are reduced when the overall amount of data transferred goes down. The result is faster page loads and lower bandwidth costs. Two key components go into these reduction:

**Minification** - minification is the process by which blocks of code are reduced in size by removing all the components that help humans understand what's happening. While an engineer needs to separate ideas into sensible variable names, spaces and comments in order to make code blocks readable and maintainable, computers can successfully run code with those characters removed.

Here's the same code block before and after minification:

Before minification: eight lines of code

![CDN Without Minification](https://www.cloudflare.com/img/learning/cdn/performance/cdn-code-before-minification.png)

After minification: reduced to a single line of code

![CDN Minification](https://www.cloudflare.com/img/learning/cdn/performance/cdn-code-after-minification.png)

Now that the code snippet has been reduced from eight lines down to a single line, the overall file size has also been reduced. This means that it takes less time to transfer the file which reduces the latency and helps load the content faster.

**File compression** - file compression is an integral component in reducing the latency and bandwidth consumption required when transferring data across the Internet. GZip is a common method of compression and is considered a best practice to use when transferring webpages. Many CDN providers have GZip enabled by default. How substantial is the savings from GZip compression? Typically compressed files will be reduced by around 50% to 70% of the initial file size.

## What hardware can a CDN use to improve speeds?

As far as CDN hardware optimizations are concerned, a substantial benefit comes from the use of solid-state hard drives (SSD) over traditional hard disk drives (HDD); solid-state drives can open files up to 30% faster than the traditional hard disk drive and are more resilient and reliable.

Akin to a record player, a traditional hard disk drive consists of a spinning circular metal disc with a magnetic coating that stores data. A read/write head on an arm accesses information when the disc spins underneath it. This process is mechanical, and is affected by how fast the disc spins. With the advent of solid-state drives, the older model of hard drives has become less commonly used, though they’re still produced today and are in wide circulation in many computer systems.

A solid-state drive (SSD) is also a form of persistent storage, but functions much more similarly to USB thumb drives or the memory cards commonly found in devices like digital cameras; there are no moving parts. If a regular hard disk is spinning and the system is jostled, the HDD may skip, resulting in read/write errors and potential downtime. Another important SSD benefit is in accessing fragmented files. File fragmentation is a situation where parts of a file are in different locations across the disk, resulting in slower access for HDD drives. Because a SSD can access non-contiguous memory locations efficiently, fragmentation is not a threat to performance.

In the first CDNs, data was stored on hard disk drives. Now with some CDN services all the edge-side caching can occur on solid-state drives. The downside of SSDs is the expense; a SSD can be up to 5 times more expensive than traditional media. For this reason, some CDN services will often avoid using SSDs and will instead opt for the older technology. [Cloudflare CDN](https://www.cloudflare.com/cdn/) exclusively uses SSDs.

# CDN Reliability & Redundancy

A CDN is designed to circumvent network congestion and be resilient against service interruption. Lean more about CDN reliability.

## CDN Benefits – Reliability & Redundancy

One of the important characteristics about a CDN is its ability to keep website content online in the face of the common network problems including hardware failures and network congestion. By load balancing Internet traffic, using intelligent failover, and by maintaining servers across many [data centers](https://www.cloudflare.com/learning/cdn/glossary/data-center/), a CDN is designed to circumvent network congestion and be resilient against service interruption.

## What is load balancing? How does a CDN load balance traffic?

The purpose of a load balancer is to distribute network traffic equally across a number of servers. Load balancing can be either hardware or software based. A CDN uses load balancing in a data center to distribute incoming requests across the available server pool to ensure that spikes in traffic are handled in the most efficient manner possible. By efficiently using available resources, load balancing is able to increase processing speeds and effectively utilize server capacity. Properly load balancing incoming traffic is a key component in mitigating spikes in traffic that occur during atypical Internet activity such as when a website is experiencing an unusually high number of visitors or during a [distributed denial-of-service attack](https://www.cloudflare.com/learning/ddos/what-is-a-ddos-attack/).

![CDN load balancer diagram](https://www.cloudflare.com/img/learning/cdn/reliability/load-balance-diagram.png)

A CDN also uses load balancing to make changes quickly and efficiently when the availability of server resources fluctuates up or down. In the event that a server fails and failover occurs, a load balancer will redirect the traffic allocated for the failed server and distribute it proportionally across the remaining servers. This provides resiliency and reliability by increasing likelihood that hardware failures will not disrupt the flow of traffic. When a new server comes online in the data center, a load balancer will proportionately remove load from other servers and increase the utilization of the new hardware. [Software-based load balancing](https://www.cloudflare.com/load-balancing/) services allow a CDN to scale load balancing capacity quickly without the bottlenecks present when using physical load-balancing hardware.

## What is failover? How does a CDN failover between servers?

In computer systems that require a high degree of reliability and near continuous availability, failover is used to prevent traffic from being lost when a server is unavailable. When a server goes down, the traffic needs to be rerouted to a server that is still functional. By automatically offloading tasks to a standby system or another machine with available capacity, intelligent failover can prevent disruption of service to users.

![CDN failover diagram](https://www.cloudflare.com/img/learning/cdn/reliability/load-balance-failover.png)

## How does a CDN reliably serve content across the Internet?

A CDN is like a GPS system coupled with express toll roads; a CDN will be able to find the optimal path to reach a distant location and will be able to use its own network to find the optimal route to get there as quickly as possible.

When a user loads content from an Internet property such a webpage or web application, a series of connections are made in order to reach the location at which the content is served. Network traffic can be thought of metaphorically as a road and highway system; smaller surface streets move local traffic around the same area and interstate highways transfer traffic into different states. When something goes wrong, like a tanker truck blocks all lanes of an interstate highway on the primary path into different state, traffic must find another path around. Like a highway crossing different states, traffic often has to move across different networks to reach its final destination. If a blockage exists in a particular network, the traffic must be redirected down a different pathway. This process can be time-consuming and inefficient.

Let's say a user in San Francisco is loading a website in Los Angeles. The connection makes many steps, but in this example, one of the most important steps is where the network signal passes through a telecommunications provider based in in San Jose on its way towards the final destination. When a network engineer accidentally pours coffee on routing equipment in San Jose, the provider goes offline, breaking the connection (stranger things have happened). When this occurs the user is no longer able to load their Internet content unless the networked traffic is rerouted to accommodate for the new network landscape. The user's request now needs to go through a different telecom provider if it ever wants to arrive in Los Angeles.

Now that the traffic is no longer able to pass through the intended network, it must instead step into an entirely different network maintained by a different organization. This process of renegotiation and switching networks may occur multiple times in a network request and instances like this can add [latency](https://www.cloudflare.com/learning/performance/glossary/what-is-latency/) and may push the traffic onto a congested pathway, resulting in a delay. A CDN of sufficient size will typically control its own network connections by placing servers in [Internet exchange points (IXPs)](https://www.cloudflare.com/learning/cdn/glossary/internet-exchange-point-ixp/) and other strategic locations. These optimized network schemas allow CDN providers to optimize the route and reduce latency.

## How can a CDN use an Anycast network to increase reliability?

Some CDNs will use an [Anycast](https://www.cloudflare.com/learning/cdn/glossary/anycast-network/) routing method to transfer Internet traffic to specific available data centers. This occurs in order to ensure improved response time and to prevent any one data center from becoming overwhelmed with traffic in the event of extraordinary demand such as during a DDoS attack.

With Anycast, multiple machines can share the same [IP address](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/). When a request is sent to an Anycast IP address, routers will direct it to the machine on the network that is closest. In the event that an entire data center fails or is otherwise incapacitated with heavy traffic, an Anycast network can respond to the outage somewhat similarly to how a load balancer transfers traffic across multiple servers inside a data center; the data is routed away from the failing location and instead is routed towards another data center that is still online and functional.

![Anycast network diagram diagram](https://www.cloudflare.com/img/learning/cdn/reliability/anycast-network-diagram.png)

DDoS attacks are currently one of the most substantial threats to the reliability of Internet properties. CDNs that use Anycast have additional flexibility in mitigating DDoS attacks. In most modern DDoS attacks many compromise computers or [“bots”](https://www.cloudflare.com/learning/bots/what-is-a-bot/) are used to form what is known as a [botnet](https://www.cloudflare.com/learning/ddos/what-is-a-ddos-botnet/). These compromised machines can generate so much Internet traffic that they can overwhelm a typical Unicast-connected machine. With an Anycast network, a portion of the botnet attack traffic can be distributed across multiple data centers, reducing the impact of the attack. Learn about the [Cloudflare CDN](https://www.cloudflare.com/cdn/) with Anycast routing.

# CDN SSL/TLS | CDN Security

CDN strategies for mitigating vulnerabilities include proper SSL/TLS encryption and specialized encryption hardware. Explore the CDN guide.



## What are the security risks to a CDN?

Like all networks exposed to the internet, CDNs must guard against [on-path attacks](https://www.cloudflare.com/learning/security/threats/on-path-attack/), [data breaches](https://www.cloudflare.com/learning/security/what-is-a-data-breach/), and attempts to overwhelm the network of the targeted [origin server](https://www.cloudflare.com/learning/cdn/glossary/origin-server/) using [DDoS attacks](https://www.cloudflare.com/learning/ddos/what-is-a-ddos-attack/). A [CDN](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/) can have multiple strategies for mitigating vulnerabilities including proper [SSL](https://www.cloudflare.com/learning/ssl/what-is-ssl/)/[TLS](https://www.cloudflare.com/learning/ssl/transport-layer-security-tls/) encryption and specialized [encryption](https://www.cloudflare.com/learning/ssl/what-is-encryption/) hardware.

## What is SSL/TLS encryption?

Transport Layer Security (TLS) is a protocol for encrypting data that is sent over the Internet. TLS grew out of Secure Sockets Layer (SSL), the first widely-adopted web encryption protocol, in order to fix most of the earlier protocol’s security flaws. The industry still uses the terms somewhat interchangeably for historical reasons. Any web site that you visit starting with [https://](https://www.cloudflare.com/learning/ssl/what-is-https/) rather than http:// is using TLS/SSL for communication between a browser and a server.

Proper encryption practices are a necessity in order to prevent attackers from accessing important data. Because the Internet is designed in such a way that data is transferred across many locations, it is possible to intercept packets of important information as they move across the globe. Through the utilization of a [cryptographic protocol](https://www.cloudflare.com/learning/ssl/what-is-a-cryptographic-key/), only the intended recipient is able to decode and read the information and intermediaries are prevented from decoding the contents of the transferred data.

#### The TLS protocol is designed to provide 3 components:

1. **Authentication** - The ability to verify the validity of the provided identifications
2. **Encryption** - The ability to obfuscate information sent from one host to another
3. **Integrity** - The ability to detect forgery and tampering

## What is an SSL Certificate?

To enable TLS, a site needs an [SSL certificate](https://www.cloudflare.com/learning/ssl/what-is-an-ssl-certificate/) and a corresponding [key](https://www.cloudflare.com/learning/ssl/what-is-a-cryptographic-key/). Certificates are files containing information about the owner of a site, and the public half of an asymmetric key pair. A certificate authority (CA) digitally signs the certificate to verify that the information in the certificate is correct. By trusting the certificate, you are trusting that the certificate authority has done its due diligence.

![SSL/TLS error graphic](https://www.cloudflare.com/img/learning/cdn/tls-ssl/https-tls-ssl-not-secure.png)

Operating systems and browsers typically have a list of certificate authorities that they implicitly trust. If a web site presents a certificate that is signed by an untrusted certificate authority, the browser warns the visitor that something could be afoot.

Certificates and the way they are implemented can also be independently rated based on strength, protocol support and other characteristics. Ratings can change over time as newer, better implementations become available or other factors result in reduction of overall security of a certification implementation. If an origin server has an older lower grade SSL security implementation it will typically be graded more poorly and may be vulnerable to compromise.

A CDN has the added benefit of providing security to visitors of properties hosted within its network using a CDN provided certificate. Because visitors connect to only the CDN, an older or less secure certificate in use between the origin server and the CDN will not affect the client’s experience.

![SSL/TLS self-signed diagram](https://www.cloudflare.com/img/learning/cdn/tls-ssl/origin-ssl-self-signed-certificate-diagram.png)

Realistically, this weaker server-to-edge security still represents a vulnerability and should be avoided, especially given the ease with which it is possible to upgrade the security of an origin server by using [free origin encryption](https://blog.cloudflare.com/cloudflare-ca-encryption-origin/).

![SSL/TLS self-signed diagram](https://www.cloudflare.com/img/learning/cdn/tls-ssl/origin-ssl-protection-diagram.png)

Proper security is also important to organic search; encrypted web properties result in stronger ranking on Google search.

An SSL/TLS connection operates differently than a traditional [TCP/IP](https://www.cloudflare.com/learning/ddos/glossary/tcp-ip/) connection. Once the initial stages of the TCP connection have been made, a separate exchange occurs to set up the secure connection. This article will refer to the device requesting the secure connection as the client and the device serving up the secure connection as the server, as is the case with a user loading a webpage encrypted with SSL/TLS.

#### First the TCP/IP handshake is made in 3 steps:

1. The client sends a SYN packet to the server in order to initiate the connection.
2. The server than responds to that initial packet with a SYN/ACK packet, in order to acknowledge the communication.
3. Finally, the client returns an ACK packet to acknowledge the receipt of the packet from the server. After completing this sequence of packet sending and receiving, the TCP connection is open and able to send and receive data.

![TCP 3-way handshake diagram](https://www.cloudflare.com/img/learning/cdn/tls-ssl/tcp-handshake-diagram.png)

Once the TCP/IP handshake has occurred, the [TLS encryption handshake](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/) begins. The detailed processes behind a TLS handshaking implementation are beyond the scope of this guide. Instead we will focus on the core purpose of the handshake and the time required to complete the process.

#### From a high level, there are three main components to a TLS handshake:

1. The client and the server negotiate TLS versions and the type of cryptography cipher to be used in the communication.
2. The client and server take steps to ensure mutually authentic communication.
3. A key is exchanged to be used in future encrypted communications.

In the diagram below, each of the steps involved in a TCP/IP handshake and a TLS handshake are visualized. Keep in mind that each arrow represents a separate communication which must travel physically between the client and the server. Since the total number of messages back and forth are increased when using TLS encryption, web page load times are increased.

![SSL/TLS handshake diagram](https://www.cloudflare.com/img/learning/cdn/tls-ssl/tls-ssl-handshake.png)

For illustrative purposes it can be said the TCP handshake takes about 50ms, the TLS handshake may take about 110ms. This is largely a result of the time it takes for data to be sent both ways between the client and server. The idea of [round-trip time (RTT)](https://www.cloudflare.com/learning/cdn/glossary/round-trip-time-rtt/), which is the amount of time it takes for information to travel from one device to another and back again, can be used to quantify how “expensive” a connection is to create. If left unoptimized and without the use of a CDN, additional RTT represents an increase in [latency](https://www.cloudflare.com/learning/performance/glossary/what-is-latency/) and a reduction in load time for end-users. Luckily, there are optimizations that can be made to improve total load time and reduce the number of trips back and forth.

## How can SSL latency be improved?

SSL optimizations can reduce RTT and improve page load time. Here are 3 of the ways a TLS connection can be optimized:

**TLS Session Resumption** - a CDN can keep a connection alive between the origin server and the CDN network for longer by resuming the same session for additional requests. Keeping the connection alive saves time spent renegotiating the connection between the CDN and the origin server when the client requires an uncached origin fetch. As long as the origin server receives additional requests while the connection to the CDN is maintained, additional visitors to the site will experience lower latency.

![session resumption real time infographic](https://www.cloudflare.com/img/learning/cdn/tls-ssl/session-resumption-real-time.png)

The overall cost of a session resumption is less than 50% of a full TLS handshake, mainly because session resumption only costs one round-trip while a full TLS handshake requires two. Additionally, a session resumption does not require any large finite field arithmetic (new sessions do), so the CPU cost for the client is almost negligible compared to that in a full TLS handshake. For mobile users, the performance improvement by session resumption means a much more reactive and battery-life-friendly surfing experience.

![session resumption CPU time infographic](https://www.cloudflare.com/img/learning/cdn/tls-ssl/session-resumption-cpu-time.png)

**Enable TLS False Start** - when a visitor is viewing the site for the first time, the session resumption mentioned above will not be helpful. TLS False Start allows the sender to send application data without a complete TLS handshake.

![SSL/TLS False Start handshake diagram](https://www.cloudflare.com/img/learning/cdn/tls-ssl/tls-ssl-false-start-handshake.png)

The False Start does not modify the TLS protocol itself, it only modifies the timing in which data is transferred. Once the client begins the key exchange, encryption can be assured and data transfer begins. This modification reduces the total number of roundtrips, cutting down the latency required by 60ms.

**Zero Round Trip Time Resumption (0-RTT)** - 0-RTT allows for session resumption without addition RTT latency added to the connection. For resumed connections using [TLS 1.3](https://www.cloudflare.com/learning/ssl/why-use-tls-1.3/) and 0-RTT, connection speed is improved, leading to a faster and smoother web experience for web sites that users visit regularly. This speed boost is especially noticeable on mobile networks.

0-RTT is an effective improvement, but isn’t without some security tradeoffs. To overcome the risk of what’s known as a replay attack, a CDN service may implement restriction on the type of HTTP requests and the allowed parameters. To learn more, explore an [introduction to 0-RTT](https://blog.cloudflare.com/introducing-0-rtt/).

## CDN Protection from DDoS attacks

One of the most substantial security vulnerabilities of web properties on the modern Internet is the use of [distributed denial-of-service (DDoS) attacks](https://www.cloudflare.com/learning/ddos/what-is-a-ddos-attack/). Over time DDoS attacks have increased in size and complexity, with attackers utilizing [botnets](https://www.cloudflare.com/learning/ddos/what-is-a-ddos-botnet/) to target websites with attack traffic. A large and properly configured CDN has the potential benefit of scale as a protective factor against DDoS; by having enough data center locations and sizable bandwidth capabilities, a CDN is able to withstand and mitigate an amount of incoming attack traffic that would easily overwhelm the targeted origin server.

# What is a Data Center?

A data center is a facility housing many networked computers that work together to process, store and share data.

## What is a data center?

A data center is a facility housing many networked computers that work together to process, store, and share data. Most major tech companies rely heavily upon data centers as a central component in delivering online services.

## What is the difference between a data center and a point-of-presence (PoP)?

The terms data center and point-of-presence (PoP) are sometimes used interchangeably, though distinctions can be made between them. Speaking generally, a PoP may refer to a company having a single server presence in a location while a data center may refer to a location that houses multiple servers. Instead of referring to multiple PoPs in one location, Cloudflare uses the term data center to indicate a location in which many of our servers are maintained.

![Map of data centers](https://www.cloudflare.com/img/learning/cdn/glossary/data-center/data-centers-map.png)

The concept of a point-of-presence rose to prominence during the court ordered breakup of the Bell telephone system. In the court decision, a point-of-presence referred to a location where long-distance carriers terminate services and shift connections onto a local network. Similarly, on the modern Internet a PoP typically refers to where [CDNs](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/) have a physical presence in a location, often in the junctures between networks known as [Internet exchange points (IxP)](https://www.cloudflare.com/learning/cdn/glossary/internet-exchange-point-ixp/).

A data center refers to a physical location in which computers are networked together in order to improve usability and reduce costs related to storage, bandwidth, and other networking components. Data centers such as IxP co-location facilities allow different Internet service providers, CDN’s, and other infrastructure companies to connect with each other to share transit.

## What are the common concerns in the design of a data center?

Many components and factors are taken into consideration when creating a modern data center. With proper planning, maintenance, and security, a data center is at lower risk of both downtime and [data breaches](https://www.cloudflare.com/learning/security/what-is-a-data-breach/).

#### Data center considerations include:

- **Redundancy/backup** - the level of redundancy varies widely based on the quality of a data center; in high tier data centers, multiple redundancies in power and backup servers are built into the infrastructure.
- **Efficiency** - the amount of electricity used at a large data center rivals that of a small town. Whenever possible, data centers attempt to cut down on costs by optimizing cooling processes and using energy-efficient hardware.
- **Security** - proper physical security, both in terms of electronic surveillance, [access controls](https://www.cloudflare.com/learning/access-management/what-is-access-control/), and on-site security guards reduce the risk associated with bad actors attempting to gain site access.
- **Environmental controls/factors** - maintaining the right environmental conditions is necessary for the proper functioning of electronic hardware. Keeping both temperature and humidity within acceptable parameters requires the proper balance of air conditioning, humidity control, and airflow regulation. In areas that are vulnerable to earthquakes, properly secured servers are also a necessary concern.
- **Maintenance and monitoring** - on-site or on-call network engineers are needed in order stay on top of server crashes and other hardware failures. Proper response helps to ensure server uptime and eliminate reductions in quality of service.
- **Bandwidth** - a data center is incomplete without the bandwidth necessary to handle all the requisite network traffic. Bandwidth considerations are a central component in data center infrastructure, with external network connections and internal data center topology both designed around sufficient network capacity.

# What is a CDN edge server?

Edge servers are a primary component of making the Internet fast.

## What is a CDN edge server?

A [CDN](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/) edge server is a computer that exists at the logical extreme or “[edge](https://www.cloudflare.com/learning/serverless/glossary/what-is-edge-computing/)” of a network. An edge server often serves as the connection between separate networks. A primary purpose of a CDN edge server is to store content as close as possible to a requesting client machine, thereby reducing [latency](https://www.cloudflare.com/learning/performance/glossary/what-is-latency/) and improving page load times.

An edge server is a type of edge device that provides an entry point into a network. Other edges devices include routers and routing switches. Edge devices are often placed inside [Internet exchange points (IxPs)](https://www.cloudflare.com/learning/cdn/glossary/internet-exchange-point-ixp/) to allow different networks to connect and share transit.

## How does an edge server work?

In any particular network layout, a number of different devices will connect to each other using one or more predefined network pattern. If a network wants to connect to another network or the larger Internet, it must have some form of bridge in order for traffic to flow from one location to another. Hardware devices that creates this bridge on the edge of a network are called edge devices.

#### Networks connect across the edge

In a typical home or office network with many devices connected, devices such as mobile phones or computers connect and disconnect to the network through a hub-and-spoke network model. All of the devices exist within the same local area network (LAN), and each device connects to a central router, through which they are able to connect with each other.

In order to connect a second network to the first network, at some point the connection must be made between the networks. The device through which the networks are able to connect with each other is, by definition, an edge device.

![CDN edge network device diagram](https://www.cloudflare.com/img/learning/cdn/glossary/edge-server/cdn-edge-network-device.png)

Now, if a computer inside Network A needs to connect to a computer inside Network B, the connection must pass from network A, across the network edge, and into the second network. This same paradigm also works in more complex contexts, such when a connection is made across the Internet. The ability for networks to share transit is bottlenecked by the availability of edge devices between them.

When a connection must traverse the Internet, even more intermediary steps must be taken between network A and network B. For the sake of simplicity, let's imagine that each network is a circle, and the place in which the circles touch is the edge of the network. In order for connection to move across the Internet, it will typically touch many networks and move across many network edge nodes. Generally speaking, the farther the connection must travel, the greater the number of networks that must be traversed. A connection may traverse different Internet service providers and Internet backbone infrastructure hardware before reaching its target.

![CDN edge server placement diagram](https://www.cloudflare.com/img/learning/cdn/glossary/edge-server/cdn-edge-server-placement.png)

A CDN provider will place servers in many locations, but some of the most important are the connection points at the edge between different networks. These edge servers will connect with multiple different networks and allow for traffic to pass quickly and efficiently between networks. Without a CDN, transit may take a slower and/or more convoluted route between source and destination. In worst case scenarios, traffic will “trombone” large distances; when connecting to another device across the street, a connection may move across the country and back again. By placing edge servers in key locations, a CDN is able to quickly deliver content to users inside different networks. To learn more about the improvements of using CDN, explore how [CDN performance works](https://www.cloudflare.com/learning/cdn/performance/).

## What is the difference between an edge server and an origin server?

An [origin server](https://www.cloudflare.com/learning/cdn/glossary/origin-server/) is the web server that receives all Internet traffic when a web property is not using a CDN. Using an origin server without a CDN means that each Internet request must return to the physical location of that origin server, regardless of where in the world it resides. This creates an increase in load times which increases the further the server is from the requesting client machine.

CDN edge servers store ([cache](https://www.cloudflare.com/learning/cdn/what-is-caching/)) content in strategic locations in order to take the load off of one or more origin servers. By moving static assets like images, HTML and JavaScript files (and potentially other content) as close as possible to the requesting client machine, an edge server cache is able to reduce the amount of time it takes for a web resource to load. Origin servers still have an important function to play when using a CDN, as important [server-side](https://www.cloudflare.com/learning/serverless/glossary/client-side-vs-server-side/) code such as a database of hashed client credentials used for authentication, typically is maintained at the origin. Learn about the [Cloudflare CDN](https://www.cloudflare.com/cdn/) with edge servers all over the globe.

# What is an origin server? | Origin server definition

The purpose of an origin server is to process and respond to incoming Internet requests from Internet clients.

## What is an Origin Server?

The purpose of an origin server is to process and respond to incoming internet requests from internet clients. The concept of an origin server is typically used in conjunction with the concept of an [edge server](https://www.cloudflare.com/learning/cdn/glossary/edge-server/) or [caching](https://www.cloudflare.com/learning/cdn/what-is-caching/) server. At its core, an origin server is a computer running one or more programs that are designed to listen for and process incoming internet requests. An origin server can take on all the responsibility of serving up the content for an internet property such as a website, provided that the traffic does not extend beyond what the server is capable of processing and [latency](https://www.cloudflare.com/learning/performance/glossary/what-is-latency/) is not a primary concern.

The physical distance between an origin server and a client making a request adds latency to the connection, increasing the time it takes for an internet resource such as a webpage to be loaded. The additional [round-trip time (RTT)](https://www.cloudflare.com/learning/cdn/glossary/round-trip-time-rtt/) between client and origin server required for a secure internet connection using [SSL/TLS](https://www.cloudflare.com/learning/cdn/cdn-ssl-tls-security/) also add additional latency to the request, directly impacting the experience of the client requesting data from the origin. By using a [Content Distribution Network (CDN)](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/) round-trip time is able to be reduced, and the amount of requests to an origin server are also able to be reduced.

## What is the difference between an Origin Server and a CDN Edge server?

Put simply, CDN edge servers are computers placed in important junctures between major internet providers in locations across the globe in order to deliver content as quickly as possible. An edge server lives inside a CDN on the “[edge](https://www.cloudflare.com/learning/serverless/glossary/what-is-edge-computing/)” of a network and is specifically designed to quickly process requests. By placing edge servers strategically inside of the [Internet Exchange Points (IxPs)](https://www.cloudflare.com/learning/cdn/glossary/internet-exchange-point-ixp/) that exist between networks, a CDN is able to reduce the amount of time it takes to get to a particular location on the Internet.

These edge servers cache content in order to take the load off of one or more origin servers. By moving static assets like images, HTML and JavaScript files (and potentially other content) as close as possible to the requesting client machine, an edge server cache is able to reduce the amount of time it takes for a web resource to load. Origin servers still have an important function to play when using a CDN, as important [server-side code](https://www.cloudflare.com/learning/serverless/glossary/client-side-vs-server-side/) such as the database of hashed client credentials used for authentication is typically maintained inside an origin server.

Here's a simple example of how an edge server and an origin server work together to serve up a login page and allow a user to login to a service. A very simple login page requires the following static assets to be downloaded for the webpage to render properly:

1. A HTML file for the webpage
2. A CSS file for the webpage styling
3. Several image files
4. Several JavaScript libraries

These files are all static files; they are not dynamically generated and are the same for all visitors to the website. As a result, these files can be both cached and served to the client from the edge server. All of these files can be loaded closer to the client machine and without any bandwidth consumption by the origin.

![CDN edge cache response](https://www.cloudflare.com/img/learning/cdn/performance/cdn-caching-second-request.png)

Next, when the user enters their login and password and presses “login,” the request for dynamic content travels back to the edge server who then proxies the request back to the origin server. The origin then verifies the user's identity in the associated database table before sending back the specific account information.

![CDN uncached origin fetch diagram](https://www.cloudflare.com/img/learning/cdn/glossary/origin-server/origin-fetch-diagram.png)

This interplay between edge servers handling static content and origin servers serving up dynamic content is a typical separation of concerns when using a CDN. The capability of some CDNs can also extend beyond this simplistic model.

## Can an origin server still be attacked while using a CDN?

The short answer is yes. A CDN does not render an origin server invincible, but when used properly it can render an origin server invisible, acting as a shield for incoming requests. Hiding the real [IP address](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/) of an origin server is an important part of setting up a CDN. As such, a CDN provider should recommend that the IP address of the origin server be changed when implementing a CDN strategy in order to prevent [DDoS attacks](https://www.cloudflare.com/learning/ddos/what-is-a-ddos-attack/) from going around the shield and hitting the origin directly. [Cloudflare's CDN](https://www.cloudflare.com/cdn/) includes comprehensive DDoS protection.

# What Is A Reverse Proxy? | Proxy Servers Explained

A reverse proxy protects web servers from attacks and can provide performance and reliability benefits. Learn more about forward and reverse proxies.

## What is a reverse proxy?

A reverse proxy is a server that sits in front of web servers and forwards client (e.g. web browser) requests to those web servers. Reverse proxies are typically implemented to help increase [security](https://www.cloudflare.com/learning/security/what-is-web-application-security/), [performance](https://www.cloudflare.com/learning/performance/why-site-speed-matters/), and reliability. In order to better understand how a reverse proxy works and the benefits it can provide, let’s first define what a proxy server is.

## What’s a proxy server?

A forward proxy, often called a proxy, proxy server, or web proxy, is a server that sits in front of a group of client machines. When those computers make requests to sites and services on the Internet, the proxy server intercepts those requests and then communicates with web servers on behalf of those clients, like a middleman.

For example, let’s name 3 computers involved in a typical forward proxy communication:

- A: This is a user’s home computer
- B: This is a forward proxy server
- C: This is a website’s origin server (where the website data is stored)

![Forward Proxy Flow](https://www.cloudflare.com/img/learning/cdn/glossary/reverse-proxy/forward-proxy-flow.svg)

In a standard Internet communication, computer A would reach out directly to computer C, with the client sending requests to the [origin server](https://www.cloudflare.com/learning/cdn/glossary/origin-server/) and the origin server responding to the client. When a forward proxy is in place, A will instead send requests to B, which will then forward the request to C. C will then send a response to B, which will forward the response back to A.

Why would anyone add this extra middleman to their Internet activity? There are a few reasons one might want to use a forward proxy:

- **To avoid state or institutional browsing restrictions** - Some governments, schools, and other organizations use firewalls to give their users access to a limited version of the Internet. A forward proxy can be used to get around these restrictions, as they let the user connect to the proxy rather than directly to the sites they are visiting.
- **To block access to certain content** - Conversely, proxies can also be set up to block a group of users from accessing certain sites. For example, a school network might be configured to connect to the web through a proxy which enables content filtering rules, refusing to forward responses from Facebook and other social media sites.
- **To protect their identity online** - In some cases, regular Internet users simply desire increased anonymity online, but in other cases, Internet users live in places where the government can impose serious consequences to political dissidents. Criticizing the government in a web forum or on social media can lead to fines or imprisonment for these users. If one of these dissidents uses a forward proxy to connect to a website where they post politically sensitive comments, the [IP address](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/) used to post the comments will be harder to trace back to the dissident. Only the IP address of the proxy server will be visible.

## How is a reverse proxy different?

A reverse proxy is a server that sits in front of one or more web servers, intercepting requests from clients. This is different from a forward proxy, where the proxy sits in front of the clients. With a reverse proxy, when clients send requests to the origin server of a website, those requests are intercepted at the [network edge](https://www.cloudflare.com/learning/serverless/glossary/what-is-edge-computing/) by the reverse proxy server. The reverse proxy server will then send requests to and receive responses from the origin server.

The difference between a forward and reverse proxy is subtle but important. A simplified way to sum it up would be to say that a forward proxy sits in front of a client and ensures that no origin server ever communicates directly with that specific client. On the other hand, a reverse proxy sits in front of an origin server and ensures that no client ever communicates directly with that origin server.

Once again, let’s illustrate by naming the computers involved:

- D: Any number of users’ home computers
- E: This is a reverse proxy server
- F: One or more origin servers

![Reverse Proxy Flow](https://www.cloudflare.com/img/learning/cdn/glossary/reverse-proxy/reverse-proxy-flow.svg)

Typically all requests from D would go directly to F, and F would send responses directly to D. With a reverse proxy, all requests from D will go directly to E, and E will send its requests to and receive responses from F. E will then pass along the appropriate responses to D.

Below we outline some of the benefits of a reverse proxy:

- **[Load balancing](https://www.cloudflare.com/learning/cdn/cdn-load-balance-reliability/)** - A popular website that gets millions of users every day may not be able to handle all of its incoming site traffic with a single origin server. Instead, the site can be distributed among a pool of different servers, all handling requests for the same site. In this case, a reverse proxy can provide a load balancing solution which will distribute the incoming traffic evenly among the different servers to prevent any single server from becoming overloaded. In the event that a server fails completely, other servers can step up to handle the traffic.
- **Protection from attacks** - With a reverse proxy in place, a web site or service never needs to reveal the IP address of their origin server(s). This makes it much harder for attackers to leverage a targeted attack against them, such as a [DDoS attack](https://www.cloudflare.com/learning/ddos/what-is-a-ddos-attack/). Instead the attackers will only be able to target the reverse proxy, such as Cloudflare’s [CDN](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/), which will have tighter security and more resources to fend off a cyber attack.
- **[Global Server Load Balancing](https://www.cloudflare.com/learning/cdn/glossary/global-server-load-balancing-gslb/) (GSLB)** - In this form of load balancing, a website can be distributed on several servers around the globe and the reverse proxy will send clients to the server that’s geographically closest to them. This decreases the distances that requests and responses need to travel, minimizing load times.
- **Caching** - A reverse proxy can also [cache](https://www.cloudflare.com/learning/cdn/what-is-caching/) content, resulting in faster performance. For example, if a user in Paris visits a reverse-proxied website with web servers in Los Angeles, the user might actually connect to a local reverse proxy server in Paris, which will then have to communicate with an origin server in L.A. The proxy server can then cache (or temporarily save) the response data. Subsequent Parisian users who browse the site will then get the locally cached version from the Parisian reverse proxy server, resulting in much faster performance.
- **SSL encryption** - [Encrypting](https://www.cloudflare.com/learning/ssl/what-is-encryption/) and decrypting [SSL](https://www.cloudflare.com/learning/security/glossary/what-is-ssl/) (or [TLS](https://www.cloudflare.com/learning/security/glossary/transport-layer-security-tls/)) communications for each client can be computationally expensive for an origin server. A reverse proxy can be configured to decrypt all incoming requests and encrypt all outgoing responses, freeing up valuable resources on the origin server.

## How to implement a reverse proxy

Some companies build their own reverse proxies, but this requires intensive software and hardware engineering resources, as well as a significant investment in physical hardware. One of the easiest and most cost-effective ways to reap all the benefits of a reverse proxy is by signing up for a CDN service. For example, the [Cloudflare CDN](https://www.cloudflare.com/cdn/) provides all the performance and security features listed above, as well as many others.

 Caching Static and Dynamic Content | How Does It Work?

Unlike static content, dynamic content is different for each user, meaning it cannot be served to multiple users and is difficult to cache. However, caching dynamic content is possible with the right technology.

## What is the difference between static and dynamic content?

Static content is any file that is stored in a server and is the same every time it is delivered to users. HTML files and images are examples of this kind of content. Static content is like a newspaper: once an issue of a newspaper is published, it features the same articles and photos all day for everyone who picks up a copy, no matter what new developments transpire during the day.

Dynamic content is content that changes based on factors specific to the user such as time of visit, location, and device. A dynamic webpage will not look the same for everybody, and it can change as users interact with it – like if a newspaper could rewrite itself as someone is reading it. This makes webpages more personalized and more interactive.

![Caching Static Content](https://www.cloudflare.com/resources/images/slt3lc6tev37/1wXlf7IjxWvr2ivJyzC9iv/4acfec55339c30d2c01eee7d80c9a183/caching-static-content.svg)

![Caching Dynamic Content](https://www.cloudflare.com/resources/images/slt3lc6tev37/6ijRQV6QxiyG4zyidpgJmi/23088f026f5b01cd671274b9b994096f/caching-dynamic-content.svg)

A modern news website is a good example of dynamic content: unlike in a newspaper, articles are updated throughout the day, and the homepage may feature different headlines based on the site visitor's location or login status. Social media pages are another example: the Facebook news feed looks totally different for each user, and users are able to interact with the content in order to change it (by liking, sharing, or commenting on posts).

Dynamic webpages are not stored as static HTML files. Instead, [server-side](https://www.cloudflare.com/learning/serverless/glossary/client-side-vs-server-side/) scripts generate an HTML file in response to events, such as user interactions or user logins, and send the HTML file to the web browser. Because dynamic content is generated server-side, it is typically served from [origin servers](https://www.cloudflare.com/learning/cdn/glossary/origin-server/), not a cache.

For a long time, dynamic content was considered uncacheable. But new technologies allow websites to serve dynamic content from a cache, significantly cutting down on [latency](https://www.cloudflare.com/learning/performance/glossary/what-is-latency/) while keeping the user experience interactive.

## How is static content cached?

The usual web caching process is for a cache to save a copy of the static file – e.g., an image, – when the content is served, so that it's closer to the user and delivered more quickly the next time. Browsers and [content delivery networks (CDNs)](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/) can cache static content for a set time period and serve it to users as long as the content continues to be requested. This is possible because static content does not change over time; the same file can be delivered to users over and over.

## How does dynamic content caching work?

Dynamic content is generated by scripts that change the content on a page. By running scripts in a CDN cache instead of in a distant origin server, dynamic content can be generated and delivered from a cache. Dynamic content is thus essentially "cached" and does not have to be served all the way from the origin, reducing the response time to client requests and speeding up dynamic webpages.

[Cloudflare Workers](https://www.cloudflare.com/products/cloudflare-workers/), for example, are [serverless JavaScript functions](https://www.cloudflare.com/learning/serverless/glossary/function-as-a-service-faas/) that run on the Cloudflare CDN. They can respond to a variety of events and inputs, including device type, time of day, user location, or data from third party APIs. Based on these parameters, dynamic content can be generated and served to client devices, or static content can be altered, cached, or purged from the cache.

Because Cloudflare Workers are JavaScript code, developers can use them to build a wide range of features or an entire application, in addition to caching dynamic content. Additionally, Cloudflare Workers can be deployed and can propagate to CDN locations around the globe in seconds.

Cloudflare Workers run in a lightweight execution environment so that the code can spin up and [execute within milliseconds](https://blog.cloudflare.com/serverless-performance-comparison-workers-lambda/). They allow developers to run code without worrying about configuring a backend. Learn more about [serverless computing](https://www.cloudflare.com/learning/serverless/what-is-serverless/).

## What is the difference between dynamic content caching and dynamic content compression?

Another approach to speeding up dynamic webpages is to compress dynamic content generated by the origin server and deliver it as quickly and efficiently as possible. With dynamic compression, the content still comes from the origin server instead of a cache, but the HTML files generated are made significantly smaller so that they can reach the client device more quickly. [Railgun](https://www.cloudflare.com/website-optimization/railgun/) is an example of a product that compresses dynamic content, improving performance.

## How does using Edge Side Includes (ESI) speed up dynamic webpages?

Often, a large amount of content on a dynamic webpage remains consistent for all users, and only certain elements on the page are dynamic. This means that much of the HTML code is duplicated in each dynamic copy of the page. To solve for this inefficiency, a number of companies worked together to develop Edge Side Includes (ESI), a markup language that specifies where dynamic content appears on a webpage. (ESI is in use on some CDNs, but is not yet accepted by the W3C, the organization that governs web standards.)

Content with an ESI tag is fetched from somewhere else, while the rest of the webpage's content can be cached. If only some of the webpage is generated dynamically and the rest is cached, the webpage will load much more quickly than if the whole page needed to be generated for each user. [ESI can be combined with Cloudflare Workers](https://blog.cloudflare.com/edge-side-includes-with-cloudflare-workers/) to make the process even more efficient.

# How can using a CDN reduce bandwidth costs?

By caching content and making multiple round trips to the origin server unnecessary, CDNs cut down on data transfer costs for website owners.

## How can using a CDN reduce bandwidth costs?

A [content delivery network (CDN)](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/) reduces the cost of transferring data because it sits between users and the website's hosting servers, or [origin servers](https://www.cloudflare.com/learning/cdn/glossary/origin-server/), cutting down on traffic between the hosting servers and the rest of the Internet. A CDN is a network of servers distributed around the world that store content closer to end users, reducing [latency](https://www.cloudflare.com/learning/performance/glossary/what-is-latency/). CDNs serve up [cached](https://www.cloudflare.com/learning/cdn/what-is-caching/) content so that the origin servers don't have to deliver the same content over and over.

Web hosting services charge for the data that is transferred to or from the origin server (this is often called "bandwidth"). But if most of a website's content is cached within a CDN, far less data needs to be transferred in and out of the website's host server, resulting in lower bandwidth costs.

To understand why, imagine that a popular pizza delivery joint located in San Francisco often gets orders from customers in Oakland (which is on the other side of the San Francisco Bay). Every time the restaurant delivers a pizza to Oakland, its drivers have to pay the toll required for crossing the bridge to Oakland, increasing costs.

However, if the restaurant opens a satellite location in Oakland, delivery drivers no longer have to cross the bridge and pay the toll to fill Oakland orders, with the added bonus that the pizza will be delivered faster.

Similarly, if a website stores some or all content in a CDN, which is closer to its users, then the website owner has to pay far less in "bridge toll" money for content served all the way from the website's original location.

## How do websites incur bandwidth costs?

First of all, "bandwidth" is not actually bandwidth in this context. When people say "bandwidth" in the context of web hosting, what they really mean is "data transfer." This is the amount of data that goes to or from the web host. (Bandwidth really means the maximum amount of data that can pass through a point on a network over time.)

Therefore, web hosting doesn't result in "bandwidth" costs, but rather data transfer costs. A certain amount of data per time period (typically per month) is allotted by hosting provider. Usually the provider will charge for either ingress (data going in) or egress (data going out), whichever is higher.

When users visit a website, their browser will load content via the Internet. If the website doesn't use a CDN, all of the content will have to come from an origin server, which means every time the website loads, [HTTP requests](https://www.cloudflare.com/learning/ddos/glossary/hypertext-transfer-protocol-http/) go to the origin server, and content goes out from that same server. The more times this happens, the more data is transferred, resulting in higher costs for the website operator.

## How do websites incur bandwidth costs?

Do CDNs add cost?

Most CDNs charge for their services, but the savings on monthly data transfers will typically outweigh the cost of using the CDN.
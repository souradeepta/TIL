# Long Polling - Concepts and Considerations

In this article, we’ll provide an insight into the technique known as “long polling”, how it came to be, how it's implemented, how you might use it in your own applications, and how we use it at Ably.

## A brief history – how did long polling come about?

In the early days of the web, [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) made sense as a simple request-response protocol because it was designed as a way to distribute structured documents to interested readers, each document generally having links to others that could be browsed at the reader’s leisure. A request-response model was the right strategy because a browser needed only make a request for the resource that the user wished to consume, and the job was done.

The technology of the time was vastly more limited than that of today, and holding a connection open would not only have been a significant issue for web server load management, but would have had little-to-no value anyway in relation to the goals and use cases that the early web was designed to solve. Early browsers such as Mosaic and the first version of Netscape Navigator were implemented with this in mind and had no support for JavaScript or other features that might position the web as anything more than a distributed resource-sharing network.

In 1995, [Netscape Communications](https://en.wikipedia.org/wiki/Netscape) hired Brendan Eich to implement scripting capabilities in Netscape Navigator, and over a ten-day period, the JavaScript language was born. Its capabilities as a language were initially very limited compared to modern-day JavaScript, and its ability to interact with the browser’s document object model (DOM) was even more limited. JavaScript was mostly useful for providing limited enhancements to enrich document consumption capabilities, examples being in-browser form validation and lightweight insertion of dynamic HTML into an existing document.

As the [browser wars](https://en.wikipedia.org/wiki/Browser_wars) heated up, with Microsoft’s Internet Explorer reaching version 4 and beyond, the battle for the most robust feature set led to Microsoft’s introduction of what ultimately became the [XMLHttpRequest](https://xhr.spec.whatwg.org/) object which all browsers have now universally supported for well over a decade.

![Browse Wars - Internet Explorer vs Netscape](https://images.ctfassets.net/ee3ypdtck0rk/5Wrkjt1K4mc9B0EUsSW1GS/61ce12ea8b81913c1574f3216a10f0a1/browser-wars.png)

`XMLHttpRequest` could be thought of as a black swan event for the web, because it opened up the potential for web developers to start building truly-dynamic web applications that could communicate with the server silently in the background, without interrupting the user’s browsing experience. Because browser DOM capabilities had also been expanding significantly, particularly as Internet Explorer approached version 6, dynamic web “applications” started to become commonplace, offering everything from rich web-based email interfaces to in-browser website content management systems.

Progress begets progress, as they say, and naturally, developers began to explore ways to implement applications having more of a “realtime” aspect to their function. Web-based chat rooms and simple games were early examples at the time. The HTTP protocol made these sorts of use cases very challenging to implement though, and it was common to see applications polling servers repeatedly to check for new data, seeing as servers had no way to proactively notify a user whenever new data became available. Given the extreme inefficiency of this approach, creative ways to manipulate the HTTP request-response model into more of a real-time medium began to emerge. Among these techniques, probably the most popular to emerge was **long polling**.

## How does long polling work?

[Long polling](https://en.wikipedia.org/wiki/Push_technology#Long_polling) is essentially a more efficient form of the original polling technique. Making repeated requests to a server wastes resources, as each new incoming connection must be established, the HTTP headers must be parsed, a query for new data must be performed, and a response (usually with no new data to offer) must be generated and delivered. The connection must then be closed, and any resources cleaned up. Rather than having to repeat this process multiple times for every client until new data for a given client becomes available, long polling is a technique where the server elects to hold a client’s connection open for as long as possible, delivering a response only after data becomes available or a timeout threshold has been reached.

Implementation is mostly a server-side concern. On the client side, only a single request to the server needs to be managed. When the response is received, the client can initiate a new request, repeating this process as many times as is necessary. The only difference to basic polling, as far as the client is concerned, is that a client performing basic polling may deliberately leave a small time window between each request so as to reduce its load on the server, and it may respond to timeouts with different assumptions than it would for a server that does not support long polling. With long polling, the client may be configured to allow for a longer timeout period (via a `Keep-Alive` header) when listening for a response – something that would usually be avoided seeing as the timeout period is generally used to indicate problems communicating with the server.

![HTTP long polling](https://images.ctfassets.net/ee3ypdtck0rk/1UuClqOdPD71XXPcFC8th1/67a75ff8542382ade6fd809671d96b44/long-polling-f6e3a73a589fe25d7c7b622a8487a2e8a27a11f00b22b574abb021fbcd7ac2db.png)

Apart from these concerns, there is little else that a client needs to do that would be different than if it were engaging in basic polling. By contrast, the server needs to manage the unresolved state of multiple connections, and it may need to implement strategies for preserving session state when multiple servers and load balancers are in use (commonly referred to as session “stickiness”). It also needs to gracefully handle connection timeout issues, which are much more likely to occur than with designed-for-purpose protocols such as [WebSockets](https://www.ably.io/topic/websockets), a standard which did not arrive until years after long polling was established as a conventional technique for pseudo-realtime communication.

## Considerations when using long polling

As long polling is really just an improvisation applied to an underlying request-response mechanism, it comes with an additional degree of complexity in its implementation. There are various concerns you’ll need to account for when considering the design of your system’s architecture.

### Message ordering and delivery guarantees

Reliable message ordering can be an issue with long polling because it's possible for multiple HTTP requests from the same client to be in flight simultaneously. For example, if a client has two browser tabs open consuming the same server resource, and the client-side application is persisting data to a local store such as `localStorage` or `IndexedDb`, there is no in-built guarantee that duplicate data won’t be written more than once. This could also happen if the client implementation uses more than one connection at a time, whether deliberately or as a result of a bug in the code.

Another issue is that a server may send a response, but network or browser issues may prevent the message from being successfully received. Unless some sort of message receipt confirmation process is implemented, a subsequent call to the server may result in missed messages.

Depending on the server implementation, confirmation of message receipt by one client instance may also cause another client instance to never receive an expected message at all, as the server could mistakenly believe that the client has already received the data it is expecting.

All of these concerns, and more need to be considered when implementing robust support for long polling in any realtime messaging system.

### Performance and scaling

Unfortunately, such complexity is difficult to scale effectively. To maintain the session state for a given client, that state must either be shareable among all servers behind a load balancer – a task with significant architectural complexity – or subsequent client requests within the same session must be routed to the same server to which their original request was processed. This form of deterministic “sticky” routing is problematic by design, especially when routing is performed on the basis of IP address, as it can place undue load on a single server in a cluster while leaving other servers mostly idle instead of spreading the load around efficiently. This can also become a potential denial-of-service attack vector – a problem which then requires further layers of infrastructure to mitigate that might otherwise have been unnecessary.

### Device support and fallbacks

In modern times (2018 at the time of this article), long polling can be less relevant for web application development given the widespread availability of real-time communication standards such as WebSockets and WebRTC. That said, there are cases where proxies and routers on certain networks will block WebSocket and WebRTC connections, or where network connectivity can make long-lived connection protocols such as these less practical. Besides, for certain client demographics, there may still be numerous devices and clients in use that lack support for newer standards. For these, long polling can serve as a good fail-safe fallback to ensure support for everyone, irrespective of their situation.

Given that long polling is implemented on the back of `XMLHttpRequest`, which is near universal in its support by devices that still have a non-negligible degree of remaining relevant on the modern web, there’s usually not much of a need to support further fallback layers. In cases where exceptions must be handled though, or where a server can be queried for new data but does not support long polling (let alone other more modern technology standards), basic polling can sometimes still be of limited use, and can be implemented using `XMLHttpRequest`, or via JSONP through simple HTML script tags.

Long polling can be implemented via JSONP if absolutely necessary. That said, given the time and effort – not to mention the inefficiency of resource consumption – involved in implementing these approaches, care should be taken to assess whether their support is worth the added cost when developing new applications and system architectures. In many cases, you may be able to get away with exclusive support for something more modern, such as [WebSockets](https://www.ably.io/concepts/websockets). Alternatively, you may wish to offload the management of these sorts of concerns to a specialist cloud provider such as [Ably](https://www.ably.io/).

## Open source long polling solutions

Most libraries don’t implement long polling in isolation from other transports because, in general, long polling is usually accompanied with other transport strategies, either as a fallback, or with those transports as fallbacks when long polling doesn’t work. In 2018 and beyond, standalone long polling libraries are particularly uncommon, given that it’s a technique that is quickly losing relevance in the face of widespread support for more modern alternatives.

Nevertheless, below are a handful of options for a few different languages:

### Go

Golongpoll is a decent library for implementing long polling in the Go language:

```
import  "github.com/jcuga/golongpoll"

// This launches a goroutine and creates channels for all the plumbing
manager, err := golongpoll.StartLongpoll(golongpoll.Options{})  // default options

// Pass the manager around or create closures and publish:
manager.Publish("subscription-category", "Some data.  Can be string or any obj convertible to JSON")
manager.Publish("different-category", "More data")

// Expose events to browsers
// See subsection on how to interact with the subscription handler
http.HandleFunc("/events", manager.SubscriptionHandler)
http.ListenAndServe("127.0.0.1:8081", nil)
```

You can find out more about this library from the original [GitHub repo](https://github.com/jcuga/golongpoll).

### PHP

As per its [README](https://github.com/panique/php-long-polling/blob/master/README.md), php-long-polling is

> *“a very simple demonstration of long-polling with AJAX (jQuery) and PHP […] This is an improved, cleaned and documented fork of* [*php-ajax-long-polling*](https://github.com/lincolnbrito/php-ajax-long-polling)*“*

As the code for this library is extremely simple you can take a look at the [implementation](https://github.com/panique/php-long-polling/blob/master/server/server.php) directly – it’s fairly self-explanatory.

You can find out more about this library from the original [GitHub repo](https://github.com/panique/php-long-polling).

### Node.js

From its [README](https://github.com/fanout/pollymer/blob/master/README.md), we find out that

> *"Pollymer is a general-purpose AJAX library that provides conveniences for long-polling applications, such as request retries, exponential backoff between requests, randomized request delaying, and workarounds for browser 'busy' indications. It also implements multiple transports to ensure cross-domain access works in all major browsers".*

Optional extras include support for JSON-P and logging.

```
var req = new Pollymer.Request();
req.on('finished', function(code, result, headers) { ... });
req.on('error', function(reason) { ... });
var headers = { ... };
var body = 'some data';
req.maxTries = 2; // try twice
req.start('POST', 'http://example.com/path', headers, body);
```

You can find out more about this library from the original [GitHub repo](https://github.com/fanout/pollymer).

### Python

Simple-Comet-Server is a minimalist HTTP long-polling server for the Python language. It supports multiple channels and cross-domain requests. It requires Python >= 2.5 and the [Twisted framework](https://twistedmatrix.com/trac/). Unlike the other libraries mentioned above, this server runs as a kind of broker that sits between the client and the server-side applications. When the server wishes to publish messages to a client, it can make a request to an HTTP endpoint that can be easily hidden behind a firewall, ensuring that is only accessible by server-side processes.

The [README ](https://github.com/jedisct1/Simple-Comet-Server/blob/master/README.markdown)provides examples for:

- Registering a channel
- Pushing data to a channel
- Registering a client
- Monitoring the server
- Reading data from channels

You can find out more about this library from the original [GitHub repo](https://github.com/jedisct1/Simple-Comet-Server).

## Long polling and Ably

Ably is an enterprise-ready [pub/sub messaging platform](https://www.ably.io/pub-sub-messaging). We make it easy to efficiently design, quickly ship, and seamlessly scale critical realtime functionality delivered directly to end-users. Everyday we deliver billions of realtime messages to millions of users for thousands of companies.

We power the apps people, organizations, and enterprises depend on everyday like **Lightspeed System’s** realtime device management platform for over seven million school-owned devices, **Vitac’s** live captioning for 100s of millions of multilingual viewers for events like the Olympic Games, and **Split’s** realtime feature flagging for one trillion feature flags per month.

We’re the only pub/sub platform with a [suite of baked-in services](https://ably.com/platform) to build complete realtime functionality: [presence](https://ably.com/documentation/realtime/presence) shows a driver’s live GPS location on a home-delivery app, [history](https://www.ably.io/documentation/core-features/history) instantly loads the most recent score when opening a sports app, [stream resume](https://www.ably.io/documentation/realtime/connection#connection-state-recovery) automatically handles reconnection when swapping networks, and our [integrations](https://ably.com/reactor) extend Ably into third-party clouds and systems like AWS Kinesis and RabbitMQ. With 25+ SDKs we target every major platform across web, mobile, and IoT.

Our platform is mathematically modelled around [Four Pillars of Dependability](https://www.ably.io/four-pillars-of-dependability) so we’re able to ensure messages don’t get lost while still being delivered at low latency over a [secure](https://ably.com/security-and-compliance), reliable, and [highly available](https://www.ably.io/uptime) [global edge network](https://ably.com/network).

[Try our APIs](https://www.ably.com/signup/?utm_source=topic-longpolling&utm_medium=textlink&utm_campaign=optforconv) and see why developers from startups to industrial giants choose to build on Ably because they simplify engineering, minimize DevOps overhead, and increase development velocity.

As part of our offering, we’ve implemented a robust suite of transports in the [client libraries](https://www.ably.io/download) that we make available for communication with our servers, with WebSockets being the primary protocol for realtime communications. However, long polling is fully-supported as an automatic fallback transport when newer standards such as WebSockets are not available on the client-side.

---------------------------

# What is HTTP Long Polling?

## In a Nutshell: HTTP Long Polling

To overcome this deficiency, Web app developers can implement a technique called HTTP long polling, where the client polls the server requesting new information. The server holds the request open until new data is available. Once available, the server responds and sends the new information. When the client receives the new information, it immediately sends another request, and the operation is repeated. This effectively emulates a server push feature. A simple diagram below:

For a technical, in depth look at HTTP long polling and implementations for Python, Ruby and JavaScript, check out our article on [WebSockets and Long Polling in JavaScript, Ruby and Python](https://www.pubnub.com/blog/websockets-and-long-polling-in-javascript-ruby-and-python/).

## Considerations for HTTP Long Polling

There are a couple of things to consider when using HTTP long polling to build realtime interactivity in your application, both in terms of developing and operations/scaling.

- As usage grows, how will you orchestrate your realtime backend?
- When mobile devices rapidly switch between WiFi and cellular networks or lose connections, and the IP address changes, does long polling automatically re-establish connections?
- With long polling, can you manage the message queue and catch up missed messages?
- Does long polling provide load balancing or failover support across multiple servers?

When building a realtime application with HTTP long polling for server push, you’ll have to develop your own communication management system. This means that you’ll be responsible for updating, maintaining, and scaling your backend infrastructure.

## Backend Infrastructure for Realtime Functionality

With these considerations in mind, that's where [a realtime communication platform like PubNub](https://www.pubnub.com/products/pubnub-platform/) comes in. PubNub takes care of the backend infrastructure for you, so you don’t have to worry about maintaining and orchestrating your realtime network.

When looking at HTTP long polling with the goal of streaming data, PubNub is a low-latency and low-overhead realtime Web app communication environment, and features the ability to send messages to a single client, client groups and all clients. Upgrading to PubNub is both rapid and easy since PubNub is based on a [publish/subscribe model](https://www.pubnub.com/learn/glossary/what-is-publish-subscribe/).

## Key Benefits of Protocol Agnostic Realtime Messaging

Instead of relying solely on HTTP long polling for realtime messaging, a protocol agnostic approach is beneficial. PubNub automatically chooses the best protocols and frameworks depending on environment, latency, etc. Any server or client code wanting to communicate makes a single API call to publish or subscribe to data channels.

The code is identical between clients and servers, making implementation much simpler than using HTTP long polling. In terms of connection management, the network handles network redundancy, routing topology, loading balancing, failover, and channel access. Additionally, PubNub offers core building blocks for building realtime into your application.

- **Presence** – Detect when users enter/leave your app and whether machines are online, for applications like [in-app chat](https://www.pubnub.com/use-case/in-app-chat/).
- **Storage & Playback** – Store realtime message streams for future retrieval and playback.
- **Mobile Push Gateway** – Manage the complexities of realtime apps on mobile devices, including [Push Notifications](https://www.pubnub.com/use-case/alerts-and-push-notifications/).
- **Access Management** – Fine grain Publish and Subscribe permissions down to person, device or channel, for [HIPAA-compliant chat](https://www.pubnub.com/industry/healthcare/) applications or other sensitive use cases.
- **Security** – Secure all communications with enterprise-grade encryption standards.
- **Analytics** – Leverage visualizations into your realtime data streams.
- **Data Sync** – Sync application state across clients in realtime, for [digital classrooms](https://www.pubnub.com/industry/elearning-and-collaboration/) and [multiplayer game development](https://www.pubnub.com/industry/gaming/).

---------------------

# Known Issues and Best Practices for the Use of Long Polling and Streaming in Bidirectional HTTP draft-loreto-http-bidirectional-07

### 1. Introduction

The Hypertext Transfer Protocol [[RFC2616\]](https://tools.ietf.org/id/draft-loreto-http-bidirectional-07.html#RFC2616) is a request/response protocol. HTTP defines the following entities: clients, proxies, and servers. A client establishes connections to a server for the purpose of sending HTTP requests. A server accepts connections from clients in order to service HTTP requests by sending back responses. Proxies are intermediate entities that can be involved in the delivery of requests and responses from the client to the server and vice versa.

In the standard HTTP model, a server cannot initiate a connection with a client nor send an unrequested HTTP response to the client; thus the server cannot push asynchronous events to clients. Therefore, in order to receive asynchronous events as soon as possible, the client needs to poll the server periodically for new content. However, continual polling can consume significant bandwidth by forcing a request/response round trip when no data is available. It can also be inefficient because it reduces the responsiveness of the application since data is queued until the server receives the next poll request from the client.

In order to improve this situation, several server push programming mechanisms have been implemented in recent years. These mechanisms, which are often grouped under the common label "Comet" [[COMET\]](https://tools.ietf.org/id/draft-loreto-http-bidirectional-07.html#COMET), enable a web server to send updates to clients without waiting for a poll request from the client. Such mechanisms can deliver updates to clients in a more timely manner while avoiding the latency experienced by client applications due to the frequent opening and closing of connections necessary to periodically poll for data.

The two most common server push mechanisms are "HTTP long polling" and "HTTP streaming":

> - HTTP Long Polling:
>
>   The server attempts to "hold open" (not immediately reply to) each HTTP request, responding only when there are events to deliver. In this way, there is always a pending request to which the server can reply for the purpose of delivering events as they occur, thereby minimizing the latency in message delivery.
>
> - HTTP Streaming:
>
>   The server keeps a request open indefinitely; that is, it never terminates the request or closes the connection, even after it pushes data to the client.



It is possible to define other technologies for bidirectional HTTP, however such technologies typically require changes to HTTP itself (e.g., by defining new HTTP methods). This document focuses only on bidirectional HTTP technologies that work within the current scope of HTTP as defined in [[RFC2616\]](https://tools.ietf.org/id/draft-loreto-http-bidirectional-07.html#RFC2616) (HTTP 1.1) and [[RFC1945\]](https://tools.ietf.org/id/draft-loreto-http-bidirectional-07.html#RFC1945) (HTTP 1.0).

The authors acknowledge that both the "HTTP long polling" and "HTTP streaming" mechanisms stretch the original semantic of HTTP and that the HTTP protocol was not designed for bidirectional communication. This document neither encourages nor discourages the use of these mechanisms, and takes no position on whether they provide appropriate solutions to the problem of providing bidirectional communication between clients and servers. Instead, this document merely identifies technical issues with these mechanisms and suggests best practices for their deployment.

The remainder of this document is organized as follows. [Section 2](https://tools.ietf.org/id/draft-loreto-http-bidirectional-07.html#polling) analyzes the "HTTP long polling" technique. [Section 3](https://tools.ietf.org/id/draft-loreto-http-bidirectional-07.html#streaming) analyzes the "HTTP streaming" technique. [Section 4](https://tools.ietf.org/id/draft-loreto-http-bidirectional-07.html#tech) provides an overview of the specific technologies that use server-push technique. [Section 5](https://tools.ietf.org/id/draft-loreto-http-bidirectional-07.html#practices) lists best practices for bidirectional HTTP using existing technologies.



### 2. HTTP Long Polling

### 2.1. Definition

With the traditional or "short" polling technique, a client sends regular requests to the server and each request attempts to "pull" any available events or data. If there are no events or data available, the server returns an empty response and the client waits for some time before sending another poll request. The polling frequency depends on the latency that the client can tolerate in retrieving updated information from the server. This mechanism has the drawback that the consumed resources (server processing and network) strongly depend on the acceptable latency in the delivery of updates from server to client. If the acceptable latency is low (e.g., on the order of seconds) then the polling frequency can cause an unacceptable burden on the server, the network, or both.

By contrast with such "short polling", "long polling" attempts to minimize both latency in server-client message delivery and the use of processing/network resources. The server achieves these efficiencies by responding to a request only when a particular event, status, or timeout has occurred. Once the server sends a long poll response, typically the client immediately sends a new long poll request. Effectively this means that at any given time the server will be holding open a long poll request, to which it replies when new information is available for the client. As a result, the server is able to asynchronously "initiate" communication.

The basic life cycle of an application using "HTTP long polling" is as follows:

1. The client makes an initial request and then waits for a response.
2. The server defers its response until an update is available, or until a particular status or timeout has occurred.
3. When an update is available, the server sends a complete response to the client.
4. The client typically sends a new long poll request, either immediately upon receiving a response or after a pause to allow an acceptable latency period.

The HTTP long polling mechanism can be applied to either persistent or non-persistent HTTP connections. The use of persistent HTTP connections will avoid the additional overhead of establishing a new TCP/IP connection [[TCP\]](https://tools.ietf.org/id/draft-loreto-http-bidirectional-07.html#TCP) for every long poll request.

### 2.2. HTTP Long Polling Issues

The HTTP long polling mechanism introduces the following issues.

> - Header Overhead:
>
>   With the HTTP long polling technique, every long poll request and long poll response is a complete HTTP message and thus contains a full set of HTTP headers in the message framing. For small, infrequent messages, the headers can represent a large percentage of the data transmitted. If the network MTU (Maximum Transmission Unit) allows all the information (including the HTTP header) to fit within a single IP packet, this typically does not represent a significant increase in the burden for networking entities. On the other hand, the amount of transferred data can be significantly larger than the real payload carried by HTTP and this can have a significant impact (e.g., when volume-based charging is in place).
>
> - Maximal Latency:
>
>   After a long polling response is sent to a client, the server needs to wait for the next long polling request before another message can be sent to the client. This means that while the average latency of long polling is close to one network transit, the maximal latency is over three network transits (long poll response, next long poll request, long poll response). However, because HTTP is carried over TCP/IP, packet loss and retransmission can occur; therefore maximal latency for any TCP/IP protocol will be more than three network transits (lost packet, next packet, negative ack, retransmit). When HTTP pipelining (see [Section 5.2](https://tools.ietf.org/id/draft-loreto-http-bidirectional-07.html#pipelining)) is available, the latency due to the server waiting for a new request can be avoided.
>
> - Connection Establishment:
>
>   A common criticism of both short polling and long polling is that these mechanisms frequently open TCP/IP connections and then close them. However, both polling mechanisms work well with persistent HTTP connections that can be reused for many poll requests. Specifically, the short duration of the pause between a long poll response and the next long poll request avoids the closing of idle connections.
>
> - Allocated Resources:
>
>   Operating systems will allocate resources to TCP/IP connections and to HTTP requests outstanding on those connections. The HTTP long polling mechanism requires that for each client, both a TCP/IP connection and an HTTP request are held open. Thus it is important to consider the resources related to both of these when sizing an HTTP long polling application. Typically the resources used per TCP/IP connection are minimal and can scale reasonably. Frequently the resources allocated to HTTP requests can be significant, and scaling the total number of requests outstanding can be limited on some gateways, proxies, and servers.
>
> - Graceful Degradation:
>
>   A long polling client or server that is under load has a natural tendency to gracefully degrade in performance at a cost of message latency. If load causes either a client or server to run slowly, then events to be pushed to the client will queue (waiting either for the client to send a long poll request or for the server to free up CPU that can be used to process a long poll request that is being held at the server). If multiple messages are queued for a client, they might be delivered in a batch within a single long poll response. This can significantly reduce the per-message overhead and thus ease the work load of the client or server for the given message load.
>
> - Timeouts:
>
>   Long polling requests need to remain pending or "hanging" until the server has something to send to the client. The timeout issues related to these pending requests are discussed under [Section 5.5](https://tools.ietf.org/id/draft-loreto-http-bidirectional-07.html#timeouts).
>
> - Caching:
>
>   Caching mechanisms implemented by intermediate entities can interfere with long polling requests. This issue is discussed under [Section 5.6](https://tools.ietf.org/id/draft-loreto-http-bidirectional-07.html#network).

### 3. HTTP Streaming

### 3.1. Definition

The "HTTP streaming" mechanism keeps a request open indefinitely. It never terminates the request or closes the connection, even after the server pushes data to the client. This mechanism significantly reduces the network latency because the client and the server do not need to open and close the connection.

The basic life cycle of an application using "HTTP streaming" is as follows:

1. The client makes an initial request and then waits for a response.
2. The server defers the response to a poll request until an update is available, or until a particular status or timeout has occurred.
3. Whenever an update is available, the server sends it back to the client as a part of the response.
4. The data sent by the server does not terminate the request or the connection. The server returns to step 3.

The HTTP streaming mechanism is based on the capability of the server to send several pieces of information on the same response, without terminating the request or the connection. This result can be achieved by both HTTP/1.1 and HTTP/1.0 servers.

An HTTP response content length can be defined using 3 options:

> - Content-Length header:
>
>   This indicates the size of the entity body in the message, in bytes.
>
> - Transfer-Encoding header:
>
>   The 'chunked' valued in this header indicates the message will break into chunks of known size if needed.
>
> - End of File (EOF):
>
>   This is actually the default approach for HTTP/1.0 where the connections are not persistent. Clients do not need to know the size of the body they are reading; instead they expect to read the body until the server closes the connection. Although with HTTP/1.1 the default is for persistent connections, it is still possible to use EOF by setting the 'Connection:close' header in either the request or the response, to indicate that the connection is not to be considered 'persistent' after the current request/response is complete. The client's inclusion of the 'Connection: close' header field in the request will also prevent pipelining.
>
> - 
>
>   The main issue with EOF is that it is difficult to tell the difference between a connection terminated by a fault and one that is correctly terminated.

An HTTP/1.0 server can use only EOF as a streaming mechanism. By contrast, both EOF and "chunked transfer" are available to an HTTP/1.1 server.

The "chunked transfer" mechanism is the one typically used by HTTP/1.1 servers for streaming. This is accomplished by including the header "Transfer-Encoding: chunked" at the beginning of the response, which enables it to send the following parts of the response in different "chunks" over the same connection. Each chunk starts with the hexadecimal expression of the length of its data, followed by CR/LF (the end of the response is indicated with a chunk of size 0).

```
        HTTP/1.1 200 OK
        Content-Type: text/plain
        Transfer-Encoding: chunked

        25
        This is the data in the first chunk

        1C
        and this is the second one

        0
```

 **Figure 1: Transfer-Encoding response**  

------

An HTTP/1.0 server will omit the Content-Length header in the response to achieve the same result, so it will be able to send the subsequent parts of the response on the same connection (in this case the different parts of the response are not explicitly separated by HTTP protocol, and the end of the response is achieved by closing the connection).

### 3.2. HTTP Streaming Issues

The HTTP streaming mechanism introduces the following issues.

> - Network Intermediaries:
>
>   The HTTP protocol allows for intermediaries (proxies, transparent proxies, gateways, etc.) to be involved in the transmission of a response from the server to the client. There is no requirement for an intermediary to immediately forward a partial response and it is legal for it to buffer the entire response before sending any data to the client (e.g., caching transparent proxies). HTTP streaming will not work with such intermediaries.
>
> - Maximal Latency:
>
>   Theoretically, on a perfect network, an HTTP streaming protocol's average and maximal latency is one network transit. However, in practice the maximal latency is higher due to network and browser limitations. The browser techniques used to terminate HTTP streaming connections are often associated with JavaScript and/or DOM (Document Object Model) elements that will grow in size for every message received. Thus in order to avoid unlimited memory growth in the client, an HTTP streaming implementation occasionally needs to terminate the streaming response and send a request to initiate a new streaming response (which is essentially equivalent to a long poll). Thus the maximal latency is at least three network transits. Also, because HTTP is carried over TCP/IP, packet loss and retransmission can occur; therefore maximal latency for any TCP/IP protocol will be more than three network transits (lost packet, next packet, negative ack, retransmit).
>
> - Client Buffering:
>
>   There is no requirement in existing HTTP specifications for a client library to make the data from a partial HTTP response available to the client application. For example, if each response chunk contains a statement of JavaScript, there is no requirement in the browser to execute that JavaScript before the entire response is received. However, in practice most browsers do execute JavaScript received in partial responses, although some require a buffer overflow to trigger execution; in most implementations, blocks of white space can be sent to achieve buffer overflow.
>
> - Framing Techniques:
>
>   Using HTTP streaming, several application messages can be sent within a single HTTP response. The separation of the response stream into application messages needs to be perfomed at the application level and not at the HTTP level. In particular it is not possible to use the HTTP chunks as application message delimiters, since intermediate proxies might "re-chunk" the message stream (for example by combining different chunks into a longer one). This issue does not affect the HTTP long polling technique, which provides a canonical framing technique: each application message can be sent in a different HTTP response.
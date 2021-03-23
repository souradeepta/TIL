# WebSockets - A Conceptual Deep Dive

The WebSocket protocol elevated the possibilities of communication over the Internet and gave way to a truly realtime web. This article is all about building a deeper understanding of what WebSockets are, how they came to be, and what’s actually going on under the hood of applications using WebSockets.

## First, a bit of background

WebSockets have been generally available only since a few years. Before they came along, the “realtime” web existed, but it was difficult to achieve, typically slower, and was delivered by hacking existing web technologies which were not designed for realtime applications.

The web has been built on the back of the [HTTP protocol](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol), which was originally designed entirely as a request-response mechanism. Open a connection, describe what you want, get back a response, then close the connection. This was fine in the early days of the web because, back then, we were really just dealing with a text document and maybe a few additional assets (usually images).

### Scripting the web with JavaScript

In 1995, [Netscape Communications](https://en.wikipedia.org/wiki/Netscape) hired Brendan Eich with the goal of embedding scripting capabilities into their Netscape Navigator browser, and thus JavaScript was born. Initially, JavaScript was kind of weird and couldn’t do a whole lot (especially with the extremely limited browser DOM that JavaScript had at its disposal), but it was useful for a few things, such as simple validation of input fields before submitting an HTML form to the server.

Microsoft soon entered the arena with Internet Explorer, which is where the original [browser wars](https://en.wikipedia.org/wiki/Browser_wars) really began. Both companies were competing with each other to have the best browser, so features and capabilities were inevitably added on a regular basis to both Netscape and Internet Explorer as each company strove to outdo the other.

![Browse Wars - Internet Explorer vs Netscape](https://images.ctfassets.net/ee3ypdtck0rk/5Wrkjt1K4mc9B0EUsSW1GS/61ce12ea8b81913c1574f3216a10f0a1/browser-wars.png)

### The birth of XMLHttpRequest and AJAX 

Two of the most significant capabilities that were soon introduced at that time were the ability to embed Java applets into a page, and Microsoft’s own offering – ActiveX controls. These were essentially precompiled components that could optionally present an embedded user interface of their own within a web page. More than that though, they allowed a whole host of additional possibilities beyond JavaScript’s own (at the time) meager suite of scripting capabilities.

While there were a few comparable networking capabilities available via Java, the most significant background communication feature first appeared in 1999, with the [Microsoft XMLHTTP](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest_in_IE6) `ActiveXObject` interface. It was available natively in Internet Explorer 5.0 without installing plugins, it could be instantiated with a single line of JavaScript, and it didn’t require any of the usual friction involved when dealing with Java applets. The XMLHTTP object made it possible to silently issue a request to a server and receive a response – all without reloading the page or otherwise interrupting the user’s experience. JavaScript code could then decipher the response and construct modifications to the page, enabling a whole host of rich experiences to be integrated into a website. Common early use cases included things like allowing a drop-down box to be populated with options based on a user’s prior input, and “instant” validation of username availability while filling out a user registration form.

Example JavaScript code that would have been used to instantiate an `XMLHTTP` object:

```js
// Note: this code only works in old versions of Internet Explorer
var xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
xmlhttp.open("GET", "/api/example", true);
xmlhttp.onreadystatechange = function () {
  if (xmlhttp.readyState == 4) {
    alert(xmlhttp.responseText);
  }
};
xmlhttp.send(null);
```

XMLHTTP later became the [XMLHttpRequest](https://xhr.spec.whatwg.org/) de facto standard due to its adoption by other browsers. This was about the time that the term “AJAX” was coined, standing for "Asynchronous JavaScript and XML”. The `JSON` standard later came along and made everything better, but the ‘X’ in AJAX (not to mention the ‘XML’ in XMLHttpRequest) never really went away, despite actual XML-formatted data having largely disappeared from standard messaging payloads.

Comparative example of modern use of the `XMLHttpRequest` object:

```JS
const req = new XMLHttpRequest();
req.addEventListener("load", () => console.log(this.responseText));
req.open("GET", "/api/example");
req.send();
```

As you can see, the modern analog is much the same as the original, albeit with a few additions to make things a little more concise.

### A world of new possibilities

In any case, [XMLHttpRequest](https://xhr.spec.whatwg.org/) was still following the same HTTP request-response model used to retrieve the original HTML document. There was no real notion of allowing a server to contact the user proactively, or to establish any kind of general two-way connection for more sophisticated use cases. All the while though, JavaScript was gaining new features and capabilities as time went on, and browsers were enhancing the document object model (DOM), leading to greater and greater potential concerning how JavaScript could be used to enrich the user’s experience of interacting with a web page.

![HTTP request-response cycle between client and server](https://images.ctfassets.net/ee3ypdtck0rk/7Ik3tj1R1gmJ0PRCZq4lFC/cd07bb4e246877a131c31548fcb62926/http-request-response.png)

As the potential for vibrant experiences started to become apparent, developers naturally gravitated toward the idea of client-server applications implemented directly in the browser. Before this, the standard paradigm for anything non-trivial was to build a dedicated software application, package it up with an installer, have the user download the installer and then install it natively on their machine. Needless to say, this was quite a barrier to entry for all but the tech-savvy user. Keeping the application up to date with fixes and enhancements was a challenge all of its own. It is easy to understand, then, how alluring it was to be able to build an application that neither required installation before it could be accessed nor user training and nagging to iterate on the software’s implementation.

### Hatching the real-time web

> *“Innovation is taking two things that already exist and putting them together in a new way.”* *-* [*Tom Freston*](https://en.wikipedia.org/wiki/Tom_Freston)

When something appears to be technically possible, and the potential reward is worth the effort, we’ll usually go to great lengths to bend what is available into a shape that serves our needs. Thus, developers took XMLHttpRequest and abused it to start emulating real-time back-and-forth communication between the web page and the server. Some of the techniques to do this became commonplace – “standard” even – and eventually started being referred to with the umbrella term *“*[*Comet*](https://en.wikipedia.org/wiki/Comet_(programming))*”.*

Probably the most common of these techniques was (is) [long polling](https://www.ably.io/topic/long-polling). This involves opening an [XMLHttpRequest](https://xhr.spec.whatwg.org/) connection to the server and leaving it open until ongoing communication is no longer required. Under normal circumstances, when making an HTTP request, the server’s response is streamed back to a client via the connection on which the request was made, the intent being allowing a browser to start rendering an HTML page while waiting for the next part of the document to be delivered by the server. With long polling, the effect of leaving an HTTP connection open is that the server can continue to deliver response data for as long as the connection remains open, and there is no technical requirement that the data be in one format or another, or that the request be closed after sending data to the client. The same applies to the HTTP request payload sent by the client. A server may begin delivering its response before the client’s request data has arrived in its entirety, and the client is not strictly required to stop sending request data until it chooses to do so. This means that, just as the server may continue delivering response data for the life of the connection, the client may do the same, resulting in a de facto two-way communication stream between server and client.

![HTTP long polling](https://images.ctfassets.net/ee3ypdtck0rk/1UuClqOdPD71XXPcFC8th1/67a75ff8542382ade6fd809671d96b44/long-polling-f6e3a73a589fe25d7c7b622a8487a2e8a27a11f00b22b574abb021fbcd7ac2db.png)

As enabling as it has been for the web application developer community in the absence of other suitable tools, long polling is tricky to do correctly and is fraught with unexpected complications that must be managed. The same goes for other Comet techniques, which are beyond the scope of this article. All in all, long polling is really just a case of repurposing the tools available to do something they weren’t really designed to do.

A real solution was needed – something which would empower developers with proper [TCP/IP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol) socket-style capabilities in a web environment. Such a solution would need to be built for the web, and it would need to address all of the concerns that arise when operating in a web environment.

## Enter, WebSockets!

Around the middle of 2008, the pain and limitations of using Comet when implementing anything truly robust were being felt particularly keenly by developers [Michael Carter](https://en.wikipedia.org/wiki/Michael_Carter_(entrepreneur)) and [Ian Hickson](https://en.wikipedia.org/wiki/Ian_Hickson). Through collaboration [on IRC](https://krijnhoetmer.nl/irc-logs/whatwg/20080618#l-1145) and [W3C mailing lists](https://lists.w3.org/Archives/Public/public-whatwg-archive/2008Jun/0165.html), they hatched a plan to introduce a new standard for modern real-time, bi-directional communication on the web, and thus [the name ‘WebSocket’ was coined](https://lists.w3.org/Archives/Public/public-whatwg-archive/2008Jun/0186.html).

The idea made its way into the W3C HTML draft standard and, shortly after, Michael Carter wrote [an article introducing the Comet community to the WebSockets.](http://cometdaily.com/2008/07/04/html5-websocket/) In 2010, Google Chrome 4 was the first browser to ship full support for WebSockets, with other browser vendors following suit over the course of the next few years. In 2011, [RFC 6455 – The WebSocket Protocol](https://tools.ietf.org/html/rfc6455) – was published to the IETF website.

Today, [all major browsers have full support for WebSockets](https://caniuse.com/#feat=websockets), even including Internet Explorer 10 and 11. Besides, browsers on both iOS and Android have supported WebSockets since 2013, which means that all in all, the modern landscape for WebSocket support is very healthy. Much of the “internet of things” or IoT runs on some version of Android as well, so WebSocket support on other types of devices is reasonably pervasive too, as of 2018.

### So what exactly are WebSockets anyway?

In a nutshell, [WebSockets](https://en.wikipedia.org/wiki/WebSocket) are a thin transport layer built on top of a device’s [TCP/IP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol) stack. The intent is to provide what is essentially an as-close-to-raw-as-possible TCP communication layer to web application developers while adding a few abstractions to eliminate certain friction that would otherwise exist concerning the way the web works. They also cater to the fact that the web has additional security considerations that must be taken into account to protect both consumers and service providers.

You may have heard WebSockets simultaneously referred to both as a “transport” and as a “protocol”. The former is more accurate, because while they are a protocol in the sense that a strict set of rules for establishing communication and enveloping the transmitted data must be adhered to, the standard does not take any stance regarding how the actual data payload is structured within the outer message envelope. In fact, part of the specification includes the option for the client and server to agree on a protocol with which the transmitted data will be formatted and interpreted. The standard refers to these as “subprotocols”, in order to avoid issues of ambiguity in the nomenclature. Examples of subprotocols are JSON, XML, MQTT, WAMP. These can ensure agreement not only about the way the data is structured, but also about the way communication must commence, continue and eventually terminate. As long as both parties understand what the protocol entails, anything goes. The WebSocket provides merely a transport layer over which that messaging process can be implemented, which is why most common subprotocols are not exclusive to WebSocket-based communications.

#### A quick note about authentication and authorization

Seeing as WebSockets is a thin layer built on top of TCP/IP, anything beyond the basic handshake and specification for message framing is really something that needs to be handled either on a per-application or per-library basis. [Quoting the RFC](https://tools.ietf.org/html/rfc6455#section-10.5):

> *"This protocol doesn’t prescribe any particular way that servers can authenticate clients during the WebSocket handshake. The WebSocket server can use any client authentication mechanism available to a generic HTTP server, such as cookies, HTTP authentication, or TLS authentication".*

In a nutshell, use the HTTP-based authentication methods you’d use anyway, or use a subprotocol such as [MQTT](https://www.ably.io/topic/mqtt) or [WAMP](https://wamp-proto.org/static/rfc/draft-oberstet-hybi-crossbar-wamp.html), both of which offer approaches for authentication and authorization.

### Getting the ball rolling with HTTP 

One of the early considerations when defining the WebSocket standard was to ensure that it “plays nicely” with the web. This meant recognizing that the web is generally addressed using URLs, not IP addresses and port numbers, and that a WebSocket connection should be able to take place with the same initial HTTP-based handshake used for any other type of web request.

Here’s what happens in a simple HTTP GET request. Let’s say there’s an HTML page hosted at http://www.example.com/index.html. Without getting too deep into [the HTTP protocol](https://tools.ietf.org/html/rfc2616) itself, it is enough to know that a request must start with what is referred to as [Request-Line](https://tools.ietf.org/html/rfc2616#section-5.1), followed by a sequence of key-value pair header lines, each telling the server something about what to expect in the subsequent request payload that will follow the header data, and what it can expect from the client regarding the kinds of responses it will be able to understand.

The very first token in the request is the HTTP method, which tells the server the type of operation that the client is attempting with respect to the referenced URL. The GET method is used when the client is merely requesting that the server deliver it a copy of the resource that is referenced by the specified URL.

A barebones example of a request header, formatted according to the HTTP RFC, looks like this:

```http
GET /index.html HTTP/1.1
Host: www.example.com
```

The request header, the server then formats a response header starting with a [Status-Line](https://tools.ietf.org/html/rfc2616#section-6.1), followed by a set of key-value header pairs that provide the client with complementary information from the server, with respect to the request that the server is responding to. The “Status-Line” tells the client the HTTP status code (usually `200` if there were no problems) and provides a brief “reason” text description explaining the status code. Key-value header pairs appear next, followed by the actual data that was requested (unless the status code indicated that the request was unable to be fulfilled for some reason).

```http
HTTP/1.1 200 OK
Date: Wed, 1 Aug 2018 16:03:29 GMT
Content-Length: 291
Content-Type: text/html
(additional headers...)

(response payload continues here...)
```

So what’s this got to do with WebSockets, you might ask?

### Ditching HTTP for something more appropriate

When making an HTTP request and receiving a response, the actual two-way network communication involved takes place over an active TCP/IP socket. The web URL that was requested in the browser is mapped via the global DNS system to an IP address, and the default port for HTTP requests is 80. This means that although a web URL was entered into the browser, the actual communication occurs via TCP/IP, using an IP address and port combination that looks something like, for example, `123.11.85.9:80`.

As we now know, WebSockets are built on top of the TCP stack as well, which means all we need is a way for the client and the server to jointly agree to hold the socket connection open and re-purpose it for ongoing communication. If they do this, then there is no technical reason why they can’t continue to use the socket to transmit any kind of arbitrary data, as long as they have both agreed as to how the binary data being sent and received should be interpreted.

To begin the process of re-purposing the TCP socket for WebSocket communication, the client can include a standard request header that was invented specifically for this kind of use case:

```http
GET /index.html HTTP/1.1
Host: www.example.com
Connection: Upgrade
Upgrade: websocket
```

![WebSockets communication between client and server](https://images.ctfassets.net/ee3ypdtck0rk/1u0ijAerVQdTEvV2RUGcGp/9b0b67b0014254f8af5064f63c4a81a6/websockets.png)

The `Connection` header tells the server that the client would like to negotiate a change in the way the socket is being used. The accompanying value, `Upgrade`, indicates that the transport protocol currently in use via TCP should change. Now that the server knows that the client wants to upgrade the protocol currently in use over the active TCP socket, the server knows to look for the corresponding `Upgrade` header, which will tell it which transport protocol the client wants to use for the remaining lifetime of the connection. As soon as the server sees `websocket` as the value of the `Upgrade` header, it knows that a WebSocket handshake process has begun.

Note that the handshake process (along with everything else) is outlined in [RFC 6455](https://tools.ietf.org/html/rfc6455), if you’d like to go into more detail than is covered in this article.

### Avoiding funny business

The first part of the WebSocket handshake, other than what is described above, involves proving that this is actually a proper [WebSocket upgrade handshake](https://en.wikipedia.org/wiki/WebSocket#Protocol_handshake) and that the process is not being circumvented or emulated via some kind of intermediate trickery either by the client or perhaps by a proxy server that sits in the middle.

When initiating an upgrade to a WebSocket connection, the client must include a `Sec-WebSocket-Key` header with a value unique to that client. Here’s an example:

```http
Sec-WebSocket-Key: BOq0IliaPZlnbMHEBYtdjmKIL38=
```

The above is automatic and handled for you if you're using the `WebSocket` class provided in modern browsers. You need only look for it on the server side and produce a response.

When responding, the server must append the special GUID value `258EAFA5-E914-47DA-95CA-C5AB0DC85B11` to the key, generate a SHA-1 hash of the resultant string, then include it as the base-64-encoded value of a `Sec-WebSocket-Accept` header that it includes in the response:

```http
Sec-WebSocket-Accept: 5fXT1W3UfPusBQv/h6c4hnwTJzk=
```

In a Node.js WebSocket server, we could write a function to generate this value like so:

```js
const crypto = require('crypto');

function generateAcceptValue (acceptKey) {
  return crypto
    .createHash('sha1')
    .update(acceptKey + '258EAFA5-E914-47DA-95CA-C5AB0DC85B11', 'binary')
    .digest('base64');
}
```

We’d then need only call this function, passing the value of the `Sec-WebSocket-Key` header as the argument, and set the function return value as the value of the `Sec-WebSocket-Accept` header when sending the response.

To complete the handshake, write the appropriate HTTP response headers to the client socket. A bare-bones response would look something like this:

```http
HTTP/1.1 101 Web Socket Protocol Handshake
Upgrade: WebSocket
Connection: Upgrade
Sec-WebSocket-Accept: m9raz0Lr21hfqAitCxWigVwhppA=
```

We’re not actually quite finished with the handshake at this point – there are a couple more things to think about.

### Subprotocols – agreeing upon a shared dialect

The client and server generally need to agree on a compatible strategy with respect to how they format, interpret and organize the data itself, both within a given message, and over time, from one message to the next. This is where subprotocols (mentioned earlier) come in. If the client knows that it can deal with one or more specific application-level protocols (such as WAMP, MQTT, etc.), it can include a list of the protocols it understands when making the initial HTTP request. If it does so, the server is then required to either select one of those protocols and include it in a response header or to otherwise fail the handshake and terminate the connection.

Example subprotocol request header:

```http
Sec-WebSocket-Protocol: mqtt, wamp
```

Example reciprocal header issued by the server in the response:

```http
Sec-WebSocket-Protocol: wamp
```

Note that the server must select **precisely one protocol** from the list provided by the client. Selecting more than one would mean that the server cannot reliably or consistently interpret the data in subsequent WebSocket messages. An example of this would be if both `json-ld` and `json-schema` were selected by the server. Both are data formats built on the JSON standard, and there would be numerous edge cases where one could be interpreted as the other, leading to unexpected errors when processing the data. **While admittedly not messaging protocols per se, the example is still applicable.**

When both the client and server are implemented to use a common messaging protocol from the outset, the `Sec-WebSocket-Protocol` header can be omitted in the initial request, in which case the server can ignore this step. Subprotocol negotiation is most useful when implementing general-purpose services, infrastructure, and tools where there can be no forward guarantee that both the client and server will understand each other once the WebSocket connection has been established.

Standardized names for common protocols [should be registered](https://tools.ietf.org/html/rfc6455#section-11.5) with the [IANA registry for WebSocket Subprotocol Names](https://www.iana.org/assignments/websocket/websocket.xml), which, at the time of this article, has 36 names already registered, including soap, xmpp, wamp, mqtt, et al. Though the registry is the canonical source for mapping a subprotocol name to its interpretation, the only strict requirement is that the client and server agree upon what their mutually-selected subprotocol actually means, irrespective of whether not it appears in the IANA registry.

Note that if the client has requested use of a subprotocol but hasn’t provided any that the server can support, the server must send a failure response and close the connection.

#### WebSocket extensions

There is also a header for defining extensions to the way the data payload is encoded and framed, but at the time of this article, only one standardized extension type exists, and it provides a kind of WebSocket-equivalent to gzip compression in messages. Another example of where extensions might come into play is multiplexing – the use of a single socket to interleave multiple concurrent streams of communication.

WebSocket extensions is a somewhat advanced topic, and are really beyond the scope of this article. For now, it is enough to know what they are, and how they fit into the picture.

## The client-side – using WebSockets in the browser

The WebSocket API is defined in the [WHATWG HTML Living Standard](https://html.spec.whatwg.org/multipage/web-sockets.html#network) and is actually pretty trivial to use. Constructing a WebSocket takes one line of code:

```js
const ws = new WebSocket('ws://example.org');
```

Note the use of `ws` where you’d normally have the `http` scheme. There’s also the option to use `wss` where you’d normally use `https`. These protocols were introduced in tandem with the `WebSocket` specification, and are designed to represent an HTTP connection that includes a request to upgrade the connection to use WebSockets.

Creating the `WebSocket` object doesn’t do a lot by itself. The connection is established asynchronously, so you’d need to listen for the completion of the handshake before sending any messages, and also include a listener for messages received from the server:

```js
ws.addEventListener('open', () => {
  // Send a message to the WebSocket server
  ws.send('Hello!');
});

ws.addEventListener('message', event => {
  // The `event` object is a typical DOM event object, and the message data sent
  // by the server is stored in the `data` property
  console.log('Received:', event.data);
});
```

There are also the `error` and `close` events. WebSockets don’t automatically recover when connections are terminated – this is something you need to implement yourself, and is part of the reason why there are many client-side libraries in existence. While the `WebSocket` class is straightforward and easy to use, it really is just a basic building block. Support for different subprotocols or additional features such as messaging channels must be implemented separately.

### Generating and parsing WebSocket message frames

Once the handshake response has been sent to the client, the client and server are free to begin communicating using their chosen subprotocol (if any). Take a quick look at [section 5 of the RFC](https://tools.ietf.org/html/rfc6455#section-5) to get a sense of what’s involved.

WebSocket messages are delivered in packages called “frames”, which begin with a message header, and conclude with the “payload” – the message data for this frame. Large messages may split the data over several frames, in which case you need to keep track of what you’ve received so far and piece the data together once it has all arrived.

## Moving forward – other things you might consider

A basic implementation of a WebSocket server is really just the first stage of the process. Here are just a few of the things you’d want to think about when taking things to the next level:

- What framing extensions will you support, such as per-message deflation?
- What degree of client interoperability are you aiming for?
- Are messages being received in the same order they were sent, and if not, how can you prevent this from putting your application into an invalid state?
- Do you need guarantees on message delivery, and if so, what strategies can you implement to this end?
- How many connections are active on your server?
- Are any connections hogging all of the server’s resources?
- Are any connections idle and should ideally be dropped?
- What is the duration of the average connection lifespan?
- Are connections being dropped prematurely/unexpectedly, and if so, how can you retain diagnostic data to explain why?
- Are you experiencing brief connection spikes ever, and if so, what is the performance impact on your server?
- How much bandwidth is being used overall, and how is it impacting your budget?
- Is your server’s capacity near its limit, and if not, how soon will that threshold be reached?
- How will you automatically add additional server capacity if and when it is needed?

Think about the messaging protocols available, such as [MQTT](https://en.wikipedia.org/wiki/MQTT), [WAMP](https://en.wikipedia.org/wiki/Web_Application_Messaging_Protocol), etc., and whether they can provide a solution to some of these questions. Consider existing libraries and frameworks, and the additional features they offer beyond simple, bare-bones management of WebSocket connections. If you have a particular need to scale, and limited workforce or expertise to do so effectively, consider leveraging [cloud-based realtime messaging solutions](https://www.ably.io/topic/websockets#web-sockets-and-ably) that have already solved these problems for you.

## Some of the open source WebSocket libraries you can use right now

There are two primary classes of WebSocket libraries; those that implement the protocol and leave the rest to the developer, and those that build on top of the protocol with various additional features commonly required by realtime messaging applications, such as restoring lost connections, pub/sub and channels, authentication, authorization, etc. The latter variety often requires that their own libraries be used on the client side, rather than just using the raw `WebSocket` API provided by the browser. As such, it becomes crucial to make sure you’re happy with how they work and what they’re offering. You may find yourself locked into your chosen solution’s way of doing things once it has been integrated into your architecture, and any issues with reliability, performance, and extensibility may come back to bite you.

I’ll start with a list of those that fall into the first of the above two categories.

Note: All of the following are open-source libraries.

### ws

[ws](https://github.com/websockets/ws) is a “simple to use, blazing fast and thoroughly tested WebSocket client and server for Node.js”. It is definitely a barebones implementation, designed to do all the hard work of implementing the protocol, however additional features such as connection restoration, pub/sub, and so forth, are concerns you’ll have to manage yourself.

Here is a list of things you need to consider when [building realtime apps with Node.js and WebSockets.](https://www.ably.io/topic/websockets-nodejs)

**Client (browser, before bundling):**

```js
const WebSocket = require('ws');

const ws = new WebSocket('ws://www.host.com/path');

ws.on('open', function open() {
  ws.send('something');
});

ws.on('message', function incoming(data) {
  console.log(data);
});
```

**Server:**

```js
const WebSocket = require('ws');

const wss = new WebSocket.Server({ port: 8080 });

wss.on('connection', function connection(ws) {
  ws.on('message', function incoming(message) {
    console.log('received: %s', message);
  });

  ws.send('something');
});
```

### μWebSockets

[μWS](https://github.com/uNetworking/uWebSockets) is a drop-in replacement for ws, implemented with a particular focus on performance and stability. To the best of my knowledge, μWS is the fastest WebSocket server implementation available by a mile. It’s actually used under the hood by SocketCluster, which I’ll talk about below.

There has been a little [controversy](https://www.reddit.com/r/node/comments/91kgte/uws_has_been_deprecated/) around μWS recently due to the author having attempted to pull it from NPM for philosophical reasons, but [the latest working version](https://www.npmjs.com/package/uws/v/10.148.1) remains on NPM and can be installed specifying that version explicitly when installing from NPM. That said, the author is working on [a new version](https://github.com/uNetworking/v0.15), with accompanying [node.js bindings](https://github.com/uNetworking/uWebSockets-node) also [in development](https://github.com/uNetworking/uWebSockets-node/issues/2).

**Server:**

```js
var WebSocketServer = require('uws').Server;
var wss = new WebSocketServer({ port: 3000 });

function onMessage(message) {
  console.log('received: ' + message);
}

wss.on('connection', function(ws) {
  ws.on('message', onMessage);
  ws.send('something');
});
```

### faye-websocket

[faye-websocket](https://github.com/faye/faye-websocket-node) is a standards-compliant WebSocket implementation for both the client and server, and originated from the Ruby-on-Rails community as part of the [Faye project](https://faye.jcoglan.com/). As per the Github, project:

> *"It does not provide a server itself, but rather makes it easy to handle WebSocket connections within an existing Node application. It does not provide any abstraction other than the standard WebSocket API."*

In the sample code below for the server, you can see that all of the work of handling the connection upgrade and translating messaging frames from inbound socket buffers is handled by the `WebSocket` class provided by the library. As with other minimal solutions, this is a no-frills implementation – you’ll need to handle application-specific concerns yourself.

**Client (browser, before bundling):**

```js
var WebSocket = require('faye-websocket'),
var ws = new WebSocket.Client('ws://www.example.com/');

ws.on('open', function(event) {
  console.log('open');
  ws.send('Hello, world!');
});

ws.on('message', function(event) {
  console.log('message', event.data);
});

ws.on('close', function(event) {
  console.log('close', event.code, event.reason);
  ws = null;
});
```

**Server:**

```js
var WebSocket = require('faye-websocket'),
var http = require('http');

var server = http.createServer();

server.on('upgrade', function(request, socket, body) {
  if (WebSocket.isWebSocket(request)) {
    var ws = new WebSocket(request, socket, body);

    ws.on('message', function(event) {
      ws.send(event.data);
    });

    ws.on('close', function(event) {
      console.log('close', event.code, event.reason);
      ws = null;
    });
  }
});

server.listen(8000);
```

### Socket.io

[Socket.io](https://github.com/socketio/socket.io) has been around for a while now and could be thought of as the “jQuery” of WebSockets. It uses [long polling](https://www.ably.io/topic/long-polling) and WebSockets for its transports, by default starting with long polling and then upgrading to WebSockets if available. Given the waning relevance of long polling, Socket.io’s main drawcards these days are its [other features](https://stackoverflow.com/questions/38546496/moving-from-socket-io-to-raw-websockets/38546537#38546537), such as restoring dropped connections, automatic support for JSON, and “namespaces”, which are essentially isolated messaging channels multiplexed over the same client connection.

Socket.io isn’t actually interchangeable with generic WebSockets solutions – either on the server side or on the client side – and attempting to connect to something other than a Socket.io client or server will fail. It has its own additional handshake protocol, and some additional metadata included in each message.

Should you use it? On the plus side, in addition to [the simplicity of getting up and running](https://socket.io/get-started/chat/), it’s well-established and there is a wealth of learning material out there if you get stuck. On the other hand, there have been plenty of reports of memory leaks and general performance issues over time, and despite there being [a mountain of issues](https://github.com/socketio/socket.io/issues) in the Github repository, responses are few, and [commits and updates](https://github.com/socketio/socket.io/commits/master) these days seem relatively infrequent. Just like jQuery, Socket.io is largely a product of a bygone era, and for new projects, you’d be better off with something a little more modern. jQuery does still have its fans though, so…

**Client (browser, before bundling):**

```js
const io = require('socket.io-client');
const socket = io();
socket.emit('chat message', 'Hello there');
```

**Server:**

```js
var app = require('express')();
var http = require('http').Server(app);
var io = require('socket.io')(http);

app.get('/', function(req, res){
  res.sendFile(__dirname + '/index.html');
});

io.on('connection', function(socket){
  console.log('a user connected');
});

http.listen(3000, function(){
  console.log('listening on *:3000');
});
```

### SocketCluster

[SocketCluster](https://socketcluster.io/) is a full-featured client-server messaging framework built entirely around WebSockets, and uses μWebSockets under the hood. From the website:

> *"SocketCluster is an open source real-time framework for Node.js. It supports both direct client-server communication and group communication via pub/sub channels. It is designed to easily scale to any number of processes/hosts and is ideal for building chat systems. See SocketCluster Design Patterns for Chat".*

Unlike simpler solutions such as Socket-io, SocketCluster requires slightly more installation, but it generally very easy to get up and running.

These are taken from the [getting started guide](https://socketcluster.io/#!/docs/getting-started) in the [SocketCluster documentation](https://socketcluster.io/#!/docs).

**Client (browser, before bundling):**

```js
var socket = socketCluster.create();
socket.emit('sampleClientEvent', {message: 'This is an object with a message property'});
```

**Server:**

```js
var SocketCluster = require('socketcluster');
var socketCluster = new SocketCluster({
  workers: 1, // Number of worker processes
  brokers: 1, // Number of broker processes
  port: 8000, // The port number on which your server should listen
  appName: 'myapp', // A unique name for your app

  // Switch wsEngine to 'sc-uws' for a MAJOR performance boost (beta)
  wsEngine: 'ws',

  /* A JS file which you can use to configure each of your
   * workers/servers - This is where most of your backend code should go
   */
  workerController: __dirname + '/worker.js',

  /* JS file which you can use to configure each of your
   * brokers - Useful for scaling horizontally across multiple machines (optional)
   */
  brokerController: __dirname + '/broker.js',

  // Whether or not to reboot the worker in case it crashes (defaults to true)
  rebootWorkerOnCrash: true
});
```

### SockJS

[SockJS](http://sockjs.org/) has as its primary feature WebSocket emulation, which puts it into an increasingly-outdated class of solutions, given that support for WebSockets is pretty pervasive these days. From their client repository:

> *"SockJS is a browser JavaScript library that provides a WebSocket-like object. SockJS gives you a coherent, cross-browser, Javascript API which creates a low latency, full duplex, cross-domain communication channel between the browser and the web server".*

It supports numerous [fallback transports](https://github.com/sockjs/sockjs-client#supported-transports-by-name), giving it a fairly robust suite of support for Comet techniques. Again though, how relevant is this in the modern web landscape?

See [SockJS: web messaging ain’t easy](https://github.com/sockjs/sockjs-client/wiki/[Article]-SockJS-web-messaging-ain’t-easy) and [SockJS: WebSocket emulation done right](https://github.com/sockjs/sockjs-client/wiki/[Article]-SockJS-WebSocket-emulation-done-right) for some insight into the philosophy behind the SockJS suite of libraries.

On the server side, check out the [SockJS-node](https://github.com/sockjs/sockjs-node) project.

**Client (browser, before bundling):**

```js
var sock = new SockJS('https://mydomain.com/my_prefix');
sock.onopen = function() {
  console.log('open');
  sock.send('test');
};

sock.onmessage = function(e) {
  console.log('message', e.data);
  sock.close();
};

sock.onclose = function() {
  console.log('close');
};
```

**Server:**

```js
var http = require('http');
var sockjs = require('sockjs');

var echo = sockjs.createServer({ sockjs_url: 'http://cdn.jsdelivr.net/sockjs/1.0.1/sockjs.min.js' });
echo.on('connection', function(conn) {
  conn.on('data', function(message) {
    conn.write(message);
  });
  conn.on('close', function() {});
});

var server = http.createServer();
echo.installHandlers(server, {prefix:'/echo'});
server.listen(9999, '0.0.0.0');
```

## Scaling beyond a single server

The number of concurrent connections a server can handle is rarely the bottleneck when it comes to server load. Most decent WebSocket servers can support thousands of concurrent connections, but what’s the workload required to process and respond to messages once the WebSocket server process has handled receipt of the actual data? Typically there will be all kinds of potential concerns, such as reading and writing to and from a database, integration with a game server, allocation and management of resources for each client, and so forth. As soon as one machine is unable to cope with the workload, you’ll need to start adding additional servers, which means now you’ll need to start thinking about load-balancing, synchronization of messages among clients connected to different servers, generalized access to client state irrespective of connection lifespan or the specific server that the client is connected to – the list goes on and on.

Such concerns are deserving of an article all of their own, and you’ll find plenty in [the Engineering section of the Ably blog](https://blog.ably.io/) – see in particular our blog post on [some of the complexities that were dealt with when building Ably](https://blog.ably.io/the-story-of-how-ably-came-to-be-1d859f2c9a90).



--------------

#### The `WebSocket` interface

```js
enum BinaryType { "blob", "arraybuffer" };
[Exposed=(Window,Worker)]
interface WebSocket : EventTarget {
  constructor(USVString url, optional (DOMString or sequence<DOMString>) protocols = []);

  readonly attribute USVString url;

  // ready state
  const unsigned short CONNECTING = 0;
  const unsigned short OPEN = 1;
  const unsigned short CLOSING = 2;
  const unsigned short CLOSED = 3;
  readonly attribute unsigned short readyState;
  readonly attribute unsigned long long bufferedAmount;

  // networking
  attribute EventHandler onopen;
  attribute EventHandler onerror;
  attribute EventHandler onclose;
  readonly attribute DOMString extensions;
  readonly attribute DOMString protocol;
  undefined close(optional [Clamp] unsigned short code, optional USVString reason);

  // messaging
  attribute EventHandler onmessage;
  attribute BinaryType binaryType;
  undefined send(USVString data);
  undefined send(Blob data);
  undefined send(ArrayBuffer data);
  undefined send(ArrayBufferView data);
};
```

In this simple example, the `bufferedAmount` attribute is used to ensure that updates are sent either at the rate of one update every 50ms, if the network can handle that rate, or at whatever rate the network *can* handle, if that is too fast.

```js
var socket = new WebSocket('ws://game.example.com:12010/updates');
socket.onopen = function () {
  setInterval(function() {
    if (socket.bufferedAmount == 0)
      socket.send(getUpdateData());
  }, 50);
};
```

The `bufferedAmount` attribute can also be used to saturate the network without sending the data at a higher rate than the network can handle, though this requires more careful monitoring of the value of the attribute over time.

------------------

## About HTML5 WebSocket

The HTML5 WebSockets specification defines an API that enables web pages to use the WebSockets protocol for two-way communication with a remote host. It introduces the WebSocket interface and defines a full-duplex communication channel that operates through a single socket over the Web. HTML5 WebSockets provide an enormous reduction in unnecessary network traffic and latency compared to the unscalable polling and long-polling solutions that were used to simulate a full-duplex connection by maintaining two connections.

HTML5 WebSockets account for network hazards such as proxies and firewalls, making streaming possible over any connection, and with the ability to support upstream and downstream communications over a single connection, HTML5 WebSockets-based applications place less burden on servers, allowing existing machines to support more concurrent connections. The following figure shows a basic WebSocket-based architecture in which browsers use a WebSocket connection for full-duplex, direct communication with remote hosts.

![img](https://www.websocket.org/img/websocket-architecture.jpg)

One of the more unique features WebSockets provide is its ability to traverse firewalls and proxies, a problem area for many applications. Comet-style applications typically employ long-polling as a rudimentary line of defense against firewalls and proxies. The technique is effective, but is not well suited for applications that have sub-500 millisecond latency or high throughput requirements. Plugin-based technologies such as Adobe Flash, also provide some level of socket support, but have long been burdened with the very proxy and firewall traversal problems that WebSockets now resolve.

A WebSocket detects the presence of a proxy server and automatically sets up a tunnel to pass through the proxy. The tunnel is established by issuing an HTTP CONNECT statement to the proxy server, which requests for the proxy server to open a TCP/IP connection to a specific host and port. Once the tunnel is set up, communication can flow unimpeded through the proxy. Since HTTP/S works in a similar fashion, secure WebSockets over SSL can leverage the same HTTP CONNECT technique. Note that WebSockets are just beginning to be supported by modern browsers (Chrome now supports WebSockets natively). However, backward-compatible implementations that enable today's browsers to take advantage of this emerging technology are available.

WebSockets—like other pieces of the HTML5 effort such as Local Storage and Geolocation—was originally part of the HTML5 specification, but was moved to a separate standards document to keep the specification focused. WebSockets has been submitted to the Internet Engineering Task Force (IETF) by its creators, the Web Hypertext Application Technology Working Group (WHATWG). Authors, evangelists, and companies involved in the standardization still refer to the original set of features, including WebSockets, as "HTML5."

## The WebSocket Protocol

The WebSocket protocol was designed to work well with the existing Web infrastructure. As part of this design principle, the protocol specification defines that the WebSocket connection starts its life as an HTTP connection, guaranteeing full backwards compatibility with the pre-WebSocket world. The protocol switch from HTTP to WebSocket is referred to as a the WebSocket handshake.

The browser sends a request to the server, indicating that it wants to switch protocols from HTTP to WebSocket. The client expresses its desire through the Upgrade header:



```http
GET ws://echo.websocket.org/?encoding=text HTTP/1.1
Origin: http://websocket.org
Cookie: __utma=99as
Connection: Upgrade
Host: echo.websocket.org
Sec-WebSocket-Key: uRovscZjNol/umbTt5uKmw==
Upgrade: websocket
Sec-WebSocket-Version: 13
```

If the server understands the WebSocket protocol, it agrees to the protocol switch through the Upgrade header.

```http
HTTP/1.1 101 WebSocket Protocol Handshake
Date: Fri, 10 Feb 2012 17:38:18 GMT
Connection: Upgrade
Server: Kaazing Gateway
Upgrade: WebSocket
Access-Control-Allow-Origin: http://websocket.org
Access-Control-Allow-Credentials: true
Sec-WebSocket-Accept: rLHCkw/SKsO9GAH/ZSFhBATDKrU=
Access-Control-Allow-Headers: content-type
```

At this point the HTTP connection breaks down and is replaced by the WebSocket connection over the same underlying TCP/IP connection. The WebSocket connection uses the same ports as HTTP (80) and HTTPS (443), by default.

![img](https://www.websocket.org/img/WebSocketFrame.png)

## Using the HTML5 WebSocket API

With the introduction of one succinct interface (see the following listing), developers can replace techniques such as long-polling and "forever frames," and as a result further reduce latency.

```js
[Constructor(in DOMString url, optional in DOMString protocol)]
interface WebSocket {
  readonly attribute DOMString URL;
  // ready state
  const unsigned short CONNECTING = 0;
  const unsigned short OPEN = 1;
  const unsigned short CLOSED = 2;
  readonly attribute unsigned short readyState;
  readonly attribute unsigned long bufferedAmount;

  // networking
  attribute Function onopen;
  attribute Function onmessage;
  attribute Function onclose;
  boolean send(in DOMString data);
  void close();
};
WebSocket implements EventTarget;
```

Utilizing the WebSocket interface couldn't be simpler. To connect to an end-point, just create a new WebSocket instance, providing the new object with a URL that represents the end-point to which you wish to connect, as shown in the following example. Note that a ws:// and wss:// prefix are proposed to indicate a WebSocket and a secure WebSocket connection, respectively.

```js
var myWebSocket = new WebSocket("ws://www.websockets.org");
```

A WebSocket connection is established by upgrading from the HTTP protocol to the WebSockets protocol during the initial handshake between the client and the server. The connection itself is exposed via the "onmessage" and "send" functions defined by the WebSocket interface.

Before connecting to an end-point and sending a message, you can associate a series of event listeners to handle each phase of the connection life-cycle as shown in the following example.

```js
myWebSocket.onopen = function(evt) { alert("Connection open ..."); };
myWebSocket.onmessage = function(evt) { alert( "Received Message: " + evt.data); };
myWebSocket.onclose = function(evt) { alert("Connection closed."); };      
```

To send a message to the server, simply call "send" and provide the content you wish to deliver. After sending the message, call "close" to terminate the connection, as shown in the following example. As you can see, it really couldn't be much easier.

```js
myWebSocket.send("Hello WebSockets!");
myWebSocket.close();
```

--------------

## HTML5 WebSocket: A Quantum Leap in Scalability for the Web

Lately there has been a lot of buzz around HTML5 Web Sockets, which defines a full-duplex communication channel that operates through a single socket over the Web. HTML5 Web Sockets is not just another incremental enhancement to conventional HTTP communications; it represents a colossal advance, especially for real-time, event-driven web applications.

HTML5 Web Sockets provides such a dramatic improvement from the old, convoluted "hacks" that are used to simulate a full-duplex connection in a browser that it prompted Google's Ian Hickson—the HTML5 specification lead—to say:

> "Reducing kilobytes of data to 2 bytes…and reducing latency from 150ms to 50ms is far more than marginal. In fact, these two factors alone are enough to make Web Sockets seriously interesting to Google."

Let's take a look at how HTML5 Web Sockets can offer such an incredibly dramatic reduction of unnecessary network traffic and latency by comparing it to conventional solutions.

## Polling, Long-Polling, and Streaming—Headache 2.0

Normally when a browser visits a web page, an HTTP request is sent to the web server that hosts that page. The web server acknowledges this request and sends back the response. In many cases—for example, for stock prices, news reports, ticket sales, traffic patterns, medical device readings, and so on—the response could be stale by the time the browser renders the page. If you want to get the most up-to-date "real-time" information, you can constantly refresh that page manually, but that's obviously not a great solution.

Current attempts to provide real-time web applications largely revolve around polling and other server-side push technologies, the most notable of which is Comet, which delays the completion of an HTTP response to deliver messages to the client. Comet-based push is generally implemented in JavaScript and uses connection strategies such as *long-polling* or *streaming*.

With ***polling***, the browser sends HTTP requests at regular intervals and immediately receives a response. This technique was the first attempt for the browser to deliver real-time information. Obviously, this is a good solution if the exact interval of message delivery is known, because you can synchronize the client request to occur only when information is available on the server. However, real-time data is often not that predictable, making unnecessary requests inevitable and as a result, many connections are opened and closed needlessly in low-message-rate situations.

With ***long-polling***, the browser sends a request to the server and the server keeps the request open for a set period. If a notification is received within that period, a response containing the message is sent to the client. If a notification is not received within the set time period, the server sends a response to terminate the open request. It is important to understand, however, that when you have a high message volume, long-polling does not provide any substantial performance improvements over traditional polling. In fact, it could be worse, because the long-polling might spin out of control into an unthrottled, continuous loop of immediate polls.

With ***streaming*,** the browser sends a complete request, but the server sends and maintains an open response that is continuously updated and kept open indefinitely (or for a set period of time). The response is then updated whenever a message is ready to be sent, but the server never signals to complete the response, thus keeping the connection open to deliver future messages. However, since streaming is still encapsulated in HTTP, intervening firewalls and proxy servers may choose to buffer the response, increasing the latency of the message delivery. Therefore, many streaming Comet solutions fall back to long-polling in case a buffering proxy server is detected. Alternatively, TLS (SSL) connections can be used to shield the response from being buffered, but in that case the setup and tear down of each connection taxes the available server resources more heavily.

Ultimately, all of these methods for providing real-time data involve HTTP request and response headers, which contain lots of additional, unnecessary header data and introduce latency. On top of that, full-duplex connectivity requires more than just the downstream connection from server to client. In an effort to simulate full-duplex communication over half-duplex HTTP, many of today's solutions use two connections: one for the downstream and one for the upstream. The maintenance and coordination of these two connections introduces significant overhead in terms of resource consumption and adds lots of complexity. Simply put, HTTP wasn't designed for real-time, full-duplex communication as you can see in the following figure, which shows the complexities associated with building a Comet web application that displays real-time data from a back-end data source using a publish/subscribe model over half-duplex HTTP.

![img](https://www.websocket.org/img/comet-apps.jpg)



Figure 1 — The complexity of Comet applications

It gets even worse when you try to scale out those Comet solutions to the masses. Simulating bi-directional browser communication over HTTP is error-prone and complex and all that complexity does not scale. Even though your end users might be enjoying something that looks like a real-time web application, this "real-time" experience has an outrageously high price tag. It's a price that you will pay in additional latency, unnecessary network traffic and a drag on CPU performance.

### HTML5 WebSocket to the Rescue!

Defined in the Communications section of the HTML5 specification, HTML5 Web Sockets represents the next evolution of web communications—a full-duplex, bidirectional communications channel that operates through a single socket over the Web. HTML5 Web Sockets provides a true standard that you can use to build scalable, real-time web applications. In addition, since it provides a socket that is native to the browser, it eliminates many of the problems Comet solutions are prone to. Web Sockets removes the overhead and dramatically reduces complexity.

To establish a WebSocket connection, the client and server upgrade from the HTTP protocol to the WebSocket protocol during their initial handshake, as shown in the following example:

Example 1 — The WebSocket handshake (browser request and server response)

```http
GET /text HTTP/1.1\r\n
Upgrade: WebSocket\r\n
Connection: Upgrade\r\n
Host: www.websocket.org\r\n
…\r\n

HTTP/1.1 101 WebSocket Protocol Handshake\r\n
Upgrade: WebSocket\r\n
Connection: Upgrade\r\n
…\r\n
```

Once established, WebSocket data frames can be sent back and forth between the client and the server in full-duplex mode. Both text and binary frames can be sent full-duplex, in either direction at the same time. The data is minimally framed with just two bytes. In the case of text frames, each frame starts with a 0x00 byte, ends with a 0xFF byte, and contains UTF-8 data in between. WebSocket text frames use a terminator, while binary frames use a length prefix.

**Note:** Although the Web Sockets protocol is ready to support a diverse set of clients, it cannot deliver raw binary data to JavaScript, because JavaScript does not support a byte type. Therefore, binary data is ignored if the client is JavaScript—but it can be delivered to other clients that support it.

## The Showdown: Comet vs. HTML5 WebSocket

So how dramatic is that reduction in unnecessary network traffic and latency? Let's compare a polling application and a WebSocket application side by side.

For the polling example, I created a simple web application in which a web page requests real-time stock data from a RabbitMQ message broker using a traditional publish/subscribe model. It does this by polling a Java Servlet that is hosted on a web server. The RabbitMQ message broker receives data from a fictitious stock price feed with continuously updating prices. The web page connects and subscribes to a specific stock channel (a topic on the message broker) and uses an XMLHttpRequest to poll for updates once per second. When updates are received, some calculations are performed and the stock data is shown in a table as shown in the following image.

![img](https://www.websocket.org/img/stock-ticker-page.gif)



Figure 2 — A JavaScript stock ticker application

**Note:** The back-end stock feed actually produces a lot of stock price updates per second, so using polling at one-second intervals is actually more prudent than using a Comet long-polling solution, which would result in a series of continuous polls. Polling effectively throttles the incoming updates here.

It all looks great, but a look under the hood reveals there are some serious issues with this application. For example, in Mozilla Firefox with Firebug (a Firefox add-on that allows you to debug web pages and monitor the time it takes to load pages and execute scripts), you can see that GET requests hammer the server at one-second intervals. Turning on Live HTTP Headers (another Firefox add-on that shows live HTTP header traffic) reveals the shocking amount of header overhead that is associated with each request. The following two examples show the HTTP header data for just a single request and response.

Example 2 — HTTP request header

```http
 GET /PollingStock//PollingStock HTTP/1.1
 Host: localhost:8080
 User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.9.1.5)
 Gecko/20091102 Firefox/3.5.5
 Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
 Accept-Language: en-us
 Accept-Encoding: gzip,deflate
 Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7
 Keep-Alive: 300
 Connection: keep-alive
 Referer: http://www.example.com/PollingStock/
 Cookie: showInheritedConstant=false; showInheritedProtectedConstant=false;
 showInheritedProperty=false; showInheritedProtectedProperty=false;
 showInheritedMethod=false; showInheritedProtectedMethod=false;
 showInheritedEvent=false; showInheritedStyle=false; showInheritedEffect=false
```

Example 3 — HTTP response header

```http
HTTP/1.x 200 OK
X-Powered-By: Servlet/2.5
Server: Sun Java System Application Server 9.1_02
Content-Type: text/html;charset=UTF-8
Content-Length: 21
Date: Sat, 07 Nov 2009 00:32:46 GMT
```

Just for fun, I counted all the characters. The total HTTP request and response header information overhead contains 871 bytes and that does not even include any data! Of course, this is just an example and you can have less than 871 bytes of header data, but I have also seen cases where the header data exceeded 2000 bytes. In this example application, the data for a typical stock topic message is only about 20 characters long. As you can see, it is effectively drowned out by the excessive header information, which was not even required in the first place!

So, what happens when you deploy this application to a large number of users? Let's take a look at the network throughput for just the HTTP request and response header data associated with this polling application in three different use cases.

- **Use case A:** 1,000 clients polling every second: Network throughput is (871 x 1,000) = 871,000 bytes = 6,968,000 bits per second (6.6 Mbps)
- **Use case B:** 10,000 clients polling every second: Network throughput is (871 x 10,000) = 8,710,000 bytes = 69,680,000 bits per second (66 Mbps)
- **Use case C:** 100,000 clients polling every 1 second: Network throughput is (871 x 100,000) = 87,100,000 bytes = 696,800,000 bits per second (665 Mbps)

That's an enormous amount of unnecessary network throughput! If only we could just get the essential data over the wire. Well, guess what? You can with HTML5 Web Sockets! I rebuilt the application to use HTML5 Web Sockets, adding an event handler to the web page to asynchronously listen for stock update messages from the message broker (check out the many how-tos and tutorials on tech.kaazing.com/documentation/ for more information on how to build a WebSocket application). Each of these messages is a WebSocket frame that has just two bytes of overhead (instead of 871)! Take a look at how that affects the network throughput overhead in our three use cases.

- **Use case A:** 1,000 clients receive 1 message per second: Network throughput is (2 x 1,000) = 2,000 bytes = 16,000 bits per second (0.015 Mbps)
- **Use case B:** 10,000 clients receive 1 message per second: Network throughput is (2 x 10,000) = 20,000 bytes = 160,000 bits per second (0.153 Mbps)
- **Use case C:** 100,000 clients receive 1 message per second: Network throughput is (2 x 100,000) = 200,000 bytes = 1,600,000 bits per second (1.526 Mbps)

As you can see in the following figure, HTML5 Web Sockets provide a dramatic reduction of unnecessary network traffic compared to the polling solution.

![img](https://www.websocket.org/img/poll-ws-compare.gif)Figure 3 — Comparison of the unnecessary network throughput overhead between the polling and the WebSocket applications

And what about the reduction in latency? Take a look at the following figure. In the top half, you can see the latency of the half-duplex polling solution. If we assume, for this example, that it takes 50 milliseconds for a message to travel from the server to the browser, then the polling application introduces a lot of extra latency, because a new request has to be sent to the server when the response is complete. This new request takes another 50ms and during this time the server cannot send any messages to the browser, resulting in additional server memory consumption.

In the bottom half of the figure, you see the reduction in latency provided by the WebSocket solution. Once the connection is upgraded to WebSocket, messages can flow from the server to the browser the moment they arrive. It still takes 50 ms for messages to travel from the server to the browser, but the WebSocket connection remains open so there is no need to send another request to the server.

![img](https://www.websocket.org/img/latency-comparison.gif)Figure 4 — Latency comparison between the polling and WebSocket applications

### HTML5 WebSocket and the Kaazing WebSocket Gateway

Today, only Google's Chrome browser supports HTML5 Web Sockets natively, but other browsers will soon follow. To work around that limitation, however, Kaazing WebSocket Gateway provides complete WebSocket emulation for all the older browsers (I.E. 5.5+, Firefox 1.5+, Safari 3.0+, and Opera 9.5+), so you can start using the HTML5 WebSocket APIs today.

WebSocket is great, but what you can do once you have a full-duplex socket connection available in your browser is even greater. To leverage the full power of HTML5 Web Sockets, Kaazing provides a ByteSocket library for binary communication and higher-level libraries for protocols like Stomp, AMQP, XMPP, IRC and more, built on top of WebSocket.

![img](https://www.websocket.org/img/kaazing-architecture.jpg)Figure 5 — Kaazing WebSocket Gateway extends TCP-based messaging to the browser with ultra high performance

### Summary

HTML5 Web Sockets provides an enormous step forward in the scalability of the real-time web. As you have seen in this article, HTML5 Web Sockets can provide a 500:1 or—depending on the size of the HTTP headers—even a 1000:1 reduction in unnecessary HTTP header traffic and 3:1 reduction in latency. That is not just an incremental improvement; that is a revolutionary jump—a quantum leap!

Kaazing WebSocket Gateway makes HTML5 WebSocket code work in all the browsers today, while providing additional protocol libraries that allow you to harness the full power of the full-duplex socket connection that HTML5 Web Sockets provides and communicate directly to back-end services. For more information about Kaazing WebSocket Gateway, visit kaazing.com and the Kaazing technology network at tech.kaazing.com.

### Resources

- HTML5 Web Sockets Specification: http://dev.w3.org/html5/websockets/
- Kaazing Corporation: [http://kaazing.com](http://kaazing.com/)
- Download the Kaazing WebSocket Gateway: http://kaazing.com/download
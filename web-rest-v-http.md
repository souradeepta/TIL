## Are REST and HTTP the same thing?

No, they are not. HTTP stands for **H**yper**T**ext **T**ransfer **P**rotocol and is a way to transfer files. This protocol is used to link pages of hypertext in what we call the world-wide-web. However, there are other transfer protocols available, like FTP and gopher, yet they are less popular.

**RE**presentational **S**tate **T**ransfer, or REST, is a set of constraints that ensure a scalable, fault-tolerant and easily extendible system. The world-wide-web is an example of such system (and the biggest example, one might say). REST by itself is not a new invention, but it's the documentation on such systems like the world-wide-web.

One thing that confuses people, is that REST and HTTP seem to be hand-in-hand. After all, the world-wide-web itself runs on HTTP, and it makes sense, a RESTful API does the same. However, there is nothing in the REST constraints that makes the usage of HTTP as a transfer protocol mandatory. It's perfectly possible to use other transfer protocols like SNMP, SMTP and others to use, and your API could still very well be a RESTful API

In practice, most - if not all - RESTful APIs currently use HTTP as a transport layer, since the infrastructure, servers and client libraries for HTTP are widely available already

Note that there is also a big difference between a RESTful API and a HTTP API. A RESTful API adheres ALL the REST constraints set out in its "format" documentation (in the dissertation of Roy Fielding). A HTTP API is ANY API that makes use of HTTP as their transfer protocol. This means that even SOAP can be considered a HTTP API, as long as it will use HTTP for transport, but most HTTP APIs will make more and better use of the infrastructure and possibilities of HTTP. Most HTTP APIs can be very close to becoming a truly RESTful API. This can be defined by their Richardsons maturity level.
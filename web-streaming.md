# What is streaming? | How video streaming works

Streaming is a method of viewing video or listening to audio content without actually downloading the media files. Streaming performance can be improved, and buffering time reduced, if the owner of the files uses a CDN.

## What is streaming?

![Streaming Media](https://www.cloudflare.com/img/learning/performance/what-is-streaming/what-is-streaming.svg)

The first websites were simple pages of text with maybe an image or two. Today, however, anyone with a fast enough Internet connection can stream high-definition movies or make a video call over the Internet. This is possible because of a technology called streaming.

Streaming is the continuous transmission of audio or video files from a server to a client. In simpler terms, streaming is what happens when consumers watch TV or listen to podcasts on Internet-connected devices. With streaming, the media file being played on the [client device](https://www.cloudflare.com/learning/serverless/glossary/client-side-vs-server-side/) is stored remotely, and is transmitted a few seconds at a time over the Internet.

## What is the difference between streaming and downloading?

Streaming is real-time, and it's more efficient than downloading media files. If a video file is downloaded, a copy of the entire file is saved onto a device's hard drive, and the video cannot play until the entire file finishes downloading. If it's streamed instead, the browser plays the video without actually copying and saving it. The video loads a little bit at a time instead of the entire file loading at once, and the information that the browser loads is not saved locally.

Think of the difference between a lake and a stream: Both contain water, and a stream may contain just as much water as a lake; the difference is that with a stream, the water is not all in the same place at the same time. A downloaded video file is more like a lake, in that it takes up a lot of hard drive space (and it takes a long time to move a lake). Streaming video is more like a stream or a river, in that the video's data is continuously, rapidly flowing to the user's browser.

## How does streaming work?

Just like other data that's sent over the Internet, audio and video data is broken down into data packets. Each packet contains a small piece of the file, and an audio or video player in the browser on the client device takes the flow of data packets and interprets them as video or audio.

## Does streaming use the User Datagram Protocol (UDP) or the Transmission Control Protocol (TCP)?

Some streaming methods use [UDP](https://www.cloudflare.com/learning/ddos/glossary/user-datagram-protocol-udp/), and some use [TCP](https://www.cloudflare.com/learning/ddos/glossary/tcp-ip/). UDP and TCP are transport [protocols](https://www.cloudflare.com/learning/network-layer/what-is-a-protocol/), meaning they are used for moving [packets of data](https://www.cloudflare.com/learning/network-layer/what-is-a-packet/) across networks. Both are used with the [Internet Protocol (IP)](https://www.cloudflare.com/learning/network-layer/internet-protocol/). TCP opens a dedicated connection before transmitting data, and it ensures all data packets arrive in order. Unlike TCP, UDP does neither of these things. As a result, TCP is more reliable, but transmitting data via UDP does not take as long as it does via TCP, although some packets are lost along the way.

If TCP is like a package delivery service that requires the recipient to sign for the package, then UDP is like a delivery service that leaves packages on the front porch without knocking on the door to get a signature. The TCP delivery service loses fewer packages, but the UDP delivery service is faster, because packages can get dropped off even if no one is home to sign for them.

For streaming, in some cases speed is far more important than reliability. For instance, if someone is in a video conference, they would prefer to interact with the other conference attendees in real time than to sit and wait for every bit of data to be delivered. Therefore, a few lost data packets is not a huge concern, and UDP should be used.

In other cases, reliability is more important for streaming. For instance, both [HTTP live streaming (HLS)](https://www.cloudflare.com/learning/video/what-is-http-live-streaming/) and [MPEG-DASH](https://www.cloudflare.com/learning/video/what-is-mpeg-dash/) are streaming protocols that use TCP for transport. Many video-on-demand services use TCP.

## What is buffering?

![Streaming and Buffering](https://www.cloudflare.com/img/learning/performance/what-is-streaming/streaming-video-buffering.svg)

Streaming media players load a few seconds of the stream ahead of time so that the video or audio can continue playing if the connection is briefly interrupted. This is known as buffering. Buffering ensures that videos can play smoothly and continuously. However, over slow connections, or if a network has a great deal of latency, a video can take a long time to buffer.

## What factors slow down streaming?

#### On the network side:

- **[Network latency](https://www.cloudflare.com/learning/performance/glossary/what-is-latency/)**: A variety of factors impact latency, including where the content that users are trying to access is stored.
- **Network congestion**: If too much data is sent through the network, this can degrade streaming performance.

#### On the user side:

- **WiFi problems**: Restarting the LAN router, or switching to Ethernet instead of WiFi, can help improve streaming performance.
- **Slowly performing client devices**: To play videos takes a good amount of processing power. If the device streaming the video has a lot of other processes running or is just slow in general, streaming performance can be impacted.
- **Not enough bandwidth**: For streaming video, home networks need about 4 Mbps of bandwidth; for high-definition video, they will likely need more.

## How can streaming be made faster?

Streaming is subject to the same kinds of delays and performance degradations as other kinds of web content. Because the streamed content is stored elsewhere, hosting location makes a big difference, as is the case with any type of content accessed over the Internet. If a user in New York is trying to stream from a Netflix server in Los Gatos, the video content will have to cross 3,000 miles in order to reach the user, and the video will have to spend a long time buffering or may not even play at all. For this reason, Netflix and other streaming providers make extensive use of distributed content delivery networks (CDN), which store content in locations around the world that are much closer to users.

CDNs have a huge positive impact on streaming performance. [Cloudflare Stream Delivery](https://www.cloudflare.com/products/stream-delivery/) leverages the Cloudflare [CDN](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/) to store video content across all Cloudflare data centers around the world; the result is reduced latency for short video startup times and reduced buffering.



# What is live streaming? | How live streaming works

Live streaming is the delivery of video or audio data to an audience over the Internet as the data is created.

## What is live streaming?

[Streaming](https://www.cloudflare.com/learning/performance/what-is-streaming/) is the method of data transmission used when someone watches video on the Internet. It is a way to deliver a video file a little bit at a time, often from a remote storage location. By transmitting a few seconds of the file at a time over the internet, [client devices](https://www.cloudflare.com/learning/serverless/glossary/client-side-vs-server-side/) do not have to download the entire video before starting to play it.

Live streaming is when the streamed video is sent over the Internet in real time, without first being recorded and stored. Today, TV broadcasts, video game streams, and social media video can all be live-streamed.

Think about the difference between regular streaming and live streaming as the difference between an actor reciting a memorized monologue and improvising a speech. In the former, the content is created beforehand, stored, and then relayed to the audience. In the latter, the audience receives the content in the same moment that the actor creates it – just like in live streaming.

The term live streaming usually refers to broadcast live streams: one-to-many connections that go out to multiple users at once. Videoconferencing technologies like Skype, FaceTime, and Google Hangouts Meet work on real-time communication (RTC) protocols rather than the protocols used by one-to-many live stream broadcasts.

## How does live streaming work on a technical level?

These are the main steps that take place behind the scenes in a live stream:

- Segmentation
- Compression
- [Encoding](https://www.cloudflare.com/learning/video/live-stream-encoding/)
- [Content delivery network (CDN)](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/) distribution
- CDN [caching](https://www.cloudflare.com/learning/cdn/what-is-caching/)
- Decoding
- Video playback

#### Video capture

Live streaming starts with raw video data: the visual information captured by a camera. Within the computing device to which the camera is attached, this visual information is represented as digital data – in other words, 1s and 0s at the deepest level.

#### Segmentation

Video includes a lot of digital information, which is why it takes longer to download a video file than to download a short PDF or an image. Because it would not be practical to send all the video data out over the Internet at once, streaming video is divided into smaller segments a few seconds in length.

#### Compression and encoding

Next, the segmented video data is compressed and encoded. The data is compressed by removing redundant visual information. For example, if the first frame of the video displays a person talking against a grey background, the grey background does not need to render for any subsequent frames that have the same background.

Think of video compression as being like adding a piece of new furniture to a living room. It is not necessary to buy entirely new furniture each time a new chair or side table is added. Instead it is possible to keep the room layout roughly the same and change out just one piece at a time, occasionally making larger rearrangements as necessary. Similarly, not every frame of a video stream needs to be rendered in total – just the parts that change from frame to frame, such as the movement of a person's mouth.

"Encoding" refers to the process of converting data into a new format. Live streaming video data is encoded into an interpretable digital format that a wide variety of devices recognize. Common video encoding standards include:

- H.264
- VP9
- AV1
- HEVC

#### CDN distribution and caching

Once the live stream has been segmented, compressed, and encoded (all of which only takes a few seconds), it needs to be made available to the dozens or millions of viewers who want to watch it. In order to maintain high quality with minimal latency while serving the stream to multiple viewers in different locations, a CDN should distribute it.

A CDN is a distributed network of servers that cache and serve content on behalf of an origin server. Using a CDN results in faster performance, because user requests no longer have to travel all the way to the [origin server](https://www.cloudflare.com/learning/cdn/glossary/origin-server/) but can instead be handled by a nearby CDN server. Handling requests and delivering content in this manner also reduces the origin server’s workload. Finally, CDNs make it possible to efficiently serve content to users around the world because their servers are located all over the world instead of clustered in a single geographic area.

A CDN will also cache – temporarily save – each segment of the live stream, so most viewers will get the live stream from the CDN cache instead of from the origin server. This actually makes the live stream closer to real-time even though the cached data is a few seconds behind, because it cuts down on [round-trip time (RTT)](https://www.cloudflare.com/learning/cdn/glossary/round-trip-time-rtt/) to and from the origin server.

#### Decoding and video playback

The CDN sends the live stream out to all the users who are watching the stream. Each user's device receives, decodes, and decompresses the segmented video data. Finally, a media player on the user's device – either a dedicated app or a video player within the browser – interprets the data as visual information, and the video plays.

#### An example

Suppose Alice starts a live stream on her smartphone and Bob, who lives on the other side of the country, tunes into the live stream on his smartphone, along with a few dozen more of Alice's friends. Alice starts out the live stream by turning the smartphone camera on herself and saying, "Hello, world!" What needs to happen for that "Hello, world" section of video to reach Bob on the other side of the country, along with all the other people watching the stream?

First, Alice's smartphone will encode and compress that little segment of video. If Alice is filming the live stream in her kitchen, her kitchen wall is recorded as the first frame of the video, and subsequent frames will leave it out since the background doesn't change.

Now the app Alice is using will send the encoded, compressed version of her saying "Hello, world" out to the app's CDN. Fortunately for Bob, one of the CDN servers is just a few miles away from his home, so his smartphone's request for the first segment of the live stream is answered almost immediately. Other live stream viewers may experience more or less latency than Bob depending on their geographic location.

Bob's phone decodes the video segment and reconstructs the compressed data, so that Alice's kitchen wall appears in every frame. And finally, only a few seconds after Alice said "Hello, world" in her kitchen on the other side of the country, her face appears on Bob's smartphone and Bob sees and hears her greeting.

## Why is a CDN important for live streaming?

**Bandwidth:** Any given point on a network can only have so much data pass through at a time; this measurement is known as "bandwidth." If a stream of data reaches a point on the network where the amount of data exceeds capacity, this is called a "choke point" because data delivery is choked off and slowed down. A choke point is similar to the way traffic slowdowns might occur when too many cars attempt to use a single-lane road.

If all viewers of a stream are getting the stream data from its point of origin, then the origin server and its surrounding network infrastructure becomes a choke point and the stream slows down. But if the main burden of stream delivery is moved to a CDN, then the choke point is eliminated.

**Global content delivery:** Because CDNs are distributed all around the world, they are able to deliver content to a global audience. An origin server in New York cannot effectively serve content to an audience in Milan, and this is especially the case with heavy content like video. However, a CDN can forward and serve content from any point on its network, so that a person in Milan watching a live stream from New York can get the stream from a server in Milan instead of waiting for it to load all the way from New York.

**Latency and RTT reduction:** Delivering content globally with a CDN reduces latency for live stream viewers by cutting down on RTT (round-trip time). The request-response round trip is shorter both in terms of distance and in terms of time, as a viewer’s request for a live stream no longer has to travel to the original source of the stream, and the streaming data no longer has to travel from the origin, either. This reduces delays and helps keep the live stream "live."

**Workload:** Replying to user requests for data costs a server some compute power. Replying to many user requests for video data can overwhelm a single server. A CDN utilizes dozens or hundreds of servers that can take much of the workload off the origin server and keep it up and running.

**Caching:** A CDN caches each video segment of a live stream. The CDN can then deliver the segments from the cache instead of getting the data from the origin server.

Serving a live stream from a cache seems like a contradiction: If the stream is live, how can saving it and delivering it a few seconds later be faster? While it's true that a cached live stream will lag behind real-time, this is still faster than going all the way to the origin server. The round trip would take so long that the stream would fall even further behind. With CDN caching, users experience minimal lag time.

## How does Cloudflare accelerate live streaming?

The [Cloudflare CDN](https://www.cloudflare.com/cdn/) uses a technique called concurrent streaming acceleration to speed up live stream delivery. Other streams cannot serve a video segment to more than one user until the segment is completely saved in the cache. The Cloudflare CDN, however, is able to serve these segments directly to multiple viewers at once as they are loading, cutting out precious seconds of wait time for the users. [Learn more about concurrent streaming acceleration](https://blog.cloudflare.com/introducing-concurrent-streaming-acceleration/).

# How does live stream encoding work? | Video encoding

Live stream encoding breaks a live video into smaller chunks, compresses them, and distributes the video data via a CDN. Updates to this process are making live streaming faster.

## What is live streaming?

Streaming is a method of delivering data over the Internet without making end users fully download the data. Live streaming is a type of streaming in which the stream is sent over the Internet in real time, without first being recorded and stored.

Video game streaming, social media streams like Periscope and Facebook Live, and professional sports broadcasts over the Internet are all examples of live streaming. Although both audio and video can be live streamed, this article will focus on live video streaming.

## What is video encoding?

Video encoding is the process of compressing video data so it can be efficiently sent to another location. The device on the receiving end of a stream – say, a tablet on which a user is watching their favorite TV show – decodes the encoded data. Video encoding follows publicly known standards so that a variety of devices can interpret the encoded stream.

Video encoding is necessary for two main reasons:

1. Uncompressed video files take far too long to send over the Internet for streaming to be practical.
2. Video has to be in a format that any user device – smartphones, laptops, PCs, etc. – can interpret.

In a video live stream, a device takes audiovisual inputs, encodes them, and sends them out to the audience all at the same time. The encoding part of this process is essential for allowing a variety of user devices to receive and play the video.

## How does live stream encoding work?

A live stream from a source that captures video – e.g., a webcam – is sent to a server, where a streaming protocol such as HLS or MPEG-DASH will break the video feed into smaller segments, each a few seconds in length.

The video content is then encoded using an encoding standard. The encoding standard in wide use today is called H.264, but standards like VP9, AV1, and HEVC are also growing in use. This encoding process compresses the video by removing redundant visual information. For example, in a stream of someone talking against the background of a blue sky, the blue sky does not need to be rendered again for every second of video, since it does not change a lot. Therefore, the blue sky can be stripped out from most frames of the video.

The compressed, segmented video data is then distributed using a [content delivery network (CDN)](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/). Without a CDN, very few viewers will actually be able to load the live stream – the final section of this article explains why.

Most mobile devices have a built-in encoder, making it easy for regular users to live stream on social media platforms and via messaging apps. Brands and companies that want a higher quality stream use their own encoding software, hardware, or both.

## How are newer technologies making live streaming faster?

With many live streams, viewers still experience 20 to 30 seconds of [latency](https://www.cloudflare.com/learning/performance/glossary/what-is-latency/) – in other words, the content they view is 20 to 30 seconds behind real time. This is partially because each segment of video has to fully load before it can play, and each segment of video can take several seconds to load.

One solution to this delay is a process called chunked encoding. This process works by "chunking,” that is, breaking up the video segments into even smaller pieces. Then those smaller pieces are encoded, and the devices receiving the stream can play these smaller chunks before the entire segment loads.

## How are CDNs getting better at accelerating live streaming?

CDNs are essential for live streaming because they make it possible to distribute the stream to users in vastly different locations. Also, CDNs have much more bandwidth for distributing the stream than a single [origin server](https://www.cloudflare.com/learning/cdn/glossary/origin-server/). Without a CDN, the live stream can easily run into bandwidth issues.

However, most CDNs still have to load a full segment of video before they can serve the segment to multiple users at once. This reintroduces the latency problem that chunked encoding is supposed to solve.

# What is HTTP Live Streaming? | HLS streaming

HTTP live streaming (HLS) is a widely used video streaming protocol that can run on almost any server and is supported by most devices. HLS allows client devices to seamlessly adapt to changing network conditions by raising or lowering the quality of the stream.

## What is HTTP live streaming (HLS)?

HTTP live streaming (HLS) is one of the most widely used video [streaming](https://www.cloudflare.com/learning/performance/what-is-streaming/) protocols. Although it is called HTTP "live" streaming, it is used for both on-demand streaming and [live streaming](https://www.cloudflare.com/learning/video/what-is-live-streaming/). HLS breaks down video files into smaller downloadable HTTP files and delivers them using the [HTTP](https://www.cloudflare.com/learning/ddos/glossary/hypertext-transfer-protocol-http/) protocol. [Client](https://www.cloudflare.com/learning/serverless/glossary/client-side-vs-server-side/) devices load these HTTP files and then play them back as video.

One advantage of HLS is that all Internet-connected devices support HTTP, making it simpler to implement than streaming protocols that require the use of specialized servers. Another advantage is that an HLS stream can increase or decrease video quality depending on network conditions without interrupting playback. This is why video quality may get better or worse in the middle of a video as a user is watching it. This feature is known as "adaptive bitrate video delivery" or "adaptive bitrate streaming," and without it, slow network conditions can stop a video from playing altogether.

HLS was developed by Apple for use on Apple products, but it is now used across a wide range of devices.



## What is HTTP?

HTTP is an [application layer](https://www.cloudflare.com/learning/ddos/what-is-layer-7/) protocol for transferring information between devices connected to a network. Every website and application accessible by regular users runs on HTTP. Data transfer over HTTP is typically based on requests and responses. Almost all HTTP messages are either a request or a response to a request.

With streaming over HTTP, the standard request-response pattern does not apply. The connection between client and server remains open for the duration of the stream, and the server pushes video data to the client so that the client does not have to request every segment of video data.

## How does HLS work?

**Server:** An HLS stream originates from a server where (in on-demand streaming) the media file is stored, or where (in live streaming) the stream is created. Because HLS is based on HTTP, any ordinary web server can originate the stream.

Two main processes take place on the server:

1. **Encoding:** The video data is reformatted so that any device can recognize and interpret the data. HLS must use H.264 or H.265 encoding.

2. Segmenting:

   The video is divided up into segments a few seconds in length. The length of the segments can vary, although the default length is 10 seconds.

   - In addition to dividing the video into segments, HLS creates an index file of the video segments to record the order they belong in.
   - HLS will also create several duplicate sets of segments at different quality levels: 480p, 720p, 1080p, and so on.

**Distribution:** The encoded video segments are pushed out to client devices over the Internet when client devices request the stream. Typically, a [content delivery network (CDN)](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/) will help distribute the stream to geographically diverse areas. A CDN will also cache the stream to serve it to clients even more quickly.

**Client device:** The client device is the device that receives the stream and plays the video – for instance, a user smartphone or laptop. The client device uses the index file as a reference for assembling the video in order, and it switches from higher quality to lower quality picture (and vice versa) as needed.

## What is adaptive bitrate streaming in HLS?

One of the advantages HLS has over some other streaming protocols is adaptive bitrate streaming. This refers to the ability to adjust video quality in the middle of a stream as network conditions change. This ability allows videos to keep playing even if network conditions get worse; conversely, it also maximizes video quality to be as high as the network can support.

If the network slows down, the user's video player detects this, and adaptive bitrate streaming lowers the quality of the stream so that the video does not stop playing. If more network bandwidth becomes available, adaptive bitrate streaming improves the quality of the stream.

Adaptive bitrate streaming is possible because HLS creates several duplicate segmented streams at different quality levels during the segmentation process. The user's video player can seamlessly switch from one of those streams to another one during video playback.

## Does HLS use TCP or UDP as its transport protocol?

[TCP](https://www.cloudflare.com/learning/ddos/glossary/tcp-ip/) and [UDP](https://www.cloudflare.com/learning/ddos/glossary/user-datagram-protocol-udp/) are transport protocols, meaning they are responsible for delivering content over the Internet. TCP tends to deliver data more reliably than UDP, but the latter is much faster, even though some data may be lost in transit.

Because UDP is faster, many streaming protocols use UDP instead of TCP. HLS, however, uses TCP. This is for several reasons:

1. HLS is over HTTP, and the HTTP protocol is built for use with TCP ([with some exceptions](https://blog.cloudflare.com/http3-the-past-present-and-future/)).
2. The modern Internet is more reliable and more efficient than it was when streaming was first developed. In many parts of the world today, user connectivity has vastly improved, especially for mobile connections. As a result, users have enough bandwidth to support the delivery of every video frame.
3. Adaptive bitrate streaming helps compensate for the potentially slower data delivery of TCP.
4. HLS streaming does not need to be "real time," as is the case with videoconferencing connections. A few extra seconds of lag does not impact the user experience as much as missing video frames would.

## What other protocols are commonly used for streaming?

There are a number of similar protocols to HLS, like [MPEG-DASH](https://www.cloudflare.com/learning/video/what-is-mpeg-dash/) and HDS, that also run over HTTP and offer adaptive bitrate streaming. Adobe Flash, which runs on the RTMP protocol, used to be the main technology used for video streaming; however, many browsers no longer support Flash. RTMP is still in wide use.

## What is MPEG-DASH?

[Streaming](https://www.cloudflare.com/learning/performance/what-is-streaming/) is a way of delivering data over the Internet so that a device can start displaying the data before it fully loads. Video is streamed over the Internet so that the [client](https://www.cloudflare.com/learning/serverless/glossary/client-side-vs-server-side/) device does not have to download the entire video file before playing it.

MPEG-DASH is a streaming method. DASH stands for "Dynamic Adaptive Streaming over [HTTP](https://www.cloudflare.com/learning/ddos/glossary/hypertext-transfer-protocol-http/)." Because it is based on HTTP, any [origin server](https://www.cloudflare.com/learning/cdn/glossary/origin-server/) can be set up to serve MPEG-DASH streams.

MPEG-DASH is similar to [HLS](https://www.cloudflare.com/learning/video/what-is-http-live-streaming/), another streaming protocol, in that it breaks videos down into smaller chunks and [encodes](https://www.cloudflare.com/learning/video/live-stream-encoding/) those chunks at different quality levels. This makes it possible to stream videos at different quality levels, and to switch in the middle of a video from one quality level to another one.

## How does MPEG-DASH work?

The main steps in the MPEG-DASH streaming process are:

1. **Encoding and segmentation:** The origin server divides the video file into smaller segments 2-4 seconds in length. The server also creates an index file – like a table of contents for the video segments. Then the segments are encoded, meaning formatted in a way that multiple devices can interpret. MPEG-DASH allows the use of any encoding standard.
2. **Delivery:** When users start watching the stream, the encoded video segments are pushed out to client devices over the Internet. In almost all cases, a [content delivery network (CDN)](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/) helps distribute the stream more efficiently.
3. **Decoding and playback:** As a user's device receives the streamed data, it decodes the data and plays back the video. The video player automatically switches to a lower or higher quality picture in order to adjust to network conditions – for example, if the user currently has very little bandwidth, the video will play at a lower quality level that uses less bandwidth.

## HLS vs. DASH: What are the main differences?

HLS is another streaming protocol in wide use today. MPEG-DASH and HLS are similar in a number of ways. Both protocols run over HTTP, use [TCP](https://www.cloudflare.com/learning/ddos/glossary/tcp-ip/) as their transport protocol, break video into segments with an accompanying index file, and offer adaptive bitrate streaming.

However, several key differences distinguish the two protocols:

**Encoding formats:** MPEG-DASH allows the use of any encoding standard. HLS, on the other hand, requires the use of H.264 or H.265.

**Device support:** HLS is the only format supported by Apple devices. iPhones, MacBooks, and other Apple products cannot play video delivered over MPEG-DASH.

**Segmentation:** MPEG-DASH usually breaks videos into smaller segments for delivery than HLS. The default segment length for HLS is 6 seconds (formerly 10 seconds), while MPEG-DASH segments are typically 2-4 seconds long. This means that MPEG-DASH enables faster switching between quality levels for quicker adjustments to network conditions.

**Standardization:** MPEG-DASH is an international standard. HLS was developed by Apple and has not been published as an international standard, even though it has wide support.

**HTML5 support:** HLS is automatically supported by HTML5, but MPEG-DASH is not. This means that some browsers or apps cannot play MPEG-DASH video streams, even on non-Apple devices.
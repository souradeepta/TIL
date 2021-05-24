### The TCP handshake

TCP uses a three-way handshake to establish a reliable connection. The connection is full duplex, and both sides synchronize (SYN) and acknowledge (ACK) each other. The exchange of these four flags is performed in three steps—SYN, SYN-ACK, and ACK—as shown in Figure 3.8.

![img](https://ars.els-cdn.com/content/image/3-s2.0-B9781597499613000030-f03-08-9781597499613.jpg?_)

[Sign in to download full-size image](https://www.sciencedirect.com/user/login?returnURL=https%3A%2F%2Fwww.sciencedirect.com%2Ftopics%2Fcomputer-science%2Fthree-way-handshake)

Figure 3.8. TCP Three-Way Handshake.

The client chooses an initial sequence number, set in the first SYN packet. The server also chooses its own initial sequence number, set in the SYN/ACK packet shown in Figure 3.8. Each side acknowledges each other's sequence number by incrementing it; this is the acknowledgement number. The use of sequence and acknowledgment numbers allows both sides to detect missing or out-of-order segments.

Once a connection is established, ACKs typically follow for each segment. The connection will eventually end with a RST (reset or tear down the connection) or FIN (gracefully end the connection).

### 24.7 Opening and closing a connection

Figure 24.7 shows a basic three-way handshake. The steps are:

![img](https://ars.els-cdn.com/content/image/3-s2.0-B9780340740767500249-f24-07-9780340740767.gif?_)

[Sign in to download full-size image](https://www.sciencedirect.com/user/login?returnURL=https%3A%2F%2Fwww.sciencedirect.com%2Ftopics%2Fcomputer-science%2Fthree-way-handshake)

Figure 24.7. TCP connection

- 1.The initial state on the initiator is CLOSED and, on the recipient, it is LISTEN (the recipient is waiting for a connection see figure 24.7).
- 2.The initiator goes into the SYN-SENT state and sends a packet with the SYN bit set and then indicates that the starting sequence number will be 999 (the current sequence number, thus the next number sent will be 1000). When this is received the recipient goes into the SYN-RECEIVED state.
- 3.The recipient sends back a TCP packet with the SYN and ACK bits set (which identifies that it is a SYN packet and also that it is acknowledging the previous SYN packet). In this case, the recipient tells the originator that it will start transmitting at a sequence number of 100. The acknowledgement number is 1000, which is the sequence number that the recipient expects to receive next. When this is received, the originator goes into the ESTABLISHED state.
- 4.The originator sends back a TCP packet with the SYN and ACK bits set and the acknowledgement number is 101, which is the sequence number it expects to see next.
- 5.The originator transmits data with the sequence number of 1000.

Note that the acknowledgement number acknowledges every sequence number up to but not including the acknowledgement number.

Figure 24.8 shows how the three-way handshake prevents old duplicate connection initiations from causing confusion. In state 3, a duplicate SYN has been received, which is from a previous connection. The recipient sends back an acknowledgement for this (4), but when this is received by the originator, the originator sends back a RST (reset) packet. This causes the recipient to go back into a LISTEN state. It will then receive the SYN packet sent in 2, and after acknowledging it, a connection is made.

![img](https://ars.els-cdn.com/content/image/3-s2.0-B9780340740767500249-f24-08-9780340740767.gif?_)



Figure 24.8. TCP connection with duplicate connections

TCP connections are half-open if one of the TCPs has closed or aborted, and the other end is still connected. They can also occur if the two connections have become desynchronised because of a system crash. This connection is automatically reset if data is sent in either direction. This is because the sequence numbers will be incorrect, otherwise the connection will time-out.

A connection is normally closed with the CLOSE call. A host who has closed cannot continue to send, but can continue to RECEIVE until it is told to close by the other side. Figure 24.9 shows a typical sequence for closing a connection. Normally the application program sends a CLOSE call for the given connection. Next, a TCP packet is sent with the FIN bit set, the originator enters into the FIN-WAIT-1 state. When the other TCP has acknowledged the FIN and sent a FIN of its own, the first TCP can ACK this FIN.

![img](https://ars.els-cdn.com/content/image/3-s2.0-B9780340740767500249-f24-09-9780340740767.gif?_)

### II.E.2 TCP Connection Establishment

As discussed above, a connection is established using a *three-way handshake* procedure. The flow of data in each direction of a connection is controlled independently so as to avoid ambiguity with initial sequence numbers. These are in turn acknowledged as part of the handshake procedure. Figure 10 shows this three-way handshake establishment.

![img](https://ars.els-cdn.com/content/image/3-s2.0-B0122272404001878-gr10.gif?_)

[Sign in to download full-size image](https://www.sciencedirect.com/user/login?returnURL=https%3A%2F%2Fwww.sciencedirect.com%2Ftopics%2Fcomputer-science%2Fthree-way-handshake)

Figure 10. TCP connection establishment.

The initiating side sends a segment with the SYN flag set and the proposed initial sequence number in the [sequence number field](https://www.sciencedirect.com/topics/computer-science/sequence-number-field) (SEQ = *X*). On receipt of this, the responding side notes the sequence number setting for the incoming direction and then returns a segment with both the SYN and ACK flags set with the sequence number field set to its own assigned value for the reverse direction (SEQ = *Y*) and a piggy backed acknowledgement field of *X* + 1 (PACK = *X* + 1) to confirm it has noted the initial value for its incoming direction. On receipt of this, the initiating returns a segment with the ACK flag set and a piggybacked acknowledgement field of *Y* + 1.

### TCP Session Hijacking

Let us recall that a TCP session starts out with a three-way handshake between the two nodes (one node is a client, and the other node is a server) that would like to establish a session between them. The nodes would exchange a sequence of TCP segments with well-defined sequence numbers to establish an active session. This active session is normally terminated by an exchange of FIN (finish) packet or abruptly with RST (reset) packets.

If a would-be hijacker were to correctly guess the sequence number of TCP segments between the two nodes, then it is quite possible that the hijacker could hijack the session before that session gets established between the original TCP client and the server. The original client would still send an ACK segment to the server, but the server would assume that it has received a duplicate segment with a matching sequence number, and thus ignore, as this happens quite a lot of times on the network. This scenario is not a complete description of session hijacking, but just an overview.

[View chapter](https://www.sciencedirect.com/science/article/pii/B9780123943972000283)[Purchase book](https://www.elsevier.com/books/T/A/9780123943972)
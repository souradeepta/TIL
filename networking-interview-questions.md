## Top Networking Interview Questions

*Here we go with the basic networking questions and answers.*

**Q #1) What is a Network?**

**Answer:** Network is defined as a set of devices connected to each other using a physical transmission medium.

**For Example,** A computer network is a group of computers connected with each other to communicate and share information and resources like hardware, data, and software. In a network, nodes are used to connect two or more networks.

**Q #2) What is a Node?**

**Answer:** Two or more computers are connected directly by an optical fiber or any other cable. A node is a point where a connection is established. It is a network component that is used to send, receive and forward the electronic information.

A device connected to a network is also termed as Node. Let’s consider that in a network there are 2 computers, 2 printers, and a server are connected, then we can say that there are five nodes on the network.

[![Nodes](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2018/01/Nodes.jpg)](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2018/01/Nodes.jpg)

*[image [source](http://mathinsight.org/)]*

**Q #3) What is Network Topology?**

**Answer:** Network topology is a physical layout of the computer network and it defines how the computers, devices, cables, etc are connected to each other.

**Q #4) What are Routers?**

**Answer:** The router is a network device that connects two or more network segments. It is used to transfer information from the source to the destination.

Routers send the information in terms of data packets and when these data packets are forwarded from one router to another router then the router reads the network address in the packets and identifies the destination network.

**Q #5) What is the OSI reference model?**

**Answer:** **O**pen **S**ystem **I**nterconnection, the name itself suggests that it is a reference model that defines how applications can communicate with each other over a networking system.

It also helps to understand the relationship between networks and defines the process of communication in a network.

**Q #6) What are the layers in OSI Reference Models? Describe each layer briefly.**

**Answer:** **Given below are the seven layers of OSI Reference Models:**

**a) Physical Layer** **(Layer 1):** It converts data bits into electrical impulses or radio signals. **Example:** Ethernet.

**b) Data Link Layer (Layer 2):** At the Data Link layer, data packets are encoded and decoded into bits and it provides a node to node data transfer. This layer also detects the errors that occurred at Layer 1.

**c) Network Layer** **(Layer 3):** This layer transfers variable length data sequence from one node to another node in the same network. This variable-length data sequence is also known as **“Datagrams”**.

**d) Transport Layer (Layer 4):** It transfers data between nodes and also provides acknowledgment of successful data transmission. It keeps track of transmission and sends the segments again if the transmission fails.

[![OSI Reference Model](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2018/01/OSI-Reference-Model.jpg)](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2018/01/OSI-Reference-Model.jpg)

*[image [source](http://searchnetworking.techtarget.com/)]*

**e) Session Layer (Layer 5):** This layer manages and controls the connections between computers. It establishes, coordinates, exchange and terminates the connections between local and remote applications.

**f) Presentation Layer (Layer 6):** It is also called as “Syntax Layer”. Layer 6 transforms the data into the form in which the application layer accepts.

**g) Application Layer** **(Layer 7):** This is the last layer of the OSI Reference Model and is the one that is close to the end-user. Both end-user and application layer interacts with the software application. This layer provides services for email, file transfer, etc.

**Q #7) What is the difference between Hub, Switch, and Router?**

**Answer:**

| Hub                                                          | Switch                                                       | Router                                                       |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| Hub is least expensive, least intelligent and least complicated of the three. It broadcast all data to every port which may cause serious security and reliability concern | Switches work similarly like Hubs but in a more efficient manner. It creates connections dynamically and provides information only to the requesting port | The router is smartest and most complicated out of these three. It comes in all shapes and sizes. Routers are similar like little computers dedicated for routing network traffic |
| In a Network, Hub is a common connection point for devices connected to the network. Hub contains multiple ports and is used to connect segments of LAN | Switch is a device in a network which forwards packets in a network | Routers are located at gateway and forwards data packets     |



**Q #8) Explain TCP/IP Model**

**Answer:** The most widely used and available protocol is TCP/IP i.e. Transmission Control Protocol and Internet Protocol. TCP/IP specifies how data should be packaged, transmitted and routed in their end to end data communication.

**There are four layers as shown in the below diagram:**

[![TCP/IP](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2018/01/TCP.jpg)](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2018/01/TCP.jpg)

**Given below is a brief explanation of each layer:**

- **Application Layer**: This is the top layer in the TCP/IP model. It includes processes that use the Transport Layer Protocol to transmit the data to their destination. There are different Application Layer Protocols such as HTTP, FTP, SMTP, SNMP protocols, etc.
- **Transport Layer**: It receives the data from the Application Layer which is above the Transport Layer. It acts as a backbone between the host’s system connected with each other and it mainly concerns about the transmission of data. TCP and UDP are mainly used as Transport Layer protocols.
- **Network or Internet Layer**: This layer sends the packets across the network. Packets mainly contain source & destination IP addresses and actual data to be transmitted.
- **Network Interface Layer**: It is the lowest layer of the TCP/IP model. It transfers the packets between different hosts. It includes encapsulation of IP packets into frames, mapping IP addresses to physical hardware devices, etc.

**Q #9) What is HTTP and what port does it use?**

**Answer:** HTTP is HyperText Transfer Protocol and it is responsible for web content. Many web pages are using HTTP to transmit the web content and allow the display and navigation of HyperText. It is the primary protocol and port used here is TCP port 80.

**Q #10) What is HTTPs and what port does it use?**

**Answer:** HTTPs is a Secure HTTP. HTTPs is used for secure communication over a computer network. HTTPs provides authentication of websites that prevents unwanted attacks.

In bi-directional communication, the HTTPs protocol encrypts the communication so that the tampering of the data gets avoided. With the help of an SSL certificate, it verifies if the requested server connection is a valid connection or not. HTTPs use TCP with port 443.

**Q #11) What are TCP and UDP?**

**Answer:** **Common factors in TCP and UDP are:**

- TCP and UDP are the most widely used protocols that are built on the top of the IP protocol.
- Both protocols TCP and UDP are used to send bits of data over the Internet, which is also known as ‘packets’.
- When packets are transferred using either TCP or UDP, it is sent to an IP address. These packets are traversed through routers to the destination.

**The difference between TCP and UDP are enlisted in the below table:**

| TCP                                                          | UDP                                                          |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| TCP stands for Transmission Control Protocol                 | UDP is stands for User Datagram Protocol or Universal Datagram Protocol |
| Once the connection is setup, data can be sent bi-directional i.e. TCP is a connection oriented protocol | UDP is connectionless, simple protocol. Using UDP, messages are sent as packets |
| The speed of TCP is slower than UDP                          | UDP is faster compared to TCP                                |
| TCP is used for the application where time is not critical part of data transmission | UDP is suitable for the applications which require fast transmission of data and time is crucial in this case. |
| TCP transmission occurs in a sequential manner               | UDP transmission also occurs in a sequential manner but it does not maintain the same sequence when it reaches the destination |
| It is heavy weight connection                                | It is lightweight transport layer                            |
| TCP tracks the data sent to ensure no data loss during data transmission | UDP does not ensure whether receiver receives packets are not. If packets are misses then they are just lost |

**Q #12) What is a Firewall?**

**Answer:** Firewall is a network security system that is used to protect computer networks from unauthorized access. It prevents malicious access from outside to the computer network. A firewall can also be built to grant limited access to outside users.

The firewall consists of a hardware device, software program or a combined configuration of both. All the messages that route through the firewall are examined by specific security criteria and the messages which meet the criteria are successfully traversed through the network or else those messages are blocked.

 **[![firewall](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/08/firewall.jpg)](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/08/firewall.jpg)**

*[image [source](https://www.comodo.com/)]*

Firewalls can be installed just like any other computer software and later can be customized as per the need and have some control over the access and security features. “

Windows Firewall” is an inbuilt Microsoft Windows application that comes along with the operating system. This “Windows Firewall” also helps to prevent viruses, worms, etc.

**Q #13) What is DNS?**

**Answer:** Domain Name Server (DNS), in a non-professional language and we can call it an Internet’s phone book. All the public IP addresses and their hostnames are stored in the DNS and later it translates into a corresponding IP address.

For a human being, it is easy to remember and recognize the domain name, however, the computer is a machine that does not understand the human language and they only understand the language of IP addresses for data transfer.

There is a “Central Registry” where all the domain names are stored and it gets updated on a periodic basis. All Internet service providers and different host companies usually interact with this central registry to get the updated DNS details.

**For Example**, When you type a website [www.softwaretestinghelp.com](https://www.softwaretestinghelp.com/), then your Internet service provider looks for the DNS associated with this domain name and translates this website command into a machine language – IP address – 151.144.210.59 (note that, this is the imaginary IP address and not the actual IP for the given website) so that you will get redirected to the appropriate destination.

**This process is explained in the below diagram:**

[![DNS](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2018/01/DNS.jpg)](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2018/01/DNS.jpg)

*[image [source](https://www.ctrlswitches.com/)]*

**Q #14) What is the difference between a Domain and a Workgroup?**

**Answer:** In a computer network, different computers are organized in different methods and these methods are – Domains and Workgroups. Usually, computers which run on the home network belong to a Workgroup.

However, computers that are running on an office network or any workplace network belong to the Domain.

**Their differences are as follows:**

| Workgroup                                                    | Domain                                                       |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| All computers are peers and no computer has control over another computer | Network admin uses one or more computer as a server and provide all accesses, security permission to all other computers in a network |
| In a Workgroup, each computer maintains their own database   | The domain is a form of a computer network in which computers, printers, and user accounts are registered in a central database. |
| Each computer has their own authentication rule for every user account | It has centralized authentication servers which set the rule of authentication |
| Each computer has set of user account. If user has account on that computer then only user able to access the computer | If user has an account in a domain then user can login to any computer in a domain |
| Workgroup does not bind to any security permission or does not require any password | Domain user has to provide security credentials whenever they are accessing the domain network |
| Computer settings need to change manually for each computer in a Workgroup | In a domain, changes made in one computer automatically made same changes to all other computers in a network |
| All computers must be on same local area network             | In a domain, computers can be on a different local network   |
| In a Workgroup, there can be only 20 computers connected     | In a domain, thousands of computers can be connected         |

**Q #15) What is a Proxy Server and how do they protect the computer network?**

**Answer:** For data transmission, IP addresses are required and even DNS uses IP addresses to route to the correct website. It means without the knowledge of correct and actual IP addresses it is not possible to identify the physical location of the network.

Proxy servers prevent external users who are unauthorized to access such IP addresses of the internal network. It makes the computer network virtually invisible to external users.

[![proxy server](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/08/proxy-server-e1504182916814.jpg)](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/08/proxy-server-e1504182916814.jpg)

Proxy Server also maintains the list of blacklisted websites so that the internal user is automatically prevented from getting easily infected by viruses, worms, etc.

**Q #16) What are IP classes and how can you identify the IP class of given an IP address?**

**Answer:** An IP address has 4 sets (octets) of numbers each with a value up to 255.

**For Example**, the range of the home or commercial connection started primarily between 190 x or 10 x. IP classes are differentiated based on the number of hosts it supports on a single network. If IP classes support more networks then very few IP addresses are available for each network.

There are three types of IP classes and are based on the first octet of IP addresses which are classified as Class A, B or C. If the first octet begins with 0 bit then it is of type Class A.

Class A type has a range up to 127.x.x.x (except 127.0.0.1). If it starts with bits 10 then it belongs to Class B. Class B having a range from 128.x to 191.x.  IP class belongs to Class C if the octet starts with bits 110. Class C has a range from 192.x to 223.x.

**Q #17) What is meant by 127.0.0.1 and localhost?**

**Answer:** IP address 127.0.0.1, is reserved for loopback or localhost connections. These networks are usually reserved for the biggest customers or some of the original members of the Internet. To identify any connection issue, the initial step is to ping the server and check if it is responding.

If there is no response from the server then there are various causes like the network is down or the cable needs to be replaced or the network card is not in good condition. 127.0.0.1 is a loopback connection on the Network Interface Card (NIC) and if you are able to ping this server successfully, then it means that the hardware is in a good shape and condition.

127.0.0.1 and localhost are the same things in most of the computer network functioning.

**Q #18) What is NIC?**

**Answer:** NIC stands for Network Interface Card. It is also known as Network Adapter or Ethernet Card. It is in the form of an add-in card and is installed on a computer so that the computer can be connected to a network.

Each NIC has a MAC address which helps in identifying the computer on a network.

**Q #19) What is Data Encapsulation?**

**Answer:** In a computer network, to enable data transmission from one computer to another, the network devices send messages in the form of packets. These packets are then added with the IP header by the OSI reference model layer.

The Data Link Layer encapsulates each packet in a frame that contains the hardware address of the source and the destination computer. If a destination computer is on the remote network then the frames are routed through a gateway or router to the destination computer.

**Q #20) What is the difference between the Internet, Intranet, and Extranet?**

**Answer:** The terminologies Internet, Intranet, and Extranet are used to define how the applications in the network can be accessed. They use similar TCP/IP technology but differ in terms of access levels for each user inside the network and outside the network.

- **Internet**: Applications are accessed by anyone from any location using the web.
- **Intranet**: It allows limited access to users in the same organization.
- **Extranet**: External users are allowed or provided with access to use the network application of the organization.

**Q #21) What is a VPN?**

**Answer:** VPN is the Virtual Private Network and is built on the Internet as a private wide area network. Internet-based VPNs are less expensive and can be connected from anywhere in the world.

VPNs are used to connect offices remotely and are less expensive when compared to WAN connections. VPNs are used for secure transactions and confidential data can be transferred between multiple offices. VPN keeps company information secure against any potential intrusion.

 **[![vpn](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/08/vpn.jpg)](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/08/vpn.jpg)**

*[image [source](http://searchnetworking.techtarget.com/)]*

**Given below are the 3 types of VPN’s:**

1. **Access VPN**: Access VPN’s provide connectivity to mobile users and telecommuters. It is an alternative option for dial-up connections or ISDN connections. It provides low-cost solutions and a wide range of connectivity.
2. **Intranet VPN**: They are useful for connecting remote offices using shared infrastructure with the same policy as a private network.
3. **Extranet VPN**: Using shared infrastructure over an intranet, suppliers, customers, and partners are connected using dedicated connections.

**Q #22) What are Ipconfig and Ifconfig?**

**Answer:** **Ipconfig** stands for Internet Protocol Configuration and this command is used on Microsoft Windows to view and configure the network interface.

The command Ipconfig is useful for displaying all TCP/IP network summary information currently available on a network.  It also helps to modify the DHCP protocol and DNS setting.

**Ifconfig** (Interface Configuration) is a command that is used on Linux, Mac, and UNIX operating systems. It is used to configure, control the TCP/IP network interface parameters from CLI i.e. Command Line Interface. It allows you to see the IP addresses of these network interfaces.

**Q #23) Explain DHCP briefly?**

**Answer:** DHCP stands for Dynamic Host Configuration Protocol and it automatically assigns IP addresses to the network devices. It completely removes the process of manual allocation of IP addresses and reduces the errors caused due to this.

This entire process is centralized so that the TCP/IP configuration can also be completed from a central location. DHCP has a “pool of IP addresses” from which it allocates the IP address to the network devices. DHCP cannot recognize if any device is configured manually and assigned with the same IP address from the DHCP pool.

In this situation, it throws the “IP address conflict” error.

[![DHCP](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2018/01/DHCP.jpg)](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2018/01/DHCP.jpg)

*[image [source](http://www.computernetworkingnotes.com/)]*

DHCP environment requires DHCP servers to set-up the TCP/IP configuration. These servers then assign, release and renew the IP addresses as there might be a chance that network devices can leave the network and some of them can join back to the network.

**Q #24) What is SNMP?**

**Answer:** SNMP stands for Simple Network Management Protocol. It is a network protocol used for collecting organizing and exchanging information between network devices. SNMP is widely used in network management for configuring network devices like switches, hubs, routers, printers, servers.

**SNMP consists of the below components:**

- SNMP Manager
- Managed device
- SNMP Agent
- Management Information Base (MIB)

**The below diagram shows how these components are connected with each other in the SNMP architecture:**

[![SNMP](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2018/01/SNMP.jpg)](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2018/01/SNMP.jpg)

*[image [source](https://computernetworkingsimplified.wordpress.com/)]*

SNMP is a part of the TCP/IP suite. There are 3 main versions of SNMP which include SNMPv1, SNMPv2, and SNMPv3.

**Q #25) What are the different types of a network? Explain each briefly.**

**Answer:** There are 4 major types of networks.

**Let’s take a look at each of them in detail.**

1. **Personal Area Network (PAN)**: It is the smallest and basic network type that is often used at home. It is a connection between the computer and another device such as phone, printer, modem tablets, etc
2. **Local Area Network (LAN)**: LAN is used in small offices and Internet cafes to connect a small group of computers to each other. Usually, they are used to transfer a file or for playing the game in a network.
3. **Metropolitan Area Network (MAN):** It is a powerful network type than LAN. The area covered by MAN is a small town, city, etc. A huge server is used to cover such a large span of area for connection.
4. **Wide Area Network (WAN)**: It is more complex than LAN and covers a large span of the area typically a large physical distance. The Internet is the largest WAN which is spread across the world. WAN is not owned by any single organization but it has distributed ownership.

**There are some other types of the network as well:**

- Storage Area Network (SAN)
- System Area Network (SAN)
- Enterprise Private Network (EPN)
- Passive Optical Local Area Network (POLAN)

------

**Part 2: Networking Questions Series**

**Q #26) Differentiate Communication and Transmission?**

**Answer:** Through Transmission the data gets transferred from source to destination (only one way). It is treated as the physical movement of data.

Communication means the process of sending and receiving data between two media (data is transferred between source and destination in both ways).

**Q #27) Describe the layers of the OSI model?**

**Answer:** OSI model stands for Open System Interconnection It is a framework that guides the applications on how they can communicate in a network.

**OSI model has seven layers. They are listed below,**

1. **Physical Layer**: Deals with transmission and reception of unstructured data through a physical medium.
2. **Data Link Layer:** Helps in transferring error-free data frames between nodes.
3. **Network Layer:** Decides the physical path that should be taken by the data as per the network conditions.
4. **Transport Layer:** Ensures that the messages are delivered in sequence and without any loss or duplication.
5. **Session Layer:** Helps in establishing a session between processes of different stations.
6. **Presentation Layer:** Formats the data as per the need and presents the same to the Application layer.
7. **Application Layer:** Serves as the mediator between Users and processes of applications.

**Q #28) Explain various types of networks based on their sizes?**

**Answer:** The size of the network is defined as the geographic area and the number of computers covered in it. **Based on the size of the network they are classified as below:**

1. **Local Area Network (LAN):** A network with a minimum of two computers to a maximum of thousands of computers within an office or a building is termed as LAN. Generally, it works for a single site where people can share resources like printers, data storage, etc.
2. **Metropolitan Area Network (MAN):** It is larger than LAN and used to connect various LANs across small regions, a city, campus of colleges or universities, etc which in turn forms a bigger network.
3. **Wide Area Network (WAN):** Multiple LANs and MAN’s connected together form a WAN. It covers a wider area like a whole country or world.

**Q #29) Define various types of Internet connections?**

**Answer:** **There are three types of Internet connections. They are listed below:**

1. **Broadband Connection:** This type of connection gives continuous high-speed Internet. In this type, if we log off from the Internet for any reason then there is no need to log in again. **For Example,** Modems of cables, Fibres, wireless connection, satellite connection, etc.
2. **Wi-Fi:** It is a wireless Internet connection between the devices. It uses radio waves to connect to the devices or gadgets.
3. **WiMAX:** It is the most advanced type of Internet connection which is more featured than Wi-Fi. It is nothing but a high-speed and advanced type of broadband connection.

**Q #30) A few important terminologies we come across networking concepts?**

**Answer:** **Below are a few important terms we need to know in networking:**

- **Network:** A set of computers or devices connected together with a communication path to share data.
- **Networking:** The design and construction of a network are termed as networking.
- **Link:** The physical medium or the communication path through which the devices are connected in a network is called a Link.
- **Node:** The devices or the computers connected to the links are named as nodes.
- **Router/Gateway:** A device/computer/node that is connected to different networks is termed as a Gateway or Router. The basic difference between these two is that Gateway is used to control the traffic of two contradictory networks whereas the router controls the traffic of similar networks.
- **The router** is a switch that processes the signal/traffic using routing protocols.
- **Protocol:** A set of instructions or rules or guidelines that are used in establishing communications between computers of a network is called Protocol.
- **Unicasting:** When a piece of information or a packet is sent from a particular source to a specified destination then it is called Unicasting.
- **Anycasting:** Sending the datagrams from a source to the nearest device among the group of servers that provide the same service as the source is termed as Anycasting.
- **Multicasting:** Sending one copy of data from a single sender to multiple clients or receivers (selected clients) of the networks which are in need of such data.
- **Broadcasting:** Sending a packet to each device of the network is termed as broadcasting.

**Q #31) Explain the characteristics of networking?**

**Answer:** **The main characteristics of networking are** **mentioned below:**

- **Topology:** This deals with how the computers or nodes are arranged in the network. The computers are arranged physically or logically.
- **Protocols:** Deals with the process of how computers communicate with one another.
- **Medium:** This is nothing but the medium used by computers for communication.

**Q #32) How many types of modes are used in data transferring through networks?**

**Answer:** **Data transferring modes in computer networks are of three types. They are listed below,**

1. **Simplex:** Data transferring which takes place only in one direction is called Simplex. In Simplex mode, the data gets transferred either from sender to receiver or from receiver to sender. **For Example,** Radio signal, the print signal given from computer to printer, etc.
2. **Half Duplex:** Data transferring can happen in both directions but not at the same time. Alternatively, the data is sent and received. **For Example,** Browsing through the internet, a user sends the request to the server and later the server processes the request and sends back the web page.
3. **Full Duplex:** Data transferring happens in both directions that too simultaneously. **For Example,** Two-lane roads where traffic flows in both directions, communication through telephone, etc.

**Q #33) Name the different types of network topologies and brief their advantages?**

**Answer:** Network Topology is nothing but the physical or logical way in which the devices (like nodes, links, and computers) of a network are arranged. Physical Topology means the actual place where the elements of a network are located.

Logical Topology deals with the flow of data over the networks. A link is used to connect more than two devices of a network. And more than two links located nearby form a topology.

**Network topologies are classified as** **below:**

**a) Bus Topology:** In Bus Topology, all the devices of the network are connected to a common cable (also called as the backbone). As the devices are connected to a single cable, it is also termed as Linear Bus Topology.

[![bus topology](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/03/bus-topology.jpg)](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/03/bus-topology.jpg)

The advantage of bus topology is that it can be installed easily. And the disadvantage is that if the backbone cable breaks then the whole network will be down.

**b) Star Topology:** In Star Topology, there is a central controller or hub to which every node or device is connected through a cable. In this topology, the devices are not linked to each other. If a device needs to communicate with the other, then it has to send the signal or data to the central hub. And then the hub sends the same data to the destination device.

[![star topology](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/03/star-topology.jpg)](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/03/star-topology.jpg)

The advantage of the star topology is that if a link breaks then only that particular link is affected. The whole network remains undisturbed. The main disadvantage of the star topology is that all the devices of the network are dependent on a single point (hub). If the central hub gets failed, then the whole network gets down.

**c) Ring Topology:** In Ring Topology, each device of the network is connected to two other devices on either side which in turn forms a loop. Data or Signal in ring topology flow only in a single direction from one device to another and reaches the destination node.

[![ring topology](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/03/ring-topology.jpg)](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/03/ring-topology.jpg)

The advantage of ring topology is that it can be installed easily. Adding or deleting devices to the network is also easy. The main disadvantage of ring topology is the data flows only in one direction. And a break at a node in the network can affect the whole network.

**d) Mesh Topology:** In a Mesh Topology, each device of the network is connected to all other devices of the network. Mesh Topology uses Routing and Flooding techniques for data transmission.

[![mesh topology](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/03/mesh-topology.jpg)](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/03/mesh-topology.jpg)

The advantage of mesh topology is if one link breaks then it does not affect the whole network. And the disadvantage is, huge cabling is required and it is expensive.

**Q #34) What is the full form of IDEA?**

**Answer:** IDEA stands for International Data Encryption Algorithm.

**Q #35) Define Piggybacking?**

**Answer:** In data transmission, if the sender sends any data frame to the receiver then the receiver should send the acknowledgment to the sender. The receiver will temporarily delay (waits for the network layer to send the next data packet) the acknowledgment and hooks it to the next outgoing data frame, this process is called Piggybacking.

**Q #36) In how many ways the data is represented and what are they?**

**Answer:** Data transmitted through the networks’ comes in different ways like text, audio, video, images, numbers, etc.

- **Audio:** It is nothing but the continuous sound which is different from text and numbers.
- **Video:** Continuous visual images or a combination of images.
- **Images:** Every image is divided into pixels. And the pixels are represented using bits. Pixels may vary in size based on image resolution.
- **Numbers:** These are converted into binary numbers and are represented using bits.
- **Text:** Text is also represented as bits.

**Q #37) What is the full form of ASCII?**

**Answer:** ASCII stands for American Standard Code for Information Interchange.

**Q #38) How a Switch is different from a Hub?**

**Answer:** Below are the differences between a Switch and a Hub,

**Below given snapshot clearly explains the difference:**

[![switch vs hub](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/03/switch-vs-hub.jpg)](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/03/switch-vs-hub.jpg)

**Q #39) Define Round Trip Time?**

**Answer:** The time taken for a signal to reach the destination and travel back to the sender with the acknowledgment is termed as Round Trip Time (RTT). It is also called Round Trip Delay (RTD).

**Q #40) Define Brouter?**

**Answer:** Brouter or Bridge Router is a device that acts as both a bridge and a router. As a bridge, it forwards data between the networks. And as a router, it routes the data to specified systems within a network.

**Q #41) Define Static IP and Dynamic IP?**

**Answer:** When a device or computer is assigned a specified IP address then it is named as Static IP. It is assigned by the Internet Service Provider as a permanent address.

Dynamic IP is the temporary IP address assigned by the network to a computing device. Dynamic IP is automatically assigned by the server to the network device.

**Q #42) How VPN is used in the corporate world?**

**Answer:** VPN stands for Virtual Private Network. With the help of a VPN, remote users can securely connect to the organization’s network. Corporate companies, educational institutions, government offices, etc use this VPN.

**Q #43) What is the difference between Firewall and Antivirus?**

**Answer:** Firewall and Antivirus are two different security applications used in networking. A firewall acts as a gatekeeper which prevents unauthorized users to access the private networks as intranets. A firewall examines each message and blocks the same which are unsecured.

Antivirus is a software program that protects a computer from any malicious software, any virus, spyware, adware, etc.

**Note:** A Firewall cannot protect the system from viruses, spyware, adware, etc.

**Q #44) Explain Beaconing?**

**Answer:** If a network self-repair its problem then it is termed as Beaconing. Mainly, it is used in the token ring and FDDI (Fiber Distributed Data Interface) networks. If a device in the network is facing any problem, then it notifies the other devices that they are not receiving any signal. Likewise, the problem gets repaired within the network.

**Q #45) Why the standard of an OSI model is termed as 802.xx?**

**Answer:** The OSI model was started in the month of February in 1980. So it is standardized as 802.XX. This ‘80’ stands for the year 1980 and ‘2’ represents the month of February.

**Q #46) Expand DHCP and describe how it works?**

**Answer:** DHCP stands for Dynamic Host Configuration Protocol.

DHCP is used to assign IP addresses automatically to the devices over the network. When a new device is added to the network, it broadcasts a message stating that it is new to the network. Then the message is transmitted to all the devices of the network.

Only the DHCP server will react to the message and assigns a new IP address to the newly added device of the network. With the help of DHCP, IP management became very easy.

**Q #47) How can a network be certified as an effective network? What are the factors affecting them?**

**Answer:** **A network can be certified as an effective network based on below-mentioned factors:**

- **Performance:** A network’s performance is based on its transmitted time and response time. The factors affecting the performance of a network are hardware, software, transmission medium types and the number of users using the network.
- **Reliability:** Reliability is nothing but measuring the probability of failures occurred in a network and the time taken by it to recover from it. The factors affecting the same are the frequency of failure and recovery time from failure.
- **Security:** Protecting the data from viruses and unauthorized users. The factors affecting the security are viruses and users who do not have permission to access the network.

**Q #48) Explain DNS?**

**Answer:** DNS stands for Domain Naming Server. DNS acts as a translator between domain names and IP addresses. As humans remember names, the computer understands only numbers. Generally, we assign names to websites and computers like Gmail.com, Hotmail, etc. When we type such names the DNS translates it into numbers and executes our requests.

Translating the names into numbers or IP address is named as a Forward lookup.

Translating the IP address to names is named as a Reverse lookup.

**Q #49) Define IEEE in the networking world?**

**Answer:** IEEE stands for the Institute of Electrical and Electronic Engineer. This is used to design or develop standards that are used for networking.

**Q #50) What is the use of encryption and decryption?**

**Answer:** Encryption is the process of converting the transmission data into another form that is not read by any other device other than the intended receiver.

Decryption is the process of converting back the encrypted data to its normal form. An algorithm called cipher is used in this conversion process.

**Q #51) Brief Ethernet?**

**Answer:** Ethernet is a technology that is used to connect computers all over the network to transmit the data between each other.

**For Example,** if we connect a computer and laptop to a printer, then we can call it as an Ethernet network. Ethernet acts as the carrier for the Internet within short distance networks like a network in a building.

The main difference between the Internet and Ethernet is security. Ethernet is safer than the Internet as Ethernet is a closed-loop and has only limited access.

**Q #52) Explain Data Encapsulation?**

**Answer:** Encapsulation means adding one thing on top of the other thing. When a message or a packet is passed through the communication network (OSI layers), every layer adds its header information to the actual packet. This process is termed as Data Encapsulation.

**Note:** Decapsulation is exactly the opposite of encapsulation. The process of removing the headers added by the OSI layers from the actual packet is termed as Decapsulation.

[![encapsulation&decapsulation](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/03/encapsulationdecapsulation.jpg)](https://cdn.softwaretestinghelp.com/wp-content/qa/uploads/2017/03/encapsulationdecapsulation.jpg)

**Q #53) How are networks classified based on their connections?**

**Answer:** Networks are classified into two categories based on their connection types. **They are mentioned below:**

- **Peer-to-peer networks (P2P):** When two or more computers are connected together to share resources without the use of a central server is termed as a peer-to-peer network. Computers in this type of network act as both server and client. It is generally used in small companies as they are not expensive.
- **Server-based networks:** In this type of network, a central server is located to store the data, applications, etc of the clients. The server computer provides the security and network administration to the network.

**Q #54) Define Pipelining?**

**Answer:** In Networking, when a task is in progress another task gets started before the previous task is finished. This is termed as Pipelining.

**Q #55) What is an Encoder?**

**Answer:** Encoder is a circuit that uses an algorithm to convert any data or compress audio data or video data for transmission purposes. An encoder converts the analog signal into the digital signal.

**Q #56) What is a Decoder?**

**Answer:** Decoder is a circuit that converts the encoded data to its actual format. It converts the digital signal into an analog signal.

**Q #57) How can you recover the data from a system which is infected with a Virus?**

**Answer:** In another system (not infected with a virus) install an OS and antivirus with the latest updates. Then connect the HDD of the infected system as a secondary drive. Now scan the secondary HDD and clean it. Then copy the data into the system.

**Q #58) Describe the key elements of the protocol?**

**Answer:** **Below are the 3 key elements of the protocol:**

- **Syntax:** It is the format of the data. That means in which order the data is displayed.
- **Semantics:** Describes the meaning of the bits in each section.
- **Timing:** At what time the data is to be sent and how fast it is to be sent.

**Q #59) Explain the difference between baseband and broadband transmission?**

**Answer:**

- **Baseband Transmission:** A single signal consumes the whole bandwidth of the cable.
- **Broadband Transmission:** Multiple signals of multiple frequencies are sent simultaneously.

**Q #60) Expand SLIP?**

**Answer:** SLIP stands for Serial Line Interface Protocol. SLIP is a protocol used for transmitting IP datagrams over a serial line.

### Conclusion

This article is useful for those who are attending the interview on Networking. As networking is a complex topic, one needs to be careful while answering the questions in an interview. If you go through the interview questions on the networking of this article, you can easily get through the interview.

I hope I have covered almost all the important networking interview questions in this article.

Meanwhile, there are several other interview questions available on the internet which you can dig out as well. However, I’m sure that if you have a clear understanding of the questions given here, then you can confidently clear any Networking interview.
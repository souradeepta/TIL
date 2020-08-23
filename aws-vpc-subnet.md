# AWS VPCs and Subnets



## Subnet (subnetwork) 

​	 				 			 		 				 				 			 		 				 			 		 			 		 		 	A subnet, or subnetwork, is a segmented piece of a larger network. More specifically, subnets are a logical partition of an IP network into multiple, smaller network segments. The Internet Protocol (IP) is the method for sending data from one computer to another over the internet. Each computer, or host, on the internet has at least one IP address as a unique identifier.

Organizations will use a subnet to subdivide large networks into smaller, [more efficient subnetworks](https://searchnetworking.techtarget.com/feature/Subnetting-to-increase-performance). One goal of a subnet is to split a large network into a grouping of smaller, interconnected networks to help minimize traffic. This way, traffic doesn't have to flow through unnecessary routs, increasing network speeds.

An [IP address](https://searchwindevelopment.techtarget.com/definition/IP-address) is divided into two fields: a Network Prefix (also called the Network ID) and a Host ID. What separates the Network Prefix and the Host ID depends on whether the address is a Class A, B or C address. Figure 1 shows an IPv4 Class B address, 172.16.37.5. Its Network Prefix is 172.16.0.0, and the Host ID is 37.5.

 

  ![CB IP address illustrated](https://cdn.ttgtmedia.com/rms/onlineimages/networking-subnet_ipv4classb_desktop.png)

The subnet mechanism uses a portion of the Host ID field to identify individual subnets. Figure 2, for example, shows the third group of the 172.16.0.0 network being used as a Subnet ID. A subnet mask is used to identify the part of the address that should be used as the Subnet ID. The subnet mask is applied to the full network address using a binary [AND](https://whatis.techtarget.com/definition/logic-gate-AND-OR-XOR-NOT-NAND-NOR-and-XNOR) operation. AND operations operate, assuming an output is "true" only when both inputs are "true." Otherwise, the output is "false." Only when two bits are both 1. This results in the Subnet ID.

 

Figure 2 shows the AND of the IP address, as well as the mask producing the Subnet ID. Any remaining address bits identify the Host ID. The subnet in Figure 2 is identified as 172.16.2.0, and the Host ID is 5. In practice, network staff will typically refer to a subnet by just the Subnet ID. It would be common to hear someone say, "Subnet 2 is having a problem today," or, "There is a problem with the dot-two subnet."

 

  ![Subnet ID illustration](https://cdn.ttgtmedia.com/rms/onlineimages/networking-subnet_id_desktop.png)

The Subnet ID is used by routers to determine the best route between subnetworks. Figure 3 shows the 172.16.0.0 network, with the third grouping as the Subnet ID. Four of the 256 possible subnets are shown connected to one router. Each subnet is identified either by its Subnet ID or the subnet address with the Host ID set to .0. The router interfaces are assigned the Host ID of .1 -- e.g., 172.16.2.1.

When the router receives a [packet](https://searchnetworking.techtarget.com/definition/packet) addressed to a host on a different subnet than the sender -- host A to host C, for example -- it knows the subnet mask and uses it to determine the Subnet ID of host C. It examines its routing table to find the interface connected to host C's subnet and forwards the packet on that interface.

​           

### Subnet segmentation

A subnet itself also may be segmented into smaller subnets, giving organizations the flexibility to create smaller subnets for things like point-to-point links or for subnetworks that support a few devices. The example below uses an 8-[bit](https://whatis.techtarget.com/de/definition/Bit-Binary-Digit) Subnet ID. The number of bits in the subnet mask depends on the organization's requirements for subnet size and the number of subnets. Other subnet mask lengths are common. While this adds some complexity to network addressing, it significantly improves the efficiency of network address utilization. 

  ![Subnet segmentation illustrated](https://cdn.ttgtmedia.com/rms/onlineimages/networking-subnet_segmentation_desktop.png)

### Beneficial uses of subnets

- **Reallocating IP addresses.** Each class has a limited number of host allocations; for example, networks with more than 254 devices need a Class B allocation. If a network administrator is working with a Class B or C network and needs to allocate 150 hosts for three physical networks located in three different cities, they would need to either request more address blocks for each network -- or divide a network into subnets that enable administrators to use one block of addresses on multiple physical networks.

- **Relieving network congestion.** If much of an organization's traffic is meant to be shared regularly between the same cluster of computers, placing them on the same subnet can reduce network traffic. Without a subnet, all computers and servers on the network would see data packets from every other computer.

- **Improving network security.** Subnetting allows network administrators to reduce network-wide threats by quarantining compromised sections of the network and by making it more difficult for trespassers to move around an organization's network.

  

## Understanding IP Addressing and CIDR Charts                 

Every device connected to the Internet needs to have an identifier. Internet Protocol (IP) addresses are the numerical addresses used to identify a particular piece of hardware connected to the Internet.

The two most common versions of IP in use today are Internet Protocol version 4 (IPv4) and Internet Protocol version 6 (IPv6). Both IPv4 and IPv6 addresses come from finite pools of numbers.

For IPv4, this pool is 32-bits (232) in size and contains 4,294,967,296 IPv4 addresses. The IPv6 address space is 128-bits (2128) in size, containing 340,282,366,920,938,463,463,374,607,431,768,211,456 IPv6 addresses.



### Allocation

IP addresses are assigned to networks in different sized ‘blocks'. The size of the ‘block' assigned is written after an oblique (/), which shows the number of IP addresses contained in that block. For example, if an Internet Service Provider (ISP) is assigned a “/16”, they receive around 64,000 IPv4 addresses. A “/26” network provides 64 IPv4 addresses. The lower the number after the oblique, the more addresses contained in that “block”.



### IPv4

The size of the prefix, in bits, is written after the oblique. This is called “slash notation”. There is a total of 32 bits in IPv4 address space. For example, if a network has the address “192.0.2.0/24”, the number “24” refers to how many bits are contained in the network. From this, the number of bits left for address space can be calculated. As all IPv4 networks have 32 bits, and each “section” of the address denoted by the decimal points contains eight bits, “192.0.2.0/24” leaves eight bits to contain host addresses. This is enough space for 256 host addresses. These host addresses are the IP addresses that are necessary to connect your machine to the Internet.

A network numbered “10.0.0.0/8” (which is one of those reserved for private use) is a network with eight bits of network prefix, denoted by “/8” after the oblique. The “8” denotes that there are 24 bits left over in the network to contain IPv4 host addresses: 16,777,216 addresses to be exact.



### Classless Inter-Domain Routing (CIDR) Chart

The Classless Inter-Domain Routing (CIDR) is commonly known as the CIDR chart and is used by those running networks and managing IP addresses. It enables them to see the number of IP addresses contained within each “slash notation” and the size of each “slash notation” in bits.



![CIDR Chart - IPv4](https://www.ripe.net/images/IPv4CIDRChart_2015.jpg)



## VPCs and subnets

A virtual private cloud (VPC) is a virtual network dedicated to your AWS account.  It is logically isolated from other virtual networks in the AWS Cloud. You can launch your AWS resources, such as Amazon EC2 instances, into your VPC.                                                              		                                 

When you create a VPC, you must specify a range of IPv4 addresses for the VPC in the form of a Classless Inter-Domain Routing (CIDR) block; for example,  `10.0.0.0/16`. This is the primary CIDR block for your VPC. For more  information about CIDR notation, see [RFC 4632](https://tools.ietf.org/html/rfc4632).                                                 

The following diagram shows a new VPC with an IPv4 CIDR block.

​			                                     				![ 				VPC with the main route table 			](https://docs.aws.amazon.com/vpc/latest/userguide/images/vpc-diagram.png)                                                                                         

The main route table has the following routes.                           

| Destination | Target |
| :---------- | :----- |
| 10.0.0.0/16 | local  |

A VPC spans all of the Availability Zones in the Region. After creating a VPC, you can  add one or more subnets in each Availability Zone. You can optionally add subnets in a Local Zone, which is an AWS infrastructure deployment that places compute, storage, database, and other select services closer to your end users. A Local Zone enables your  end users to run applications that require single-digit millisecond latencies. When you create a subnet, you specify the CIDR block for the subnet, which is a subset of the VPC CIDR block. Each subnet must reside entirely within one Availability Zone and cannot span zones. Availability Zones are distinct locations that are engineered to be isolated from failures in other Availability Zones. By launching instances in separate Availability Zones, you can protect your applications from the failure of a single location. We assign a unique                                    ID  to each subnet.                                                              

You can also optionally assign an IPv6 CIDR block to your VPC, and assign IPv6 CIDR  blocks to your subnets.                                                  

The following diagram shows a VPC that has been configured with subnets in multiple                                    Availability Zones. 1A, 2A, and 3A are instances in your VPC. An IPv6 CIDR block is                                    associated with the VPC, and an IPv6 CIDR block is associated with subnet 1. An internet                                    gateway enables communication over the internet, and a virtual private network (VPN)                                    connection enables communication with your corporate network.                                 

​           				![ 				VPC with multiple Availability Zones 			](https://docs.aws.amazon.com/vpc/latest/userguide/images/subnets-diagram.png)                                    			                                     			                                     		                                 

If a subnet's traffic is routed to an internet gateway, the subnet is known as a *public subnet*. In this diagram, subnet 1 is a public subnet. If you want your instance in a public subnet to communicate with the internet over IPv4, it must have a public IPv4 address or an Elastic IP address (IPv4). If you want your instance in the public subnet to communicate with the internet over IPv6, it must have an IPv6 address.                                                                

If a subnet doesn't have a route to the internet gateway, the subnet is known as a *private subnet*. In this diagram, subnet 2 is a private subnet.                                 

If a subnet doesn't have a route to the internet gateway, but has its traffic routed  to a virtual private gateway for a Site-to-Site VPN connection, the subnet is known as a *VPN-only subnet*. In this diagram, subnet 3 is a VPN-only Subnet. Currently, we do not support IPv6 traffic over a Site-to-Site VPN connection.
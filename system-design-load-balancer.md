# Load Balancers



When you have multiple Servers handling the requests coming from the end users then it is essential for the requests to be heavily distributed among the servers. Here the Load Balancer comes in play. A Load Balancer acts as a layer between the server and the end user which distributes the requests evenly to all the servers. With a load balancer present no user request can directly go to any servers, it first needs to go through a load balancer and then it can be further be directed to one of the servers.

![img](https://systemsthatscale.org/articles/article-media/Load-Balancer.png)



## Designing an approach to route requests from the Load Balancer

Let’s design a basic mechanism to route the requests to one of the servers when it reaches the Load Balancer.

Suppose we have **N** Servers with us and a Load Balancer **L** which routes the requests to these **N** servers.

Now a request with a Request ID **R1** reaches the Load Balancer. Our Load Balancer **L** could possibly use some hashed function in order to hash the request id **R1** and then further use this hashed request id say **h(R1)** to further route it to the server. Now we need to do some math in order to bring this hashed number in the range of our server numbers **N** so that it can be routed further. We can do it possible by taking modulo **N** of the hashed id and then use that number as a server id.

Let’s See how :

- **R1** : Request ID coming to get served
- **h** : Evenly distributed Hash Function
- **h(R1)** : Hashed Request ID
- **N** : Number of Servers



![img](https://systemsthatscale.org/articles/article-media/load-balancer-code.png)


Suppose we have 8 Servers having IDs : **S0, S1, S2 ……, S7**

A request with ID **17** reaches the Load Balancer.

Now the hash function is used to hash that ID and we get **110** as the hashed value **h(17) = 110**

Now we can bring it in the range of our server numbers **110 % 8 = 6**

So we can route this request to Server **S2**.

In this way we can distribute all the requests coming to our Load Balancer evenly to all the servers. But is it an optimal approach? Yes it distributes the requests evenly but what if we need to increase the number of our servers. Increasing the server will change the destination servers of all the incoming requests. What if we were storing the cache related to that request in its destination server? Now as that request is no longer routed to the earlier server, our entire cache can go in trash probably. Think!

There are various Load Balancers which are used and can be essential to know about.



## L4 Load Balancer

L4 Load Balancer routes the request on the basis of the `address information` of the incoming requests. It does not inspect the content of the request. The Layer 4 (L4) Load Balancer makes the routing decisions based on address information extracted from the first few packets in the TCP stream.



## L7 Load Balancer

L7 Load Balancer routes the request on the basis of the `packet content`. There can be dedicated servers which could serve the requests based on their content, like URLs, Images, Graphics and Video contents. You can set up the system in such a way that the static contents are served by one dedicated server and requests demanding certain information which needs a db call can be served by another dedicated server.
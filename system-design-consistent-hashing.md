# Consistent Hashing



Consistent Hashing is a concept extensively used in Load Balancing. In a typical Load Balancer we use hashing in order to map the requests to their corresponding Servers. Here when we had to add or remove any Server then the Hash Values of all the earlier requests got modified which caused our cache to become obsolete. In order to avoid this problem we use the concept of Consistent Hashing. Using consistent hashing in our system we can avoid the change in the hash function of all the earlier Requests whenever any new server is added or removed. It allows only a small amount of requests to change hence making our cache very much in use.



## Understanding the Problem

Suppose you have a Load Balancer which distributes your requests among three servers : **S1, S2 and S3**. We have used a hash function to direct the requests to one of the servers in the system. The entire method used looks like this :

Server ID (Destination) = **h( Request ID )** % 3

Here we can clearly see that the request id of the incoming request is hashed by a hash function and then we did a modulo of the result with the total number of servers which is 3 in this case.

Suppose 5 Requests come initially then they will be directed in the following order :



- [ Request ID ] 3 ---> h(3) = 101 ---> 101%3 = 2 [ Server ID ]
- [ Request ID ] 12 ---> h(12) = 39 ---> 39%3 = 0 [ Server ID ]
- [ Request ID ] 41 ---> h(41) = 22 ---> 22%3 = 1 [ Server ID ]
- [ Request ID ] 62 ---> h(62) = 98 ---> 98%3 = 2 [ Server ID ]
- [ Request ID ] 92 ---> h(92) = 10 ---> 10%3 = 1 [ Server ID ]


As you can see, by using the earlier method we directed our requests to 3 Servers. Now what happens when an extra server is added to the System. Now we have 4 Servers with us and this will have a large impact on the address of the destination servers of the earlier requests.

When the same 5 requests reach the System, it is directed in the following manner.



- [ Request ID ] 3 ---> h(3) = 101 ---> 101%4 = 1 [ Server ID ]
- [ Request ID ] 12 ---> h(12) = 39 ---> 39%4 = 3 [ Server ID ]
- [ Request ID ] 41 ---> h(41) = 22 ---> 22%4 = 2 [ Server ID ]
- [ Request ID ] 62 ---> h(62) = 98 ---> 98%4 = 2 [ Server ID ]
- [ Request ID ] 92 ---> h(92) = 10 ---> 10%4 = 2 [ Server ID ]


We can clearly observe that the destinations of almost all the requests have been changed. Hence by adding or removing even one server will cause the requests to be directed to a completely different server. This can have huge drawbacks. Suppose if a server stored some critical data in cache about a certain request in the hope of re-using it when that request visits again. Now the entire data in the cache is obsolete.

So our current method of Hashing is not quite reliable in order to direct the requests to our multiple servers. We need something which could cause a very less amount of change in the destination of the requests when a new server is added or removed. Here Consistent Hashing comes into play. Let’s see how it solves our current problem.



## Discussing Consistent Hashing

It is a method which is independent of the number of servers present in the System. It hashes all the Servers to a hash ID which is plotted on a circular ring. This ring like structure allows a very minimal change in the requests when a server is added or removed.

Suppose our Hash Function returns a value in the range of 0 to N. The ring starts from 0 and ends at N. Now each Server present is hashed using this function and is plotted over the ring. Suppose we have three servers S1, S2 and S3 and have 5 requests coming to the system.

We used our hash function say **H1** and hashed every server and request to get the values like this :



- Server : S1 ------> H1( S1 ) ------> Hashed Value : HS1
- Server : S2 ------> H1( S2 ) ------> Hashed Value : HS2
- Server : S3 ------> H1( S3 ) ------> Hashed Value : HS3



- Request : R1 ------> H1( R1 ) ------> Hashed Value : HR1
- Request : R2 ------> H1( R2 ) ------> Hashed Value : HR2
- Request : R3 ------> H1( R3 ) ------> Hashed Value : HR3
- Request : R4 ------> H1( R4 ) ------> Hashed Value : HR4
- Request : R5 ------> H1( R5 ) ------> Hashed Value : HR5


Now they can be plotted on the circular pie as described below :



![img](https://systemsthatscale.org/articles/article-media/consistent-hashing.png)

Here every Request is served by the Server which is adjacent to it when we move in a clockwise manner. Hence Requests **R1** and **R2** will be served by Server **S1**, Requests **R3** and **R4** will be served by the Server **S2** and Request **R5** will be served by Server **S3**.

Suppose Server **S1** got shut down due to some issue. Then Requests **R1** and **R2** which was earlier served by **S1** will now be served by server **S2** leaving all the other requests completely unchanged. This caused only those requests to change which was earlier served by the obsolete server. Rest of the requests were completely unaffected by the change in the number of servers. This is one of the major advantages of using Consistent Hashing in a system.



## Skewed Load in Consistent Hashing

Earlier when me removed Server S1 from the system Requests **R1** and **R2** were served by Server **S2**. Now requests **R1**, **R2**, **R3** and **R4** are served by Server **S2**. That means 4 out of 5 requests are served by a single server. The entire load is skewed on a single server and this can cause that System to wear out or shut down.

What can be done to avoid this problem here. Instead of using a single hash function we can possibly use multiple Hash Functions to hash the servers. Suppose we used **K** hash functions. Now each server will have **K** points on the circular ring and then it will be less likely to have a skewed load over a single server.

We can select the value of **K** accordingly in order to reduce the chance of having skewed load to none. Possibly **log(N)** or **log(M)** where **N** is the Number of available Servers and **M** is the number of Requests.
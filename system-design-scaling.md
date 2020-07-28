# Horizontal and Vertical Scaling



When you have a small system having less amount of load then it’s quite easy to maintain. You don’t have to think much about increasing computing power, handling large numbers of read/write requests, your server getting crashed and much more. But what happens when the load over your system increases. Now you will need to worry about the above mentioned problems. While designing a system one should keep in mind the amount of load which it has to bear and should meet the end user’s requirements at the same time. This is probably called Scaling. When such a situation occurs you have two things under your sleeves. Either you can increase the computation power of your underlying system or else you can increase the number of systems so that it can handle the increasing load. This brings us to our topic i.e. horizontal and vertical scaling.



## Horizontal Scaling

In order to handle the increasing load you can increase the number of machines. This process is known as Horizontal Scaling. The process has its own advantages and drawbacks. When you increase the number of machines you will need to have a `Load Balancer` which will ensure that the requests coming to your system are distributed uniformly over all the machines. We will be discussing Load Balancers in the next article. But your system can scale really well even the amount of load increases. You will just be needed to put extra machines according to the increasing load and route the requests efficiently to all the systems.

If you are scaling horizontally then your system will have following characteristics :

- **Resilient** : You system will be resilient. It will be able to handle large amounts of loads or requests and will be able to recover in less time in case of any failure.
- **Network Calls** : As you are using multiple machines to handle your load, you will be needing a procedure to set up a contact between these machines. You will be required to set up a network or a remote procedure call among the machines.
- **Need of Load Balancer** : As discussed earlier you will be needing a load balancer to distribute the requests among the machines. As you are using multiple machines, there can be scenarios where one machine gets a huge number of requests and another may be sitting idle. In order to reduce this uneven distribution of requests we will have to use a Load Balancer which will ensure that every machine gets an equal amount of requests and your system can handle huge loads efficiently.
- **Scales Well** : Your system will have the capacity to handle an increasing number of loads. You can increase the number of machines according to the load and route the requests evenly to these machines.

![img](https://systemsthatscale.org/articles/article-media/Horizontal-Scaling.png)



## Vertical Scaling

To handle the increasing load you could increase the computation capacity of your existing system. This involves adding more processing power, more storage, more memory etc. to your system. In this part you won’t be needing any load balancer or network calls as there is a single system with a huge computation capacity handling the entire load. This can be fast. But there can be a scenario where you won’t be able to add more resources to your underlying machine because of increasing cost or anything and buying identical machines will be a cheaper and better idea.

If you are scaling vertically your system will be having following categories :

- **Consistent** : As your system has a single machine the data will be consistent as it will be stored at one place. You can avoid Data Inconsistency when using Vertical Scaling.
- **Single Point of Failure** : Being dependent on a single machine can cause a single point of failure. If there is any fault in the machine and it is unable to handle the requests in the anticipated way, then your entire system can fail.
- **Inter Process Communication** : With vertical scaling you can achieve inter process communication. The processes can communicate with each other and synchronize their actions. This can make the system handle requests faster.

![img](https://systemsthatscale.org/articles/article-media/Vertical-Scaling.png)

So, which is better: Horizontal Scaling or Vertical. I guess it can vary in different situations. Suppose your system is needed to handle the amount of load which can be handled by adding some extra computation power to the existing machine then you can go with Vertical Scaling. You will be able to achieve a consistent system and moreover won’t be needing any Load Balancers or Network Calls. But if adding computation power to the machine seems unfeasible then you can add more identical machines instead. Then you will be able to handle an increasing amount of loads with ease having a resilient system.

Another idea can be to build a Hybrid System taking the advantage of both the techniques. Each machine has high computation power and has multiple such machines to handle the increasing loads.
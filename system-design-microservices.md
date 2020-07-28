# Microservices and Monolithic Architecture



Systems are becoming large and complex with time. Now a single system or an application is responsible for performing multiple functions, providing users with multiple features and services. Such a large system can get complex sometimes as they have multiple components functioning altogether. Hence writing modular codes at the backend will not be enough to ensure a structured design. Apart from that, the system should also be structured at a higher level. There are multiple ways to keep your system structured so that it can be easy to maintain and understand.

A System having multiple components can keep all their components at a single machine or at a single place and such an architecture is known as a **Monolithic Architecture**. While there can be other cases where systems may store their different components individually at different systems all of them connected through network calls, such an architecture is known as **Microservice Architecture**. We will be discussing both the architectures in detail, looking into their advantages and disadvantages. Let’s take an example to discuss both the architectures so that you all can get a crystal clear understanding of these concepts.

Suppose we are designing an E-commerce Application having multiple components. Let’s see how we can design it through a monolithic architecture and microservices.



## Monolithic Architecture

A system implementing **Monolithic Architecture** stores all of its components at a single place or in a single system. Suppose we are designing an E-commerce Application which has multiple components like : Product Catalogue, User Profile, User Cart and many more components.

![img](https://systemsthatscale.org/articles/article-media/monolithic-arch.png)


Being a Monolithic Architecture the system stores all its components at a single machine. Such a system can have following characteristics :



- **Fast** : A system following a monolithic architecture can be comparatively fast as no network call or Remote Procedure Call (RPC) is required between multiple components. As the components are located at a single machine, hence a simple function call can get the job done. And these function calls are much faster than the network calls, hence the entire system can be fast.

- **Large and Complex Systems** : As all the components of the System are stored at a single place or a single machine, hence the system can get large. And if there are large components in a system then the entire system can become complex and hard to understand. This can cause several maintenance issues in future.

- **Hard to Scale** : Systems having a monolithic architecture can be hard or costlier to scale. They can take up unnecessary resources which can be saved by adopting a Microservice architecture. Earlier we had an E-commerce application with multiple components. Suppose there is a sale going on and this has a huge load over the Product Catalogue section. A large number of users are trying to access the Catalogue. So you need to scale that component so that it can respond to the increasing load. You will be increasing the number of servers according to the increasing load. But in this architecture this will be a costly approach as all the components are stored in a single system and increasing the number of servers will indeed scale all the components present in the System.

  This is one of the major drawbacks with Monolithic Architecture. The component which is not facing a spike in the load is also scaled along unnecessarily.

- **Hefty Deployments** : Suppose you made a small change in the code base of the User Profile component of the E-commerce application. At the time of deployment you will need to deploy the entire system which can be costly and time taking as well. This is also a major drawback as all the components reside in a single machine.

- **Hard to Understand** : Suppose a new developer joins your team and is assigned to work on one of the components of your system. As your system implements a monolithic architecture, hence all the components are clubbed in a single machine and that new developer has to go through the entire system in order to get a clear understanding of a single component. This makes a monolithic architecture hard and time taking to understand. The system may appear to be complex to a new individual. It can appear to be unstructured.

- **Complex Testing process** : A monolithic architecture is complex to test. Suppose you have performed a small change in one of the components and have deployed the system. Then you will be needed to test the entire system as there is a chance of having a bug introduced anywhere in the system as all the components are clubbed into a single machine.



## Microservice Architecture

A system implementing Microservice Architecture has multiple components known as microservice which are loosely coupled and functioning independently. Suppose we have an E-commerce Application having multiple components as discussed earlier. In a microservice architecture all these components are stored in separate machines or servers which communicate remotely with each other through a Network Call or a Remote Procedure Call (RPC). These components function individually and solely as a separate entity. They come up together to form an entire system.

Basically a large system is broken down into multiple components which are then stored in distinct machines and are set to operate independently from each other. All these components function as an independent and complete system.

![img](https://systemsthatscale.org/articles/article-media/microservice-arch.png)

A Microservice Architecture have following characteristics :



- **Network Calls required** : In a microservice architecture all the components are stored in different machines and hence requires a medium to communicate. These components communicate with each other through a set of APIs or Network Calls. An individual component receives the request from the network call , processes it and returns the response again through a network call.
- **Highly Scalable** : A microservice architecture is highly scalable. As all the components function individually hence it is easier to scale a particular component according to the requirements. In an E-commerce application when there is a sudden spike in the load on the Catalogue component during the time of sale then we can easily scale that particular component only. As these components are stored in separate machines, we can increase the count of the machines which hold that particular component according to the increasing load. The rest of the components which didn’t receive any load hike are kept untouched. Hence using the resources efficiently.
- **Easy to Test** : A system implementing a microservice architecture is easy to test. Suppose we need to make a change in the User Profile component of the E-commerce application. In that case we can make the change in that system and test it independently for various test cases and once we are satisfied with the testing we can deploy the changes. We are not required to test the entire system in case of a microservice architecture. We only need to test that particular component or microservice in which the change has been made.
- **Easy to Understand** : A microservice architecture is easy to understand. As all the components function independently and are not dependent on others in any ways. Hence when a new developer comes up and has to work on a particular component then in that case he/she is not required to go through the entire system. They can simply understand the working of that particular component or microservice which they have been assigned to work on. As these components are loosely coupled, allow the developers to work independently on the services.
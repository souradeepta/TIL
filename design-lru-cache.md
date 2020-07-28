# Designing LRU Cache



Design an LRU (Least Recently Used) Cache. The Cache must support put and get function that will allow the data items to be added in the cache and fetched from the cache through a key.

An LRU Cache is widely implemented in many services and caching systems. As the name suggests Least Recently Used cache, decides the existence of a piece of data in the cache on the basis of their last access time. It basically discards the least recently used item from the cache. When a data is fetched from the cache its access time is updated and it is promoted all the way at the top of the cache. In this way the data items which are not used frequently eventually accumulate at the bottom and are finally removed from the cache. LRU works on the principle that : ***There is a high chance of a request to demand a piece of data which was recently accessed in the past***.

Here we will design an LRU Cache from scratch that will be working as described above.

Data Structures Used

1. **Doubly Linked List** : We will be using a doubly linked list to store the data values and their keys in a single node. The idea of using a Doubly Linked List is to achieve an O(1) Time Complexity while inserting and deleting a node from a particular position in the List.
2. **HashMap** : We will also be using HashMap to have a record of the keys currently present in our LRU cache. The basic aim of introducing the hashmap along with the linked list is to achieve O(1) Time Complexity when performing a get operation in our cache.
3. 

## Designing our Data Structures

Now as we know which data structures we will be using to design our cache, let's move ahead with actually designing them.

Our Linked List Data Structure will look like this :



![img](https://systemsthatscale.org/problems/problem-media/lru-cache-1.png)


Our HashMap Data Structure will look like this :



![img](https://systemsthatscale.org/problems/problem-media/lru-cache-2.png)

Here the HashMap stores the Key and Node pair. Where Key is an integer value and Node is a doubly linked list node having the following structure above.

We can visualize our combined structure as below:

![img](https://systemsthatscale.org/problems/problem-media/lru-cache-dsa.png)



## Fetching Data from the Cache

Data is stored in the cache in a key-value pair. When a particular data is required then we can lookup that data through its key in the cache. If that key is present in the cache then it is considered as a Cache Hit and the corresponding value is returned. If that key is not present in the Cache then it is considered as a Cache Miss and a null value is returned from the Cache.

As it is an LRU Cache we need to make some updates while fetching the data. If we search a data item through its key in the cache and it’s present in the cache as well. Then we need to update that data item in the cache making it the latest item. As the most outdated data items are removed from the cache incase of overflow, hence we could preserve those data items which are recently used from the cache. In our data structure we could simply bring the used data items at the head of the list and remove the rear nodes when the capacity of the list exceeds.

We can design the get operation for our LRU Cache as described below :



![img](https://systemsthatscale.org/problems/problem-media/lru-cache-3.png)



## Putting Data into the Cache

We have already discussed that the data item will be stored in a key-value pair in our cache. Now two scenarios can come up here, the key to be added doesn’t exist in the cache or either it already exists in the cache.

For the first case, if we are putting a data item which doesn’t exist in the cache we can simply put that item at the front of the list. So that it can be the latest data item in our cache. Or if we are putting a data item which already exists in the cache, then we can simply update the timestamp of that item so that it becomes the latest item in the cache. This means that we need to move the existing node to the head of the List.

Our put function can be implemented in this manner :



![img](https://systemsthatscale.org/problems/problem-media/lru-cache-4.png)



![img](https://systemsthatscale.org/problems/problem-media/lru-cache-5.png)


Here we are done with the implementation of both get and put operation and have successfully designed a working LRU Cache from scratch. Not only we have implemented the cache but we have also implemented it efficiently. Both the functions have a time complexity of **O(1)**.
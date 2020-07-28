# Caching

When you design a system it will need to process the requests of the end users. There will be scenarios where a request call will demand a certain data and your system will have to retrieve the data through a database call and then return that response back to the end user. What happens if that same request comes multiple times. You will be making the same DB call multiple times to fetch the same data. Here you can do one thing, when the request comes for the first time you make the DB call and then save the response close to your system, so that when next time the same request comes in then you won’t be needed to go all the way to your database to fetch the response. Instead you can return your response which you earlier saved close to your system. This process is known as Caching and the place where you store your response or result for a certain amount of time is known as Cache.

![img](https://systemsthatscale.org/articles/article-media/Cache-flow-chart.png)



Some characteristics of Caching :

- **Fast** : Caching can reduce the response time of your system to a great extent. You should be wondering why caching is fast. Basically a cache uses an SSD to store your response and they can be accessed very quickly by the system as compared to a database call.

- **Less Database Calls** : Caching when used efficiently could reduce the DB calls to a large extent. Suppose there is a piece of data which is in a really high demand by the end user and approx 70% of the response your system receives demands this piece of data only. Then if you cached that data then you could possibly reduce your DB calls by approx. 70% as most of the requests will be processed through the cache.

- **Reduces Computation** : Caching can also reduce the computation performed by the system. Suppose you have a request which demands the maximum percentage obtained in a CS coursework, then you will have to make a DB call which may go through every percentage and return the maximum through comparison. You could store this maximum percentage value in your cache so that when next time any request demands this data then you could avoid making all the computations and directly return the end result.

- **Data Inconsistency** : There is a chance of data inconsistency in case of caching. Because during caching you have the same piece of data located in the cache as well as in your database. So if there are any updates in the database then it may cause the data to become inconsistent. Various cache writing methods like write back or write through are used to ensure the data consistency.

  

## Cache Policy

A cache has a limited storage and it is also advised to store a limited amount of data into the cache. This is what makes it beautiful. If you store the data in bulk in your cache then it will again take longer to fetch the response and then there will be no point of having a cache in the first place. As now you can store only a limited amount of data, it’s your choice to decide what piece of data you want to keep in the cache and what you want to evict. Now there are many existing policies which help to decide this and they are known as **Cache Policy**. A cache policy can be basically an algorithm which helps in deciding whether a data should be cached or evicted. A better cache policy can reduce a lot of database calls and can make your system incredibly fast. But a poor Cache Policy can turn out to be a nightmare.

One of the most popular Cache Policy is **Least Recently Used** (LRU) which decides the existence of a piece of data in the cache on the basis of their last access time. It basically discards the least recently used item from the cache. When a data is fetched from the cache its access time is updated and it is promoted all the way at the top of the cache. In this way the data items which are not used frequently eventually accumulate at the bottom and are finally removed from the cache. LRU works on the principle that : **There is a high chance of a request to demand a piece of data which was recently accessed in the past**.

**Note** : We will be discussing **“How to design an LRU Cache”** as a System Design Problem.

Write Back, Write Through and Write Around Cache

When updates are made in your system then you will need to ensure that the data present in your cache is also updated accordingly, in order to avoid any data inconsistency in future. Your cache should hold the latest version of your data.

- **Write Through Cache** : In this method the data is updated in the cache as well as in the database eventually at the same time. The process is considered to be completed when the data has been successfully written in cache as well as in the backend database. The method ensures the data consistency but you could face latency as the data is needed to be written at two places.
- **Write Around Cache** : In this method data is updated in the backend database only without writing it in the cache. The process is not bothered by a successful write into the cache. As the data is written to the database the process is considered to be completed. You could face a cache miss for the data recently written into the database.
- **Write Back Cache** : In this method data is updated in the cache only. The completion of the process is not backed by a successful write to the backend database. The data may be written in the backend but that’s not the part of this process and hence not essential here. If the cache fails then your system may experience data loss.



## Distributed Cache - Memcached

A distributed cache functions like a simple cache but in order to provide a large storage space they are distributed over multiple machines. When a request hits your system then your system first checks the distributed cache for the presence of required data items. The cache maps every request to a machine id where that can be further processed. It then fetches the response from that particular machine.

**Memcached** is an example of a distributed cache. It maintains the data in a key-value pair format. You could hit the cache with a particular key and it will return the corresponding value to that key. It may return false or null value if that key is not present in the cache. Memcache also follows LRU (Least Recently Used) Policy for keeping the data items in the cache.

A sample code for fetching data from Memcached



![img](https://systemsthatscale.org/articles/article-media/memcached-get-data.png)


A sample code for Updating data in Memcached



![img](https://systemsthatscale.org/articles/article-media/memcached-update-data.png)
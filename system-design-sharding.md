# Database Sharding



Initially when you have a small user base then you could have your entire data in a single database placed in one local machine. And your system will still perform accordingly. But what if the user base starts growing. The size of the database will increase and then keeping your entire db in a single place could make your system slow. The queries may take a lot of time to get executed. So, there are multiple solutions which you could adopt in order to optimize your queries. You can go with Indexing, use NoSQL Databases or can perform Sharding. When your database grows, then you could divide your database into multiple smaller parts and store each part separately in different machines. The smaller parts are called **Shards** and the entire process is known as **Database Sharding**. It can be a possible solution when your database becomes too large to be stored in a single machine. There are various ways in which you could possibly shard your database. These are discussed below.



## Horizontal Sharding

When a single table in your database becomes large then you could possibly divide that one table by rows. Now that one large table gets divided into multiple smaller tables with the same schema but having different dataset. This process is known as **Horizontal Sharding** or Horizontal Partitioning.

Suppose you have a table having 1 Million Dataset and you are planning to store them in 5 different local machines. Then you could divide the entire dataset into 5 smaller tables each having a size of 200,000 datasets. You could possibly use some algorithm to divide your dataset into multiple machines in a well distributed manner.

![img](https://systemsthatscale.org/articles/article-media/horizontal-sharding.png)



## Vertical Sharding

When you have multiple tables in your database then you could put multiple tables in multiple different machines. Or you could also divide a single large table column wise and store them in multiple machines. This process of partitioning is known as **Vertical Sharding** or **Vertical Partitioning**.

Suppose you have 3 multiple tables in your database each storing different types of datasets. One storing user Information, second one storing all the media files and the last one storing organization details. Now you could store these three tables in 3 different machines. When you are required to query the database then you would be required to know which type of data you are querying for and then look for that data in the machine which is responsible for storing that table.

![img](https://systemsthatscale.org/articles/article-media/vertical-sharding.png)



## Sharding can be a bad Idea

Sharding is not always a good idea. Before going with Sharding you should also look into other possible alternatives which could be less complex and still your system can be reliable.



- **Programming and Operational Complexity** : When you partition your database and store them in different machines then it could increase the complexity of your system. When you will query your database then there could be a situation where your query will be sent to multiple machines and then datasets will be fetched according to the query and will be needed to join back when these results are returned to the system. This entire process can be expensive and complex as well. Maintaining a Sharded Database is also difficult. As you have multiple machines with yourself you need to be responsible for maintaining all of them and if any one of them fails then it could lead to data unavailability.
- **Hotspots in Sharding** : As you have multiple machines which are responsible for storing your data, you need to ensure that the incoming data to be stored gets equally distributed among the available machines. If this doesn't happen and one of the machines gets most of the data then in turn it will be responsible for answering most of the queries. Hence it will be having a maximum portion of the entire load and may wear out. This phenomena of concentration of the entire load at a single machine is known as the existence of Hotspot in sharding and that single machine is known as **Hotspot**.
- **Less Flexible** : If you have a certain number of machines among which your data sets are distributed then it’s really complex to increase or decrease that number. It’s complex to add or remove machines from the existing sharded database design. In order to avoid this problem Consistent Hashing is used which can provide flexibility to your system implementing sharded Database.



## Algorithmic Sharding

There are multiple ways to partition the data into several machines. One of the methods is Algorithmic Sharding where a certain algorithm is used to determine the machine where the dataset is needed to be stored. The process is fast as you don’t need to be dependent on any external device or service to determine the location of your data storage. The algorithm can be in the form of a Partition Function or a Sharding Function which can take the Partition ID or Partition Key as an input and return the Address or ID of the machine where that corresponding dataset is stored. This sharding function distributes the dataset into multiple machines. You should ensure that your sharding function distributes the data uniformly among all the available machines and avoid the existence of hotspots.

Suppose you have 7 machines which you will be using to shard your Database. You can build your algorithm which can distribute the datasets into these 7 machines. One possible algorithm for dividing the datasets can be :



![img](https://systemsthatscale.org/articles/article-media/db_sharding_code.png)


According to the above algorithm, when multiple datasets having following IDs enter the system then it gets directed to the following machine IDs.



- Dataset ID : 121 ------> shard_function( 121 ) ----------> Machine ID : 2
- Dataset ID : 45 ------> shard_function( 45 ) ----------> Machine ID : 3
- Dataset ID : 195 ------> shard_function( 195 ) ----------> Machine ID : 6
- Dataset ID : 21 ------> shard_function( 21 ) ----------> Machine ID : 0
- Dataset ID : 67 ------> shard_function( 67 ) ----------> Machine ID : 4



## Dynamic Sharding

In Dynamic Sharding an external service is used in order to determine the location of the machine in which the dataset will be present. It doesn’t use any algorithm to determine the address of the destination machine. Every request coming to the system needs to go through the external services in order to fetch the address of the machine. Hence this process is comparatively slower than Algorithmic Sharding. Moreover every request coming to the system is somewhere dependent upon the external locator service and if the service fails then the entire system can get jeopardized. This introduces a Single point of Failure in the System. On the other hand Dynamic Sharding is more resilient to non uniform distribution of data.
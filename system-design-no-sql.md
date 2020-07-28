# NoSQL Databases

So let's start with defining basically what a NoSQL database is and why do we need it, we already have a relational database model which we can use to store and maintain our data. We require a NoSQL model when our data is unstructured and messy. Basically in a relational model we have to specify the columns for our table which is required to be filled for every row or data set. Suppose we have a scenario where we need to store the scores of students where every student is free to take an exam of any number of subjects from a given set of 'N' subjects. If we model this data in a Relational Database then our data may look like :



![img](https://systemsthatscale.org/articles/article-media/nosql-1.png)

As we can see the data is really unstructured and when the size of this table increases we end up wasting a lot of memory which was not really required in the first place. Moreover, applying joins on large relational tables also takes up a lot of time.

NoSQL database allows data to be stored in a key value format (basically JSON) where all the details of a particular data set is present in a single block in a key-value pair format.For instance if we have to store the above student details in a NoSQL format a single dataset would look like:

![img](https://systemsthatscale.org/articles/article-media/no-sql-data.png)

Here we can see how the above unstructured data can be efficiently stored in a NoSQL format. We don't need to keep those columns in which data does not exist for a particular data set. It is simply discarded in a NoSQL design.

About Amazon DynamoDB

Amazon DynamoDB is a fully managed NoSQL database service that provides fast and predictable performance with seamless scalability. DynamoDB allows us to design and store our data in a NoSql format and the rest is taken care of by the Amazon services. We don't have to take care about scaling or concurrency. When we create our table in DynamoDb we only need to define a Primary Key for our table which should be unique for every data set. After defining the primary key one can create his/her table and start populating it.

Primary Key in Dynamo DB

As discussed above, the primary key should be unique for every data set. In DynamoDb we have two types of primary keys:



- **Partition Key** : Our primary key can comprise one value which can be called as a partition key. Partition Key should be unique for every data set if our primary key consists of only partition keys. DynamoDb uses this partition key to store our data in different partitions and hence when we query our data through a given partition key then dynamo db figures out the partition in which this data set is stored and fetches it from that partition.
- **Composite Primary Key** (Partition Key + Sort Key): The composite primary key comprises a combination of Partition Key and Sort Key. Here Partition Key is used to obtain the physical address of the partition where the data set will be stored and all the data sets in a single partition are sorted by their sort key. Here two data sets can have similar partition keys but the combination of partition and sort key must be unique.

Index in Dynamo DB

Apart from this Amazon DynamoDb has another amazing feature of index for querying data in the NoSQL model in an efficient way. Dynamo Db have two types of index:



- **Global Secondary Index (GSI)**: A global secondary index comprises partition key and sort key. Partition key of a GSI can be different from that of the primary key of the table. When we query in a NoSQL table through a GSI having partition and sort key then dynamo db figures out the physical address of the partition where data-set is stored using the given partition key, then it further fetches the data from that partition with the help of the given sort key (as the data sets are stored in a sorted order on the basis of sort key) very efficiently and quickly (I guess it would be using binary search to achieve this). So basically for every particular GSI, DynamoDb builds a different view of that table as it requires querying and storing data in a completely different manner (due to different partitions and sort keys).
- **Local Secondary Index (LSI)**: A local secondary index comprises partition key and sort key. The partition key is the same as that of the table's primary key. Hence dynamo doesn't need to create a different view for every LSI.



**Note:** There is a limit of 20 GSI and 5 LSI per table in DynamoDB.

Read Consistency in Dynamo DB

DynamoDb supports two types of read consistency:



- **Eventually Consistent Reads** : When we perform an eventually consistent read on an item there can be a scenario where we might not get the recently updated version of the item and may end up getting the previous version of that item. But if we perform our read after some time then we may get the most recent version of the item.
- **Strongly Consistent Reads** : When we perform a strongly consistent read then Dynamo Db provides us with the most recent version of that item. But using strongly consistent read has its own trade offs like high throughput capacity, unavailability or high latency. Moreover we can not perform a strongly consistent read over a Global Secondary Index.



When we go under the hood we will get to know that Amazon DynamoDb doesn't actually store our data in a single region. It stores three copies of our data set in three different regions, hence when one region is down or unavailable then still we can get our data from the second or third region. Hence when we perform an eventually consistent read on an item the dynamo db gives back that item from any of the three regions and hence it takes less time. But when we do a strongly consistent read then dynamo db takes the copy of item from all the three regions and makes them agree on a single copy of that item which is needed to be the latest using the Paxos Algorithm.

Apart from all these concepts Amazon DynamoDB also provides various other features like Point in time recovery, Global Tables, Transaction, DAX and many more. One can also design efficient and scalable NoSQL tables using the concepts like GSI Overloading, GSI Sharding and many others.

If you want to dive deep into Amazon DynamoDB and learn about all the above concepts and more then you can follow this link Deep dive into Dynamo DB.

Through this article I am not conveying that:

*"NoSQL Database will always scale well for every type of data and it's the only solution when one need to scale his/her database"*

SQL Database can also scale well when used efficiently. It totally depends upon your product and the structure of data you need to store. It can vary from scenarios to scenarios.
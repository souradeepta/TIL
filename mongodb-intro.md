# Introduction to MongoDB

MongoDB offers both a *Community* and an *Enterprise* version of the database

## Document Database

A record in MongoDB is a document, which is a data structure composed of field and value pairs. MongoDB documents are similar to JSON objects. The values of fields may include other documents, arrays, and arrays of documents.

![A MongoDB document.](https://docs.mongodb.com/manual/_images/crud-annotated-document.bakedsvg.svg)

The advantages of using documents are:

- Documents (i.e. objects) correspond to native data types in many programming languages.
- Embedded documents and arrays reduce need for expensive joins.
- Dynamic schema supports fluent polymorphism.

### Collections/Views/On-Demand Materialized Views

MongoDB stores documents in [collections](https://docs.mongodb.com/manual/core/databases-and-collections/#collections). Collections are analogous to tables in relational databases.

## Key Features

### High Performance

MongoDB provides high performance data persistence. In particular,

- Support for embedded data models reduces I/O activity on database system.
- Indexes support faster queries and can include keys from embedded documents and arrays.



### Rich Query Language

MongoDB supports a rich query language to support [read and write operations (CRUD)](https://docs.mongodb.com/manual/crud/) as well as:

- [Data Aggregation](https://docs.mongodb.com/manual/core/aggregation-pipeline/)
- [Text Search](https://docs.mongodb.com/manual/text-search/) and [Geospatial Queries](https://docs.mongodb.com/manual/tutorial/geospatial-tutorial/).

### High Availability

MongoDB’s replication facility, called [replica set](https://docs.mongodb.com/manual/replication/), provides:

- *automatic* failover
- data redundancy.

A [replica set](https://docs.mongodb.com/manual/replication/) is a group of MongoDB servers that maintain the same data set, providing redundancy and increasing data availability.

### Horizontal Scalability

MongoDB provides horizontal scalability as part of its *core* functionality:

- [Sharding](https://docs.mongodb.com/manual/sharding/#sharding-introduction) distributes data across a cluster of machines.
- Starting in 3.4, MongoDB supports creating [zones](https://docs.mongodb.com/manual/core/zone-sharding/#zone-sharding) of data based on the [shard key](https://docs.mongodb.com/manual/reference/glossary/#term-shard-key). In a balanced cluster, MongoDB directs reads and writes covered by a zone only to those shards inside the zone. See the [Zones](https://docs.mongodb.com/manual/core/zone-sharding/#zone-sharding) manual page for more information.

### Support for Multiple Storage Engines

MongoDB supports [multiple storage engines](https://docs.mongodb.com/manual/core/storage-engines/):

- [WiredTiger Storage Engine](https://docs.mongodb.com/manual/core/wiredtiger/) (including support for [Encryption at Rest](https://docs.mongodb.com/manual/core/security-encryption-at-rest/))
- [In-Memory Storage Engine](https://docs.mongodb.com/manual/core/inmemory/).

In addition, MongoDB provides pluggable storage engine API that allows third parties to develop storage engines for MongoDB.
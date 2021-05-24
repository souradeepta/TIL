# SQL to MongoDB Mapping Chart



n addition to the charts that follow, you might want to consider the [Frequently Asked Questions](https://docs.mongodb.com/manual/faq/) section for a selection of common questions about MongoDB.

## Terminology and Concepts

The following table presents the various SQL terminology and concepts and the corresponding MongoDB terminology and concepts.

| SQL Terms/Concepts                                           | MongoDB Terms/Concepts                                       |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| database                                                     | [database](https://docs.mongodb.com/manual/reference/glossary/#term-database) |
| table                                                        | [collection](https://docs.mongodb.com/manual/reference/glossary/#term-collection) |
| row                                                          | [document](https://docs.mongodb.com/manual/reference/glossary/#term-document) or [BSON](https://docs.mongodb.com/manual/reference/glossary/#term-bson) document |
| column                                                       | [field](https://docs.mongodb.com/manual/reference/glossary/#term-field) |
| index                                                        | [index](https://docs.mongodb.com/manual/reference/glossary/#term-index) |
| table joins                                                  | [`$lookup`](https://docs.mongodb.com/manual/reference/operator/aggregation/lookup/#pipe._S_lookup), embedded documents |
| primary key       Specify any unique column or column combination as primary key. | [primary key](https://docs.mongodb.com/manual/reference/glossary/#term-primary-key)        In MongoDB, the primary key is automatically set to the [_id](https://docs.mongodb.com/manual/reference/glossary/#term-id) field. |
| aggregation (e.g. group by)                                  | aggregation pipeline. See the [SQL to Aggregation Mapping Chart](https://docs.mongodb.com/manual/reference/sql-aggregation-comparison/). |
| SELECT INTO NEW_TABLE                                        | [`$out`](https://docs.mongodb.com/manual/reference/operator/aggregation/out/#pipe._S_out)See the [SQL to Aggregation Mapping Chart](https://docs.mongodb.com/manual/reference/sql-aggregation-comparison/). |
| MERGE INTO TABLE                                             | [`$merge`](https://docs.mongodb.com/manual/reference/operator/aggregation/merge/#pipe._S_merge) (Available starting in MongoDB 4.2)See the [SQL to Aggregation Mapping Chart](https://docs.mongodb.com/manual/reference/sql-aggregation-comparison/). |
| UNION ALL                                                    | [`$unionWith`](https://docs.mongodb.com/manual/reference/operator/aggregation/unionWith/#pipe._S_unionWith) (Available starting in MongoDB 4.4) |
| transactions                                                 | [transactions](https://docs.mongodb.com/manual/core/transactions/)TIP: For many scenarios, the [denormalized data model (embedded documents and arrays)](https://docs.mongodb.com/manual/core/data-model-design/#data-modeling-embedding) will continue to be optimal for your data and use cases instead of multi-document transactions. That is, for many scenarios, modeling your data appropriately will minimize the need for multi-document transactions. |

## Executables

The following table presents some database executables and the corresponding MongoDB executables. This table is *not* meant to be exhaustive.

|                 | MongoDB                                                      | MySQL    | Oracle    | Informix    | DB2          |
| :-------------- | :----------------------------------------------------------- | :------- | :-------- | :---------- | :----------- |
| Database Server | [`mongod`](https://docs.mongodb.com/manual/reference/program/mongod/#bin.mongod) | `mysqld` | `oracle`  | `IDS`       | `DB2 Server` |
| Database Client | [`mongo`](https://docs.mongodb.com/manual/reference/program/mongo/#bin.mongo) | `mysql`  | `sqlplus` | `DB-Access` | `DB2 Client` |

## Examples

The following table presents the various SQL statements and the corresponding MongoDB statements. The examples in the table assume the following conditions:

- The SQL examples assume a table named `people`.

- The MongoDB examples assume a collection named `people` that contain documents of the following prototype:

  ```json
  {
    _id: ObjectId("509a8fb2f3f4948bd2f983a0"),
    user_id: "abc123",
    age: 55,
    status: 'A'
  }
  ```

### Create and Alter

The following table presents the various SQL statements related to table-level actions and the corresponding MongoDB statements.

| SQL Schema Statements                                        | MongoDB Schema Statements                                    |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| `CREATE TABLE people (    id MEDIUMINT NOT NULL        AUTO_INCREMENT,    user_id Varchar(30),    age Number,    status char(1),    PRIMARY KEY (id) ) ` | Implicitly created on first [`insertOne()`](https://docs.mongodb.com/manual/reference/method/db.collection.insertOne/#db.collection.insertOne) or [`insertMany()`](https://docs.mongodb.com/manual/reference/method/db.collection.insertMany/#db.collection.insertMany) operation. The primary key `_id` is automatically added if `_id` field is not specified.`db.people.insertOne( {    user_id: "abc123",    age: 55,    status: "A" } ) `However, you can also explicitly create a collection:`db.createCollection("people") ` |
| `ALTER TABLE people ADD join_date DATETIME `                 | Collections do not describe or enforce the structure of its documents; i.e. there is no structural alteration at the collection level.However, at the document level, [`updateMany()`](https://docs.mongodb.com/manual/reference/method/db.collection.updateMany/#db.collection.updateMany) operations can add fields to existing documents using the [`$set`](https://docs.mongodb.com/manual/reference/operator/update/set/#up._S_set) operator.`db.people.updateMany(    { },    { $set: { join_date: new Date() } } ) ` |
| `ALTER TABLE people DROP COLUMN join_date `                  | Collections do not describe or enforce the structure of its documents; i.e. there is no structural alteration at the collection level.However, at the document level, [`updateMany()`](https://docs.mongodb.com/manual/reference/method/db.collection.updateMany/#db.collection.updateMany) operations can remove fields from documents using the [`$unset`](https://docs.mongodb.com/manual/reference/operator/update/unset/#up._S_unset) operator.`db.people.updateMany(    { },    { $unset: { "join_date": "" } } ) ` |
| `CREATE INDEX idx_user_id_asc ON people(user_id) `           | `db.people.createIndex( { user_id: 1 } ) `                   |
| `CREATE INDEX       idx_user_id_asc_age_desc ON people(user_id, age DESC) ` | `db.people.createIndex( { user_id: 1, age: -1 } ) `          |
| `DROP TABLE people `                                         | `db.people.drop() `                                          |

For more information on the methods and operators used, see:

| [`db.collection.insertOne()`](https://docs.mongodb.com/manual/reference/method/db.collection.insertOne/#db.collection.insertOne)[`db.collection.insertMany()`](https://docs.mongodb.com/manual/reference/method/db.collection.insertMany/#db.collection.insertMany)[`db.createCollection()`](https://docs.mongodb.com/manual/reference/method/db.createCollection/#db.createCollection) | [`db.collection.updateMany()`](https://docs.mongodb.com/manual/reference/method/db.collection.updateMany/#db.collection.updateMany)[`db.collection.createIndex()`](https://docs.mongodb.com/manual/reference/method/db.collection.createIndex/#db.collection.createIndex)[`db.collection.drop()`](https://docs.mongodb.com/manual/reference/method/db.collection.drop/#db.collection.drop) | [`$set`](https://docs.mongodb.com/manual/reference/operator/update/set/#up._S_set)[`$unset`](https://docs.mongodb.com/manual/reference/operator/update/unset/#up._S_unset) |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
|                                                              |                                                              |                                                              |

SEE ALSO

- [Databases and Collections](https://docs.mongodb.com/manual/core/databases-and-collections/)
- [Documents](https://docs.mongodb.com/manual/core/document/)
- [Indexes](https://docs.mongodb.com/manual/indexes/)
- [Data Modeling Concepts](https://docs.mongodb.com/manual/core/data-models/).

### Insert

The following table presents the various SQL statements related to inserting records into tables and the corresponding MongoDB statements.

| SQL INSERT Statements                                        | MongoDB insertOne() Statements                               |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| `INSERT INTO people(user_id,                  age,                  status) VALUES ("bcd001",        45,        "A") ` | `db.people.insertOne(   { user_id: "bcd001", age: 45, status: "A" } ) ` |

For more information, see [`db.collection.insertOne()`](https://docs.mongodb.com/manual/reference/method/db.collection.insertOne/#db.collection.insertOne).

SEE ALSO

- [Insert Documents](https://docs.mongodb.com/manual/tutorial/insert-documents/)
- [`db.collection.insertMany()`](https://docs.mongodb.com/manual/reference/method/db.collection.insertMany/#db.collection.insertMany)
- [Databases and Collections](https://docs.mongodb.com/manual/core/databases-and-collections/)
- [Documents](https://docs.mongodb.com/manual/core/document/)

### Select

The following table presents the various SQL statements related to reading records from tables and the corresponding MongoDB statements.

NOTE

The [`find()`](https://docs.mongodb.com/manual/reference/method/db.collection.find/#db.collection.find) method always includes the `_id` field in the returned documents unless specifically excluded through [projection](https://docs.mongodb.com/manual/tutorial/project-fields-from-query-results/#projection). Some of the SQL queries below may include an `_id` field to reflect this, even if the field is not included in the corresponding [`find()`](https://docs.mongodb.com/manual/reference/method/db.collection.find/#db.collection.find) query.

| SQL SELECT Statements                                        | MongoDB find() Statements                                    |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| `SELECT * FROM people `                                      | `db.people.find() `                                          |
| `SELECT id,       user_id,       status FROM people `        | `db.people.find(    { },    { user_id: 1, status: 1 } ) `    |
| `SELECT user_id, status FROM people `                        | `db.people.find(    { },    { user_id: 1, status: 1, _id: 0 } ) ` |
| `SELECT * FROM people WHERE status = "A" `                   | `db.people.find(    { status: "A" } ) `                      |
| `SELECT user_id, status FROM people WHERE status = "A" `     | `db.people.find(    { status: "A" },    { user_id: 1, status: 1, _id: 0 } ) ` |
| `SELECT * FROM people WHERE status != "A" `                  | `db.people.find(    { status: { $ne: "A" } } ) `             |
| `SELECT * FROM people WHERE status = "A" AND age = 50 `      | `db.people.find(    { status: "A",      age: 50 } ) `        |
| `SELECT * FROM people WHERE status = "A" OR age = 50 `       | `db.people.find(    { $or: [ { status: "A" } , { age: 50 } ] } ) ` |
| `SELECT * FROM people WHERE age > 25 `                       | `db.people.find(    { age: { $gt: 25 } } ) `                 |
| `SELECT * FROM people WHERE age < 25 `                       | `db.people.find(   { age: { $lt: 25 } } ) `                  |
| `SELECT * FROM people WHERE age > 25 AND   age <= 50 `       | `db.people.find(   { age: { $gt: 25, $lte: 50 } } ) `        |
| `SELECT * FROM people WHERE user_id like "%bc%" `            | `db.people.find( { user_id: /bc/ } ) `-or-`db.people.find( { user_id: { $regex: /bc/ } } ) ` |
| `SELECT * FROM people WHERE user_id like "bc%" `             | `db.people.find( { user_id: /^bc/ } ) `-or-`db.people.find( { user_id: { $regex: /^bc/ } } ) ` |
| `SELECT * FROM people WHERE status = "A" ORDER BY user_id ASC ` | `db.people.find( { status: "A" } ).sort( { user_id: 1 } ) `  |
| `SELECT * FROM people WHERE status = "A" ORDER BY user_id DESC ` | `db.people.find( { status: "A" } ).sort( { user_id: -1 } ) ` |
| `SELECT COUNT(*) FROM people `                               | `db.people.count() `*or*`db.people.find().count() `          |
| `SELECT COUNT(user_id) FROM people `                         | `db.people.count( { user_id: { $exists: true } } ) `*or*`db.people.find( { user_id: { $exists: true } } ).count() ` |
| `SELECT COUNT(*) FROM people WHERE age > 30 `                | `db.people.count( { age: { $gt: 30 } } ) `*or*`db.people.find( { age: { $gt: 30 } } ).count() ` |
| `SELECT DISTINCT(status) FROM people `                       | `db.people.aggregate( [ { $group : { _id : "$status" } } ] ) `or, for distinct value sets that do not exceed the [BSON size limit](https://docs.mongodb.com/manual/reference/limits/#limit-bson-document-size)`db.people.distinct( "status" ) ` |
| `SELECT * FROM people LIMIT 1 `                              | `db.people.findOne() `*or*`db.people.find().limit(1) `       |
| `SELECT * FROM people LIMIT 5 SKIP 10 `                      | `db.people.find().limit(5).skip(10) `                        |
| `EXPLAIN SELECT * FROM people WHERE status = "A" `           | `db.people.find( { status: "A" } ).explain() `               |

The [`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html) statement provides information about how MySQL executes statements:

- [`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html) works with [`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/select.html), [`DELETE`](https://dev.mysql.com/doc/refman/5.7/en/delete.html), [`INSERT`](https://dev.mysql.com/doc/refman/5.7/en/insert.html), [`REPLACE`](https://dev.mysql.com/doc/refman/5.7/en/replace.html), and [`UPDATE`](https://dev.mysql.com/doc/refman/5.7/en/update.html) statements.

For more information on the methods and operators used, see

| [`db.collection.find()`](https://docs.mongodb.com/manual/reference/method/db.collection.find/#db.collection.find)[`db.collection.distinct()`](https://docs.mongodb.com/manual/reference/method/db.collection.distinct/#db.collection.distinct)[`db.collection.findOne()`](https://docs.mongodb.com/manual/reference/method/db.collection.findOne/#db.collection.findOne)[`limit()`](https://docs.mongodb.com/manual/reference/method/cursor.limit/#cursor.limit)[`skip()`](https://docs.mongodb.com/manual/reference/method/cursor.skip/#cursor.skip)[`explain()`](https://docs.mongodb.com/manual/reference/method/cursor.explain/#cursor.explain)[`sort()`](https://docs.mongodb.com/manual/reference/method/cursor.sort/#cursor.sort)[`count()`](https://docs.mongodb.com/manual/reference/method/cursor.count/#cursor.count) | [`$ne`](https://docs.mongodb.com/manual/reference/operator/query/ne/#op._S_ne)[`$and`](https://docs.mongodb.com/manual/reference/operator/query/and/#op._S_and)[`$or`](https://docs.mongodb.com/manual/reference/operator/query/or/#op._S_or)[`$gt`](https://docs.mongodb.com/manual/reference/operator/query/gt/#op._S_gt)[`$lt`](https://docs.mongodb.com/manual/reference/operator/query/lt/#op._S_lt)[`$exists`](https://docs.mongodb.com/manual/reference/operator/query/exists/#op._S_exists)[`$lte`](https://docs.mongodb.com/manual/reference/operator/query/lte/#op._S_lte)[`$regex`](https://docs.mongodb.com/manual/reference/operator/query/regex/#op._S_regex) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
|                                                              |                                                              |

SEE ALSO

- [Query Documents](https://docs.mongodb.com/manual/tutorial/query-documents/)
- [Query and Projection Operators](https://docs.mongodb.com/manual/reference/operator/query/)
- [mongo Shell Methods](https://docs.mongodb.com/manual/reference/method/)

### Update Records

The following table presents the various SQL statements related to updating existing records in tables and the corresponding MongoDB statements.

| SQL Update Statements                                 | MongoDB updateMany() Statements                              |
| :---------------------------------------------------- | :----------------------------------------------------------- |
| `UPDATE people SET status = "C" WHERE age > 25 `      | `db.people.updateMany(   { age: { $gt: 25 } },   { $set: { status: "C" } } ) ` |
| `UPDATE people SET age = age + 3 WHERE status = "A" ` | `db.people.updateMany(   { status: "A" } ,   { $inc: { age: 3 } } ) ` |

For more information on the method and operators used in the examples, see:

[`db.collection.updateMany()`](https://docs.mongodb.com/manual/reference/method/db.collection.updateMany/#db.collection.updateMany)[`$gt`](https://docs.mongodb.com/manual/reference/operator/query/gt/#op._S_gt)[`$set`](https://docs.mongodb.com/manual/reference/operator/update/set/#up._S_set)[`$inc`](https://docs.mongodb.com/manual/reference/operator/update/inc/#up._S_inc)

SEE ALSO

- [Update Documents](https://docs.mongodb.com/manual/tutorial/update-documents/)
- [Update Operators](https://docs.mongodb.com/manual/reference/operator/update/)
- [`db.collection.updateOne()`](https://docs.mongodb.com/manual/reference/method/db.collection.updateOne/#db.collection.updateOne)
- [`db.collection.replaceOne()`](https://docs.mongodb.com/manual/reference/method/db.collection.replaceOne/#db.collection.replaceOne)

### Delete Records

The following table presents the various SQL statements related to deleting records from tables and the corresponding MongoDB statements.

| SQL Delete Statements                    | MongoDB deleteMany() Statements            |
| :--------------------------------------- | :----------------------------------------- |
| `DELETE FROM people WHERE status = "D" ` | `db.people.deleteMany( { status: "D" } ) ` |
| `DELETE FROM people `                    | `db.people.deleteMany({}) `                |

For more information, see [`db.collection.deleteMany()`](https://docs.mongodb.com/manual/reference/method/db.collection.deleteMany/#db.collection.deleteMany).

SEE ALSO

- [Delete Documents](https://docs.mongodb.com/manual/tutorial/remove-documents/)
- [`db.collection.deleteOne()`](https://docs.mongodb.com/manual/reference/method/db.collection.deleteOne/#db.collection.deleteOne)

## Further Reading

If you are considering migrating your SQL application to MongoDB, download the [MongoDB Application Modernization Guide](https://www.mongodb.com/modernize?tck=docs_server).

The download includes the following resources:

- Presentation on the methodology of data modeling with MongoDB
- White paper covering best practices and considerations for migrating to MongoDB from an [RDBMS](https://docs.mongodb.com/manual/reference/glossary/#term-rdbms) data model
- Reference MongoDB schema with its RDBMS equivalent
- Application Modernization scorecard
# MongoDB Tutorial



## MongoDB - Create a Database

In MongoDB, you create a database by switching to a non-existent database, then inserting data into it.

There is no `CREATE DATABASE` statement in MongoDB like there is in [SQL](https://www.quackit.com/sql/tutorial/sql_create_database.cfm). To create a database in MongoDB, simply switch to a non-existent database, then insert data into it.

To switch databases, run the `use` statement. If the database doesn't already exist, it will be created:

```json
use music
```

This results in the following message:

```json
switched to db music
```

However, the database isn't actually created until you insert data into it:

```json
db.artists.insert({ artistname: "The Tea Party" })
```

The above statement creates a collection and inserts a document into it.

It will generate the following message:

```
WriteResult({ "nInserted" : 1 })
```

You can see the database in your list of databases by issuing the following command:

```
show databases
```

Here's an example of the output:

```
local  0.000GB
music  0.000GB
test   0.005GB
```

In this case, three databases are displayed, one of which is our newly created database (music).

You can also run the following line to view the contents of your database:

```
db.artists.find()
```

Which should result in output like this:

```
{ "_id" : ObjectId("5780fbf948ef8c6b3ffb0149"), "artistname" : "The Tea Party" }
```

As you can see, our name/value pair is now stored in the new database. MongoDB has also inserted an `_id` field. If you don't provide an `_id` field, MongoDB provides it for you.
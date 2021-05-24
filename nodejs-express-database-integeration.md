## MySQL

**Module**: [mysql](https://github.com/felixge/node-mysql/)

### Installation

```sh
$ npm install mysql
```

### Example

```javascript
var mysql = require('mysql')
var connection = mysql.createConnection({
  host: 'localhost',
  user: 'dbuser',
  password: 's3kreee7',
  database: 'my_db'
})

connection.connect()

connection.query('SELECT 1 + 1 AS solution', function (err, rows, fields) {
  if (err) throw err

  console.log('The solution is: ', rows[0].solution)
})

connection.end()
```

## MongoDB

**Module**: [mongodb](https://github.com/mongodb/node-mongodb-native)

### Installation

```sh
$ npm install mongodb
```

### Example (v2.*)

```javascript
var MongoClient = require('mongodb').MongoClient

MongoClient.connect('mongodb://localhost:27017/animals', function (err, db) {
  if (err) throw err

  db.collection('mammals').find().toArray(function (err, result) {
    if (err) throw err

    console.log(result)
  })
})
```

### Example (v3.*)

```javascript
var MongoClient = require('mongodb').MongoClient

MongoClient.connect('mongodb://localhost:27017/animals', function (err, client) {
  if (err) throw err

  var db = client.db('animals')

  db.collection('mammals').find().toArray(function (err, result) {
    if (err) throw err

    console.log(result)
  })
})
```

If you want an object model driver for MongoDB, look at [Mongoose](https://github.com/LearnBoost/mongoose).

## Redis

**Module**: [redis](https://github.com/mranney/node_redis)

### Installation

```sh
$ npm install redis
```

### Example

```javascript
var redis = require('redis')
var client = redis.createClient()

client.on('error', function (err) {
  console.log('Error ' + err)
})

client.set('string key', 'string val', redis.print)
client.hset('hash key', 'hashtest 1', 'some value', redis.print)
client.hset(['hash key', 'hashtest 2', 'some other value'], redis.print)

client.hkeys('hash key', function (err, replies) {
  console.log(replies.length + ' replies:')

  replies.forEach(function (reply, i) {
    console.log('    ' + i + ': ' + reply)
  })

  client.quit()
})
```

## SQL Server

**Module**: [tedious](https://github.com/tediousjs/tedious)

### Installation

```sh
$ npm install tedious
```

### Example

```javascript
var Connection = require('tedious').Connection
var Request = require('tedious').Request

var config = {
  server: 'localhost',
  authentication: {
    type: 'default',
    options: {
      userName: 'your_username', // update me
      password: 'your_password' // update me
    }
  }
}

var connection = new Connection(config)

connection.on('connect', function (err) {
  if (err) {
    console.log(err)
  } else {
    executeStatement()
  }
})

function executeStatement () {
  request = new Request("select 123, 'hello world'", function (err, rowCount) {
    if (err) {
      console.log(err)
    } else {
      console.log(rowCount + ' rows')
    }
    connection.close()
  })

  request.on('row', function (columns) {
    columns.forEach(function (column) {
      if (column.value === null) {
        console.log('NULL')
      } else {
        console.log(column.value)
      }
    })
  })

  connection.execSql(request)
}
```
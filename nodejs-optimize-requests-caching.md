# How to Optimize Node Requests with Simple Caching Strategies

One of the things that affect how users interact with our applications is its speed. Even though some users generally have a poor connection, they are expecting some minimum level of speed when using the application. To give our users a seamless experience, we are going to consider the possibility of optimizing our requests in node by using the concept of caching.

Trust me, your users will thank you.

In this article, we are going to look at how to optimize requests in our Node applications by caching responses to requests. Now, you may be wondering what caching is all about. Caching generally involves storing and retrieving data from a high-performance memory

### Table of Contents

- [Why Use A Cache](https://scotch.io/tutorials/how-to-optimize-node-requests-with-simple-caching-strategies#toc-why-use-a-cache)
- [Requirements](https://scotch.io/tutorials/how-to-optimize-node-requests-with-simple-caching-strategies#toc-requirements)
- [Building A Basic Inventory Application](https://scotch.io/tutorials/how-to-optimize-node-requests-with-simple-caching-strategies#toc-building-a-basic-inventory-application)
- [Different Caching Options](https://scotch.io/tutorials/how-to-optimize-node-requests-with-simple-caching-strategies#toc-different-caching-options)
- [Results and Recommendation](https://scotch.io/tutorials/how-to-optimize-node-requests-with-simple-caching-strategies#toc-results-and-recommendation)
- [Conclusion](https://scotch.io/tutorials/how-to-optimize-node-requests-with-simple-caching-strategies#toc-conclusion)

## [Why Use A Cache](https://scotch.io/tutorials/how-to-optimize-node-requests-with-simple-caching-strategies#toc-why-use-a-cache)

The goal of caching is to save your server the stress of having to do the same thing over and over again that results in the same output. Let’s take for example, you have a page that generates a report of a companies inventory for the previous day for accountability. Now, let’s imagine that the report page will be viewed at least a thousand times daily by different auditors. Generating a single report wouldn’t be a problem, but if the same report is going to be generated every single time a thousand times, then you need to look for a better way to do it and that’s where caching comes in.

## [Requirements](https://scotch.io/tutorials/how-to-optimize-node-requests-with-simple-caching-strategies#toc-requirements)

To enable you to follow through the article, you need to have the following :

- [Node](https://nodejs.org/) installed on your machine,
- Node Package Manager ( [NPM](https://www.npmjs.com/) ) installed on your machine
- Some Basic Javascript Knowledge

To confirm your installations, run the following in your terminal :

```bash
    node --version
    npm --version
```

If you get version numbers as result, then you’re good to go.

## [Building A Basic Inventory Application](https://scotch.io/tutorials/how-to-optimize-node-requests-with-simple-caching-strategies#toc-building-a-basic-inventory-application)

We need to make a simple inventory reporting application to test caching capabilities with. The main goal is to look at the different approaches to caching in our node applications.

Since you already have `npm` installed on your machine, create a new project folder and install the necessary packages :

```bash
    mkdir inventory-app
    cd inventory-app 
    npm init && npm install --save express memory-cache flat-cache redis sqlite3
```

Let’s take a look at the `index.js`.

Import express and sqlite:

```js
    // index.js

    const express = require('express')
    const sqlite3 = require('sqlite3').verbose();
    const PORT = process.env.PORT || 3128 ;
    const app = express();

    [...]
```

`express` is a routing framework for Node which simplifies HTTP task. `sqlite3` will power the database for this app. The application port is also specified and an express application is created.

Next thing we need to do is to create the server route. Just one route that points to a `/products` endpoint:

```js
    // index.js
    [...]

    app.get('/products',  function(req, res){
        setTimeout(() => {
          let db = new sqlite3.Database('./NodeInventory.db');
          let sql = `SELECT * FROM products`;

          db.all(sql, [], (err, rows) => {
              if (err) {
                  throw err;
              }
              db.close();
              res.send( rows );
          });
          // this was wrapped in a setTimeout function to intentionally simulate a slow 
          // request
        }, 3000);
    });

    [...]
```

When a request is made to the `/products` route, a connection is made to our sqlite database and then the query’s executed to fetch all our products. Once this is done, the connection is closed and the response is sent back to the user with all the products that exist in our inventory.

> The `sqlite` file is in the [Github](https://github.com/christiannwamba/inventory-app) repository for reference.

To start listening to requests on the port, add the following at the end:

```js
    // index.js
    [...]

    app.listen(PORT, function(){
        console.log(`App running on port ${PORT}`);
    });

    [...]
```

Head to Postman or the browser and visit the `/products` route to get a list of the items stored in the database:

![First run without caching gave a response time of 3043ms](https://d2mxuefqeaa7sj.cloudfront.net/s_4D2B5F611F0674CED43ABA807069FB343D6C1AC5431149683C24F112350911DB_1522325607309_nocaching.png)

So far our single-route app works, but imagine having to do this over and over again which is what will be the case when our daily inventory is being reviewed over a hundred times. Let’s take a look at how to optimize requests by making use of some caching functions

## [Different Caching Options](https://scotch.io/tutorials/how-to-optimize-node-requests-with-simple-caching-strategies#toc-different-caching-options)

**Using** `Memory-Cache` **for caching ( In - Memory )** Earlier on, the `memory-cache` module was installed. Now, let’s look at how to use it.

Import the module:

```js
    // index.js
    [...]

    cache cache = require('memory-cache');

    [...]
```

Create and configure cache middleware :

```js
    // index.js

    // configure cache middleware
    let memCache = new cache.Cache();
    let cacheMiddleware = (duration) => {
        return (req, res, next) => {
            let key =  '__express__' + req.originalUrl || req.url
            let cacheContent = memCache.get(key);
            if(cacheContent){
                res.send( cacheContent );
                return
            }else{
                res.sendResponse = res.send
                res.send = (body) => {
                    memCache.put(key,body,duration*1000);
                    res.sendResponse(body)
                }
                next()
            }
        }
    }

    // app routes
    [...]
```

The duration representing how long the values need to stored in the cache will be specified. In the middleware, a unique `key` based on the `request url` is generated and a check is made to see if there is already content stored for that `key`. If content exists, the data is sent back as the response without having to make the extra query to our database.

Now, if there is no content in the cache for the particular key, the request is processed as usual and the result of the request is stored in our cache before the response is sent to the user.

To use the `cacheMiddleware` edit the `index.js` to look like this :

```js
    // index.js

    app.get('/products', cacheMiddleware(30), function(req, res){
         [...]    
    });
```

> Notice how specified the duration we want the response for the request to be cached for was specified

We just looked at in-memory method for caching responses. One of the downsides of this method is that once the server goes down, all cached content is lost.

![Returns Products with a response time of 3045ms](https://d2mxuefqeaa7sj.cloudfront.net/s_4D2B5F611F0674CED43ABA807069FB343D6C1AC5431149683C24F112350911DB_1522325458838_inmemorypic.png)

Now, let's take a look at the using a caching method that persists the data on the server even after it has been restarted.

**Using Flat file for caching** We are going to now take a look at how to use files to persist our cached data on the server. To do this, we are going to make use of this package - [flat-cache](https://www.npmjs.com/package/flat-cache)

Install the module by running :

```bash
    npm install --save flat-cache
```

Now, tweak the `index.js` to make use of our new module. The `index.js` will look like this :

```js
    // index.js
    [...]

    const flatCache = require('flat-cache')

    [...]
```

Then the new cache is loaded

```js
    // index.js 
    [...]

    // load new cache
    let cache = flatCache.load('productsCache');
    // optionally, you can go ahead and pass the directory you want your
    // cache to be loaded from by using this
    // let cache = flatCache.load('productsCache', path.resolve('./path/to/folder')

    [...]
```

Now that the cache has been loaded, the next thing to do now is to edit our application route so :

```js
    // index.js
    [...]

    // create flat cache routes
    let flatCacheMiddleware = (req,res, next) => {
        let key =  '__express__' + req.originalUrl || req.url
        let cacheContent = cache.getKey(key);
        if( cacheContent){
            res.send( cacheContent );
        }else{
            res.sendResponse = res.send
            res.send = (body) => {
                cache.setKey(key,body);
                cache.save();
                res.sendResponse(body)
            }
            next()
        }
    };

    // create app routes 

    [...]
```

The logic behind this is similar to the in-memory cache. Check if a response already exists for our request using the `key` and it there already exists a response, it is returned to the user else, the request is executed and the response is saved to the cache before going to the user.

> Notice that we ran the `cache.save()` . Doing this specifies that the cache is saved as a file in the directory we specified - if you specified any. If no directory was specified the module will determine where to save the ‘cache-file’

To use the `flatCacheMiddleware` , edit the application routes so :

```js
    // index.js
    [...]

    // create app routes

    app.get('/products', flatCacheMiddleware, function(req, res){
      [...]
    });
```

Now, when the `/products` route is visited, the following result is obtained :

![Products are returned, with a response time of  3047ms](https://d2mxuefqeaa7sj.cloudfront.net/s_4D2B5F611F0674CED43ABA807069FB343D6C1AC5431149683C24F112350911DB_1522325083007_cache2file.png)

A `productsCache` file is also created with the following content :

```
    {"__express__/products":"[{\"ID\":1,\"NAME\":\"Shoes\",\"COUNT\":20},{\"ID\":2,\"NAME\":\"Bags\",\"COUNT\":12},{\"ID\":3,\"NAME\":\"Tables\",\"COUNT\":50},{\"ID\":5,\"NAME\":\"Laptop\",\"COUNT\":140},{\"ID\":6,\"NAME\":\"Chair\",\"COUNT\":200},{\"ID\":7,\"NAME\":\"Mouse\",\"COUNT\":20},{\"ID\":8,\"NAME\":\"Pen\",\"COUNT\":1000},{\"ID\":9,\"NAME\":\"Chips\",\"COUNT\":6303},{\"ID\":10,\"NAME\":\"Slides\",\"COUNT\":22},{\"ID\":11,\"NAME\":\"Console\",\"COUNT\":32}]"}
```

**Using MemCached ( A Service )** Another option to consider for caching is [memcached](https://github.com/3rd-Eden/memcached). `Memcached` is a caching client built for node JS with scaling in mind.

To use the `Memcached` node client, you need to have `memcached` installed on your machine. Head over [here](https://github.com/memcached/memcached/wiki/Install) to get it installed. When you have it installed, you then install the `memcached` node client by running :

```bash
    npm install --save memcached
```

Now that the client is installed, import the module in the `index.js`:

```js
    // index.js 
    [...]

    const Memcached = require('memcached');

    [...]
```

Now, go ahead and configure the `memcachedMiddleware` by tweaking the `index.js` to look as follows :

```js
    // index.js
    [...]

    let memcached = new Memcached("127.0.0.1:11211")

    let memcachedMiddleware = (duration) => {
        return  (req,res,next) => {
        let key = "__express__" + req.originalUrl || req.url;
        memcached.get(key, function(err,data){
            if(data){
                res.send(data);
                return;
            }else{
                res.sendResponse = res.send;
                res.send = (body) => {
                    memcached.set(key, body, (duration*60), function(err){
                        // 
                    });
                    res.sendResponse(body);
                }
                next();
            }
        });
    }
    };

    [...]
```

A `memcached` object is created and the object is connected to the PORT our Memcached instance is running on - in this case, `11211`. Afterwards, a `memcachedMiddleware` is also created which is similar to the other middlewares created earlier. If there is no content for the specified key in the cache, the request is completed as usual and the response is stored in our cache. If there is a response for the specified key, the content is obtained from the cache and returned to the user without having to process the request.

Now, edit the `/products` route and update it to use the `memcachedMiddleware` :

```js
    // index.js
    [...]

    app.get("/products", memcachedMiddleware(20), function(req, res) {
      [...]
    });

    [...]
```

The `memcachedMiddleware(20)` above, specifies the duration ( in minutes ) for how long the `key` is to be stored in the cache.

When a request is made to the `/products` route from Postman, the following result is obtained :

![Returns the products with a response time of 3049ms](https://d2mxuefqeaa7sj.cloudfront.net/s_4D2B5F611F0674CED43ABA807069FB343D6C1AC5431149683C24F112350911DB_1522334488251_memcached.png)

**Using Redis for caching** Now, we have seen how to use `in-memory` cache and also how to persist our caches using files. Let’s take a look at using `redis` for caching in our application. Redis stands for `**RE**``mote` `**D**``ictionary` `**S**``erver` that has the ability to store and manipulate high-level data types.

To get started using redis for caching application, install the redis node client by running :

```bash
    npm install -save redis
```

We also need to have a redis server running on our local machine. You can head over [here](https://redis.io/topics/quickstart) to find out how.

When you are sure your redis server is installed and working properly, the next thing to do is to import the necessary modules and create your redis client:

```js
    // index.js
    [...]

    const redis = require('redis')
    const client = redis.createClient();

    [...]
```

After this, create a `redisMiddleware` and that’ll look like this :

```js
    // index.js
    [...]

    // create redis middleware
    let redisMiddleware = (req, res, next) => {
      let key = "__expIress__" + req.originalUrl || req.url;
      client.get(key, function(err, reply){
        if(reply){
            res.send(reply);
        }else{
            res.sendResponse = res.send;
            res.send = (body) => {
                client.set(key, JSON.stringify(body));
                res.sendResponse(body);
            }
            next();
        }
      });
    };

    // app routes
    [...]
```

A check is made to see if a `response` already exists for our request. If a response exists, then we fetch the value from the cache and return the response. If no response exists in the cache, then the request is executed the response is stored in the redis cache for future requests.

The `/products` route is also updated to use the `redisMiddleware` :

```js
    // index.js 
    [...]

    app.get("/products", redisMiddleware, function(req, res) {
      [...]
    });

    // set port
    [...]
```

When a request to the `/products` server, you get :

![Returns the products with a response time of 3049ms](https://d2mxuefqeaa7sj.cloudfront.net/s_4D2B5F611F0674CED43ABA807069FB343D6C1AC5431149683C24F112350911DB_1522324606694_redis_server_first.png)

## [Results and Recommendation](https://scotch.io/tutorials/how-to-optimize-node-requests-with-simple-caching-strategies#toc-results-and-recommendation)

Now, we are going to compare performance for the different approaches we have taken in this article :

- No Caching at all
- In-Memory Caching
- Caching to File
- Using Memcached
- Using Redis for Caching

- **No Caching at All**

| **Run**      | **Response Time** |
| :----------- | :---------------- |
| Ist Run      | 3043ms            |
| 2nd Run      | 3027ms            |
| 3rd Run      | 3019ms            |
| 4th Run      | 3009ms            |
| **Avg Time** | 3024.5ms          |

- **In-Memory Caching**

| **Run**      | **Response Time** |
| :----------- | :---------------- |
| Ist Run      | 3045ms            |
| 2nd Run      | 23ms              |
| 3rd Run      | 4ms               |
| 4th Run      | 17ms              |
| **Avg Time** | 772.25ms          |

- **Caching To File**

| **Run**      | **Response Time** |
| :----------- | :---------------- |
| Ist Run      | 3047ms            |
| 2nd Run      | 31ms              |
| 3rd Run      | 8ms               |
| 4th Run      | 13ms              |
| **Avg Time** | 774.75ms          |

- **Using MemCached**

| **Run**      | **Response Time** |
| :----------- | :---------------- |
| Ist Run      | 3049ms            |
| 2nd Run      | 9ms               |
| 3rd Run      | 10ms              |
| 4th Run      | 16ms              |
| **Avg Time** | 771ms             |

- **Using Redis for Caching**

| **Run**      | **Response Time** |
| :----------- | :---------------- |
| Ist Run      | 3049ms            |
| 2nd Run      | 6ms               |
| 3rd Run      | 18ms              |
| 4th Run      | 10ms              |
| **Avg Time** | 770.75ms          |

From the results above, we can see that deciding not caching the files at all does not help improve the response time. The different caching options we have looked at so far have proven to be successful in reducing the average load time.

## [Conclusion](https://scotch.io/tutorials/how-to-optimize-node-requests-with-simple-caching-strategies#toc-conclusion)

Caching comes in handy a lot of times, but you need to know when to use it. it may be an overkill especially when the requests made to the application aren’t frequent. Also, `POST`, `PUT` and `DELETE` methods should never be cached because you want unique resources to be affected each time a request is made.

Here’s a link to the [GitHub](https://github.com/christiannwamba/inventory-app) repository. Feel free to leave your comments and questions below
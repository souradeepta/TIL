# Caching like a boss in NodeJS







From a **673ms** average response time and uncountable errors, to **29ms** response time and **0** errors. That’s the immediate result of adding a cache mechanism to my NodeJS service.

In this article I’m going to share some of the techniques I used to achieve that.

Cache is great for improving your app’s performance. It could save you expensive database, 3rd parties API calls, and server to server requests by saving a copy of previous requests’ results locally on your server.

However, caching could be tricky, and if you’re not using it right you could easily find yourself pulling your hair out while trying to debug and understand why, for example, a data object your user has been deleted from his app still appears on his dashboard (based on a true, sad, annoying, and frustrating story).





This bug made me realize a few things about cache and helped me to meditate some caching techniques which I’m going to share with you here.

# App Overview

Before I start, let me give you a quick overview about the app we’re going to cache.

The app includes 2 main entities:

1. Users
2. [Feeds](https://www.feederninja.com/)

Ultimately, this app allows users to add social feed widgets to their websites. A user may create multiple feeds, as well as, get, update, and delete feeds from his account.

We will be focusing on the “Feeds” entity, but these techniques applies to any model.

I wrote the service using [ExpressJS](https://expressjs.com/) as my MVC framework, and here’s how the **Feeds** **model** looked like before adding the cache mechanism into it:

```js
// Feeds model before cache

import DB from '../db';

const FeedModel = {

	getUserFeeds(userID) {
		const selectQuery = `SELECT * FROM feeds WHERE userID = ${userID}`;

		return DB.then((connection) =>
			connection.query(selectQuery).then((rows) => Promise.resolve(rows))
		);
	},

	getFeedById(feedID, allFields = false) {
		const selectQuery = `SELECT * FROM feeds WHERE feedID = ${feedID}`;

		return DB.then((connection) =>
			connection.query(selectQuery).then((rows) => {
				return rows[0];
			})
		);
	},

	countFeeds(userID) {
		const selectQuery = `SELECT COUNT(*) as count FROM feeds WHERE userID = ${userID}`;
		return DB.then((connection) =>
			connection.query(selectQuery).then((rows) => rows[0].count)
		);
	},

	createFeed(userID, feedData = {}) {
		const insertQuery = 'INSERT INTO feeds SET ?';
		feedData.userID = userID;

		return DB.then((connection) => {
			return connection.query(insertQuery, feedData).then(() => 
				Promise.resolve(feedData)
			);
		});
	},

	update(userID, feedID, feedData) {
		const whereQuery = `WHERE feedID = ${feedID} AND userID = ${userID}`;
		const updateQuery = `UPDATE feeds SET ? ${whereQuery}`;

		return DB.then((connection) => connection.query(updateQuery, feedData).then(() => true));
	},

	delete(userID, feedID) {
		const deleteQuery = `DELETE from feeds WHERE userID = ${userID} AND feedID = ${feedID}`;
		return DB.then((connection) => connection.query(deleteQuery).then(() => true));
	}
};

export default FeedModel;
```



<iframe src="https://medium.com/media/11bbe407382cd5719d094f1eca039eb6" allowfullscreen="" frameborder="0" height="1266" width="680" title="" class="fe et ep hn w" scrolling="auto" style="box-sizing: inherit; width: 680px; left: 0px; top: 0px; position: absolute; height: 1266px;"></iframe>

As you can see, it’s a pretty straightforward model. When the service needs to fetch/update data it’s connecting to the database (in this case — MySQL) and returning the results, meaning, each time there’s a feed impression on a website, the service calls the database. So, even if you think about 100 social feeds, and multiply it with 100 impressions (one for each website) per minute — which considers as small numbers in terms of traffic, you’ll get **10K database calls per minute** and as you know, MySQL databases are not known for their high performance.

As you may see, caching solution would be perfect here.

# Picking the right cache package

As almost every module you need in node, cache modules also have endless options. After exploring 5 different packages I chose “[**node-cache**](https://github.com/mpneuried/nodecache)” for two reasons:

1. I really liked their API (“meh” reason).
2. They had the most stars (great reason!).

Installing the package:

```
npm install node-cache --save
```

# Creating a custom cache service

Next, we’re going to create a reusable custom cache service for our app:

```js

import NodeCache from 'node-cache';

class Cache {

  constructor(ttlSeconds) {
    this.cache = new NodeCache({ stdTTL: ttlSeconds, checkperiod: ttlSeconds * 0.2, useClones: false });
  }

  get(key, storeFunction) {
    const value = this.cache.get(key);
    if (value) {
      return Promise.resolve(value);
    }

    return storeFunction().then((result) => {
      this.cache.set(key, result);
      return result;
    });
  }

  del(keys) {
    this.cache.del(keys);
  }

  delStartWith(startStr = '') {
    if (!startStr) {
      return;
    }

    const keys = this.cache.keys();
    for (const key of keys) {
      if (key.indexOf(startStr) === 0) {
        this.del(key);
      }
    }
  }

  flush() {
    this.cache.flushAll();
  }
}


export default Cache;

```



<iframe src="https://medium.com/media/c5fbf726a5358a04a6e955c8bc23cc74" allowfullscreen="" frameborder="0" height="1006" width="680" title="" class="fe et ep hn w" scrolling="auto" style="box-sizing: inherit; width: 680px; left: 0px; top: 0px; position: absolute; height: 1006px;"></iframe>

You’re probably wondering why we’re creating a **custom cache service** and not simply using the API provided by the node-cache module. Well, that’s a brilliant question, I’m glad you asked it! J

Wrapping the cache module with our own service will allow us to:

- Create a reusable service that we could use on multiple components in our app.
- “Normalize” the cache API, and add more methods our app needs and the module doesn’t provide.
- Easily replace the cache module we chose with another one (if needed).

Let’s break down the cache service a piece by piece.

**constructor**: Expect one parameter which is the TTL (Time to live), meaning, how much time this cache should be stored before being deleted.

**get**: Get a value of specific key in the store. If key exists, return the cached value. Else, get a new value from the store function, and set a new value in the cache service. The store function promises the flexibility we need when we store the value.

**set**: Set a new value for a specific key.

**del**: Delete a key from the cache.

**flush**: Delete all keys, and flush the cache.

**delStartWith**: A custom method I needed — Look for all keys starting with X, and delete them from cache.

# Initializing cache service

Now that the cache service is ready, let’s initialize it in our **feeds model**:

```js
// Initializing cache service in feeds model

import DB from '../db';
import CacheService from '../cache.service';

const ttl = 60 * 60 * 1; // cache for 1 Hour
const cache = new CacheService(ttl); // Create a new cache service instance

const FeedModel = {
  ...
};
  
export default FeedModel;

```



<iframe src="https://medium.com/media/7087a36938908932e96a1f5bb19123a9" allowfullscreen="" frameborder="0" height="320" width="680" title="" class="fe et ep hn w" scrolling="auto" style="box-sizing: inherit; width: 680px; left: 0px; top: 0px; position: absolute; height: 320px;"></iframe>

As you may see, I imported the cache service, set a TTL for one hour, and created a new **CacheService** instance.

This way provides me the option to create one CacheService for each one of the models in my app, and set them a different TTL.

# Cache keys Namespacing

The cache service is initialized and ready to use. It’s time to add it to our model, and we’ll start with the **getFeedById** method:

```js
// Feeds model after cache

import DB from '../db';
import CacheService from '../cache.service';

const ttl = 60 * 60 * 1; // cache for 1 Hour
const cache = new CacheService(ttl); // Create a new cache service instance

const FeedModel = {
  // ...

  getFeedById(feedID) {
    const selectQuery = `SELECT * FROM feeds WHERE feedID = ${feedID}`;

    return cache.get(feedID, () => DB.then((connection) =>
      connection.query(selectQuery).then((rows) => {
        return rows[0];
      })
    )).then((result) => {
      return result;
    });
  },

  // ...
};

export default FeedModel;
```



<iframe src="https://medium.com/media/7a3e99a1ea18fe8251a6e2829c93bc9e" allowfullscreen="" frameborder="0" height="622" width="680" title="" class="fe et ep hn w" scrolling="auto" style="box-sizing: inherit; width: 680px; left: 0px; top: 0px; position: absolute; height: 622px;"></iframe>

Starting with line 15 — I’m calling the “get” method in the cache service, passing it 2 parameters. The first one is the key, which I set for the feedID field, and the second one is a callback (store function) in case the key doesn’t exists or has value, where I’m making a database call to retrieve it.

The thing is, that the cache key don’t tell me anything about the value I’m trying to retrieve. Also, and more importantly — the key is not unique enough, and I could find myself rewriting it in other method.

Having said that, you should give the key a namespace.

I like to use the name of the method calling the cache key following a unique identifier. For example, in our case the key name will be `"getFeedById_${feedID}"`.

With cache, our feeds model will finally look like the following:

<iframe src="https://medium.com/media/1bedea4a481d03970c404605ae50a1fa" allowfullscreen="" frameborder="0" height="1530" width="680" title="" class="fe et ep hn w" scrolling="auto" style="box-sizing: inherit; width: 680px; left: 0px; top: 0px; position: absolute; height: 1530px;"></iframe>

Note that after updating or deleting a feed, I’m cleaning the cache for the relevant keys (lines 55, 64). Obviously, you may extend the cache service and add an update method for updating a key rather than deleting it, but remember that in worse case it will be cached again in the next request.

**A word about security** — obviously, the database calls you see in the examples above are very naive and do not considering security risks such as SQL injections (which could be easily accomplished here). My point being, **don’t** just copy & paste this code and run it in production. Use it as a guideline for adding a server-side cache mechanism to your service.

# Flush them all!

At the beginning of the article I mentioned a bug I tried to solve that triggered me to add cache mechanism to my service. That could have easily solved by clearing the service’s cache.

My last tip here will be to expose an endpoint on your server that will flush all the cache in your service so you won’t have to restart your servers. Just in case.

# Summary

So, just to recap, here’s the list of techniques we used:

- Choose the right cache package for your app.
- Create a custom cache service that wraps this package.
- Initialize one cache handler for each model in your app.
- Add a namespace to the cache key names with the name of the methods.
- Add the cache keys relevant meta data so it will be easy to clean them afterward.
- Create an endpoint in your service to flush the cache in your app.

That’s it. You’re now cache qualified, and may start **caching like a boss** your NodeJS service.
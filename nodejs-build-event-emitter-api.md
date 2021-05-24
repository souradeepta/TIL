# How to code your own event emitter in Node.js: a step-by-step guide



#### Understand Node internals by coding small packages/modules

If you are new to Node.js there are many tutorials here on Medium and elsewhere. You can check out my article [All About Core Node.JS](https://codeburst.io/all-about-core-nodejs-part-1-b9f4b0a83278), for example.

But without further ado, let's get to the topic under discussion: “Event Emitters”. Event Emitters play a very important role in the Node.js ecosystem.

The EventEmitter is a module that facilitates communication/interaction between objects in Node. EventEmitter is at the core of Node asynchronous event-driven architecture. Many of Node’s built-in modules inherit from EventEmitter including prominent frameworks like Express.js.

The concept is quite simple: emitter objects emit named events that cause previously registered listeners to be called. So, an emitter object basically has two main features:

- Emitting name events.
- Registering and unregistering listener functions.

It’s kind of like a pub/sub or observer design pattern (though not exactly).

#### What we will be building in this tutorial

- EventEmitter class
- on / addEventListener method
- off / removeEventListener method
- once method
- emit method
- rawListeners method
- listenerCount method

The above basic features are sufficient to implement a full system using the eventing model.

Before we get into the coding, let’s take a look at how we will be using the EventEmitter class. Please note that our code will mimic the exact API of the Node.js ‘events’ module.

In fact, if you replace our EventEmitter with Node.js’s built-in ‘events’ module you will get the same result.

#### Example 1 — Create an event emitter instance and register a couple of callbacks

```js
const myEmitter = new EventEmitter();

function c1() {
   console.log('an event occurred!');
}

function c2() {
   console.log('yet another event occurred!');
}

myEmitter.on('eventOne', c1); // Register for eventOne
myEmitter.on('eventOne', c2); // Register for eventOne
```

When the event ‘eventOne’ is emitted, both the above callbacks should be invoked.

```js
myEmitter.emit('eventOne');
```

The output in the console will be as follows:

```js
an event occurred!
yet another event occurred!
```

#### Example 2— Registering for the event to be fired only one time using once.

```js
myEmitter.once('eventOnce', () => console.log('eventOnce once fired'));  
```

Emitting the event ‘eventOnce’:

```js
myEmitter.emit('eventOne');
```

The following output should appear in the console:

```
eventOnce once fired
```

Emitting events registered with once again will have no impact.

```js
myEmitter.emit('eventOne');
```

Since the event was only emitted once, the above statement will have no impact.

#### Example 3— Registering for the event with callback parameters

```js
myEmitter.on('status', (code, msg)=> console.log(`Got ${code} and ${msg}`));
```

Emitting the event with parameters:

```js
myEmitter.emit('status', 200, 'ok');
```

The output in the console will be as below:

```
Got 200 and ok
```

NOTE: You can emit events multiple times (except the ones registered with the once method).

#### Example 4— Unregistering events

```js
myEmitter.off('eventOne', c1);
```

Now if you emit the event as follows, nothing will happen and it will be a noop:

```js
myEmitter.emit('eventOne');  // noop
```

#### Example 5— Getting Listener count

```js
console.log(myEmitter.listenerCount('eventOne'));
```

NOTE: If the event has been unregistered using off or removeListener method, then the count will be 0.

#### Example 6— Getting Raw Listeners

```js
console.log(myEmitter.rawListeners('eventOne'));
```

#### Example 7— Async Example demo

```js
// Example 2->Adapted and thanks to Sameer Buna
class WithTime extends EventEmitter {
  execute(asyncFunc, ...args) {
    this.emit('begin');
    console.time('execute');
    this.on('data', (data)=> console.log('got data ', data));
    asyncFunc(...args, (err, data) => {
      if (err) {
        return this.emit('error', err);
      }
      this.emit('data', data);
      console.timeEnd('execute');
      this.emit('end');
    });
  }
}
```

Using the withTime event emitter:

```js
const withTime = new WithTime();

withTime.on('begin', () => console.log('About to execute'));
withTime.on('end', () => console.log('Done with execute'));

const readFile = (url, cb) => {
  fetch(url)
    .then((resp) => resp.json()) // Transform the data into json
    .then(function(data) {
      cb(null, data);
    });
}

withTime.execute(readFile, 'https://jsonplaceholder.typicode.com/posts/1');
```

Check the output in the console. The list of posts will be displayed along with other logs.

#### The Observer Pattern for Our Event Emitter

![img](https://cdn-media-1.freecodecamp.org/images/sPkTz3OExo-FXteQwtFkoDVQmZeFfHE56-WJ)

#### Visual Diagram 1 (Methods in our EventEmitter)

![img](https://cdn-media-1.freecodecamp.org/images/plokwquvVbN4xa25kuk-QvYJ9b2ICqXWZ4Hb)

Since we now understand the usage API, let’s get to coding the module.

#### The complete boilerplate code for the EventEmitter class

We will be filling in the details incrementally in the next couple sections.

```js
class EventEmitter {
  listeners = {};  // key-value pair
  
  addListener(eventName, fn) {}
  on(eventName, fn) {}
  
  removeListener(eventName, fn) {}
  off(eventName, fn) {}
  
  once(eventName, fn) {}
  
  emit(eventName, ...args) { }
  
  listenerCount(eventName) {}
  
  rawListeners(eventName) {}
}
```

We begin by creating the template for the EventEmitter class along with a hash to store the listeners. The listeners will be stored as a key-value pair. The value could be an array (since for the same event we allow multiple listeners to be registered).

#### 1. The addListener() method

Let us now implement the addListener method. It takes in an event name and a callback function to be executed.

```js
  addListener(event, fn) {
    this.listeners[event] = this.listeners[event] || [];
    this.listeners[event].push(fn);
    return this;
  }
```

#### A little explanation:

The addListener event checks if the event is already registered. If yes, returns the array, otherwise empty array.

```js
this.listeners[event] // will return array of events or undefined (first time registration)
```

For example…

Let’s understand this with a usage example. Let’s create a new eventEmitter and register a ‘test-event’. This is the first time the ‘test-event’ is being registered.

```js
const eventEmitter = new EventEmitter();
eventEmitter.addListener('test-event', 
 ()=> { console.log ("test one") } 
);
```

Inside addListener () method:

```js
this.listeners[event] =>  this.listeners['test-event'] 
                  => undefined || []
                  => []
```

The result will be:

```js
this.listeners['test-event'] = [];  // empty array
```

and then the ‘fn’ will be pushed to this array as shown below:

```js
this.listeners['test-event'].push(fn);
```

I hope this makes the ‘addListener’ method very clear to decipher and understand.

**A note: Multiple callbacks can be registered against that same event.**

![img](https://cdn-media-1.freecodecamp.org/images/OOZkAQBbwteVdom2igLABEgFRmvsrkR9E9AB)

#### 2. The on method

This is just an alias to the ‘addListener’ method. We will be using the ‘on’ method more than the ‘addListener’ method for the sake of convenience.

```js
on(event, fn) {
  return this.addListener(event, fn);
}
```

#### 3. The removeListener(event, fn) method

The removeListener method takes an eventName and the callback as the parameters. It removes said listener from the event array.

NOTE: If the event has multiple listeners then other listeners will not be impacted.

First, let’s take a look at the full code for removeListener.

```js
removeListener (event, fn) {
    let lis = this.listeners[event];
    if (!lis) return this;
    for(let i = lis.length; i > 0; i--) {
      if (lis[i] === fn) {
        lis.splice(i,1);
        break;
      }
    }
    return this;
}
```

Here’s the removeListener method explained step-by-step:

- Grab the array of listeners by ‘event’
- If none found return ‘this’ for chaining.
- If found, loop through all listeners. If the current listener matches with the ‘fn’ parameter use the splice method of the array to remove it. Break from the loop.
- Return ‘this’ to continue chaining.

#### 4. The off(event, fn) method

This is just an alias to the ‘removeListener’ method. We will be using the ‘on’ method more than the ‘addListener’ method for sake of convenience.

```js
  off(event, fn) {
    return this.removeListener(event, fn);
  }
```

#### 5. The once(eventName, fn) method

Adds a **one-time** `listener` function for the event named `eventName`. The next time `eventName` is triggered, this listener is removed and then invoked.

Use for setup/init kind of events.

Let’s take a peek at the code.

```js
once(eventName, fn) {
    this.listeners[event] = this.listeners[eventName] || [];
    const onceWrapper = () => {
      fn();
      this.off(eventName, onceWrapper);
    }
    this.listeners[eventName].push(onceWrapper);
    return this;
}
```

Here’s the **once** method explained step-by-step:

- Get the event array object. Empty array if the first time.
- Create a wrapper function called onceWrapper which will invoke the fn when the event is emitted and also removes the listener.
- Add the wrapped function to the array.
- Return ‘this’ for chaining.

#### 6. The emit (eventName, ..args) method

Synchronously calls each of the listeners registered for the event named `eventName`, in the order they were registered, passing the supplied arguments to each.

Returns `true` if the event had listeners, `false` otherwise.

```js
emit(eventName, ...args) {
    let fns = this.listeners[eventName];
    if (!fns) return false;
    fns.forEach((f) => {
      f(...args);
    });
    return true;
}
```

![img](https://cdn-media-1.freecodecamp.org/images/6RYZbGvcpf5QgkKVfVUTeWUvBrdy6NbBUiw6)

Here’s the **emit** method explained step-by-step:

- Get the functions for said eventName parameter
- If no listeners, return false
- For all function listeners, invoke the function with the arguments
- Return true when done

#### 7. The listenerCount (eventName) method

Returns the number of listeners listening to the event named `eventName`.

Here’s the source code:

```js
listenerCount(eventName) {
    let fns = this.listeners[eventName] || [];
    return fns.length;
}
```

Here’s the listenerCount method explained step-by-step:

- Get the functions/listeners under consideration or an empty array if none.
- Return the length.

#### 8. The rawListeners(eventName) method

Returns a copy of the array of listeners for the event named `eventName`, including any wrappers (such as those created by `.once()`). The once wrappers in this implementation will not be available if the event has been emitted once.

```js
rawListeners(event) {
    return this.listeners[event];
}
```

The full source code for reference:

```js
class EventEmitter {
  listeners = {}
  
  addListener(eventName, fn) {
    this.listeners[eventName] = this.listeners[eventName] || [];
    this.listeners[eventName].push(fn);
    return this;
  }

  on(eventName, fn) {
    return this.addListener(eventName, fn);
  }

  once(eventName, fn) {
    this.listeners[eventName] = this.listeners[eventName] || [];
    const onceWrapper = () => {
      fn();
      this.off(eventName, onceWrapper);
    }
    this.listeners[eventName].push(onceWrapper);
    return this;
  }

  off(eventName, fn) {
    return this.removeListener(eventName, fn);
  }

  removeListener (eventName, fn) {
    let lis = this.listeners[eventName];
    if (!lis) return this;
    for(let i = lis.length; i > 0; i--) {
      if (lis[i] === fn) {
        lis.splice(i,1);
        break;
      }
    }
    return this;
  }

  emit(eventName, ...args) {
    let fns = this.listeners[eventName];
    if (!fns) return false;
    fns.forEach((f) => {
      f(...args);
    });
    return true;
  }

  listenerCount(eventName) {
    let fns = this.listeners[eventName] || [];
    return fns.length;
  }

  rawListeners(eventName) {
    return this.listeners[eventName];
  }
}
```

The complete code is available here:

https://jsbin.com/gibofab/edit?js,console,output

As an exercise feel free to implement other events’ APIs from the documentation https://nodejs.org/api/events.html.
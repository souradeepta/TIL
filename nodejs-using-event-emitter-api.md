# Using Events in Node.js (Part 1)

The core feature of Node.js is asynchronous programming. This means that code in Node.js may not be executed sequentially. Therefore, data may not be determined in a fixed amount of time. This means that to get all the data we need, we have to pass data around the app when the data is obtained. This can be done by emitting, listening to, and handling events in a Node.js app. When an event with a given name is emitted, the event can listen to the listener, if the listener is specified to listen to the event with the name. Event emitter functions are called synchronously. The event listener code is a callback function that takes a parameter for the data and handles it. Node.js has an `EventEmitter` class — it can be extended by a new class created to emit events that can be listened to by event listeners.

# Define Event Emitters

Here’s a simple example of creating and using the `EventEmitter` class:

```js
const EventEmitter = require('events');
class Emitter extends EventEmitter {}
const eventEmitter = new Emitter();
eventEmitter.on('event', () => {
  console.log('event emitted!');
});
eventEmitter.emit('event');
```

We should get ‘event emitted!’ in the console log. In the code above, we created the `Emitter` which extends the `EventEmitter` class, which has the `emit` function we called in the last line. The argument of the `emit` function is the name of the event, which we listen to in this block of code:

```js
eventEmitter.on('event', () => {
  console.log('event emitted!');
});
```

The callback function, after the `'event'` argument above, is the event handler function, that runs when the event is received.

In the code above, we emitted an event. However, it’s not very useful since we didn’t pass any data with the emitted event when we emit the event so it doesn’t do much. Therefore, we want to send data with the event so that we can pass data around so that we can do something useful in the event listener. To pass data when we emit an event, we can pass in extra arguments after the first argument, which is the event name. For instance, we can write the following code:

```js
const EventEmitter = require('events');
class Emitter extends EventEmitter {}
const eventEmitter = new Emitter();
eventEmitter.on('event', (a, b) => {
  console.log(a, b);
});
eventEmitter.emit('event', 'a', 'b');
```

If we run the code above, we get ‘a’ and ‘b’ in the `console.log` statement insider the event handler callback function. As we see, we can pass in multiple arguments with the `emit` function to pass data into event handlers that subscribe to the event. After the first one, the arguments in the `emit` function call are all passed into the event listener’s callback function as parameters, so they can be accessed within the event listener callback function.

We can also access the event emitter object inside the event listener callback function. All we have to do is change the arrow function of the callback to a tradition function, as in this code:

```js
const EventEmitter = require('events');
class Emitter extends EventEmitter {}
const eventEmitter = new Emitter();
eventEmitter.on('event', function(a, b){
  console.log(a, b);
  console.log(`Instance of EventEmitter: ${this instanceof EventEmitter}`);
  console.log(`Instance of Emitter: ${this instanceof Emitter}`);
});
eventEmitter.emit('event', 'a', 'b');
```

If we run the code above, we get this logged in the `console.log` statements inside the event listener callback function:

```
a b
Instance of EventEmitter: true
Instance of Emitter: true
```

On the other hand, if we have the following:

```js
const EventEmitter = require('events');
class Emitter extends EventEmitter {}
const eventEmitter = new Emitter();
eventEmitter.on('event', (a, b) => {
  console.log(a, b);
  console.log(`Instance of EventEmitter: ${this instanceof EventEmitter}`);
  console.log(`Instance of Emitter: ${this instanceof Emitter}`);
});
eventEmitter.emit('event', 'a', 'b');
```

Then we get this logged in the `console.log` statements inside the event listener callback function.:

```js
a b
Instance of EventEmitter: false
Instance of Emitter: false
```

This is because arrow functions do not change the `this` object inside it. However, tradition functions *do* change the content of the `this` object.

`EventEmitter` calls *all listeners synchronously, in the order that they’re registered. This eliminates the chance of race conditions and other logic errors*. To handle events asynchronously, we can use the `setImmediate()` or the `process.nextTick()` methods:

```js
const EventEmitter = require('events');
class Emitter extends EventEmitter {}
const eventEmitter = new Emitter();
eventEmitter.on('event', (a, b) => {
  setImmediate(() => {
    console.log('event handled asychronously');
  });
});
eventEmitter.emit('event', 'a', 'b');
```

In the code above, we put the `console.log` inside a callback function of the `setImmediate` function, which will run the event handling code asynchronously instead of synchronously.

Events are handled every time they’re emitted. For example, if we have:

```js
const EventEmitter = require('events');
class Emitter extends EventEmitter {}
const eventEmitter = new Emitter();
let x = 1;
eventEmitter.on('event', (a, b) => {
  console.log(x++);
});

for (let i = 0; i < 5; i++){
  eventEmitter.emit('event');
}
```

Since we emitted the ‘event’ event five times, we get this:

```
1
2
3
4
5
```

If we want to emit an event and handle it only the first time it’s emitted, then we use the `eventEmitter.once()` function, as in this code:

```js
const EventEmitter = require('events');
class Emitter extends EventEmitter {}
const eventEmitter = new Emitter();
let x = 1;
eventEmitter.once('event', (a, b) => {
  console.log(x++);
});
for (let i = 0; i < 5; i++){
  eventEmitter.emit('event');
}
```

As expected, we only get this logged in the `console.log` statement of the event handler above:

```
1
```

# Error Handling

If an error event is emitted in the case of errors, it’s treated as a special case within Node.js. If the `EventEmitter` doesn’t have at least one error event listener register and an error is emitted, the error is thrown, and the stack trace of the error will be printed, and the process will exit. For example, if we have the following code:

```js
const EventEmitter = require('events');
class Emitter extends EventEmitter {}
const eventEmitter = new Emitter();
eventEmitter.emit('error', new Error('Error occured'));
```

Then we get something like this and the program exits:

```js
Error [ERR_UNHANDLED_ERROR]: Unhandled error. (Error: Error occured
    at evalmachine.<anonymous>:5:28
    at Script.runInContext (vm.js:133:20)
    at Object.runInContext (vm.js:311:6)
    at evaluate (/run_dir/repl.js:133:14)
    at ReadStream.<anonymous> (/run_dir/repl.js:116:5)
    at ReadStream.emit (events.js:198:13)
    at addChunk (_stream_readable.js:288:12)
    at readableAddChunk (_stream_readable.js:269:11)
    at ReadStream.Readable.push (_stream_readable.js:224:10)
    at lazyFs.read (internal/fs/streams.js:181:12))
    at Emitter.emit (events.js:187:17)
    at evalmachine.<anonymous>:5:14
    at Script.runInContext (vm.js:133:20)
    at Object.runInContext (vm.js:311:6)
    at evaluate (/run_dir/repl.js:133:14)
    at ReadStream.<anonymous> (/run_dir/repl.js:116:5)
    at ReadStream.emit (events.js:198:13)
    at addChunk (_stream_readable.js:288:12)
    at readableAddChunk (_stream_readable.js:269:11)
    at ReadStream.Readable.push (_stream_readable.js:224:10)
```

To prevent the Node.js program from crashing, we can listen to the error event with a new event listener and handle the error gracefully in the error event handler. For example, we can write:

```js
const EventEmitter = require('events');
class Emitter extends EventEmitter {}
const eventEmitter = new Emitter();
eventEmitter.on('error', (error) => {
  console.log('Error occurred');
});
eventEmitter.emit('error', new Error('Error occurred'));
```

Then we get “error occurred” logged. We can also get the error content with the `error` parameter of the event handler callback function. If we log it, as in this code:

```js
const EventEmitter = require('events');
class Emitter extends EventEmitter {}
const eventEmitter = new Emitter();
eventEmitter.on('error', (error) => {
  console.log(error);
});
eventEmitter.emit('error', new Error('Error occurred'));
```

We will get something like this:

```js
Error: Error occurred
    at evalmachine.<anonymous>:7:28
    at Script.runInContext (vm.js:133:20)
    at Object.runInContext (vm.js:311:6)
    at evaluate (/run_dir/repl.js:133:14)
    at ReadStream.<anonymous> (/run_dir/repl.js:116:5)
    at ReadStream.emit (events.js:198:13)
    at addChunk (_stream_readable.js:288:12)
    at readableAddChunk (_stream_readable.js:269:11)
    at ReadStream.Readable.push (_stream_readable.js:224:10)
    at lazyFs.read (internal/fs/streams.js:181:12)
```

# More Ways to Deal with Events

`Node.js` will emit one special event without writing any code to emit the event: The `newListener` . The `newListener` event is emitted before a listener is added to the internal array of listeners. For example, if we have the following code:

```js
const EventEmitter = require('events');
class Emitter extends EventEmitter {}
const eventEmitter = new Emitter();
eventEmitter.on('newListener', (event, listener) => {
  console.log(event);
});
```

Then we get something like this logged:

```js
Emitter {
  _events: [Object: null prototype] { newListener: [Function] },
  _eventsCount: 1,
  _maxListeners: undefined }
```

This happens even when no events are emitted. Whatever is in the handler will be run before the code in event handlers for any other events.

The `removeListener` function can be used to stop event listener functions from listening to events. This takes two arguments: The first is a string that represents the event name, the second is the function that you want to stop using to listen to events. For example, if we want to stop listening to the “event” event with our listener function, then we can write this:

```js
const EventEmitter = require('events');
class Emitter extends EventEmitter { }
const eventEmitter = new Emitter();
const listener = () => {
  console.log('listening');
}
eventEmitter.on('event', listener) setInterval(() => {
  eventEmitter.emit('event');
}, 300);

setTimeout(() => {
  console.log("removing");
  eventEmitter.removeListener('event', listener);
}, 2000);
```

Then we get something like this in the output:

```js
Timeout {
  _called: false,
  _idleTimeout: 2000,
  _idlePrev: [TimersList],
  _idleNext: [TimersList],
  _idleStart: 1341,
  _onTimeout: [Function],
  _timerArgs: undefined,
  _repeat: null,
  _destroyed: false,
  [Symbol(unrefed)]: false,
  [Symbol(asyncId)]: 10,
  [Symbol(triggerId)]: 7 }listening
listening
listening
listening
listening
listening
removing
```

The event emitter emits the “event” event in the code above once every 300 milliseconds. This is listened to by the `listener` function, until it’s been prevented from listening again by calling the `removeListener` function with the “event” as the event name the `listener` event listener function in the callback of the `setTimeout` function.

Multiple event listeners can register for a single event. By default, the limit for the maximum number of event listeners is ten. We can change this with the `defaultMxListeners` function in the `EventEmitter` class. We can set it to any positive number. If it’s not a positive number, then a `TypeError` is thrown. If more listeners than the limit are registered then a warning will be output. For example, if we run the following code to register 11 event listeners for the “event” event:

```js
const EventEmitter = require('events');
class Emitter extends EventEmitter { }
const eventEmitter = new Emitter();
const listener = () => {
  console.log('listening');
}
for (i = 1; i <= 11; i++){
  eventEmitter.on('event', listener);
}eventEmitter.emit('event');
```

When we run the code above, we get this:

```js
listening
listening
listening
listening
listening
listening
listening
listening
listening
listening
listening
(node:345) MaxListenersExceededWarning: Possible EventEmitter memory leak detected. 11 event listeners added. Use emitter.setMaxListeners() to increase limit
```

However, if we call `setMaxListeners` to set it to `getMaxListeners() + 1`, which is 11 listeners, as seen in the following code:

```js
const EventEmitter = require('events');
class Emitter extends EventEmitter { }
const eventEmitter = new Emitter();
eventEmitter.setMaxListeners(eventEmitter.getMaxListeners() + 1);
const listener = () => {
  console.log('listening');
}
for (i = 1; i <= 11; i++){
  eventEmitter.on('event', listener);
}
eventEmitter.emit('event');
```

Then we get the following logged:

```js
listening
listening
listening
listening
listening
listening
listening
listening
listening
listening
listening
```

An important feature of Node.js is asynchronous programming. This means that code in Node.js may not be executed sequentially. Data may not be determined in a fixed amount of time. This means that to get all the data we need, we have to pass data around the app when the data obtained. We can emit events and handle them within a Node.js app.

When an event with a given name is emitted, the event can listen to the listener, if the listener is specified to listen to the event with the name. Event emitter functions are called synchronously. The event listener code is a callback function that takes a parameter for the data and handles it. Node.js has an `EventEmitter` class that can be extended by a new class that we create to emit events that can be listened to by event listeners. With the `EventEmitter` class, we can create new `EventEmitter` classes that can emit and listen to events. We can attach multiple event listeners to one event that can do different things. Also, we can set the maximum number of event listeners to as many as we want. Finally, we can choose to handle events asynchronously instead of synchronously.

# Using Events in Node.js (Part 2)

The core feature of [Node.js](https://nodejs.org/) is asynchronous programming. This means that code in Node.js may not be executed sequentially. Therefore, data may not be determined in a fixed amount of time.

This means that to get all the data we need, we have to pass data around the app when the data is obtained. This can be done by emitting and listening to and handling events in a Node.js app.

When an event with a given name is emitted, the event can listen to the listener if the listener is specified to listen to the event with the name. Event emitter functions are called synchronously. The event listener code is a callback function that takes a parameter for the data and handles it.

Node.js has an `EventEmitter` class that can be extended by a new class that we create to emit events that can be listened to by event listeners. In this article, we continue from part 1.

In part 1, we showed you how to define `EventEmitters`, attach event listeners, and control when they listen with the `once` and `removeListener` functions.

In this article, we will continue from part 1 and explore other functions that are available for controlling how events are emitted and listened to. The `EventEmitter` also has many getter functions for getting data about the `EventEmitter` we defined.

To get the names of the events that are listened to, we can use the `eventNames` function. The function takes no arguments and returns an array of event identifiers, which may include strings or symbols.

For example, we can use it as in the following code:

```js
const EventEmitter = require('events');
const eventEmitter = new EventEmitter();
eventEmitter.on('event1', () => {});
eventEmitter.on('event2', () => {});const sym = Symbol('event3');
eventEmitter.on(sym, () => {});console.log(eventEmitter.eventNames());
```

When we run the code above, we get the following logged:

```js
[ 'event1', 'event2', Symbol(event3) ]
```

To get the maximum number of listeners that can be attached to a single event, we can use the `getMaxListeners` function. It takes no arguments and returns the current maximum number of listeners that can be attached to one event.

The maximum number of event listeners that can be attached to the event can be set by the `setMaxListeners` function and the default value is 10. For example, we can use it as in the following example:

```js
const EventEmitter = require('events');
const eventEmitter = new EventEmitter();
console.log(eventEmitter.getMaxListeners());
```

We can use the `listenerCount` function to get the number of event listeners attached to an event. It takes one string or symbol argument for the event name and returns an integer with the number of event listeners attached to the event with the event name passed into the argument.

For example, we can use it as in the following code:

```js
const EventEmitter = require('events');
const eventEmitter = new EventEmitter();
for (let i = 1; i <= 5; i++) {
  eventEmitter.on('event', () => { });
}
console.log(eventEmitter.listenerCount('event'));
```

If the code above is run, we get 5 logged as we attached five event listeners to the `event` event.

To get the event listeners functions attached to a given event, we can use the `listeners` function. It takes a string or symbol with the event name and returns an array of event listener functions.

It returns a copy of the listeners rather than the original ones. For instance, we can use it as in the following example:

```js
[ '() => console.log(`Listener ${i} for \'event\' event invoked`)',
  '() => console.log(`Listener ${i} for \'event\' event invoked`)',
  '() => console.log(`Listener ${i} for \'event\' event invoked`)',
  '() => console.log(`Listener ${i} for \'event\' event invoked`)',
  '() => console.log(`Listener ${i} for \'event\' event invoked`)' ]
```

To attach an event listener to the beginning of the array listeners for a given event, we can use the `prependListener` function.

The function takes the event name as the first argument and the event listener function as the second argument. For example, we can write the following code:

```js
const EventEmitter = require('events');
const eventEmitter = new EventEmitter();for (let i = 1; i <= 5; i++) {
  eventEmitter.on('event', () => console.log(`Listener ${i} for 'event' event invoked`));
}eventEmitter.prependListener('event', () => console.log('Prepended listener invoked'))console.log(eventEmitter.listeners('event').map(f => f.toString()));
```

If we run the code above, we get the following logged:

```js
[ '() => console.log(\'Prepended listener invoked\')',
  '() => console.log(`Listener ${i} for \'event\' event invoked`)',
  '() => console.log(`Listener ${i} for \'event\' event invoked`)',
  '() => console.log(`Listener ${i} for \'event\' event invoked`)',
  '() => console.log(`Listener ${i} for \'event\' event invoked`)',
  '() => console.log(`Listener ${i} for \'event\' event invoked`)' ]
```

As we can see, the prepended event listener is in the first slot of the array. If we add `eventEmitter.emit('event')` to emit the `event` event, as in the following code:

```js
const EventEmitter = require('events');
const eventEmitter = new EventEmitter();for (let i = 1; i <= 5; i++) {
  eventEmitter.on('event', () => console.log(`Listener ${i} for 'event' event invoked`));
}eventEmitter.prependListener('event', () => console.log('Prepended listener invoked'))console.log(eventEmitter.listeners('event').map(f => f.toString()));
```

Then, we get the following logged with the `console.log` statements:

```js
[ '() => console.log(\'Prepended listener invoked\')',
  '() => console.log(`Listener ${i} for \'event\' event invoked`)',
  '() => console.log(`Listener ${i} for \'event\' event invoked`)',
  '() => console.log(`Listener ${i} for \'event\' event invoked`)',
  '() => console.log(`Listener ${i} for \'event\' event invoked`)',
  '() => console.log(`Listener ${i} for \'event\' event invoked`)' ]
Prepended listener invoked
Listener 1 for 'event' event invoked
Listener 2 for 'event' event invoked
Listener 3 for 'event' event invoked
Listener 4 for 'event' event invoked
Listener 5 for 'event' event invoked
```

We also see that the listener that we prepended to the event listener array is invoked first since the listeners are handled in the same order as they’re added into the array.

If we want to prepend an event listener that only handles the emitted event once, we can use the `prependOnceListener` instead. For example, we can use it as in the following code:

```js
const EventEmitter = require('events');
const eventEmitter = new EventEmitter();
for (let i = 1; i <= 5; i++) {
  eventEmitter.on('event', () => console.log(`Listener ${i} for 'event' event invoked`));
}
eventEmitter.prependOnceListener('event', () => console.log('Prepended once listener invoked'))eventEmitter.emit('event');
eventEmitter.emit('event');
```

If we run the code above, we can see the following logged in the `console.log` statements:

```js
Prepended once listener invoked
Listener 1 for 'event' event invoked
Listener 2 for 'event' event invoked
Listener 3 for 'event' event invoked
Listener 4 for 'event' event invoked
Listener 5 for 'event' event invoked
Listener 1 for 'event' event invoked
Listener 2 for 'event' event invoked
Listener 3 for 'event' event invoked
Listener 4 for 'event' event invoked
Listener 5 for 'event' event invoked
```

As we can see, the listener that we prepended with the `prependOnceListener` only runs once. This is what we expect if we attach an `event` listener with the `prependOnceListener` function.

If we want to remove all event listeners for a given event, we can use the `removeAllListeners` function. It takes a string or symbol argument for the event identifier.

Note that it’s bad practice to remove event listeners that are attached elsewhere in the code when the `EventEmitter` is created in some other component of your program. It returns a reference to the `EventEmitter` so that the calls can be chained.

For instance, we can write the following code to remove all event listeners for the `event` event:

```js
const EventEmitter = require('events');
const eventEmitter = new EventEmitter();
for (let i = 1; i <= 5; i++) {
  eventEmitter.on('event', () => console.log(`Listener ${i} for 'event' event invoked`));
}
eventEmitter.prependOnceListener('event', () => console.log('Prepended once listener invoked'))console.log('Before remove all listeners\n', eventEmitter.listeners('event').map(f => f.toString()));eventEmitter.removeAllListeners('event');console.log('After remove all listeners\n', eventEmitter.listeners('event'));
```

When the code above is run, we get the following logged:

```js
Before remove all listeners
 [ '() => console.log(\'Prepended once listener invoked\')',
  '() => console.log(`Listener ${i} for \'event\' event invoked`)',
  '() => console.log(`Listener ${i} for \'event\' event invoked`)',
  '() => console.log(`Listener ${i} for \'event\' event invoked`)',
  '() => console.log(`Listener ${i} for \'event\' event invoked`)',
  '() => console.log(`Listener ${i} for \'event\' event invoked`)' ]
After remove all listeners
 []
```

To remove a single event listener for a given event, we can use the `removeListener` function. It takes an argument with the string or symbol for an event identifier, and a reference to the listener that’s attached to the given event.

It returns a reference to the `EventEmitter` so that the calls can be chained. We can use it as in the following code:

```js
const EventEmitter = require('events');
const eventEmitter = new EventEmitter();const listener1 = () => console.log('listener1 invoked');
const listener2 = () => console.log('listener2 invoked');eventEmitter.on('event', listener1);
eventEmitter.on('event', listener2);console.log('Before remove listeners\n', eventEmitter.listeners('event').length);eventEmitter
  .removeListener('event', listener1)
  .removeListener('event', listener2);console.log('After remove listeners\n', eventEmitter.listeners('event').length);
```

When we run the code above, we get the following logged with the `console.log` statements:

```js
Before remove listeners
 2
After remove listeners
 0
```

As we can see, the `removeListener` can be chained and lets us remove the listeners one at a time. The listeners that were removed are no longer listening to the events.

If a single listener is attached to an event multiple times, then it might be called the number of times that the event listener is attached to remove all the event listeners.

If the `removeListener` function is called once in this scenario, it will remove the most recently added event listener. The event listeners array for the given event will be updated when the event listener is removed.

To get an array of event listeners for an event including the wrappers, we can use the `rawListeners` function. For example, we can use it as the following code:

```js
const EventEmitter = require('events');
const eventEmitter = new EventEmitter();const listener1 = () => console.log('listener1 invoked');
const listener2 = () => console.log('listener2 invoked');eventEmitter.once('event', listener1);
eventEmitter.on('event', listener2);const rawListeners = eventEmitter.rawListeners('event');
rawListeners[0].listener();
```

If we run the code above, we see that `listener1 invoked` is logged. This is because that the `eventEmitter.once` wraps the listener function that’s passed into it.

This is returned along with the actual event listener. Therefore, we can run the actual event listener function by running `rawListeners[0].listener();`.

This doesn’t work with the second listener because there’s no wrapper to wrap the listener function when we call `eventEmitter.on` to attach a listener.

An important feature of Node.js is asynchronous programming. This means that code in Node.js may not be executed sequentially. Therefore, data may not be determined in a fixed amount of time.

This means that to get all the data we need, we have to pass data around the app when the data is obtained. We can emit events and handle them within a Node.js app.

When an event with a given name is emitted, the event can listen to the listener if the listener is specified to listen to the event with the name. Event emitter functions are called synchronously.

The event listener code is a callback function that takes a parameter for the data and handles it. Node.js has an `EventEmitter` class that can be extended by a new class that we create to emit events that can be listened to by event listeners.

With the `EventEmitter` class, we can add and remove event listeners with built-in functions. We can remove all of them at the same time or one at a time.

However, we shouldn’t remove listeners from event emitters that aren’t written by us as it causes confusion to other people that are working on the code since other programmers do not expect built-in listeners from other modules to be removed.
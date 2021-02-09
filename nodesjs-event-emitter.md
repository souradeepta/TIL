## Handling Events in Node.js with EventEmitter

### Introduction

In this tutorial, we are going to take a look at Node's native `EventEmitter` class. You'll learn about events, what you can do with an `EvenEmitter`, and how to leverage events in your application.

We'll also cover what other native modules extend from the `EventEmitter` class and some examples to understand what is going behind the scenes.

So in a nutshell, we will cover almost everything you need to know about the `EventEmitter` class.

We will be using some basic ES6 features such as JavaScript classes and [arrow functions](https://stackabuse.com/arrow-functions-in-javascript/) in this tutorial. It's helpful, but not mandatory, if you have some prior knowledge of ES6 syntax.

### What Is an Event?

A whole software paradigm revolves around events and their usage. Event-driven architecture is relatively common nowadays and event-driven applications produce, detect, and react to different kinds of events.

We can arguably say that the core of the Node.js is partly event-driven since many native modules such as the file system (`fs`), and `stream` module are written as `EventEmitter`s themselves.

In event-driven programming, an *event* is the result of a single or multiple actions. This can be a user action or a periodic output from a sensor, for example.

You can view event-driven programs as publish-subscribe models where a publisher triggers events and subscribers listen to them and act accordingly.

For example, let's assume that we have an image server where users can upload images. In event-driven programming, an action such as uploading the image would emit an event. To make use of it, there would also be `1..n` subscribers to that event.

Once the upload event is fired, a subscriber can react to it by sending an email to the administrator of the website, letting them know that a user has uploaded a photo. Another subscriber might collect information about the action and persist them in the database.

These events are typically independent of each other, though they could also be dependent.

### What Is an EventEmitter?

The `EventEmitter` class is a built-in class which resides in the [`events`](https://nodejs.org/api/events.html) module. According to the documentation:

> Much of the Node.js core API is built around an idiomatic asynchronous event-driven architecture in which certain kinds of objects (called "emitters") emit named events that cause `Function` objects ("listeners") to be called"

This class can, to an extent, be described as a helper implementation of the pub/sub model since it helps *event emitters* (publishers) to publish *events* (messages) and *listeners* (subscribers) to act on these events - in a simple way.

### Creating EventEmitters

That being said, let's go ahead and create an `EventEmitter`. This can be done either via creating an instance of the class itself, or by implementing it through a custom class and then creating an instance of that class.

#### Creating an *EventEmitter* Object

Let's start off with a simple event-emitting object. We'll create an `EventEmitter` which will emit an event that contains information about the application's uptime, every second.

First, import the `EventEmitter` class from the `events` modules:

```javascript
const { EventEmitter } = require('events');
```

Then let's create an `EventEmitter`:

```javascript
const timerEventEmitter = new EventEmitter();
```

To publish an event from this object is as easy as:

```javascript
timerEventEmitter.emit("update");
```

We've specified the event name and published it as an event. Nothing happens since there's no listener to react to this event, though. Let's make this event repeat every second.

Using the `setInterval()` method, a timer is created which will publish the `update` event every second:

```javascript
let currentTime = 0;

// This will trigger the update event each passing second
setInterval(() => {
    currentTime++;
    timerEventEmitter.emit('update', currentTime);
}, 1000);
```

The `EventEmitter` instance accepts an event name and an arbitrary set of arguments. In this case, we've passed the `eventName` as `update` and the `currentTime` as the time from the start of the application.

We trigger the emitter via the `emit()` method, which pushes the event with the information we've provided.

With our event-emitter ready, let's subscribe an event-listener to it:

```javascript
timerEventEmitter.on('update', (time) => {
    console.log('Message Received from publisher');
    console.log(`${time} seconds passed since the program started`);
});
```

Using the `on()` method, passing the event name to specify which one we'd like to attach a listener to, allows us to create listeners. *On* the `update` event, a method is run which logs the time. You can add the same listener over and over again, and each one will subscribe to the event.

The second argument of the `on()` function is a callback that can accept any number of the extra data that was emitted by the event. Each listener can choose what data they want, once the order is kept.

Running this script should yield:

```plaintext
Message Received from publisher
1 seconds passed since the program started
Message Received from publisher
2 seconds passed since the program started
Message Received from publisher
3 seconds passed since the program started
...
```

By contrast, we can use the `once()` method to subscribe - if you need to execute something only the first time an event triggers:

```javascript
timerEventEmitter.once('update', (time) => {
    console.log('Message Received from publisher');
    console.log(`${time} seconds passed since the program started`);
});
```

Running this code will yield:

```plaintext
Message Received from publisher
1 seconds passed since the program started
```

#### *EventEmitter* with Multiple Listeners

Now, let's make a different kind of event-emitter with three listeners. This one will be a countdown. One listener will update the user on each second, one listener will notify the user when the countdown is nearing its end and the last listener will trigger once the countdown has ended:

- `update` - This event will trigger on every second
- `end` - This event will trigger at the end of the countdown
- `end-soon` - This event will trigger 2 seconds before the countdown is finished

Let's create a function that creates this event-emitter and returns it:

```javascript
const countDown = (countdownTime) => {
    const eventEmitter = new EventEmitter();

    let currentTime = 0;

    // This will trigger the update event each passing second
    const timer = setInterval(() => {
        currentTime++;
        eventEmitter.emit('update', currentTime);

        // Check if countdown has reached to the end
        if (currentTime === countdownTime) {
            clearInterval(timer);
            eventEmitter.emit('end');
        }

        // Check if countdown will end in 2 seconds
        if (currentTime === countdownTime - 2) {
            eventEmitter.emit('end-soon');
        }
    }, 1000);
    return eventEmitter;
};
```

In this function, we have started an interval-based event that emits the `update` event in an interval of a second.

At the first `if` condition, we check if the countdown has reached the end and stop the interval-based event. If so, we fire an `end` event.

In the second condition, we check if the countdown is 2 seconds away from ending, and publish the `end-soon` event if so.

Now, let's add a few subscribers to this event emitter:

```javascript
const myCountDown = countDown(5);

myCountDown.on('update', (t) => {
    console.log(`${t} seconds since the timer started`);
});

myCountDown.on('end', () => {
    console.log('Countdown is completed');
});

myCountDown.on('end-soon', () => {
    console.log('Count down will end in 2 seconds');
});
```

This code should yield:

```plaintext
1 seconds has been passed since the timer started
2 seconds has been passed since the timer started
3 seconds has been passed since the timer started
Count down will end in 2 seconds
4 seconds has been passed since the timer started
5 seconds has been passed since the timer started
Countdown is completed
```

#### Extending *EventEmitter*

In this section, let's make an event emitter with the same functionality, by extending the `EventEmitter` class. First, create a `CountDown` class that will handle the events:

```javascript
const { EventEmitter } = require('events');

class CountDown extends EventEmitter {
    constructor(countdownTime) {
        super();
        this.countdownTime = countdownTime;
        this.currentTime = 0;
    }

    startTimer() {
        const timer = setInterval(() => {
            this.currentTime++;
            this.emit('update', this.currentTime);
    
            // Check if countdown has reached to the end
            if (this.currentTime === this.countdownTime) {
                clearInterval(timer);
                this.emit('end');
            }
    
            // Check if countdown will end in 2 seconds
            if (this.currentTime === this.countdownTime - 2) {
                this.emit('end-soon');
            }
        }, 1000);
    }
}
```

As you can see, we can use `this.emit()` inside the class directly. Also, the `startTimer()` function is used to allow us control over when the countdown starts. Otherwise, it would start as soon as the object is created.

Let's create a new object of `CountDown` and subscribe to it:

```javascript
const myCountDown = new CountDown(5);

myCountDown.on('update', (t) => {
    console.log(`${t} seconds has been passed since the timer started`);
});

myCountDown.on('end', () => {
    console.log('Countdown is completed');
});

myCountDown.on('end-soon', () => {
    console.log('Count down will be end in 2 seconds');
});

myCountDown.startTimer();
```

#### Subscribe to our Newsletter

Get occassional tutorials, guides, and jobs in your inbox. No spam ever. Unsubscribe at any time.

Newsletter Signup

Subscribe

Running this will result in:

```plaintext
1 seconds has been passed since the timer started
2 seconds has been passed since the timer started
3 seconds has been passed since the timer started
Count down will be end in 2 seconds
4 seconds has been passed since the timer started
5 seconds has been passed since the timer started
Countdown is completed
```

An *alias* for the `on()` function is `addListener()`. Consider the `end-soon` event listener:

```javascript
myCountDown.on('end-soon', () => {
    console.log('Count down will be end in 2 seconds');
});
```

We could have done the same with `addListener()` like this:

```javascript
myCountDown.addListener('end-soon', () => {
    console.log('Count down will be end in 2 seconds');
});
```

They both work. They're almost like synonyms. However, most coders prefer to use `on()`.

### Important Functions of *EventEmitter*

Let's take a look at some of the important functions we can use on `EventEmitter`s.

#### *eventNames()*

This function will return all active listener names as an array:

```javascript
const myCountDown = new CountDown(5);

myCountDown.on('update', (t) => {
    console.log(`${t} seconds has been passed since the timer started`);
});

myCountDown.on('end', () => {
    console.log('Countdown is completed');
});

myCountDown.on('end-soon', () => {
    console.log('Count down will be end in 2 seconds');
});

console.log(myCountDown.eventNames());
```

Running this code will result in:

```plaintext
[ 'update', 'end', 'end-soon' ]
```

If we were to subscribe to another event such as `myCount.on('some-event', ...)`, the new event will be added to the array as well.

Keep in mind that this method does not return the published events. It returns a list of events that are subscribed to it.

#### *removeListener()*

As the name suggests, this function removes a subscribed handler from an `EventEmitter`:

```javascript
const { EventEmitter } = require('events');

const emitter = new EventEmitter();

const f1 = () => {
    console.log('f1 Triggered');
}

const f2 = () => {
    console.log('f2 Triggered');
}

emitter.on('some-event', f1);
emitter.on('some-event', f2);

emitter.emit('some-event');

emitter.removeListener('some-event', f1);

emitter.emit('some-event');
```

After the first event triggers, since both `f1` and `f2` are active - both functions will be executed. After that, we've removed `f1` from the `EventEmitter`. When we emit the event again, only `f2` will execute:

```plaintext
f1 Triggered
f2 Triggered
f2 Triggered
```

An *alias* for `removeListener()` is `off()`. For example, we could have written:

```javascript
emitter.removeListener('some-event', f1);
```

As:

```javascript
emitter.off('some-event', f1);
```

They both have the same effect.

#### *removeAllListeners()*

Again, as the name suggests - this function will remove all listeners from all events of an `EventEmitter`:

```javascript
const { EventEmitter } = require('events');

const emitter = new EventEmitter();

const f1 = () => {
    console.log('f1 Triggered');
}

const f2 = () => {
    console.log('f2 Triggered');
}

emitter.on('some-event', f1);
emitter.on('some-event', f2);

emitter.emit('some-event');

emitter.removeAllListeners();

emitter.emit('some-event');
```

The first `emit()` will fire both `f1` and `f2` since they're active at that time. After removing them, the `emit()` function will emit the event, but no listeners will respond to it:

```bash
f1 Triggered
f2 Triggered
```

### Error Handling

If you want to emit an error with your `EventEmitter`, it must be done with an `error` event-name. This is standard for all `EventEmitter` objects in Node.js. This event *must* also be accompanied by an `Error` object. For example, an error event can be emitted like this:

```javascript
myEventEmitter.emit('error', new Error('Something bad happened'));
```

Any listeners for the `error` event should have a callback with one argument to capture the `Error` object and gracefully handle it. If an `EventEmitter` emits an `error` event, but there are no listeners subscribed for `error` events, the Node.js program would throw the `Error` that was emitted.

This will ultimately stop the Node.js process from running and exit your program, while displaying the stacktrace for the error in the console.

Let's assume, in our `CountDown` class, the `countdownTime` parameter cannot start off being less than 2 because we won't be able to trigger the event `end-soon` otherwise.

In such a case, let's emit an `error` event:

```javascript
class CountDown extends EventEmitter {
    constructor(countdownTime) {
        super();

        if (countdownTimer < 2) {
            this.emit('error', new Error('Value of the countdownTimer cannot be less than 2'));
        }

        this.countdownTime = countdownTime;
        this.currentTime = 0;
    }

    // ...........
}
```

Handling this error is handled the same as other events:

```javascript
myCountDown.on('error', (err) => {
    console.error('There was an error:', err);
});
```

It's considered good practice to always have a listener for `error` events.

### Native Modules using *EventEmitter*

Many native modules in Node.js extend the `EventEmitter` class and thus are event-emitters themselves.

A great example is the [`Stream` class](https://stackabuse.com/introduction-to-node-js-streams/). The official documentation states:

> Streams can be readable, writable, or both. All streams are instances of `EventEmitter`.

Let's take a look at some classic `Stream` usage:

```javascript
const fs = require('fs');
const writer = fs.createWriteStream('example.txt');

for (let i = 0; i < 100; i++) {
  writer.write(`hello, #${i}!\n`);
}

writer.on('finish', () => {
  console.log('All writes are now complete.');
});

writer.end('This is the end\n');
```

However, between the writing operation and the `writer.end()` call, we've added a listener. `Stream`s emit a `finished` event upon completion. Other events, such as `error`, `pipe` and `unpipe` are emitted when an error occurs or a read-stream is piped to or unpiped from a write-stream.

Another notable class is the `child_process` class and its `spawn()` method:

```javascript
const { spawn } = require('child_process');
const ls = spawn('ls', ['-lh', '/usr']);

ls.stdout.on('data', (data) => {
  console.log(`stdout: ${data}`);
});

ls.stderr.on('data', (data) => {
  console.error(`stderr: ${data}`);
});

ls.on('close', (code) => {
  console.log(`child process exited with code ${code}`);
});
```

When the `child_process` writes into the standard output pipe, the `data` event of the `stdout` (which also `extends EventEmitter`) will fire. When the output stream encounters an error, the `data` event is sent from the `stderr` pipe.

Finally, after the process exits, the `close` event is fired.

### Conclusion

Event-driven architecture allows us to create systems which are *decoupled* but *highly cohesive*. Events represent the result of a certain action, and `1..n` listeners can be defined to listen and react to them.

In this article, we've dived into the `EventEmitter` class and its functionality. We've instantiated it and used it directly, as well as extended its behavior into a custom object.

Finally, we've covered some notable functions of the class.

As always the source code is available on [GitHub](https://github.com/jkasun/sa-event-emitter).
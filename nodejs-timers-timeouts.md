# setTimeout vs setImmediate vs process.nextTick

[#eventloop](https://dev.to/t/eventloop) [#node](https://dev.to/t/node) [#javascript](https://dev.to/t/javascript)

[![logicmason profile image](https://res.cloudinary.com/practicaldev/image/fetch/s--8NNeEnGe--/c_fill,f_auto,fl_progressive,h_50,q_auto,w_50/https://dev-to-uploads.s3.amazonaws.com/uploads/user/profile_image/158342/a81fa956-ee17-4eb6-a106-15196e0c296d.jpg)Mark](https://dev.to/logicmason)Apr 21, 2019 *Originally published at [logicmason.com](https://logicmason.com/2019/settimeout-vs-setimmediate-vs-process-nexttick/) on Apr 21, 2019* ・2 min read

What's the difference between `setTimeout(callback, 0)` and `process.nextTick(callback)`? How about Node's `setImmediate(callback)`?

On the surface it appears that all three functions do the same thing—they execute the callback after the current event loop, but before anything else. The natural question to ask is, why are there three different functions? Let's run an experiment:

```js
let racer = function() {
  setTimeout(() => console.log("timeout"), 0);
  setImmediate(() => console.log("immediate"));
  process.nextTick(() => console.log("nextTick"));
  console.log("current event loop");
}

racer()
```

We can see from the output that these callbacks aren't executed in the same order they were written in the source code.

```js
[Running] node "/Users/logicmason/timeouts.js"
current event loop
nextTick
timeout
immediate

[Done] exited with code=0 in 0.203 seconds
```

### Explanation

The first one executed was `process.nextTick`, which puts its callback at the front of the event queue. It will execute after the code currently being executed but before any I/O events or timers.

Next is "timeout". Since we passed `setTimeout` a timeout of 0, there's no additional enforced delay before its execution, and it is placed on into the timer queue during the next loop.

Finally, we have `setImmediate`, which is clearly not as immediate as its name suggests! Its callback is placed in the check queue of the next cycle of the event loop. Since the check queue occurs later than the timer queue, setImmediate will be slower than setTimeout 0.

All in all, the event loop looks like this:

`timers` -> `IO` -> `poll` -> `check` ->`close` -> `timers` -> ...

**Timers**: callbacks from `setInterval` or `setTimeout`
**IO callbacks**: callbacks from I/O events
**Idle**: used internally by Node between IO and Poll phases
**Poll**: retrieve new I/O events
**Check**: callbacks from `setImmediate` execute here
**Close**: handle closed connections like sockets

### Challenge time!

What do you expect the output of the following code in Node to be?

```js
let racer1 = function() {
  setTimeout(() => console.log("timeout"), 0);
  setImmediate(() => console.log("immediate"));
  process.nextTick(() => console.log("nextTick"));
}

let racer2 = function() {
  process.nextTick(() => console.log("nextTick"));
  setTimeout(() => console.log("timeout"), 0);
  setImmediate(() => console.log("immediate"));
}

let racer3 = function() {
  setImmediate(() => console.log("immediate"));
  process.nextTick(() => console.log("nextTick"));
  setTimeout(() => console.log("timeout"), 0);
}

racer1()
racer2()
racer3()
```

Was it what you expected?

```js
[Running] node "/Users/logicmason/timeouts.js"
nextTick
nextTick
nextTick
timeout
timeout
timeout
immediate
immediate
immediate

[Done] exited with code=0 in 0.355 seconds
```


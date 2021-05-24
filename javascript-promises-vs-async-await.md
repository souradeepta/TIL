# JavaScript: Promises or async-await

![Image for post](https://miro.medium.com/max/1270/1*wBiJ82CJVPCyymYYzN1I9Q.png)

I recently read a medium post where the author claimed that using `async-await` is better than using `promises`.

While this might be true in general cases, I think that generalisation is too broad and doesn’t do justice to either `async-await` or `promises`.

For someone new to JavaScript, making sense of these and deciding which one to use can be a challenge. In this post, I will list things that I have learned about these and how I decide when to use which.

I read somewhere that `async-await` is syntactical sugar for using `promises`. So, before getting to know `async-await` or deciding which approach to use, make sure that you have a good understanding of [promises](https://medium.com/better-programming/understanding-promises-in-javascript-13d99df067c1) and [async-await](https://medium.com/better-programming/understanding-async-await-in-javascript-1d81bb079b2c).

Here are some thumb rules that I follow.

# Thumb Rules for Using Promises

![Image for post](https://miro.medium.com/max/60/1*0mBlni5vsYZE2wFzfVv8EA.png?q=20)

![Image for post](https://miro.medium.com/max/1001/1*0mBlni5vsYZE2wFzfVv8EA.png)

1. Use promises whenever you are using asynchronous or blocking code.
2. `resolve` maps to `then` and `reject` maps to `catch` for all practical purposes.
3. Make sure to write both `.catch` and `.then` methods for all the promises.
4. If something needs to be done in both cases use `.finally`.
5. We only get one shot at mutating each promise.
6. We can add multiple handlers to a single promise.
7. The return type of all the methods in the `Promise` object, regardless of whether they are static methods or prototype methods, is again a `Promise`.
8. In `Promise.all`, the order of the promises are maintained in the values variable, irrespective of which promise was first resolved.

Once you have wrapped your head around promises, check out `async-await`. It helps you to write code that is much more readable. When it is not used properly, it has its downsides.

# Thumb Rules for async-await

Here’s a list of thumb rules that I use to stay sane while using `async` and `await`.

1. `async` functions return a promise.
2. `async` functions use an implicit `Promise` to return results. Even if you don’t return a promise explicitly, the `async` function makes sure that your code is passed through a promise.
3. `await` blocks the code execution within the `async` function, of which it (`await statement`) is a part.
4. There can be multiple `await` statements within a single `async` function.
5. When using `async await`, make sure you use `try catch` for error handling.
6. Be extra careful when using `await` within loops and iterators. You might fall into the trap of writing sequentially-executing code when it could have been easily done in parallel.
7. `await` is always for a single `Promise`.
8. `Promise` creation starts the execution of asynchronous functionality.
9. `await` only blocks the code execution within the `async` function. It only makes sure that the next line is executed when the `promise` resolves. So, if an asynchronous activity has already started, `await` will not have any effect on it.

# Should I Use Promises or async-await

The answer is that we will use both.

Here are the thumb rules that I use to decide when to use `promises` and when to use `async-await`.

1. The `async function` returns a `promise`. The converse is also true. Every function that returns a `promise` can be considered as `async function`.
2. `await` is used for calling an `async function` and waits for it to `resolve` or `reject`.
3. `await` blocks the execution of the code within the `async` function in which it is located.
4. If the output of `function2` is dependent on the output of `function1`, I use `await`.
5. If two functions can be run in parallel, create two different `async functions` and then run them in parallel.
6. To run promises in parallel, create an array of promises and then use `Promise.all(promisesArray)`.
7. Every time you use `await` remember that you are writing blocking code. Over time we tend to neglect this.
8. Instead of creating huge `async functions` with many `await asyncFunction()` in it, it is better to create smaller `async functions`. This way, we will be aware of not writing too much blocking code.
9. Another advantage of using smaller `async functions` is that you force yourself to think of which async functions can be run in parallel.
10. If your code contains blocking code, it is better to make it an `async `function. By doing this, you are making sure that somebody else can use your function asynchronously.
11. By making async functions out of blocking code, you are enabling the user (who will call your function) to decide on the level of asynchronicity they want.

I hope this helps you decide when to use `promises` and when to use `async-await`.

You can subscribe to my newsletter https://understandingx.substack.com/ I am lazy to write and hence you may not hear from me for a while.
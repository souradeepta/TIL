# Error handling with Async/Await in JS

This will be a small article, based on some issues I picked up during code reviews and discussions with other developers. This article is rather focused on the novice JS developers.

# **A Simple Try Catch**

Let’s start with the simple `try...catch` example.

```js

function thisThrows() {
    throw new Error("Thrown from thisThrows()");
}

try {
    thisThrows();
} catch (e) {
    console.error(e);
} finally {
    console.log('We do cleanup here');
}

// Output:
// Error: Thrown from thisThrows()
//   ...stacktrace
// We do cleanup here
view rawsimple_async_await.js hosted with ❤ by GitHub
```



<iframe src="https://itnext.io/media/bbf1011223be0922e33f15884630024d" allowfullscreen="" frameborder="0" height="385" width="680" title="simple_async_await.js" class="t u v gp aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 385px;"></iframe>

This works as expected, we call the function `thisThrows()` which throws a regular error, we catch it, log the error and optionally we run some code in the `finally` block. No rocket science here.

# **A Try Catch with a Rejecting Promise**

Now we modify `thisThrows()` so it actually rejects with a promise instead of a regular error. For simplicity, I will make the `thisThrows()` function `async` . Remember that an `async` function **always** returns a promise:

- When no return statement defined, or a return statement without a value, it returns a resolving promise, equivalent to `return Promise.Resolve()` .

- When a return statement is defined with a value, it will return a resolving promise with the given return value, equivalent to `return Promise.Resolve("My return String")`

- When an error is thrown, a rejected promised will be returned with the thrown error, equivalent to `return Promise.Reject(error)` .

- ```js
  
  async function thisThrows() {
      throw new Error("Thrown from thisThrows()");
  }
  
  try {
      thisThrows();
  } catch (e) {
      console.error(e);
  } finally {
      console.log('We do cleanup here');
  }
  
  // output:
  // We do cleanup here
  // UnhandledPromiseRejectionWarning: Error: Thrown from thisThrows()
  view rawerror_unhandled_rejection.js hosted with ❤ by GitHub
  ```

  

<iframe src="https://itnext.io/media/b53244640445b70dde92e3d8026de0b5" allowfullscreen="" frameborder="0" height="363" width="680" title="error_unhandled_rejection.js" class="t u v gp aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 363px;"></iframe>

Now we have the classic problem, `thisThrows` returns a rejecting promise, so the regular `try...catch` is not able to catch the error. As `thisThrows()` is `async` , so when we call it, it dispatches a promise, the code does not wait, so the `finally` block is executed first and then the promise executes, which then rejects. So we don’t have any code that handles this rejected promise.

We can handle this in two ways:

- We call `thisThrows()` in an `async` function and `await` the `thisThrows()` function.
- We chain the `thisThrows()` function call with a `.catch()` call.

The first solution would look like this:

```js

async function thisThrows() {
    throw new Error("Thrown from thisThrows()");
}

async function run() {
    try {
        await thisThrows();
    } catch (e) {
        console.error(e);
    } finally {
        console.log('We do cleanup here');
    }
}

run();

// Output:
// Error: Thrown from thisThrows()
//   ...stacktrace
// We do cleanup here
view rawerror_handle_async_try_catch.js hosted with ❤ by GitHub
```



<iframe src="https://itnext.io/media/7112111bc5890c4832d09c64316edb80" allowfullscreen="" frameborder="0" height="471" width="680" title="error_handle_async_try_catch.js" class="t u v gp aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 471px;"></iframe>

And the second one:

```js

async function thisThrows() {
    throw new Error("Thrown from thisThrows()");
}

thisThrows()
    .catch(console.error)
    .then(() => console.log('We do cleanup here'));

// Output:
// Error: Thrown from thisThrows()
//   ...stacktrace
// We do cleanup here
view rawerror_handle_catch_chain.js hosted with ❤ by GitHub
```



<iframe src="https://itnext.io/media/4b7bfbed605d2d5d204064a1955f1c68" allowfullscreen="" frameborder="0" height="298" width="680" title="error_handle_catch_chain.js" class="t u v gp aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 298px;"></iframe>

Both solutions work fine, but the `async/await` one is easier to reason about (at least in my personal opinion).

# Caveats

We had a look at simple error handling with and without errors. Let’s have a look now at some special cases.

## Returning from an async function

Let’s start with a brain teaser, what will happen with the following code?

```js

async function thisThrows() {
    throw new Error("Thrown from thisThrows()");
}

async function myFunctionThatCatches() {
    try {
        return thisThrows();
    } catch (e) {
        console.error(e);
    } finally {
        console.log('We do cleanup here');
    }
    return "Nothing found";
}

async function run() {
    const myValue = await myFunctionThatCatches();
    console.log(myValue);
}

run();
view rawerror_return_promise.js hosted with ❤ by GitHub
```



<iframe src="https://itnext.io/media/ce138dcc10108f75f79032880ced52dc" allowfullscreen="" frameborder="0" height="493" width="680" title="error_return_promise.js" class="t u v gp aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 493px;"></iframe>

At first, we might expect the output to be:

```
We do cleanup here
Nothing Found
```

But instead, we got a `UnhandledPromiseRejection` ! But why? Let’s step through the code:

- `thisThrows()` is an `async` method
- We throw an error in `thisThrows()`
- As `thisThrows()` is `async` the thrown error is returned as a rejected promises from the function.
- We return that rejected promised in `myFunctionThatCatches()` with the `return` statement.
- Our rejected promise reaches the `await` keyword. The `await` keyword discovers that the Promise is resolved with the status “rejected” and propagates the error as an unhandled promise rejection.

Based on how our code is structured, our error snuck past our `try...catch` block and propagated further down in the call tree. Not good!

We solve this by using the `await` keyword in the return statement.

```js

async function thisThrows() {
    throw new Error("Thrown from thisThrows()");
}

async function myFunctionThatCatches() {
    try {
        return await thisThrows(); // <-- Notice we added here the "await" keyword.
    } catch (e) {
        console.error(e);
    } finally {
        console.log('We do cleanup here');
    }
    return "Nothing found";
}

async function run() {
    const myValue = await myFunctionThatCatches();
    console.log(myValue);
}

run();

// Outptut:
// Error: Thrown from thisThrows()
//   ...stacktrace
// We do cleanup here
// Nothing found

view rawerror_handle_async_try_catch_with_await.js hosted with ❤ by GitHub
```



<iframe src="https://itnext.io/media/aadf6e5459c2973af5539f87ed9319e4" allowfullscreen="" frameborder="0" height="644" width="680" title="error_handle_async_try_catch_with_await.js" class="t u v gp aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 644px;"></iframe>

Now our `try..catch` works as expected. Now the `await` keyword on line `7` will first await the returned promise of `thisThrows()` and if that promise rejects, the error will be propagated to the `catch` block on line `8` . Problem solved!

## Resetting your stack trace

How to handle these use cases will be different on a per-case basis, make sure to be aware of this common mistake and then decide if it’s relevant for you or not.

It’s not uncommon to see code where someone catches an error and wraps it in a new error, like in the following snippet:

```js

function thisThrows() {
    throw new Error("Thrown from thisThrows()");
}

function myFunctionThatCatches() {
    try {
        return thisThrows();
    } catch (e) {
        throw new TypeError(e.message);
    } finally {
        console.log('We do cleanup here');
    }
}

async function run() {
    try {
        await myFunctionThatCatches();
    } catch (e) {
        console.error(e);
    }
}

run();

// Outputs:
// We do cleanup here
// TypeError: Error: Thrown from thisThrows()
//    at myFunctionThatCatches (/repo/error_stacktrace_1.js:9:15) <-- Error points to our try catch block
//    at run (/repo/error_stacktrace_1.js:17:15)
//    at Object.<anonymous> (/repo/error_stacktrace_1.js:23:1)
view rawerror_stacktrace_1.js hosted with ❤ by GitHub
```



<iframe src="https://itnext.io/media/f85f7230f4833f9814183b49723f466d" allowfullscreen="" frameborder="0" height="704" width="680" title="error_stacktrace_1.js" class="t u v gp aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 704px;"></iframe>

Notice that our stack trace only starts where we caught the original exception. When we create an error on line `2`and catch it on `9` , we lose the original stack trace as we now create a new error of type `TypeError` and only keep the original error message (sometimes we don’t even do that).

Imagine that the `thisThrows()` function has far more logic in it, and somewhere in that function an error is thrown, we won’t see in our logged stack trace the origin of the problem, as we created a new error which will build a brand new stack trace. If we just re-throw our original error, we won’t have that problem:

```js

function thisThrows() {
    throw new Error("Thrown from thisThrows()");
}

function myFunctionThatCatches() {
    try {
        return thisThrows();
    } catch (e) {
        // Maybe do something else here first.
        throw e;
    } finally {
        console.log('We do cleanup here');
    }
}

async function run() {
    try {
        await myFunctionThatCatches();
    } catch (e) {
        console.error(e);
    }
}

run();

// Outputs:
// We do cleanup here
// Error: Thrown from thisThrows()
//     at thisThrows (/repo/error_stacktrace_2.js:2:11) <-- Notice we now point to the origin of the actual error
//     at myFunctionThatCatches (/repo/error_stacktrace_2.js:7:16)
//     at run (/repo/error_stacktrace_2.js:18:15)
//     at Object.<anonymous> (/repo/error_stacktrace_2.js:24:1)
view rawerror_stacktrace_2.js hosted with ❤ by GitHub
```



<iframe src="https://itnext.io/media/01171cdff9ba1ed21869f43488b121e7" allowfullscreen="" frameborder="0" height="747" width="680" title="error_stacktrace_2.js" class="t u v gp aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 747px;"></iframe>

Notice that the stack trace now points to the origin of the actual error, being on line `2` from our script.

It is important to be aware of this problem when handling errors. Sometimes this might be desirable, but often this obfuscates the origin of your problem, making it hard to debug the root of a problem. If you create your own custom errors for wrapping errors, make sure you keep track of the original stack trace so that debugging doesn’t turn into a nightmare.

# Summary

- We can use `try...catch` for synchronous code.
- We can use `try...catch` (in combination with `async` functions) and the `.catch()` approaches to handle errors for asynchronous code.
- When returning a promise within a `try` block, make sure to `await` it if you want the `try...catch` block to catch the error.
- Be aware when wrapping errors and rethrowing, that you lose the stack trace with the origin of the error.
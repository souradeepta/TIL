# JavaScript ES6+: var, let, or const?

Perhaps the most important thing you can learn to be a better coder is to **keep things simple**. In the context of identifiers, that means that **a single identifier should only be used to represent a single concept.**

Sometimes it’s tempting to create an identifier to represent some data and then use that identifier as a temporary place to store values in transition from one representation to another.

For instance, you may be after a query string parameter value, and start by storing an entire URL, then just the query string, then the value. This practice should be avoided.

It’s easier to understand if you use one identifier for the URL, a different one for the query string, and finally, an identifier to store the parameter value you were after.

This is why I favor *`const`* over *`let`* in ES6. In JavaScript, ***`const`\* means that the identifier can’t be reassigned.** (Not to be confused with *immutable values.* Unlike true immutable datatypes such as those produced by Immutable.js and Mori, a *`const`* object can have properties mutated.)

If I don’t need to reassign, ***`const`\* is my default choice** over *`let`* because I want the usage to be as clear as possible in the code.

I use *`let`* when I need to reassign a variable. Because I **use one variable to represent one thing,** the use case for *`let`* tends to be for loops or mathematical algorithms.

I don’t use *`var`* in production ES6 code. There is value in block scope for loops, but I can’t think of a situation where I’d prefer *`var`* over *`let`*. It can be handy in a REPL (Read Evaluate Print Loop) environment like the debugger or Node console when you’re just experimenting and reassignment could be useful.

***`const`\*** is a signal that **the identifier won’t be reassigned.**

***`let`\*** is a signal that **the variable may be reassigned**, such as a counter in a loop, or a value swap in an algorithm. It also signals that the variable will be used **only in the block it’s defined in**, which is not always the entire containing function.

***`var`\*** is now **the weakest signal available** when you define a variable in JavaScript. The variable may or may not be reassigned, and the variable may or may not be used for an entire function, or just for the purpose of a block or loop.

## Warning:

With *`let`* and *`const`* in ES6, it’s no longer safe to check for an identifier’s existence using *`typeof`:*

```js
function foo () {
  typeof bar;
  let bar = ‘baz’;
}
foo(); // ReferenceError: can't access lexical declaration
       // `bar' before initialization
```

But you’ll be fine because you took my advice from [“Programming JavaScript Applications”](http://pjabook.com/) and you always initialize your identifiers before you try to use them…

## P.S.

If you need to deallocate a value by unsetting it, you may consider *`let`* over*`const`*.
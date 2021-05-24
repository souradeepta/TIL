## The Node.js Way - Designing Singletons

December 20, 2013 (Updated December 18, 2014)

In most languages, sharing an object across your entire application can be a complex process. These [Singletons](http://en.wikipedia.org/wiki/Singleton_pattern) are usually quite complex, and require some [advanced modifications](http://www.galloway.me.uk/tutorials/singleton-classes/) to get working. In Node, however, this type of object is the default. Every module that you require is shared across your application, so there’s no need for any special classes or extra code.

Node doesn’t impose much structure on it’s developers, so it’s up to you to choose the right design for your modules. In this post, I’ll share a few simple patterns that will help you get the most out of your singletons by designing them as cleanly and clearly as possible.

### PRIVATE VS. PUBLIC

When creating a new module, the variables and functions you define are kept private by default. You’ll need to use [module.exports](http://nodejs.org/api/modules.html#modules_module_exports) to define your interface and make them public. This is a straight-forward process: whatever you set to `module.exports` becomes public, and is visible when your module is required.

```javascript
// module.js
var a = 1; // Private
module.exports = {
  b: 2 // Public
};
var m = require('module');
console.log(m.a); // undefined
console.log(m.b); // 2
```

### EXPORTING A SINGLE SOURCE OF TRUTH

There are a million different decisions to make when designing your module. Where should I declare my functions? Where should I export my public interface? Should I declare all my public functions as properties of module.exports directly, or one by one?

While these questions all come down to personal preference, the decisions you make can have major effects on the clarity of your code. Declaring and exporting properties one at a time, for example, leaves you scanning each function declaration to understand the public interface. Exporting all of your functions at the bottom means you have to jump around your file to see what’s public. Neither design gives you the full picture, and both can be burdensome to understand.

To get the most out of your code, keep a single source of truth to your design. You should be able to look in one place to see exactly what’s public, what’s private, and what the hell is going on. Kill two birds with one stone: define AND export your public variables at the same time. You’ll have an explicit separation of private and public as well as a complete picture of your public interface, instead of constantly searching your code base to find which functions are being exported.

```javascript
// Private
var TWO = 2;
function sum(x, y) {
    return x + y;
}

// Public
module.exports = {
    x: 5,
    add2: function(num) {
        return sum(TWO, num);
    },
    addX: function(num) {
        return sum(module.exports.x, num);
    }
}
```

### AVOIDING THE EXPORTS ALIAS

A seasoned Node veteran will know there’s a second way to export your code. `exports` is an alias to `module.exports`, which you can also use to define your public interface. But be careful: it’s just an alias to `module.exports`. Setting individual properties works fine, but if you set the entire `exports` or `module.exports` object (as we do above) you’ll break the connection between the two and `exports` will no longer work.

While there’s nothing wrong with using the exports alias instead of `module.exports`, losing the ability to set more than one property at a time forces you to lose our single point of truth. For that reason, stick with `module.exports` when defining your public interface.

The one exception where `exports` may still be valuable is when referencing the public module from private code. Instead of calling `module.exports.x` every time you want the value of `x`, you can save yourself some characters by using `exports.x` instead. Just remember that for this to work properly, you’ll need to re-assign the alias yourself:

```javascript
// Public
exports = module.exports = {
    /* … */
}
```

### BRINGING IT ALL TOGETHER

To see all these rules come together, check out a [complete singleton](https://github.com/FredKSchott/the-node-way/blob/master/01-singleton-example.js). But remember, rules are meant to be broken. Use what works, discard what doesn’t, and you’ll be well on your way to Node Nirvana.
## The Node.js Way - Designing Factories

January 6, 2015

Previously, we introduced the [Custom Type Module Pattern](https://fredkschott.com/posts/designing-custom-types/) and how it can be used to create custom objects in your application. These custom types play a major role in JavaScript development, but creating them directly can quickly become a messy business. In this post I’ll introduce a new kind of module that makes working with custom types much easier: The Factory.

### WHAT IS A FACTORY?

To understand what a factory does, you should first know the problem that it tries to solve. Creating a new custom object directly requires a call to the constructor with the `new` keyword and any required arguments. This offers complete control over creation: Powerful… but not always feasible.

```javascript
/* Using Custom Types Only */
var Widget = require('./lib/widget');

// Somewhere in your application...
var redWidget = new Widget(42, true);
redWidget.paintPartA('red');
redWidget.paintPartB('red');
redWidget.paintPartC('red');

// Somewhere else in your application...
var blueWidget = new Widget(42, true);
blueWidget.paintPartA('blue');
blueWidget.paintPartB('blue');
blueWidget.paintPartC('blue');

// And somewhere else again...
var greenWidget = new Widget(42, true);
greenWidget.paintPartA('green');
greenWidget.paintPartB('green');
greenWidget.paintPartC('green');
```

Working with a complex constructor may be overkill in some simple cases, or you may find yourself passing the same arguments to the same constructors over and over and over again. Both situations are at best annoying, and at worst completely unscalable. No one likes copypasta in their code.

Lets introduce a factory to help clean up this mess. A factory’s job is to create objects so that you don’t have to, which can be advantageous for several reasons. If you are creating a public module to share on npm or even privately within your own team, adding a factory interface can make your module easier to work with. This is especially true if setup requires additional steps after calling the constructor. A factory can consolidate and standardize on all of this custom-creation for both you and your users, dictating exactly how new objects should be created.

Using a factory, the example above becomes:

```javascript
/* Refactored to use a Factory */
var widgetFactory = require('./lib/widget-factory');

var redWidget = widgetFactory.getRedWidget();
var blueWidget = widgetFactory.getBlueWidget();
var greenWidget = widgetFactory.getGreenWidget();
```

If you find yourself struggling with complex object creation across multiple parts of your application, this pattern could be the answer. By centralizing this logic in a single place, you keep it from spreading across the rest of your code base where it shouldn’t belong.

### SEPARATE THE PRODUCT

Before creating a factory, make sure that any [custom types](https://fredkschott.com/posts/designing-custom-types/) that you plan to use are already defined as separate modules. It can be tempting to keep both the factory and the product (the object returned by a factory) in the same file, but remember your training: [small, single purpose modules](https://fredkschott.com/introduction/#build-small-single-purpose-modules). Separating the two modules will reduce confusion between the different responsibilities of each. In practice, testing coverage and code re-use are both improved as well.

```javascript
/* Bad! */
function Widget(cogs, bool) {
    // Define widget custom type here
}

module.exports = { // ...
/* Good! */
var Widget = require('./widget');

module.exports = { // ...
```

### THE FACTORY DESIGN PATTERN

Both singleton and custom type patterns can be used as the base for your new factory. If you know that your entire application can share a single factory object, then building it as a singleton is almost always the right choice. A singleton can be easily required anywhere, and won’t need any complex construction or passing around of its own. But remember, your state is limited to the single module.

```javascript
/* A Factory Implemented as a Singleton */
var Widget = require('./widget');

module.exports = {
    getRedWidget: function getRedWidget() {
        var widget = new Widget(42, true);
        widget.paintPartA('red');
        widget.paintPartB('red');
        widget.paintPartC('red');
        return widget;
    },
    getBlueWidget: function getBlueWidget() {
        // ...
    }
}
```

But sharing the same factory across your application won’t always cut it. Sometimes, a factory needs some set up and customization of its own. If this is the case, design your factory using the custom type pattern instead. This way your factory can expose some basic level of flexibility with each instance, while still encapsulating the product customization.

```javascript
/* A Factory Implemented as a Custom Type */
var Widget = require('./lib/widget');

var WidgetFactory = module.exports = function WidgetFactory(options) {
    this.cogs = options.cogs;
    this.bool = options.bool;
}

WidgetFactory.prototype.getRedWidget = function getRedWidget() {
    var widget = new Widget(this.cogs, this.bool);
    widget.paintPartA('red');
    widget.paintPartB('red');
    widget.paintPartC('red');
    return widget;
};

WidgetFactory.prototype.getBlueWidget = function getBlueWidget() {
    // ...
};
/* Usage of a Custom-Type Factory */
var WidgetFactory = require('./lib/widget-factory');

var widgetFactory42  = new WidgetFactory({cogs: 42,  bool: true});
var widgetFactory101 = new WidgetFactory({cogs: 101, bool: true});
```

### ABSTRACT FACTORIES

Once you are familiar with factories, you can begin to get creative with how you use them. The **Abstract Factory** pattern, for instance, offers even greater flexibility by taking the whole concept one step further. If you have several factories that each create different products from the same parent type, you can create a common interface between them. This lets you to swap between the factories themselves (depending on which type of object you’d like to create) without modifying the rest of your code.

Consider the Express application below:

```javascript
/* Original Code */

function someMiddleware(req, res, next) {
    if(/* user is logged in */) {
        req.loggedInUserFactory = new LoggedInUserFactory(req.session.userID);
    } else {
        req.loggedOutUserFactory = new LoggedOutUserFactory();
    }
    next(); // next() will call nextMiddleware() with the same req/res objects
}

function nextMiddleware(req, res, next) {
    var user, credentials;
    if(/* user is logged in */) {
        user = req.loggedInUserFactory.createUser();
        credentials = req.loggedInUserFactory.createCredentials();
    } else {
        user = req.loggedOutUserFactory.createAnonymousUser();
        credentials = req.loggedOutUserFactory.createAnonymousCredentials();
    }
    res.send('Welcome back, ' + user.fullName);
}
```

Here you have two different factories for logged in and logged out users. They don’t share a common interface (`createUser()` vs. `createAnonymousUser()`) which means you will always need to check which kind of factory you’re working with before using it. This leads to lots of unnecessary `/*user is logged in */` checks throughout the application, when really all you need is one.

If you can consolidate each interface into something common across both, you will have implemented an abstract factory. Then, each factory can be used without worrying which type of user is being created. That decision is made once, and then carried on by the rest of your application.

```javascript
/* Original Code Refactored to Use an Abstract Factory */

function someMiddleware(req, res, next) {
    if(/* user is logged in */) {
        req.userFactory = new LoggedInUserFactory(req.session.userID);
    } else {
        req.userFactory = new LoggedOutUserFactory();
    }
    next(); // This will call nextMiddleware() with the same req/res objects
}

function nextMiddleware(req, res, next) {
    var user = req.userFactory.getUser();
    var credentials = req.userFactory.getCredentials();
    res.send('Welcome back, ' + user.fullName);
}
```

### CONCLUSION

A good custom type interface should be flexible enough to support all reasonable use cases. But this flexibility can come at a cost. As the number of supported options grows, the interface and required setup can also grow more complex. Worse, creation complexity can bleed out all over your application, with copy and pasted code blocks for each new object. Factories are a clean solution to return sanity to your application, by making complex and repeated customizations simpler and more maintainable.


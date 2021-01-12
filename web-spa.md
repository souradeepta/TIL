# Do I really need a SPA framework?	

> If you work with frontend and unless you haven't been living under a rock, then you might have heard terms like SPA or React, Angular or even Vue. Do you know what it is and when you should use a SPA framework? More often than not it's too easy to fall into the trap of "I must find an excuse to use framework X". Too seldom are we asking the question, is it the right tool for the job?

In this article, we will discuss what a SPA is and when to use it. We will also build a micro SPA framework together and realize that we, with very little code, can build something that works decently. Let's also, therefore, confront ourselves with the question:

> do I need a SPA framework with all batteries included or is this micro SPA framework hack enough for most cases?

I know you most likely are going to use a SPA framework anyway, regardless of what you answer on that question, but at least know if you actually need a framework.

Here is a link to a repo with the full solution:

> [Repo](https://github.com/softchris/vanilla-spa)

## What is a SPA framework

SPA stands for single page application. It means your application lives on one page and one page only.

> That doesn't seem very useful, most of my apps are multi-page?

I didn't say you can't have multiple pages, you just never leave that page

> You're talking in riddles, please explain *confused*

Ok, here's the thing. You stay on this one page but we can still switch out part of the content on that page, giving the impression that you are routing from one page to the next. So you would have static parts on the page like *header* and *footer* but some middle part that would change based on for example selecting a menu option.

> Ok that makes sense. But what about the URL in the browser, won't it look the same?

Actually, what we are changing is something called the hash, `#` so instead of your route going from say `home.html to products.html`, your route would shift from `someBaseUrl#/home` to `someBaseUrl#/products`.

> But I'm sure I've seen normal routes even when people use SPA applications?

Yes, most SPA frameworks have a way to rewrite the URL using `history.pushState` and also use a catch-all route to ensure you can write `someBaseUrl/products` instead.

> Ok fair enough.

##  Why use a SPA framework

Like everything else in tech and life, use the right tool for the job. As tempting as it is to use a SPA framework for everything frontend, it's not always the right approach.

So what problem does it solve? Flickering and sluggish UI is what SPAs are here to solve. Back in the days when there were no SPA frameworks, an app would completely reload the page when going from one page to the next. This caused a sense of it not feeling fast and snappy like a client application. So with SPA frameworks we suddenly got *client-like* web applications.

This came with the drawback though of not indexing well on search engines as most pages were dynamic and not *crawlable*. This problem has been and is being, addressed at most major SPA frameworks and the solution is often to generate static pages from your app. This is not a concern for all apps though. For productivity apps, it doesn't really matter but for e-commerce websites, ending up high on SEO can make or break your company.

So definitely use SPA frameworks, you will build fast apps, but also have knowledge of the drawbacks and be sure to find solutions that address those drawbacks.

##  Building a micro SPA framework

> Building a SPA framework, are you sure you've taken your meds? :)

It's fine, we are only building a small part, in order to understand those first crucial bits and in the process we will hopefully show when it goes from "I can do this with an ugly hack", to "I probably need a framework/library".

Our plan is the following:

- **Implement routing**, routing is crucial for any SPA application, we need to be able to define static parts of our page as well as dynamic parts that we can replace easily
- **Define templates and render data**, not all SPAs uses templates but a fair amount of them do like Vue.js, AngularJS, Angular, Svelte. I'll cover Reacts approach in a future article though :) What we want to achieve is to be able to render out data exactly where we need it and we should be able to do things like rendering a list of data, render data conditionally and so on

### Implement Routing

Let's start by creating two files:

```javascript
app.js
index.html
```

As we said earlier in this article, routing in a SPA is about the hash `#` sign and when it changes. The good news is that we can listen to that change with the following code:

```javascript
// app.js

async function hashHandler() {
  console.log('The hash has changed!', location.hash);
}

window.addEventListener('hashchange', hashHandler, false);
```

> Ok, now what?

Well, we just need to map different routes to different actions, like so:

```javascript
// app.js 

const appEl = document.getElementById('app');

const routes = {
  '#/': () => {
    return 'default page'
  }, 
  '#/products':() => {
    return 'Products'
  }
}

async function hashHandler() {
  console.log('The hash has changed!', location.hash);
  const hash = !location.hash ? '#/' : location.hash;
  appEl.innerHTML = await routes[hash]();
}
```

Then we can just update our `index.html` to this:

```javascript
<!-- index.html -->
<html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link href="https://fonts.googleapis.com/css?family=Roboto:300,400,500&display=swap" rel="stylesheet" />
  </head>
  <body>
    <div class="menu">
      <div class="item"><a href="#/">Home</a></div>
      <div class="item"><a href="#/products">Products</a></div>
    </div>
    <div class="app" id="app">
    </div>
  </body>
</html>
```

### Templatingv

The above isn't awfully exciting as we only can render strings based on a route change. We have a router but we want more.

> It would be great if we could somehow define a template and some data and then be able to merge the two and display that, can we have that?

We can, there are many templating libraries out there but we'll go with handlebars.

You could get a CDN link for it or download it via NPM

```
npm install handlebars --save
```

> Now what?

Now we do two things:

1. Define a template
2. Render a template on a route change

**Define a template**

We can define a template as an external file or as a `script` element in the DOM tree, we'll do the latter to keep this simple:

```javascript
<script id="hello" type="text/x-handlebars-template">
  <div>
    {{title}}
  </div>
  <div>
    {{description}}
  </div>
</script>
```

Note above that we give our template an `id` with value `hello` and also that we set the type to `text/x-handlebars-template`. This makes it possible for `handlebars` to find this template.

**Rendering the template**

Rendering the template is as simple as calling the following code:

```
var template = $('#hello').html();

// Compile the template data into a function
var templateScript = Handlebars.compile(template);
var html = templateScript({ title: 'some title', description: 'some description' });
```

At this point, our variable `html` contains a piece of HTML that we can append to our DOM tree. Let's make this code fit into our app, like so:

```javascript
// app.js 

const appEl = document.getElementById('app');

function buildTemplate(tmpId, context) {
  var template = $('#' + tmpId).html();

  // Compile the template data into a function
  var templateScript = Handlebars.compile(template);
  var html = templateScript(context);
  return html;
}

const routes = {
  '#/': () => {
    return buildTemplate('hello', { title: 'my title', description: 'my description' })
  }, 
  '#/products':() => {
    return 'Products'
  }
}

async function hashHandler() {
  console.log('The hash has changed!', location.hash);
  const hash = !location.hash ? '#/' : location.hash;
  appEl.innerHTML = await routes[hash]();
}
```

Ok great, we got some basic templating, what about lists? Well, the way handlebars solve that is by the following syntax in the template:

```javascript
<script id="cats-list" type="text/x-handlebars-template">
  <div class="products">
  {{#each products}}
    <div class="product">
    {{title}} {{description}}
    </div>
  {{/each}}
  </div>
</script>
```

Let's zoom in `{{#each products}}` and the end tag `{{/each}}`, this allows us to render a list. Now to `app.js` and update our `/products` route:

```javascript
// app.js 

const appEl = document.getElementById('app');

function buildTemplate(tmpId, context) {
  var template = $('#' + tmpId).html();

  // Compile the template data into a function
  var templateScript = Handlebars.compile(template);
  var html = templateScript(context);
  return html;
}

const routes = {
  '#/': () => {
    return buildTemplate('hello', { title: 'my title', description: 'my description' })
  }, 
  '#/products':() => {
    return buildTemplate('products', { products: [{ id:1, title: 'IT', scary book }, { id:2, title: 'The Shining', 'not a fan of old houses' }] })
  }
}

async function hashHandler() {
  console.log('The hash has changed!', location.hash);
  const hash = !location.hash ? '#/' : location.hash;
  appEl.innerHTML = await routes[hash]();
}
```

There's way more that `handlebars` can do for us like conditional logic and built-in directives and ability to define your own. Have a look here for the full reference:

> https://handlebarsjs.com/

## Event handling

> What about events?

Well, it's pure JavaScript so just hook up whatever event you have with a handler like so:

```javascript
<script id="cats-list" type="text/x-handlebars-template">
  <div class="products">
  {{#each products}}
    <div class="product">
    {{title}} {{description}}
    </div>
    <button onclick="buy({{id}})">Buy</button>
  {{/each}}
  </div>
</script>
```

and in `app.js` we just need a method `buy()`, like so:

```javascript
function buy(id) {
  console.log('should call an endpoint', id);
}
```

### Async data

Ok, how do we work with the backend, simple, through `fetch()`, like so:

```javascript
'#/products': async() => {
    const res = await fetch('http://localhost:3000/products')
    const json = await res.json();
    return buildTemplate('products', { products: json })
  }
```

## Summary

So do you need a SPA? It depends if all you want is to render lists and have a few conditional logic here and there I would argue you don't. SPAs comes with a lot of other things though like optimized rendering. I bet this approach is gasping for air with a few 100 rendered elements. SPAs usually are accompanied by things like state management that easily hooks into the SPA itself and you get things like Server Side Rendering and Progressive Web Apps with almost no effort. So it might sound like I'm arguing for YAGNI (You ain't gonna need it)? As we all know though, that small hack you did on a Friday is two years later part of a critical business system so you should probably go with React, Angular, Vue.js or Svelte, etc.

At least I hope I've shown you that you can get a lot of SPA-like behavior in under 30 min. The important part I wanted to convey is - know when you need a SPA approach, and also know that in some cases going for a full framework might be overkill, just sayin ;)
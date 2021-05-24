# How to Build and Structure a Node.js MVC Application

By [James Kolce](https://www.sitepoint.com/author/jkolce), [Nilson Jacques](https://www.sitepoint.com/author/njacques)

**In a non-trivial application, the architecture is as important as the quality of the code itself. We can have well-written pieces of code, but if we don’t have good organization, we’ll have a hard time as the complexity increases. There’s no need to wait until the project is half-way done to start thinking about the architecture; the best time is before starting, using our goals as beacons for our choices.**

Node.js doesn’t have a de facto framework with strong opinions on architecture and code organization in the same way that Ruby has the Rails framework, for example. As such, it can be difficult to get started with building full web applications with Node.

In this tutorial, we’re going to build the basic functionality of a note-taking app using the MVC architecture. To accomplish this, we’re going to employ the [Hapi.js](https://hapi.dev/) framework for [Node.js](https://nodejs.org/en/) and [SQLite](http://www.sqlite.org/) as a database, using [Sequelize.js](https://sequelize.org/v3/), plus other small utilities, to speed up our development. We’re going to build the views using [Pug](https://pugjs.org/api/getting-started.html), the templating language.

## What is MVC?

Model-View-Controller (or MVC) is probably one of the most popular architectures for applications. As with a lot of other [cool things in computer history](https://youtu.be/NdSD07U5uBs?t=29m2s), the MVC model was conceived at [PARC](https://en.wikipedia.org/wiki/PARC_(company)) for the Smalltalk language as a solution to the problem of organizing applications with graphical user interfaces. It was created for desktop applications, but since then, the idea has been adapted to other mediums including the Web.

We can describe the MVC architecture in simple terms:

- **Model**: the part of our application that will deal with the database or any data-related functionality.
- **View**: everything the user will see — basically, the pages that we’re going to send to the client.
- **Controller**: the logic of our site, and the glue between models and views. Here we call our models to get the data, then we put that data on our views to be sent to the users.

Our application will allow us to create, view, edit and delete plain-text notes. It won’t have other functionality, but because we’ll have a solid architecture already defined we won’t have a lot of trouble adding things later.

This tutorial assumes you have a recent version of Node installed on your machine. If this isn’t the case, please consult our [tutorial on getting up and running with Node](https://www.sitepoint.com/quick-tip-multiple-versions-node-nvm/).

You can check out the final application in the [accompanying GitHub repository](https://github.com/sitepoint-editors/notes-board), so you get a general overview of the application structure.

## Laying out the Foundation

The first step when building any Node.js application is to create a `package.json` file, which is going to contain all of our dependencies and scripts. Instead of creating this file manually, npm can do the job for us using the `init` command:

```bash
mkdir notes-board
cd notes-board
npm init -y
```

After the process is complete, we’ll have a `package.json` file ready to use.

*Note: if you’re not familiar with these commands, checkout our [Beginner’s Guide to npm](https://www.sitepoint.com/beginners-guide-node-package-manager/).*

We’re going to proceed to install Hapi.js — the framework of choice for this tutorial. It provides a good balance between simplicity, stability and features that will work well for our use case (although there are other options that would also work just fine).

```bash
npm install @hapi/hapi@18.4.0
```

This command will download Hapi.js and add it to our `package.json` file as a dependency.

*Note: We’ve specified v18.4.0 of Hapi.js, as it’s compatible with Node versions 8, 10, and 12. If you’re using Node 12, you can opt to install the latest version (Hapi v19.1.0).*



Now we can create our entry file — the web server that will start everything. Go ahead and create a `server.js` file in your application directory and add the following code to it:

```javascript
"use strict";

const Hapi = require("@hapi/hapi");
const Settings = require("./settings");

const init = async () => {
  const server = new Hapi.Server({ port: Settings.port });

  server.route({
    method: "GET",
    path: "/",
    handler: (request, h) => {
      return "Hello, world!";
    }
  });

  await server.start();
  console.log(`Server running at: ${server.info.uri}`);
};

process.on("unhandledRejection", err => {
  console.log(err);
  process.exit(1);
});

init();
```

This is going to be the foundation of our application.

First, we indicate that we’re going to use [strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode), which is a [common practice](https://hapi.dev/policies/styleguide/#strict-mode) when using the Hapi.js framework.

Next, we include our dependencies and instantiate a new server object where we set the connection port to `3000` (the port can be any number [above 1023 and below 65535](http://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xml)).



Our first route for our server will work as a test to see if everything is working, so a “Hello, world!” message is enough for us. In each route, we have to define the HTTP method and path (URL) that it will respond to, and a handler, which is a function that will process the HTTP request. The handler function can take two arguments: `request` and `h`. The first one contains information about the HTTP call, and the second will provide us with methods to handle our response to that call.

Finally, we start our server with the `server.start()` method.

## Storing Our Settings

It’s good practice to store our configuration variables in a dedicated file. This file exports a JSON object containing our data, where each key is assigned from an environment variable — but without forgetting a fallback value.

In this file, we can also have different settings depending on our environment (such as development or production). For example, we can have an in-memory instance of SQLite for development purposes, but a real SQLite database file on production.

Selecting the settings depending on the current environment is quite simple. Since we also have an `env` variable in our file which will contain either `development` or `production`, we can do something like the following to get the database settings:

```javascript
const dbSettings = Settings[Settings.env].db;
```

So `dbSettings` will contain the setting of an in-memory database when the `env` variable is `development`, or will contain the path of a database file when the `env` variable is `production`.

Also, we can add support for a `.env` file, where we can store our environment variables locally for development purposes. This is accomplished using a package like [dotenv](https://www.npmjs.com/package/dotenv) for Node.js, which will read a `.env` file from the root of our project and automatically add the found values to the environment.

*Note: if you decide to also use a `.env` file, make sure you install the package with `npm install dotenv` and add it to `.gitignore` so you don’t publish any sensitive information.*

Our `settings.js` file will look like this:



```javascript
// This will load our .env file and add the values to process.env,
// IMPORTANT: Omit this line if you don't want to use this functionality
require("dotenv").config({ silent: true });

module.exports = {
  port: process.env.PORT || 3000,
  env: process.env.NODE_ENV || "development",

  // Environment-dependent settings
  development: {
    db: {
      dialect: "sqlite",
      storage: ":memory:"
    }
  },
  production: {
    db: {
      dialect: "sqlite",
      storage: "db/database.sqlite"
    }
  }
};
```

Now we can start our application by executing the following command and navigating to [http://localhost:3000](http://localhost:3000/) in our web browser:

```bash
node server.js
```

*Note: this project was tested on Node v12.15.0. If you get any errors, ensure you have an updated installation.*

## Defining the Routes

The definition of routes gives us an overview of the functionality supported by our application. To create our additional routes, we just have to replicate the structure of the route that we already have in our `server.js` file, changing the content of each one.

Let’s start by creating a new directory called `lib` in our project. Here we’re going to include all the JS components.

Inside `lib`, let’s create a `routes.js` file and add the following content:

```javascript
"use strict";
const Path = require("path");

module.exports = [
  // we’re going to define our routes here
];
```

In this file, we’ll export an array of objects that contain each route of our application. To define the first route, add the following object to the array:

```javascript
{
  method: "GET",
  path: "/",
  handler: (request, h) => {
    return "All the notes will appear here";
  },
  config: {
    description: "Gets all the notes available"
  }
},
```

Our first route is for the home page (`/`), and since it will only return information, we assign it a `GET` method. For now, it will only give us the message “All the notes will appear here”, which we’re going to change later for a controller function. The `description` field in the `config` section is only for documentation purposes.

Then, we create the four routes for our notes under the `/note/` path. Since we’re building a [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) application, we’ll need one route for each action with the corresponding [HTTP methods](http://www.w3schools.com/tags/ref_httpmethods.asp).

Add the following definitions next to the previous route:

```javascript
{
  method: "POST",
  path: "/note",
  handler: (request, h) => {
    return "New note";
  },
  config: {
    description: "Adds a new note"
  }
},
{
  method: "GET",
  path: "/note/{slug}",
  handler: (request, h) => {
    return "This is a note";
  },
  config: {
    description: "Gets the content of a note"
  }
},
{
  method: "PUT",
  path: "/note/{slug}",
  handler: (request, h) => {
    return "Edit a note";
  },
  config: {
    description: "Updates the selected note"
  }
},
{
  method: "GET",
  path: "/note/{slug}/delete",
  handler: (request, h) => {
    return "This note no longer exists";
  },
  config: {
    description: "Deletes the selected note"
  }
}
```

We’ve done the same as in the previous route definition, but this time we’ve changed the method to match the action we want to execute.

The only exception is the delete route. In this case, we’re going to define it with the `GET` method rather than `DELETE` and add an extra `/delete` in the path. This way, we can call the delete action just by visiting the corresponding URL.

*Note: if you plan to implement a strict REST interface, then you would have to use the `DELETE` method and remove the `/delete` part of the path.*

We can name parameters in the path by surrounding the word in curly braces. Since we’re going to identify notes by a slug, we add `{slug}` to each path, with the exception of the `POST` route; we don’t need it there because we’re not going to interact with a specific note, but to create one.



You can read more about Hapi.js routes on the [official documentation](https://hapi.dev/tutorials/routing/?lang=en_US).

Now, we have to add our new routes to the `server.js` file. Let’s import the routes file at the top of the file:

```javascript
const Routes = require("./lib/routes");
```

Then let’s replace our current test route with the following:

```javascript
server.route(Routes);
```

## Building the Models

Models allow us to define the structure of the data and all the functions to work with it.

In this example, we’re going to use the [SQLite](https://www.sqlite.org/index.html) database with [Sequelize.js](https://sequelize.org/v3/), which is going to provide us with a better interface using the ORM ([Object-Relational Mapping](https://en.wikipedia.org/wiki/Object-relational_mapping)) technique. It will also provide us a database-independent interface.

### Setting up the Database

You can install SQLite and Sequelize by executing the following command:

```bash
npm install sequelize sqlite3
```

Now create a `models` directory inside `lib/` with a file called `index.js`, which is going to contain the database and Sequelize.js setup, and include the following content:

```javascript
"use strict";

const Fs = require("fs");
const Path = require("path");
const Sequelize = require("sequelize");
const Settings = require("../../settings");
const dbSettings = Settings[Settings.env].db;

const sequelize = new Sequelize(
  dbSettings.database,
  dbSettings.user,
  dbSettings.password,
  dbSettings
);
const db = {};

Fs.readdirSync(__dirname)
  .filter(file => file.indexOf(".") !== 0 && file !== "index.js")
  .forEach(file => {
    const model = sequelize.import(Path.join(__dirname, file));
    db[model.name] = model;
  });

db.sequelize = sequelize;
db.Sequelize = Sequelize;

module.exports = db;
```

First, we include the modules that we’re going to use:

- `Fs`, to read the files inside the `models` folder, which is going to contain all the models
- `Path`, to join the path of each file in the current directory
- `Sequelize`, which will allow us to create a new Sequelize instance
- `Settings`, which contains the data of our `settings.js` file from the root of our project

Next, we create a new `sequelize` variable that will contain a `Sequelize` instance with our database settings for the current environment. We’re going to use `sequelize` to import all the models and make them available in our `db` object.

The `db` object is going to be exported and will contain our database methods for each model. It will be available in our application when we need to do something with our data.

To load all the models, instead of defining them manually, we look for all the files inside the `models` directory (with the exception of the `index.js` file) and load them using the `import` function. The returned object will provide us with the CRUD methods, which we then add to the `db` object.

At the end, we add `sequelize` and `Sequelize` as part of our `db` object. The first one is going to be used in our `server.js` file to connect to the database before starting the server, and the second one is included for convenience if you need it in other files too.

### Creating Our Note Model

In this section, we’re going to use the [Moment.js](http://momentjs.com/) package to help with date formatting. You can install it and include it as a dependency with the following command:



```bash
npm install moment
```

We’re going to create a `note.js` file inside the `models` directory, which is going to be the only model in our application. It will provide us with all the functionality we need.

Add the following content to that file:

```javascript
"use strict";

const Moment = require("moment");

module.exports = (sequelize, DataTypes) => {
  const Note = sequelize.define("Note", {
    date: {
      type: DataTypes.DATE,
      get: function() {
        return Moment(this.getDataValue("date")).format("MMMM Do, YYYY");
      }
    },
    title: DataTypes.STRING,
    slug: DataTypes.STRING,
    description: DataTypes.STRING,
    content: DataTypes.STRING
  });

  return Note;
};
```

We export a function that accepts a `sequelize` instance, to define the model, and a `DataTypes` object with all the types available in our database.

Next, we define the structure of our data using an object where each key corresponds to a database column and the value of the key defines the type of data we’re going to store. You can see the list of data types in the [Sequelize.js documentation](http://docs.sequelizejs.com/en/latest/api/datatypes/). The tables in the database are going to be created automatically based on this information.

In the case of the date column, we also define a how Sequelize should return the value using a [getter](https://sequelize.org/master/manual/getters-setters-virtuals.html) function (`get` key). We indicate that before returning the information. It should be first passed through the Moment utility to be formatted in a more readable way (`MMMM Do, YYYY`).

*Note: although we’re getting a simple and easy-to-read date string, it’s stored as a precise date string product of JavaScript’s [Date](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Date) object. So this is not a destructive operation.*

Finally, we return our model.

### Synchronizing the Database

We now have to synchronize our database before we’re able to use it in our application. In `server.js`, import the models at the top of the file:

```javascript
// Import the index.js file inside the models directory
const Models = require("./lib/models/");
```

Next, remove the following code block:

```javascript
await server.start();
console.log(`Server running at: ${server.info.uri}`);
```

Replace it with this one:

```javascript
await Models.sequelize.sync();

await server.start();
console.log(`Server running at: ${server.info.uri}`);
```

This code is going to synchronize the models to our database. Once that’s done, the server will be started.

## Building the Controllers

Controllers are functions that accept the [request](https://hapi.dev/api#request-properties) and [response toolkit](https://hapi.dev/api/#response-toolkit) objects from Hapi.js. The `request` object contains information about the requested resource, and we use `reply` to return information to the client.

In our application, we’re going to return only a JSON object for now, but we’ll add the views once we build them.



We can think of controllers as functions that will join our models with our views; they will communicate with our models to get the data, and then return that data inside a view.

### The Home Controller

The first controller that we’re going to build will handle the home page of our site. Create a `home.js` file inside a `lib/controllers` directory with the following content:

```javascript
"use strict";

const Models = require("../models/");

module.exports = async (request, h) => {
  const result = await Models.Note.findAll({
    order: [["date", "DESC"]]
  });

  return {
    data: {
      notes: result
    },
    page: "Home — Notes Board",
    description: "Welcome to my Notes Board"
  };
};
```

First, we get all the notes in our database using the [`findAll`](https://sequelize.org/master/manual/model-querying-basics.html) method of our model. This function will return a Promise and, if it resolves, we’ll get an array containing all the notes in our database.

We can arrange the results in descending order, using the `order` parameter in the options object passed to the `findAll` method, so the last item will appear first. You can check all the available options in the [Sequelize.js documentation](https://sequelize.org/master/class/lib/model.js~Model.html#static-method-findAll).

Once we have the home controller, we can edit our `routes.js` file. First, we import the module at the top of the file, next to the `Path` module import:

```javascript
const Home = require("./controllers/home");
```

Then we add the controller we just made to the array:

```javascript
{
  method: "GET",
  path: "/",
  handler: Home,
  config: {
    description: "Gets all the notes available"
  }
},
```

You can check things are working at this point by restarting the server (`node server.js`) and visiting http://localhost:3000/. You should see the following response:

```javascript
{
  "data": { "notes": [] },
  "page":"Home — Notes Board",
  "description":"Welcome to my Notes Board"
}
```

### Boilerplate of the Note Controller

Since we’re going to identify our notes with a slug, we can generate one using the title of the note and the [slug](https://www.npmjs.com/package/slug) library, so let’s install it and include it as a dependency with the following command:

```bash
npm install slug
```

The last controller that we have to define in our application will allow us to create, read, update, and delete notes.

We can proceed to create a `note.js` file inside the `lib/controllers` directory and add the following content:

```javascript
"use strict";

const { Note } = require("../models/");
const Slugify = require("slug");
const Path = require("path");

module.exports = {
  // Here we’re going to include our functions that will handle the remaining requests in the routes.js file.
};
```

### The `create` Function

To add a note to our database, we’re going to write a `create` function that’s going to wrap the `create` method on our model using the data contained in the payload object.

Add the following inside the object that we’re exporting:

```javascript
create: async (request, h) => {
  const result = await Note.create({
    date: new Date(),
    title: request.payload.noteTitle,
    slug: Slugify(request.payload.noteTitle, { lower: true }),
    description: request.payload.noteDescription,
    content: request.payload.noteContent
  });

  // Generate a new note with the 'result' data
  return result;
},
```

Once the note is created, we’ll get back the note data and send it to the client as JSON using the `reply` function.



For now, we just return the result, but once we build the views in the next section, we’ll be able to generate the HTML with the new note and add it dynamically on the client. Although this is not completely necessary and will depend on how you’re going to handle your front-end logic, we’re going to return an HTML block to simplify the logic on the client.

Also, note that the date is being generated on the fly when we execute the function, using `new Date()`.

### The `read` Function

To search just one element, we use the `findOne` method on our model. Since we identify notes by their slug, the `where` filter must contain the slug provided by the client in the URL (`http://localhost:3000/note/:slug:`):

```javascript
read: async (request, h) => {
  const note = await Note.findOne({
    where: {
      slug: request.params.slug
    }
  });

  return note;
},
```

As in the previous function, we’ll just return the result, which is going to be an object containing the note information. The views are going to be used once we build them in the [Building the Views](https://www.sitepoint.com/node-js-mvc-application/#buildingtheviews) section.

### The `update` Function

To update a note, we use the `update` method on our model. It takes two objects — the new values that we’re going to replace, and the options containing a `where` filter with the note slug, which is the note that we’re going to update:

```javascript
update: async (request, h) => {
  const values = {
    title: request.payload.noteTitle,
    description: request.payload.noteDescription,
    content: request.payload.noteContent
  };

  const options = {
    where: {
      slug: request.params.slug
    }
  };

  await Note.update(values, options);
  const result = await Note.findOne(options);

  return result;
},
```

After updating our data, since our database won’t return the updated note, we can find the modified note again to return it to the client, so we can show the updated version as soon as the changes are made.

### The `delete` Function

The delete controller will remove the note by providing the slug to the `destroy` function of our model. Then, once the note is deleted, we redirect to the home page. To accomplish this, we use the [redirect](https://hapi.dev/api/?v=18.4.0#-hredirecturi) function of Hapi’s response toolkit:

```javascript
delete: async (request, h) => {
  await Note.destroy({
    where: {
      slug: request.params.slug
    }
  });

  return h.redirect("/");
}
```

### Using the Note Controller in Our Routes

At this point, we should have our note controller file ready with all the CRUD actions. But to use them, we have to include it in our routes file.

First, let’s import our controller at the top of the `routes.js` file:

```javascript
const Note = require("./controllers/note");
```

We have to replace each handler with our new functions, so we should have our routes file as follows:

```javascript
{
  method: "POST",
  path: "/note",
  handler: Note.create,
  config: {
    description: "Adds a new note",
    payload: {
      multipart: true,
    }
  }
},
{
  method: "GET",
  path: "/note/{slug}",
  handler: Note.read,
  config: {
    description: "Gets the content of a note"
  }
},
{
  method: "PUT",
  path: "/note/{slug}",
  handler: Note.update,
  config: {
    description: "Updates the selected note",
    payload: {
      multipart: true,
    }
  }
},
{
  method: "GET",
  path: "/note/{slug}/delete",
  handler: Note.delete,
  config: {
    description: "Deletes the selected note"
  }
}
```

*Note: we’re including our functions without `()` at the end, because we’re referencing our functions without calling them.*

In Hapi v19, [`request.payload.multipart` was changed to `false` by default](https://github.com/hapijs/hapi/issues/4017). We need to set it back to `true` for the `POST` and `PUT` routes, as we’ll be using a `FormData` object to transmit data to the server, and the transmitted data will be in the `multipart/form-data` format.

## Building the Views

At this point, our site is receiving HTTP calls and responding with JSON objects. To make it useful to everybody, we have to create the pages that render our information in a nice way.



In this example, we’re going to use the [Pug](https://pugjs.org/api/getting-started.html) (formerly Jade) templating language, although this isn’t mandatory, and we can use other languages with Hapi.js. We’re going to use the [Vision](https://github.com/hapijs/vision) plugin to enable the view functionality in our server.

*Note: if you’re not familiar with Jade/Pug, see our [Beginner’s Guide to Pug](https://www.sitepoint.com/a-beginners-guide-to-pug/).*

You can install the packages with the following command:

```bash
npm install @hapi/vision@5.5.4 pug
```

Here we’re installing v5.5.4 of the vision plugin, which is compatible with Hapi v18. If you’ve opted to install Hapi v19, you can simply type `npm i @hapi/vision` to pull in the latest version.

### The Note Component

First, we’re going to build the note component that’s going to be reused across our views. Also, we’re going to use this component in some of our controller functions to build a note on the fly in the back end to simplify the logic on the client.

Create a file in `lib/views/components` called `note.pug` with the following content:

```pug
article.content
  h2.title: a(href=`/note/${note.slug}`)= note.title
  p.subtitle.is-6 Published on #{note.date}
  p=note.content
```

It’s composed of the title of the note, the publication date and the content of the note.

### The Base Layout

The base layout contains the common elements of our pages — or in other words, for our example, everything that’s not content. Create a file in `lib/views/` called `layout.pug` with the following content:

```pug
doctype html
head
  meta(charset='utf-8')
  meta(name='viewport' content='width=device-width, initial-scale=1')
  title=page
  meta(name='description' content=description)
  link(rel='stylesheet' href='https://cdn.jsdelivr.net/npm/bulma@0.8.0/css/bulma.min.css')
  script(defer='' src='https://use.fontawesome.com/releases/v5.3.1/js/all.js')
body
  block content
  script(src='/scripts/main.js')
```

The content of the other pages will be loaded in place of `block content`. Also, note that we’ll display a page variable in the `title` element, and a `description` variable in the `meta(name='description')` element. We’ll create those variables in our routes later.

For styling purposes, we’re including the [Bulma CSS framework](https://bulma.io/) and [Font Awesome](https://fontawesome.com/) from a CDN. We’re also including a `main.js` file at the bottom of the page, which will contain all of our custom JavaScript code for the front end. Please create that file now in a `static/public/scripts/` directory.

### The Home View

On our home page, we’ll show a list of all the notes in our database and a button that will show a modal window with a form that allows us to create a new note via Ajax.

Create a file in `lib/views` called `home.pug` with the following content:

```pug
extends layout

block content
  section.section
    .container

      h1.title.has-text-centered
        | Notes Board

      .tabs.is-centered
        ul
          li
            a.show-modal(href='#') Publish

      main(container).notes-list
        each note in data.notes
          include components/note
          hr

      .modal
        .modal-background
        .modal-card
          header.modal-card-head
            p.modal-card-title Add note
            button.delete(aria-label='close')
          section.modal-card-body
            form(action='/note' method='POST').note-form#note-form
              .field
                .control
                  input.input(name='noteTitle' type='text' placeholder='Title')
              .field
                .control
                  input.input(name='noteDescription' type='text' placeholder='Short description')
              .field
                .control
                  textarea.textarea(name='noteContent' placeholder='Contents')
              .field
                .control
                  button.button.is-link Save
```

### The Note View

The note page is pretty similar to the home page, but in this case, we show a menu with options specific to the current note, the content of the note and the same form as in the home page, but with the current note information already filled, so it’s there when we update it.



Create a file in `lib/views` called `note.pug` with the following content:

```pug
extends layout

block content
  section.section
    .container
      h1.title.has-text-centered
          | Notes Board

      .tabs.is-centered
        ul
          li: a(href='/') Home
          li: a.show-modal(href='#') Update
          li: a(href=`/note/${note.slug}/delete`) Delete

      include components/note

      .modal
        .modal-background
        .modal-card
          header.modal-card-head
            p.modal-card-title Edit note
            button.delete(aria-label='close')
          section.modal-card-body
            form(action=`/note/${note.slug}` method='PUT').note-form#note-form
              .field
                .control
                  input.input(name='noteTitle' type='text' placeholder='Title' value=note.title)
              .field
                .control
                  input.input(name='noteDescription' type='text' placeholder='Short description' value=note.description)
              .field
                .control
                  textarea.textarea(name='noteContent' placeholder='Contents') #{note.content}
              .field
                .control
                  button.button.is-link Save
```

### The JavaScript on the Client

To create and update notes, we’ll use some JavaScript, both to show/hide a modal with a form, and to submit the requests via Ajax. Although this is not strictly necessary, we feel it provides a better experience for the user.

This is the content of our `main.js` file in the `static/public/scripts/` directory:

```javascript
// Modal

const modal = document.querySelector(".modal");
const html = document.querySelector("html");

const showModal = () => {
  modal.classList.add("is-active");
  html.classList.add("is-clipped");
};

const hideModal = () => {
  modal.classList.remove("is-active");
  html.classList.remove("is-clipped");
};

document.querySelector("a.show-modal").addEventListener("click", function(e) {
  e.preventDefault();
  showModal();
});

modal.querySelector(".modal .delete").addEventListener("click", function(e) {
  e.preventDefault();
  hideModal();
});

// Form submition

const form = document.querySelector("#note-form");
const url = form.getAttribute("action");
const method = form.getAttribute("method");

const prependNote = html => {
  const notesList = document.querySelector(".notes-list");
  const div = document.createElement("div");
  div.innerHTML = html;
  notesList.insertBefore(div.firstChild, notesList.firstChild);
};

const updateNote = html => {
  const article = document.querySelector("article");
  const div = document.createElement("div");
  div.innerHTML = html;
  article.parentNode.replaceChild(div.firstChild, article);
};

const onSuccess = html => {
  hideModal();
  form.reset();

  if (method === "POST") {
    prependNote(html);
  } else if (method === "PUT") {
    updateNote(html);
  }
};

form.addEventListener("submit", e => {
  e.preventDefault();

  fetch(url, {
    method,
    body: new FormData(form)
  })
    .then(response => response.text())
    .then(text => onSuccess(text))
    .catch(error => console.error(error));
});
```

Every time the user submits the form in the modal window, we get the information from the form elements and send it to our back end, depending on the action URL and the method (`POST` or `PUT`). Then, we’ll get the result as a block of HTML containing our new note data. When we add a note, we’ll just add it on top of the list on the home page, and when we update a note we replace the content for the new one in the note view.

### Adding Support for Views on the Server

To make use of our views, we have to include them in our controllers and add the required settings.

In our `server.js` file, let’s import the Node [Path](https://nodejs.org/api/path.html) utility at the top of the file, since we’re using it in our code to indicate the path of our views:

```javascript
const Path = require("path");
```

Now, replace the `server.route(Routes);` line with the following code block:

```javascript
await server.register([require("@hapi/vision")]);

server.views({
  engines: { pug: require("pug") },
  path: Path.join(__dirname, "lib/views"),
  compileOptions: {
    pretty: false
  },
  isCached: Settings.env === "production"
});

// Add routes
server.route(Routes);
```

In the code we’ve added, we first register the [Vision](https://github.com/hapijs/vision) plugin with our Hapi.js server, which is going to provide the view functionality. Then we add the settings for our views — like the engine we’re going to use and the path where the views are located. At the end of the code block, we add our routes back in.

This will make our views work on the server, but we still have to declare the view that we’re going to use for each route.

### Setting the Home View

Open the `lib/controllers/home.js` file and replace the `return` statement with the following:

```javascript
return h.view('home', {
  data: {
    notes: result
  },
  page: 'Home — Notes Board',
  description: 'Welcome to my Notes Board'
});
```

After registering the Vision plugin, we now have a `view` method available on the reply object. We’re going to use it to select the `home` view in our `views` directory and to send the data that’s going to be used when rendering the views.

In the data that we provide to the view, we also include the page title and a meta description for search engines.

If you’d like to try things out at this point, head to http://localhost:3000/. You should see a nicely styled notes board, with a **Publish** button that doesn’t do anything.



### Setting the Note View: `create` Function

Right now, every time we create a note we send a JSON object from the server to the client. But since we’re doing this process with Ajax, we can send the new note as HTML ready to be added to the page. To do this, we render the *note* component with the data we have.

Start off by requiring Pug at the top of the `controllers/note.js` file:

```javascript
const Pug = require("pug");
```

Then, in the `create` method, replace the line `return result;` with the following code block:

```javascript
// Generate a new note with the 'result' data
return Pug.renderFile(
  Path.join(__dirname, "../views/components/note.pug"),
  {
    note: result
  }
);
```

We use the `renderFile` method from Pug to render the note template with the data we just received from our model.

### Setting the Note View: `read` Function

When we enter a note page, we should get the note template with the content of our note. To do this, we have to replace the `read` function’s `return note;` line with this:

```javascript
return h.view("note", {
  note,
  page: `${note.title} — Notes Board`,
  description: note.description
});
```

As with the home page, we select a view as the first parameter and the data that we’re going to use as the second one.

### Setting the Note View: `update` Function

Every time we update a note, we’ll reply similarly to when we create new notes. Replace the `return result;` line in the `update` function with the following code:

```javascript
// Generate a new note with the updated data
return Pug.renderFile(
  Path.join(__dirname, "../views/components/note.pug"),
  {
    note: result
  }
);
```

*Note: the delete function doesn’t need a view, since it will just redirect to the home page once the note is deleted.*

## Serving Static Files

The JavaScript and CSS files that we’re using on the client side are provided by Hapi.js from the `static/public/` directory. But it won’t happen automatically; we have to indicate to the server that we want to define this folder as public. This is done using the [Inert](https://github.com/hapijs/inert) package, which you can install with the following command:

```bash
npm install @hapi/inert
```

In the `server.register` function inside the `server.js` file, import the Inert plugin and register it with Hapi like this:

```javascript
await server.register([require("@hapi/vision"), require("@hapi/inert")]);
```

Now we have to define the route where we’re going to provide the static files, and their location on our server’s filesystem. Add the following entry at the end of the exported object in `routes.js`:

```javascript
{
  // Static files
  method: "GET",
  path: "/{param*}",
  handler: {
    directory: {
      path: Path.join(__dirname, "../static/public")
    }
  },
  config: {
    description: "Provides static resources"
  }
}
```

This route will use the `GET` method, and we’ve replaced the handler function with an object containing the directory that we want to make public.

You can find more information about serving static content in the [Hapi.js documentation](https://hapi.dev/tutorials/servingfiles/?lang=en_US).

## Conclusion

At this point, we have a very basic Hapi.js application using the MVC architecture. Although there are still things we should take care of before putting our application in production (such as input validation, error handling, error pages, and so on) this should work as a foundation to learn and build your own applications.

If you’d like to take this example a bit further, after finishing all the small details (not related the architecture) to make this a robust application, you could implement an authentication system so only registered users are able to publish and edit notes. But your imagination is the limit, so feel free to fork the [application repository](https://github.com/sitepoint-editors/notes-board) and go to town!

Dive deeper into Node.js with further reading:
# How To Create a Node.js Module

[Node.js](https://www.digitalocean.com/community/tags/node-js)[JavaScript](https://www.digitalocean.com/community/tags/javascript)

- [![img](https://community-cdn-digitalocean-com.global.ssl.fastly.net/variants/e4BHBZaHGDp2qEvvpWrAi1jS/6a540d0de9b44df9962f62205489ba6e2a5beab748fa47e816b60597a256ba9b)](https://www.digitalocean.com/community/users/stackabuse)

- By [Stack Abuse](https://www.digitalocean.com/community/users/stackabuse)

  Published onDecember 2, 2019 81.1kviews

English

*The author selected the [Open Internet/Free Speech Fund](https://www.brightfunds.org/funds/open-internet-free-speech) to receive a donation as part of the [Write for DOnations](https://do.co/w4do-cta) program.*

### Introduction

In Node.js, a *module* is a collection of JavaScript functions and objects that can be used by external applications. Describing a piece of code as a module refers less to what the code is and more to what it does—any Node.js file or collection of files can be considered a module if its functions and data are made usable to external programs.

Because modules provide units of functionality that can be reused in many larger programs, they enable you to create loosely coupled applications that scale with complexity, and open the door for you to share your code with other developers. Being able to write modules that export useful functions and data will allow you to contribute to the wider Node.js community—in fact, all packages that you use on npm were bundled and shared as modules. This makes creating modules an essential skill for a Node.js developer.

In this tutorial, you will create a Node.js module that suggests what color web developers should use in their designs. You will develop the module by storing the colors as an array, and providing a function to retrieve one randomly. Afterwards, you will run through various ways of importing a module into a Node.js application.



## Prerequisites

- You will need Node.js and npm installed on your development environment. This tutorial uses version 10.17.0. To install this on macOS or Ubuntu 18.04, follow the steps in [How To Install Node.js and Create a Local Development Environment on macOS](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-and-create-a-local-development-environment-on-macos) or the **Installing Using a PPA** section of [How To Install Node.js on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-18-04). By having Node.js installed you will also have npm installed; this tutorial uses version 6.11.3.
- You should also be familiar with the `package.json` file, and experience with npm commands would be useful as well. To gain this experience, follow [How To Use Node.js Modules with npm and package.json](https://www.digitalocean.com/community/tutorials/how-to-use-node-js-modules-with-npm-and-package-json), particularly the **Step 1 — Creating a `package.json` File**.
- It will also help to be comfortable with the Node.js REPL (Read-Evaluate-Print-Loop). You will use this to test your module. If you need more information on this, read our guide on [How To Use the Node.js REPL](https://www.digitalocean.com/community/tutorials/how-to-use-the-node-js-repl).



## Step 1 — Creating a Module

This step will guide you through creating your first Node.js module. Your module will contain a collection of colors in an array and provide a function to get one at random. You will use the Node.js built-in `exports` property to make the function and array available to external programs.

First, you’ll begin by deciding what data about colors you will store in your module. Every color will be an object that contains a `name` property that humans can easily identify, and a `code` property that is a string containing an HTML color code. HTML color codes are six-digit hexadecimal numbers that allow you to change the color of elements on a web page. You can learn more about HTML color codes by reading this [HTML Color Codes and Names](https://www.computerhope.com/htmcolor.htm) article.

You will then decide what colors you want to support in your module. Your module will contain an array called `allColors` that will contain six colors. Your module will also include a function called `getRandomColor()` that will randomly select a color from your array and return it.

In your terminal, make a new folder called `colors` and move into it:

```bash
mkdir colors
cd colors
```

 

Copy

Initialize npm so other programs can import this module later in the tutorial:

```bash
npm init -y
```

 

Copy

You used the `-y` flag to skip the usual prompts to customize your `package.json`. If this were a module you wished to publish to npm, you would answer all these prompts with relevant data, as explained in [How To Use Node.js Modules with npm and package.json](https://www.digitalocean.com/community/tutorials/how-to-use-node-js-modules-with-npm-and-package-json#step-1-—-creating-a-packagejson-file).

In this case, your output will be:

```
Output{
  "name": "colors",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

Now, open up a command-line text editor such as `nano` and create a new file to serve as the entry point for your module:

```bash
nano index.js
```

 

Copy

Your module will do a few things. First, you’ll define a `Color` [class](https://www.digitalocean.com/community/tutorials/understanding-classes-in-javascript). Your `Color` class will be instantiated with its name and HTML code. Add the following lines to create the class:

~/colors/index.js

```javascript
class Color {
  constructor(name, code) {
    this.name = name;
    this.code = code;
  }
}
```

 

Copy

Now that you have your data structure for `Color`, add some instances into your module. Write the following highlighted [array](https://www.digitalocean.com/community/tutorials/understanding-arrays-in-javascript) to your file:

~/colors/index.js

```javascript
class Color {
  constructor(name, code) {
    this.name = name;
    this.code = code;
  }
}

const allColors = [
  new Color('brightred', '#E74C3C'),
  new Color('soothingpurple', '#9B59B6'),
  new Color('skyblue', '#5DADE2'),
  new Color('leafygreen', '#48C9B0'),
  new Color('sunkissedyellow', '#F4D03F'),
  new Color('groovygray', '#D7DBDD'),
];
```

 

Copy

Finally, enter a [function](https://www.digitalocean.com/community/tutorials/how-to-define-functions-in-javascript) that randomly selects an item from the `allColors` array you just created:

~/colors/index.js

```javascript
class Color {
  constructor(name, code) {
    this.name = name;
    this.code = code;
  }
}

const allColors = [
  new Color('brightred', '#E74C3C'),
  new Color('soothingpurple', '#9B59B6'),
  new Color('skyblue', '#5DADE2'),
  new Color('leafygreen', '#48C9B0'),
  new Color('sunkissedyellow', '#F4D03F'),
  new Color('groovygray', '#D7DBDD'),
];

exports.getRandomColor = () => {
  return allColors[Math.floor(Math.random() * allColors.length)];
}

exports.allColors = allColors;
```

 

Copy

The `exports` keyword references a global object available in every Node.js module. All functions and objects stored in a module’s `exports` object are exposed when other Node.js modules import it. The `getRandomColor()` function was created directly on the `exports` object, for example. You then added an `allColors` property to the `exports` object that references the local constant `allColors` array created earlier in the script.

When other modules import this module, both `allColors` and `getRandomColor()` will be exposed and available for usage.

Save and exit the file.

So far, you have created a module that contains an array of colors and a function that returns one randomly. You have also exported the array and function, so that external programs can use them. In the next step, you will use your module in other applications to demonstrate the effects of `export`.



## Step 2 — Testing your Module with the REPL

Before you build a complete application, take a moment to confirm that your module is working. In this step, you will use the REPL to load the `colors` module. While in the REPL, you will call the `getRandomColor()` function to see if it behaves as you expect it to.

Start the Node.js REPL in the same folder as the `index.js` file:

```bash
node
```

 

Copy

When the REPL has started, you will see the `>` prompt. This means you can enter JavaScript code that will be immediately evaluated. If you would like to read more about this, follow our guide on [using the REPL](https://www.digitalocean.com/community/tutorials/how-to-use-the-node-js-repl).

First, enter the following:

```bash
colors = require('./index');
```

 

Copy

In this command, `require()` loads the `colors` module at its entry point. When you press `ENTER` you will get:

```
Output{
  getRandomColor: [Function],
  allColors: [
    Color { name: 'brightred', code: '#E74C3C' },
    Color { name: 'soothingpurple', code: '#9B59B6' },
    Color { name: 'skyblue', code: '#5DADE2' },
    Color { name: 'leafygreen', code: '#48C9B0' },
    Color { name: 'sunkissedyellow', code: '#F4D03F' },
    Color { name: 'groovygray', code: '#D7DBDD' }
  ]
}
```

The REPL shows us the value of `colors`, which are all the functions and objects imported from the `index.js` file. When you use the `require` keyword, Node.js returns all the contents within the `exports` object of a module.

Recall that you added `getRandomColor()` and `allColors` to `exports` in the `colors` module. For that reason, you see them both in the REPL when they are imported.

At the prompt, test the `getRandomColor()` function:

```bash
colors.getRandomColor();
```

 

Copy

You’ll be prompted with a random color:

```
OutputColor { name: 'groovygray', code: '#D7DBDD' }
```

As the index is random, your output may vary. Now that you confirmed that the `colors` module is working, exit the Node.js REPL:

```bash
.exit
```

 

Copy

This will return you to your terminal command line.

You have just confirmed that your module works as expected using the REPL. Next, you will apply these same concepts and load your module into an application, as you would do in a real project.



## Step 3 — Saving your Local Module as a Dependency

While testing your module in the REPL, you imported it with a *relative path*. This means you used the location of the `index.js` file in relation to the working directory to get its contents. While this works, it is usually a better programming experience to import modules by their names so that the import is not broken when the context is changed. In this step, you will install the `colors` module with npm’s local module `install` feature.

Set up a new Node.js module outside the `colors` folder. First, go to the previous directory and create a new folder:

```bash
cd ..
mkdir really-large-application
```

 

Copy

Now move into your new project:

```bash
cd really-large-application
```

 

Copy

Like with the `colors` module, initialize your folder with npm:

```bash
npm init -y
```

 

Copy

The following `package.json` will be generated:

```
Output{
  "name": "really-large-application",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

Now, install your `colors` module and use the `--save` flag so it will be recorded in your `package.json` file:

```bash
npm install --save ../colors
```

 

Copy

You just installed your `colors` module in the new project. Open the `package.json` file to see the new local dependency:

```bash
nano package.json
```

 

Copy

You will find that the following highlighted lines have been added:

~/really-large-application/package.json

```json
{
  "name": "really-large-application",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "colors": "file:../colors"
  }
}
```

 

Copy

Exit the file.

The `colors` module was copied to your `node_modules` directory. Verify it’s there with the following command:

```bash
ls node_modules
```

 

Copy

This will give the following output:

```
Outputcolors
```

Use your installed local module in this new program. Re-open your text editor and create another JavaScript file:

```bash
nano index.js
```

 

Copy

Your program will first import the `colors` module. It will then choose a color at random using the `getRandomColor()` function provided by the module. Finally, it will print a message to the console that tells the user what color to use.

Enter the following code in `index.js`:

~/really-large-application/index.js

```javascript
const colors = require('colors');

const chosenColor = colors.getRandomColor();
console.log(`You should use ${chosenColor.name} on your website. It's HTML code is ${chosenColor.code}`);
```

 

Copy

Save and exit this file.

Your application will now tell the user a random color option for a website component.

Run this script with:

```bash
node index.js
```

 

Copy

Your output will be similar to:

```
OutputYou should use leafygreen on your website. It's HTML code is #48C9B0
```

You’ve now successfully installed the `colors` module and can manage it like any other npm package used in your project. However, if you added more colors and functions to your local `colors` module, you would have to run `npm update` in your applications to be able to use the new options. In the next step, you will use the local module `colors` in another way and get automatic updates when the module code changes.



## Step 4 — Linking a Local Module

If your local module is in heavy development, continually updating packages can be tedious. An alternative would be to *link* the modules. Linking a module ensures that any updates to the module are immediately reflected in the applications using it.

In this step, you will link the `colors` module to your application. You will also modify the `colors` module and confirm that its most recent changes work in the application without having to reinstall or upgrade.

First, uninstall your local module:

```bash
npm un colors
```

 

Copy

npm links modules by using *symbolic links* (or symlinks), which are references that point to files or directories in your computer. Linking a module is done in two steps:

1. Creating a *global link* to the module. npm creates a symlink between your global `node_modules` directory and the directory of your module. The global `node_modules` directory is the location in which all your system-wide npm packages are installed (any package you install with the `-g` flag).
2. Create a *local link*. npm creates a symlink between your local project that’s using the module and the global link of the module.

First, create the global link by returning to the `colors` folder and using the `link` command:

```bash
cd ../colors
sudo npm link
```

 

Copy

Once complete, your shell will output:

```
Output/usr/local/lib/node_modules/colors -> /home/sammy/colors
```

You just created a symlink in your `node_modules` folder to your `colors` directory.

Return to the `really-large-application` folder and link the package:

```bash
cd ../really-large-application
sudo npm link colors
```

 

Copy

You will receive output similar to the following:

```
Output/home/sammy/really-large-application/node_modules/colors -> /usr/local/lib/node_modules/colors -> /home/sammy/colors
```

**Note**: If you would like to type a bit less, you can use `ln` instead of `link`. For example, `npm ln colors` would have worked the exact same way.

As the output shows, you just created a symlink from your `really-large-application`’s local `node_modules` directory to the `colors` symlink in your global `node_modules`, which points to the actual directory with the `colors` module.

The linking process is complete. Run your file to ensure it still works:

```bash
node index.js
```

 

Copy

Your output will be similar to:

```
OutputYou should use sunkissedyellow on your website. It's HTML code is #F4D03F
```

Your program functionality is intact. Next, test that updates are immediately applied. In your text editor, re-open the `index.js` file in the `colors` module:

```bash
cd ../colors
nano index.js
```

 

Copy

Now add a function that selects the very best shade of blue that exists. It takes no arguments, and always returns the third item of the `allColors` array. Add these lines to the end of the file:

~/colors/index.js

```javascript
class Color {
  constructor(name, code) {
    this.name = name;
    this.code = code;
  }
}

const allColors = [
  new Color('brightred', '#E74C3C'),
  new Color('soothingpurple', '#9B59B6'),
  new Color('skyblue', '#5DADE2'),
  new Color('leafygreen', '#48C9B0'),
  new Color('sunkissedyellow', '#F4D03F'),
  new Color('groovygray', '#D7DBDD'),
];

exports.getRandomColor = () => {
        return allColors[Math.floor(Math.random() * allColors.length)];
        }

exports.allColors = allColors;

exports.getBlue = () => {
  return allColors[2];
}
```

 

Copy

Save and exit the file, then re-open the `index.js` file in the `really-large-application` folder:

```bash
cd ../really-large-application
nano index.js
```

 

Copy

Make a call to the newly created `getBlue()` function, and print a sentence with the color’s properties. Add these statements to the end of the file:

~/really-large-application/index.js

```javascript
const colors = require('colors');

const chosenColor = colors.getRandomColor();
console.log(`You should use ${chosenColor.name} on your website. It's HTML code is ${chosenColor.code}`);

const favoriteColor = colors.getBlue();
console.log(`My favorite color is ${favoriteColor.name}/${favoriteColor.code}, btw`);
```

 

Copy

Save and exit the file.

The code now uses the newly create `getBlue()` function. Execute the file as before:

```bash
node index.js
```

 

Copy

You will get output like:

```
OutputYou should use brightred on your website. It's HTML code is #E74C3C
My favorite color is skyblue/#5DADE2, btw
```

Your script was able to use the latest function in your `colors` module, without having to run `npm update`. This will make it easier to make changes to this application in development.

As you write larger and more complex applications, think about how related code can be grouped into modules, and how you want these modules to be set up. If your module is only going to be used by one program, it can stay within the same project and be referenced by a relative path. If your module will later be shared separately or exists in a very different location from the project you are working on now, installing or linking might be more viable. Modules in active development also benefit from the automatic updates of linking. If the module is not under active development, using `npm install` may be the easier option.



## Conclusion

In this tutorial, you learned that a Node.js module is a JavaScript file with functions and objects that can be used by other programs. You then created a module and attached your functions and objects to the global `exports` object to make them available to external programs. Finally, you imported that module into a program, demonstrating how modules come together into larger applications.

Now that you know how to create modules, think about the type of program you want to write and break it down into various components, keeping each unique set of activities and data in their own modules. The more practice you get writing modules, the better your ability to write quality Node.js programs on your learning journey. To work through an example of a Node.js application that uses modules, see our [How To Set Up a Node.js Application for Production on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-node-js-application-for-production-on-ubuntu-18-04) tutorial.
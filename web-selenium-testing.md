# End to End (e2e) Testing React Apps With Selenium WebDriver And Node.js is Easier Than You Think



End to end (e2e) testing is the process of executing a tests scenario against a real browser to test the entire stack of a multi-tier application. The best tool for the job is [Selenium](http://www.seleniumhq.org/), which can pilot any web browser using a standardized API, and is actively maintained. But writing e2e tests that work every time is challenging. If you've ever given e2e tests a try, you might face intermittent failures with no good reason.

This tutorial shows how to configure and write efficient tests using Selenium Web Driver on Node.js. The use case is a simple autocomplete form - something you can only test using a real browser. Read on to grab pragmatic advice on making your e2e tests work every time.

## What Is Selenium Web Driver?

To automate browser testing, Selenium contains a server that opens a browser, and establishes a communication with it. The server waits for a client to send browser commands to be executed, then transmits these commands to the browser. The WebDriver is a client for this server: it allows to send commands from a programming language - Node.js in our example.

```text
Node.js <=> WebDriver <=> Selenium Server <=> Firefox/Chrome/IE/Safari
              Client         Server               Browser
```

Check [the WebDriver documentation](http://seleniumhq.github.io/selenium/docs/api/javascript/index.html) for an overview of its API.

Setting up the WebDriver from Node.js is easy:

```js
const webdriver = require("selenium-webdriver");

const driver = new webdriver.Builder().forBrowser("firefox").build();

// ask the browser to open a page
driver.navigate().to("http://path.to.test.app/");
```

The code examples use Node.js V5, which supports a large subset of ES6 natively.

## Promises and ControlFlow

WebDriver methods are asynchronous. For instance, locating an element by CSS selector is an async operation. Getting its value property is also an async operation. All async operations return Promises in WebDriver, so you can chain them using `then()`.

```js
const By = webdriver.By; // useful Locator utility to describe a query for a WebElement
// open a page, find autocomplete input by CSS selector, then get its value
driver
  .navigate()
  .to("http://path.to.test.app/")
  .then(() => driver.findElement(By.css(".autocomplete")))
  .then(element => element.getAttribute("value"))
  .then(value => console.log(value));
```

The `driver.findElement()` command returns a Promise for a WebElement, which is the interface to manipulate and inspect DOM elements in Selenium. WebElements promises have shortcuts to web element methods, so you can also compact lines 2 and 3 in the previous example:

```js
driver
  .navigate()
  .to("http://path.to.test.app/")
  .then(() => driver.findElement(By.css(".autocomplete")).getAttribute("value"))
  .then(value => console.log(value));
```

But as soon as you need to chain a few actions, Promises are getting annoying:

```js
const until = webdriver.until; // useful utility to wait for something to happen
// fill the input with 'John', wait for the suggestion list to appear, then click on the first suggestion ('John Doe')
driver.navigate().to('http://path.to.test.app/')
    .then(() => driver.findElement(By.css('.autocomplete')).sendKeys('John'))
    .then(() => driver.wait(until.elementLocated(By.css('.suggestion'))))
    .then(() => driver.findElement(By.css('.suggestion')).click()))
```

WebDriver promises aren't ordinary Promises. Each time you call an async WebDriver task without resolving it, the WebDriver actually pushes this task to a global queue. As soon as you resolve one promise (using `then()`), or after the next tick of the JavaScript event loop, all queued tasks are executed in the order in which they were scheduled, as if they were synchronous. This is called [ControlFlow](https://github.com/SeleniumHQ/selenium/blob/master/javascript/node/selenium-webdriver/lib/promise.js#L20):

```js
// add a promise to the task queue
driver.navigate().to('http://path.to.test.app/');
// add a second promise to the task queue, as if using then()
driver.findElement(By.css('.autocomplete')).sendKeys('John');
// add a third promise to the task queue, as if using then()
driver.wait(until.elementLocated(By.css('.suggestion')));
// add a fourth promise to the task queue, as if using then()
driver.findElement(By.css('.suggestion')).click()
// execute all promises in the task queue
    .then(() => ...)
```

Now you only need a promise on the last element, and the code is much more readable. This seems a bit magical, because there are four async operations here, each returning a promise, but they get executed sequentially. ControlFlow makes Selenium tests easier to write, so we use it extensively.

## Promises In Tests

The previous example returns a promise. If you must use it for testing, this forces you to use asynchronous tests. Inside [Mocha](https://mochajs.org/), this means telling the test runner when the test is finished - by calling the `done` function parameter.

```js
//in e2e/tests/autocomplete.js
const webdriver = require("selenium-webdriver");

const driver = new webdriver.Builder().forBrowser("firefox").build();

describe("login form", () => {
  // e2e tests are too slow for default Mocha timeout
  this.timeout(10000);

  before(function(done) {
    driver
      .navigate()
      .to("http://path.to.test.app/")
      .then(() => done());
  });

  it("autocompletes the name field", function(done) {
    driver.findElement(By.css(".autocomplete")).sendKeys("John");
    driver.wait(until.elementLocated(By.css(".suggestion")));
    driver
      .findElement(By.css(".suggestion"))
      .click()
      .then(() => done());
  });

  after(function(done) {
    driver.quit().then(() => done());
  });
});
```

Don't forget to quit the driver at the end of your test suite ; this closes the test browser window.

There is a better way: instead of calling `done()`, Mocha accepts a promise as return value. If a test returns a promise, Mocha understands that it's asynchronous, and waits for the Promise to be resolved before passing to the next test.

```js
before(() => driver.navigate().to("http://path.to.test.app/"));

it("autocompletes the name field", function() {
  driver.findElement(By.css(".autocomplete")).sendKeys("John");
  driver.wait(until.elementLocated(By.css(".suggestion")));
  return driver.findElement(By.css(".suggestion")).click();
});

after(() => driver.quit());
```

**Note**: If you forget to return the last promise, Mocha won't notice that the test is asynchronous, so it will proceed to the next test without waiting for the end of the execution of this one. So it's important to **always return a promise** at the end of an async test in Mocha.

## Async assertions

With ControlFlow and Mocha, the test remains quite readable so far, without too much indentation or Promise complications. The problem comes with assertions.

```js
const chai = require("chai");
const expect = chai.expect;
it("autocompletes the name field", function() {
  driver.findElement(By.css(".autocomplete")).sendKeys("John");
  driver.wait(until.elementLocated(By.css(".suggestion")));
  driver.findElement(By.css(".suggestion")).click();
  return driver
    .findElement(By.css(".autocomplete"))
    .getAttribute("value")
    .then(inputValue => expect(inputValue).to.equal("John Doe"));
});
```

Here comes the promise chain again, and this is painful. A first workaround is to use an assertion library that accepts promises - for instance [`chai-as-promised`](https://github.com/domenic/chai-as-promised). You can then use `expect()` on a promise in conjunction with the `eventually` keyword:

```js
const chai = require("chai");
const expect = chai.expect;
const chaiAsPromised = require("chai-as-promised");
chai.use(chaiAsPromised);
it("autocompletes the name field", function() {
  driver.findElement(By.css(".autocomplete")).sendKeys("John");
  driver.wait(until.elementLocated(By.css(".suggestion")));
  driver.findElement(By.css(".suggestion")).click();
  return expect(
    driver.findElement(By.css(".autocomplete")).getAttribute("value")
  ).to.eventually.equal("John Doe");
});
```

The problem is that this only allows one single assertion, because the test must return a promise. If you have to make several assertions, you must combine them into a single promise:

```js
it('autocompletes the name field', function() {
    driver.findElement(By.css('.autocomplete')).sendKeys('John');
    driver.wait(until.elementLocated(By.css('.suggestion')))
    driver.findElement(By.css('.suggestion')).click();
    return Promise.all([
        expect(driver.findElement(By.css('.autocomplete')).getAttribute('value')).to.eventually.equal('John Doe'),
        expect(driver.isElementPresent(By.css('.suggestion')).to.eventually.be.false,
    ]);
});
```

That's not very readable, is it? Another solution is to use `co-mocha`, which allows to use generators. You can remove the `.eventually`, and `yield` WebDriver promises when you need them. Besides, you don't need to return a promise anymore, since the generator allows for blocking commands:

```js
it('autocompletes the name field', function*() {
    driver.findElement(By.css('.autocomplete')).sendKeys('John');
    driver.wait(until.elementLocated(By.css('.suggestion')))
    driver.findElement(By.css('.suggestion')).click();
    expect(yield driver.findElement(By.css('.autocomplete')).getAttribute('value')).to.equal('John Doe');
    expect(yield driver.isElementPresent(By.css('.suggestion')).to.be.false;
});
```

To include `co-mocha` in your Mocha test suite, simply use the `--require` CLI option:

```bash
./node_modules/.bin/mocha --require co-mocha "./e2e/tests/*.js"
```

This solution is elegant, and gives a glimpse of the future One True Way to deal with async operations in the future: `async`/`await` (ES7).

## Wait Often

Selenium tests tend to fail randomly. To troubleshoot failures, `driver.wait(() => {})` is your friend: it keeps the test browser window open, and lets you use the Firefox Developer Tools to inspect the running application.

The most common reason for intermittent failures is that the effect of a browser action may take time (e.g. for processing JS on the client side, redrawing the browser window, etc). Your assertions may come too early, so you should wait until you're sure that the browser has finished working before making an assertion:

```js
it('autocompletes the name field', function*() {
    driver.findElement(By.css('.autocomplete')).sendKeys('John');
    driver.wait(until.elementLocated(By.css('.suggestion')))
    driver.findElement(By.css('.suggestion')).click();
    // wait until the suggestion disappears
    driver.wait(until.elementIsNotVisible(By.css('.suggestion')));
    // only then check the value
    expect(yield driver.findElement(By.css('.autocomplete')).getAttribute('value')).to.equal('John Doe');
    expect(yield driver.isElementPresent(By.css('.suggestion')).to.be.false;
});
```

This bug will bite you very often while writing e2e tests, so remember to **always use `wait()` before making an assertion**.

## Page objects

After writing a few tests, you will probably see more and more code duplication in tests. In addition, e2e tests tend to adhere a lot to the HTML code. As soon as you change the HTML code, many e2e tests will break, and fixing them will require a lot of work.

The solution is to use [Page objects](http://martinfowler.com/bliki/PageObject.html). These objects centralize the logic about a page for e2e testing.

You don't need a fancy library to build a page object. A simple JavaScript object will do the job:

```js
// in e2e/pages/home.js
const webdriver = require('selenium-webdriver');
const By = webdriver.By;
const until = webdriver.until;

module.exports = function(driver) {
    const elements = {
        nameInput: By.css('.autocomplete'),
        nameSuggestion: By.css('.suggestion'),
        submitButton: By.css('.submit'),
    };
    return {
        url:  'http://localhost:8081/frontend#/',
        elements: elements,
        waitUntilVisible: function() {
            return driver.wait(until.elementLocated(elements.nameInput));
        },
        navigate: function() {
            driver.navigate().to(this.url);
            return this.waitUntilVisible();
        },
        enterName: function(value) {
            driver.findElement(elements.nameInput).sendKeys(value);
            driver.wait(until.elementLocated(elements.nameSuggestion));
            driver.findElement(elements.nameSuggestion).click();
            return driver.wait(until.elementIsNotVisible(By.css(elements.nameSuggestion)));
        },
        getName: function() {
            return driver.findElement(elements.nameInput).getAttribute('value')
        }
        submit: function() {
            return driver.findElement(elements.submitButton).click();
        },
    };
};
```

Using this page object in e2e tests is straightforward:

```js
// in e2e/tests/autocomplete.js
const webdriver = require("selenium-webdriver");

const driver = new webdriver.Builder().forBrowser("firefox").build();

const homePage = require("../pages/home")(driver);

before(() => homePage.navigate());

it("autocompletes the name field", function*() {
  homePage.enterName("John");
  expect(yield homePage.getName()).to.equal("John Doe");
});
```

Now the test is extremely readable, and the HTML manipulation is centralized in page objects.

Page objects don't contain assertions, so you can keep using promises there. Use generators in conjunction with `yield` only in test files.

Organize your page objects by page, or by page component. This will help you write more maintainable code:

```js
const homePage = require("../pages/home")(driver);
const resultPage = require("../pages/result")(driver);
it("prefills the name form in the result page", function*() {
  homePage.enterName("John");
  homePage.submit();
  expect(yield resultPage.getName()).to.equal("John Doe");
});
```

The syntax of these e2e tests tends to look more and more like [Behavior-Driven Development](https://fr.wikipedia.org/wiki/Behavior_driven_development) (BDD) tests. However, our experience is that BDD assertions require an additional step (translation from [Gherkin](https://github.com/cucumber/cucumber/wiki/Gherkin) to e2e tests) that is often costly to develop, and Product Owners writing BDD tests are rare. So keep with e2e unless you really have to do BDD.

## Alternatives to raw selenium-webdriver

You may find the Selenium WebDriver API a bit verbose. There are many libraries trying to make it simpler:

- Nightwatch.js
- webdriver.io
- Protractor (specialized for Angluar.js apps)
- Chimp.io

However, each of these libs adds some magic on top of the (already magic enough) WebDriver API. When your tests break, you want to have the least components possible involved, to better isolate the problem. Our experience with Nightwatch.js, for instance, shows that it uses a patched version of Mocha to handle asynchronous assertions magically. This is confusing, and causes trouble in many cases. Using Nightwatch with the standard Mocha isn't easy, and defeats the purpose of a helper library.

If what you want is a quicker syntax for common operations (value lookup on an input, content lookup on a text element, etc.), write your own minimal utility function:

```js
// in e2e/utils/driverUtils.js
const until = require("selenium-webdriver").until;

module.exports = driver => ({
  waitForElementVisible: function(selector) {
    return driver.wait(until.elementLocated(selector));
  },
  getText: function(selector) {
    return driver.findElement(selector).getText();
  },
  getValue: function(selector) {
    return driver.findElement(selector).getAttribute("value");
  },
  setValue: function(selector, value) {
    return driver.findElement(selector).sendKeys(value);
  },
  click: function(selector) {
    return driver.findElement(selector).click();
  },
});
```

## Conclusion

Writing efficient and reliable e2e tests implies understanding a few basic concepts about Selenium WebDriver. Then, e2e tests become extremely easy and fun to write, and help you increase the test coverage of your application.

For Single Page Applications (SPA) using a frontend framework, like React.js, e2e tests are a must. Don't be frightened by Selenium, and give it a try. You won't be disappointed!
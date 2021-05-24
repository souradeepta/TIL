# Run painless test suites in Python with Unittest

*A gentle introduction to aggregate tests that will be executed together.*



Writing unit tests in Python is fairly easy, thanks to the well-known [Unittest](https://docs.python.org/3/library/unittest.html) module. What I found less obvious was how to organize those tests properly and run them all together.

Let me begin with a little bit of theory on how that module works and what it expects from your code. I will be using Python 3 for the following tutorial, but everything will work fine also with previous versions.



## Cases, suites, runners, fixtures

The Unittest module borrows four basic concepts from the unit testing philosophy.

A **text fixture** is a function that contains the preparation of your test environment. Here you usually initialize databases, create files, prepare stuff so that your tests can work properly. Fixtures are also used to clean up the scene once finished.

A **test case** is a class that represents an individual unit of testing. That's the place where you make sure your code works correctly. It contains fixtures and calls to [assert methods](https://docs.python.org/3/library/unittest.html#assert-methods) to check for and report failures.

A **test suite** is just a bunch of test cases together.

A **test runner** is a script that takes care of running the test suite.



## My app needs to be tested

Now suppose that you are writing a minimalistic game made of the following objects: Player, Scenario and Thing. You are following a test-driven development so you write tests along the actual code.

Each test is contained in a module (i.e. a Python file), so you would end up with three modules/files. Let's set up a barebone test case for the Player object: with Unittest it would look something like that:

```python
# tests/player.py

import unittest

class TestPlayer(unittest.TestCase):

    def setUp(self):
        ...

    def test_run(self):
        ...

    def test_attack(self):
        ...

    def tearDown(self):
        ...
```

First of all import the `unittest` module. That was obvious. The class `TestPlayer` is the actual test case and follows a naming convention: `Test[whatYouWantToTest]`. It also extends the `unittest.TestCase` base object to work: a test case is always created by subclassing the parent class.

Then, each test case begins and ends with `setUp()` and `tearDown()`: those are fixtures. There you put the code that will be executed before and after each test method. They are not mandatory: you can just omit them if you don't need specific initializations or cleanups.

The "body" of the test case is composed of test methods: `test_run` and `test_attack` in the example above. It's the place where you check that your code is running properly, with the aid of the [assert methods](https://docs.python.org/3/library/unittest.html#assert-methods).

Individual test methods' name must start with the letters `test_`. That's another naming convention required by the test runner to know which methods are the actual tests. More on that in a couple of seconds.

Just rinse and repeat the procedure for each class of your game and you eventually end up with several test cases, one for each component. You can then run the test cases one by one by hand, but that would be totally annoying. It's now time to set up a nice test suite and let it work for you on its own.

## Organize tests in a test suite

The basic idea here is to have a new Python file `runner.py` alongside your tests that contains our runner. It looks something like the following:

```python
# tests/runner.py
import unittest

# import your test modules
import player
import scenario
import thing

# initialize the test suite
loader = unittest.TestLoader()
suite  = unittest.TestSuite()

# add tests to the test suite
suite.addTests(loader.loadTestsFromModule(player))
suite.addTests(loader.loadTestsFromModule(scenario))
suite.addTests(loader.loadTestsFromModule(thing))

# initialize a runner, pass it your suite and run it
runner = unittest.TextTestRunner(verbosity=3)
result = runner.run(suite)
```

First of all import your modules containing your tests (`player`, `scenario` and `thing`, remember?). Then initialize the suite and the loader by calling `unittest.TestLoader()` and `unittest.TestSuite()`.

Add your tests to the test suite with `suite.addTests(loader.loadTestsFromModule([your-module-here]))`, then initialize the test runner and fire it with `runner.run(suite)`.

I also set the verbosity level of the test runner to 3: that's how much information you'll see in the console output.

Launch the script and all your tests will be executed nicely.

## Sources

Python Official Documentation - *Unit testing framework* ([link](https://docs.python.org/3.5/library/unittest.html))
Python Testing - *unittest introduction* ([link](http://pythontesting.net/framework/unittest/unittest-introduction/))
Wikipedia - *Test fixture* ([link](https://en.wikipedia.org/wiki/Test_fixture#Software))
Voidspace - *Introduction to unittest* ([link](http://www.voidspace.org.uk/python/articles/introduction-to-unittest.shtml))
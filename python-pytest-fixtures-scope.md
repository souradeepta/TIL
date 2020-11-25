# Understand 5 Scopes of Pytest Fixtures

There are three mainstream unit test frameworks for Python: the built-in `unittest`, `nosetests` and `pytest`. According to many [*unofficial* rankings](https://www.google.com/search?newwindow=1&safe=active&sxsrf=ALeKk03skdFP02yOonee-FrLmk_-A5N1GA%3A1591645399975&ei=15TeXveFO4G1sAfhkbLgAw&q=the+best+unit+test+framework+for+python&oq=&gs_lcp=CgZwc3ktYWIQARgHMgcIIxDqAhAnMgcIIxDqAhAnMgcIIxDqAhAnMgcIIxDqAhAnMgcIIxDqAhAnMgcIIxDqAhAnMgcIIxDqAhAnMgcIIxDqAhAnMgcIIxDqAhAnMgcIIxDqAhAnOgQIABBHUJDOAViQzgFg1uUBaAFwA3gAgAEAiAEAkgEAmAEAoAEBqgEHZ3dzLXdperABCg&sclient=psy-ab) on the internet, `Pytest` is the most popular test framework thanks to its minimal amount of boilerplate, rich features, and [a growing number of plugins](https://docs.pytest.org/en/2.7.3/plugins_index/index.html).

A [test fixture](https://en.wikipedia.org/wiki/Test_fixture) is a concept used in both electronics and software. It’s a piece of software or device that sets up a system to satisfy certain preconditions of the process. Its biggest advantage is that it provides consistent results so that the test results can be repeatable. Examples of fixtures could be loading test set to the database, reading a configuration file, setting up environment variables, etc.

# What’s a Fixture in Pytest?

Fixture plays a quite important role in Pytest. Compared to the classic xUnit style where you normally have a `setup()` and `teardown()` function, the fixture in Pytest is more flexible and flat. Each fixture is defined using the `@pytest.fixture` decorator on top of a function and this function will do the job like reading a configuration file.

```python
import pytest
import json
import logging

@pytest.fixture
def read_config():
    with open("app.json") as f:
        # app.json is an empty JSON file
        config = json.load(f)
    return config

def test1(read_config):
    assert read_config == {}
```

[pytest-example.py](https://gist.github.com/highsmallxu/b01368fce447b176b80b9a06451fe5e2)

The way we invoke a fixture function is different from what we normally do in the source code. In the source code, we invoke a function inside another function, and then we will get the return object, like `config = read_config()`.

In Pytest, however, we can *use the fixture function as an input parameter of the test function*, and that input parameter is already the return object. This behavior is called [dependency injection](https://docs.pytest.org/en/stable/fixture.html#fixtures-a-prime-example-of-dependency-injection). We can inject the fixture function as many times as we want.

But each injection will execute the fixture function once. So, in the example below, it opens the same file twice.

Is there a way to control the execution of fixtures to make our test code more efficient? Yes, which is why we need to understand different scopes of Pytest fixtures.

```python
import pytest
import json
import logging

@pytest.fixture()
def read_config():
    with open("app.json") as f:
        config = json.load(f)
        logging.info("Read config")
    return config

def test1(read_config):
    logging.info("Test function 1")
    assert read_config == {}

def test2(read_config):
    logging.info("Test function 2")
    assert read_config == {}
    
# test/test_code.py::test1 
# -------- live log setup --------
# INFO     root:test_code.py:10 Read config
# -------- live log call --------
# INFO     root:test_code.py:15 Test function 1
# PASSED   [ 50%]
# test/test_code.py::test2 
# -------- live log setup --------
# INFO     root:test_code.py:10 Read config
# -------- live log call --------
# INFO     root:test_code.py:20 Test function 2
# PASSED   [ 100%]
```



[pytest-function.py](https://gist.github.com/highsmallxu/25de62736ce0f73bf167d5c141273c0d)

# 5 Scopes of Pytest Fixtures

Pytest fixtures have five different scopes: function, class, module, package, and session. The scope basically controls how often each fixture will be executed.

## Function

Function is the default scope without explicitly adding `scope="function"`. The fixture will be executed per test function. This scope works well if the fixture is only used once or it contains a very light operation or you want a different value each time.

```python
import pytest
from datetime import datetime

@pytest.fixture()
def only_used_once():
    with open("app.json") as f:
        config = json.load(f)
    return config

@pytest.fixture()
def light_operation():
    return "I'm a constant"

@pytest.fixture()
def need_different_value_each_time():
    return datetime.now()
```

[pytest-fixture-function.py](https://gist.github.com/highsmallxu/dfd023286097e903b7524119891a0f27)

## Class

The scope class runs the fixture per test class. If you have a couple of test functions that do similar things, such as arithmetic operations or database queries, you can put them in the same test class with the decorator `@pytest.mark.usefixtures("fixture-name")`. This special decorator adds the fixture to a test class, and the fixture will be executed *before* any test function. Check out the logs below.

```python
@pytest.fixture(scope="class")
def dummy_data(request):
    request.cls.num1 = 10
    request.cls.num2 = 20
    logging.info("Execute fixture")

@pytest.mark.usefixtures("dummy_data")
class TestCalculatorClass:
    def test_distance(self):
        logging.info("Test distance function")
        assert distance(self.num1, self.num2) == 10

    def test_sum_of_square(self):
        logging.info("Test sum of square function")
        assert sum_of_square(self.num1, self.num2) == 500
        
# test/test_code.py::TestCalculatorClass::test_distance 
# ------- live log setup -------
# INFO     root:test_code.py:59 Execute fixture
# ------- live log call -------
# INFO     root:test_code.py:65 Test distance function
# PASSED                                                                                                                                         [ 50%]
# test/test_code.py::TestCalculatorClass::test_sum_of_square 
# ------- live log call -------
# INFO     root:test_code.py:69 Test sum of square function
# PASSED 

# source code
def distance(num1, num2):
    return abs(num1 - num2)

def sum_of_square(num1, num2):
    return num1 ** 2 + num2 ** 2
```

https://gist.github.com/highsmallxu/7ab383bee1bf1f375a66dc8d85ee403c)

There is a special usage of `yield` statement in Pytest that allows you to execute the fixture *after* all the test functions. The code before `yield` serves as the setup code, and the code after `yield` serves as the teardown code. A classic example would be a testing database.

```python
@pytest.fixture(scope="class")
def prepare_db(request):
    # pseudo code
    connection = db.create_connection()
    request.cls.connection = connection
    yield
    connection = db.close()

@pytest.mark.usefixtures("prepare_db")
class TestDBClass:
    def test_query1(self):
        assert self.connection.execute("..") == "..."

    def test_query2(self):
        assert self.connection.execute("..") == "..."
```

[pytest-fixture-class-db.py](https://gist.github.com/highsmallxu/c33fb362226637267223dca1f60d38e0)

## Module and package

Until now, you must have observed the pattern here. So, the scope module runs the fixture per module and the scope package runs the fixture per package. The scope module is used more often than package. Let’s go back to the first example where we have a `read_config` fixture that opens a configuration file. This is a good use case of using a scope module or a higher scope because we want to open the file only once and reuse the config object multiple times. You can compare the logs below with the logs of [pytest-function.py](https://gist.github.com/highsmallxu/25de62736ce0f73bf167d5c141273c0d). Can you see the difference?

```python
@pytest.fixture(scope="module")
def read_config():
    with open("app.json") as f:
        config = json.load(f)
        logging.info("Read config")
    return config

def test1(read_config):
    logging.info("Test function 1")
    assert read_config == {}

def test2(read_config):
    logging.info("Test function 2")
    assert read_config == {}

# test/test_code.py::test1 
# ----------- live log setup -----------
# INFO     root:test_code.py:88 Read config
# ----------- live log call -----------
# INFO     root:test_code.py:93 Test function 1
# PASSED                                                                                                                                         [ 75%]
# test/test_code.py::test2 
# ----------- live log call -----------
# INFO     root:test_code.py:98 Test function 2
# PASSED 
```

https://gist.github.com/highsmallxu/f5b0be8eb689c0e78c4d73516f19728e)

## ***Session\***

The last and the most aboard scope is session. Fixtures with scope session will only be executed once per session. Every time you run `pytest`, it’s considered to be one session. Scope session is designed for expensive operations like truncating table and loading a test set to the database.

## **conftest.py**

With scope function, class and module, it makes sense to put fixtures in the same file as tests. But with scope session, there is a better place to put these fixtures. This is the `conftest.py` file, a local per-directory plugin of Pytest. Pytest will make the fixtures in `conftest.py` available to all the tests **within the same directory**. The example below is extended from the first example.

In `conftest.py`, we have the fixture `read_config` with *scope* session. This fixture is used by four test functions in two test files. As fixtures in `conftest.py` are available to the whole directory, *we don’t need to explicitly import those fixtures in the test files, Pytest will do it for us.*

```python
# test/conftest.py
@pytest.fixture(scope="session")
def read_config():
    with open("app.json") as f:
        config = json.load(f)
        logging.info("Read config")
    return config

# test/test_code1.py
def test1(read_config):
    logging.info("Test function 1")
    assert read_config == {}

def test2(read_config):
    logging.info("Test function 2")
    assert read_config == {}

# test/test_code2.py
def test3(read_config):
    logging.info("Test function 3")
    assert read_config == {}

def test4(read_config):
    logging.info("Test function 4")
    assert read_config == {}
    
# test/test_code1.py::test1 
# -------- live log setup --------
# INFO     root:conftest.py:10 Read config
# -------- live log call --------
# INFO     root:test_code1.py:93 Test function 1
# PASSED                                                                                                                                         [ 50%]
# test/test_code1.py::test2 
# -------- live log call --------
# INFO     root:test_code1.py:98 Test function 2
# PASSED                                                                                                                                         [ 66%]
# test/test_code2.py::test3 
# -------- live log call --------
# INFO     root:test_code2.py:5 Test function 3
# PASSED                                                                                                                                         [ 83%]
# test/test_code2.py::test4 
# -------- live log call --------
# INFO     root:test_code2.py:10 Test function 4
# PASSED 
```

​	https://gist.github.com/highsmallxu/ad3618d0176364e32307360484a03f1b)

# Autouse Fixtures

So far, we have had to trigger the fixture by using it as an input parameter of the test function. But actually, a fixture can also be automatically triggered with another keyword `autouse=True`. In this example, the fixture `function_autouse` is automatically executed without being mentioned anywhere.

But `autouse=True` doesn’t mean it will be the first fixture to be triggered in the whole session. It’s still related to its scope:

```python
@pytest.fixture(autouse=True)
def function_autouse():
    logging.info("scope function with autouse")
    
def test_autouse():
    assert True
    
# test/test_code1.py::test_autouse 
# -------------- live log setup --------------
# INFO     root:test_code1.py:28 scope function with autouse
# PASSED
```

[-autouse.py](https://gist.github.com/highsmallxu/b3f2a47380ffeb2ba52935104ca4cfbe)

# Execution Order of Fixtures

Let’s first remember the basic execution order: *session > package > module > class > function.* Check out the logs below.

```python
import pytest
import logging

@pytest.fixture(scope="session")
def session():
    logging.info("scope session")
    
@pytest.fixture(scope="package")
def package():
    logging.info("scope package")

@pytest.fixture(scope="module")
def module():
    logging.info("scope module")

@pytest.fixture(scope="class")
def class_():
    logging.info("scope class")

@pytest.fixture(scope="function")
def function():
    logging.info("scope function")

def test_order(module, class_, session, function, package):
    assert True

# test/test_code1.py::test_order 
# ---------------- live log setup ----------------
# INFO     root:test_code1.py:8 scope session
# INFO     root:test_code1.py:13 scope package
# INFO     root:test_code1.py:18 scope module
# INFO     root:test_code1.py:23 scope class
# INFO     root:test_code1.py:28 scope function
# PASSED
```

[pytest-fixture-order.py](https://gist.github.com/highsmallxu/7096835e696cbc8b119574796ffcb7f1)

Now, let’s add one more fixture with `autouse=True` to each scope and think about what the order will be:

```bash
test/test_code1.py::test_order 
-------- live log setup --------
INFO     root:test_code1.py:13 scope session with autouse
INFO     root:test_code1.py:8 scope session
INFO     root:test_code1.py:23 scope package with autouse
INFO     root:test_code1.py:18 scope package
INFO     root:test_code1.py:33 scope module with autouse
INFO     root:test_code1.py:28 scope module
INFO     root:test_code1.py:43 scope class with autouse
INFO     root:test_code1.py:38 scope class
INFO     root:test_code1.py:48 scope function with autouse
INFO     root:test_code1.py:53 scope function
```

Is it what you expect? Basically, the order is based on two rules:

- Fixtures of higher-scopes are executed first.
- Fixtures with `autouse=True` are executed before other fixtures within the same scope.

# Conclusion

Pytest fixtures improve the efficiency of the test code by reusing the same objects or the same functions multiple times using dependency injection or automatic triggering (autouse). Five different scopes of fixtures control how often each fixture will be executed. The higher scope the fixture has, the earlier the fixture will be executed.

Besides, `conftest.py` is a nice place to put fixtures with scope session. They’re available to the whole directory and they don’t need to be imported explicitly.

I hope you enjoyed this article! Leave your comments below if you have any thoughts.
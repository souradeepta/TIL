# Python Programming/Scoping

### Variables[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Scoping&action=edit&section=1)]

Variables in Python are automatically declared by assignment. Variables are always references to objects, and are never typed. Variables exist only in the current scope or global scope. When they go out of scope, the variables are destroyed, but the objects to which they refer are not (unless the number of references to the object drops to zero).

Scope is delineated by function and class blocks. Both functions and their scopes can be nested. So therefore

```python
def foo():
    def bar():
        x = 5 # x is now in scope
        return x + y # y is defined in the enclosing scope later
    y = 10
    return bar() # now that y is defined, bar's scope includes y
```

Now when this code is tested,

```python
>>> foo()
15
>>> bar()
Traceback (most recent call last):
  File "<pyshell#26>", line 1, in -toplevel-
    bar()
NameError: name 'bar' is not defined
```

The name 'bar' is not found because a higher scope does not have access to the names lower in the hierarchy.

It is a common pitfall to fail to lookup an attribute (such as a method) of an object (such as a container) referenced by a variable before the variable is assigned the object. In its most common form:

```python
>>> for x in range(10):
         y.append(x) # append is an attribute of lists

Traceback (most recent call last):
  File "<pyshell#46>", line 2, in -toplevel-
    y.append(x)
NameError: name 'y' is not defined
```

Here, to correct this problem, one must add y = [] before the for loop.

A loop does not create its own scope:

```python
for x in [1, 2, 3]:
  inner = x
print inner # 3 rather than an error
```

### Keyword global[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Scoping&action=edit&section=2)]

Global variables of a Python module are read-accessible from functions in that module. In fact, if they are mutable, they can be also modified via method call. However, they cannot modified via a plain assignment unless declared *global* in the function.

An example to clarify:

```python
count1 = 1
count2 = 1
list1 = []
list2 = []

def test1():
  print count1    # Read access is unproblematic, referring to the global

def test2():
  try:
    print count1  # This print would be unproblematic, but it throws an error ...
    count1 += 1   # ... since count1 += 1 causes count1 to be local.
  except UnboundLocalError as error:
    print "Error caught:", error

def test3():
  list1 = [2]     # No outside effect; this rebinds list1 to be a local variable

def test4():
  global count2, list2
  print count1    # Read access is unproblematic, referring to the global
  count2 += 1     # We can modify count2 via assignment
  list1.append(1) # Impacts the global list1 even without global declaration
  list2 = [2]     # Impacts the global list2

test1()
test2()
test3()
test4()

print "count1:", count1  # 1
print "count2:", count2  # 2
print "list1:", list1    # [1]
print "list2:", list2    # [2]
```

Links:

- [6.13. The global statement](https://docs.python.org/2/reference/simple_stmts.html#grammar-token-global_stmt), docs.python.org
- [What are the rules for local and global variables in Python?](https://docs.python.org/2/faq/programming.html#what-are-the-rules-for-local-and-global-variables-in-python) in Programming FAQ, docs.python.org

### Keyword nonlocal[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Scoping&action=edit&section=3)]

Keyword nonlocal, available since Python 3.0, is an analogue of *global* for nested scopes. It enables a nested function of assign-modify a variable that is local to the outer function.

An example:

```python
# Requires Python 3
def outer():
  outerint = 0
  outerint2 = 10
  def inner():
    nonlocal outerint
    outerint = 1 # Impacts outer's outerint only because of the nonlocal declaration
    outerint2 = 1 # No impact
  inner()
  print(outerint)
  print(outerint2)

outer()
```

Simulation of nonlocal in Python 2 via a mutable object:

```python
def outer():
  outerint = [1]           # Technique 1: Store int in a list
  class outerNL: pass      # Technique 2: Store int in a class
  outerNL.outerint2 = 11
  def inner():
    outerint[0] = 2        # List members can be modified
    outerNL.outerint2 = 12 # Class members can be modified
  inner()
  print outerint[0]
  print outerNL.outerint2

outer()
```

Links:

- [7.13. The nonlocal statement](https://docs.python.org/3/reference/simple_stmts.html#nonlocal), docs.python.org

### globals and locals[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Scoping&action=edit&section=4)]

To find out which variables exist in the global and local scopes, you can use *locals()* and *globals()* functions, which return dictionaries:

```python
int1 = 1
def test1():
  int1 = 2
  globals()["int1"] = 3  # Write access seems possible
  print locals()["int1"] # 2
  
test1()

print int1               # 3
```

Write access to locals() dictionary is discouraged by the Python documentation.

### [Why do lambdas defined in a loop with different values all return the same result?](https://docs.python.org/2/faq/programming.html#id11)

Assume you use a for loop to define a few different lambdas (or even plain functions), e.g.:

```python
>>> squares = []
>>> for x in range(5):
...     squares.append(lambda: x**2)
```

This gives you a list that contains 5 lambdas that calculate `x**2`. You might expect that, when called, they would return, respectively, `0`, `1`, `4`, `9`, and `16`. However, when you actually try you will see that they all return `16`:

```python
>>> squares[2]()
16
>>> squares[4]()
16
```

This happens because `x` is not local to the lambdas, but is defined in the outer scope, and it is accessed when the lambda is called — not when it is defined. At the end of the loop, the value of `x` is `4`, so all the functions now return `4**2`, i.e. `16`. You can also verify this by changing the value of `x` and see how the results of the lambdas change:

```python
>>> x = 8
>>> squares[2]()
64
```

In order to avoid this, you need to save the values in variables local to the lambdas, so that they don’t rely on the value of the global `x`:

```
>>> squares = []
>>> for x in range(5):
...     squares.append(lambda n=x: n**2)
```

Here, `n=x` creates a new variable `n` local to the lambda and computed when the lambda is defined so that it has the same value that `x` had at that point in the loop. This means that the value of `n` will be `0` in the first lambda, `1` in the second, `2` in the third, and so on. Therefore each lambda will now return the correct result:

```python
>>> squares[2]()
4
>>> squares[4]()
16
```

Note that this behaviour is not peculiar to lambdas, but applies to regular functions too.

### [How do I share global variables across modules?](https://docs.python.org/2/faq/programming.html#id12)

The canonical way to share information across modules within a single program is to create a special module (often called config or cfg). Just import the config module in all modules of your application; the module then becomes available as a global name. Because there is only one instance of each module, any changes made to the module object get reflected everywhere. For example:

config.py:

```python
x = 0   # Default value of the 'x' configuration setting
```

mod.py:

```python
import config
config.x = 1
```

main.py:

```python
import config
import mod
print config.x
```

Note that using a module is also the basis for implementing the Singleton design pattern, for the same reason.
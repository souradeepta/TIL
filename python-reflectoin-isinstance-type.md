# Python Reflection and Introspection

In programming, *introspection* is the ability to find out information about an object at runtime. *Reflection* takes this a step further by enabling objects to be *modified* at runtime.

For me, these two language features really make Python fun and set it apart from less-dynamic languages. Python supports both introspection and reflection, and in this article, I will explain how they work.

# Introspection

The most basic information you can introspect at runtime is an object’s *type*. This can be achieved with the `type()` function.

```python
>>> type(1)
<class 'int'>>>> type(1.0)
<class 'float'>>>> type(int)
<class 'type'>
```

Here, the interpreter is telling us that the integer `1` is of class `int`, `1.0` is of class `float`, and `int` is of class `type`.

The return value from the `type()` function is called a *type object*. A type object tells us what *class* the argument is an instance of.

We can confirm this neatly with `isinstance()`:

```python
>>> isinstance(1, int)
True
>>> isinstance(int, type)
True
```

Type objects support the `is` operator, so we can write:

```python
>>> type(1) is int
True
>>> type(int) is type
True
```

However, `type()` and `isinstance()` aren’t directly equivalent since `isinstance()` also considers the base class of an object:

```python
>>> class A:     
    pass
>>> class B(A):
    pass
>>> type(A()) is A
True
>>> type(B()) is A
False
>>> isinstance(B(), A)
True
```



<iframe src="https://medium.com/media/4f7112e8eb8ac8cc87c1001a3fe1624a" allowfullscreen="" frameborder="0" height="255" width="680" title="isinstance.py" class="t u v hs aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 255px;"></iframe>

So, we may want to use `type()` and `isinstance()` together, like this:

```python
if isinstance(obj, A):
    # do something for all children of A
    
    if type(obj) is B:
        # do something specifically for instances of B
```

Other techniques we can use to find out about an object in Python include:

- `hasattr(obj,'a')` — This returns `True` if `obj` has an attribute `a`.
- `id(obj)` — This returns the unique ID of an object.
- `dir(obj)` — Returns all the attributes and methods of an object in a list.
- `vars(obj)` — Returns all the instance variables of an object in a dictionary.
- `callable(obj)` — Returns `True` if `obj` is callable.

We can also directly access some of this information using attributes that are automatically added to an object on creation. For example:

- `obj.__class__` stores the *type object* for `obj`.
- `obj.__class__.__name__` stores the *class name* for `obj`.
- For function objects, `obj.__code__` stores a code object with information about the code in the function.

For more information about code objects see [the Python docs](https://docs.python.org/3.8/c-api/code.html).

## Introspection example

Putting all of this together, we can create a simple `introspect()` function:

```python
class Test:
    def __init__(self):
        self.x = 1
        self.y = 2
        

def introspect(obj):
  for func in [type, id, dir, vars, callable]:
        print("%s(%s):\t\t%s" % (func.__name__, introspect.__code__.co_varnames[0], func(obj)))
        

introspect(Test())
```



<iframe src="https://medium.com/media/0a3e48768497c200fdcfaa473dee2890" allowfullscreen="" frameborder="0" height="315" width="680" title="Introspection Function" class="t u v hs aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 315px;"></iframe>

Notice how we even use introspection within our function to print the name of the function being called (`func.__name__`) and the name of the introspect’s argument (`introspect.__code__.co_varnames[0]`)!

The output would look like this:

```python
type(obj):              <class '__main__.Test'>                                                                       
id(obj):                139779613404408                                                                               
dir(obj):               ['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__
', '__getattribute__', '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__red
uce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'x', '
y']
vars(obj):              {'y': 2, 'x': 1}                                                                              
callable(obj):          False
```

Not bad for essentially two lines of code!

## Advanced introspection

The tools outlined above are already very powerful. However, should you want to dig deeper into introspection, Python’s [inspect module](https://docs.python.org/3/library/inspect.html) provides further capabilities for introspecting live objects.

# Modifying Objects Dynamically

So far, we’ve discussed how to find information about an object at runtime. We’re now going to learn how to dynamically *modify* or even create new objects and classes!

Firstly, we should know that attributes can be added to a class or object at runtime. So, we can write:

```python

>>> class A:
    pass
>>> A.x = 1
>>> a = A()
>>> a.y = 2
>>> a.y
2
>>> a.x
1
```



<iframe src="https://medium.com/media/4d9fb0a0d0a1a0a6bd42caafc1d23ff6" allowfullscreen="" frameborder="0" height="234" width="680" title="ClassA.py" class="t u v hs aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 234px;"></iframe>

Since methods are just a special type of attribute, this means we can also add methods at runtime. Let’s modify our class by dynamically adding an `__init__` method to it.

```python
>>> def init(self):    # the function and argument can have any name
    self.x = 1
>>> class A:
    pass
>>> setattr(A, '__init__', init)
>>> a = A()
>>> a.x
1
```



<iframe src="https://medium.com/media/37ea0ee4545e3e3d22fb5a18daffa651" allowfullscreen="" frameborder="0" height="212" width="680" title="init.py" class="t u v hs aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 212px;"></iframe>

Notice how we use the `setattr` function to set the `__init__` method of `A` to `init`. This allows the name of the attribute we are setting to also be determined dynamically.

We can take this concept a step further by modifying a function’s `__code__` attribute. This time by simple assignment:

```python
>>> def test():
    print("Test")
>>> test()
"Test"
>>> test.__code__ = (lambda: print("Hello")).__code__
>>> test()
"Hello"
```



<iframe src="https://medium.com/media/afd977b4fcd2bede2599eb4e6805a01f" allowfullscreen="" frameborder="0" height="190" width="680" title="test.py" class="t u v hs aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 190px;"></iframe>

This could be used, for example, to create a function that executes only once:

```python
>>> def test():
    test.__code__ = (lambda: None).__code__
    print ("Test")
>>> test()       # First call prints "Test"
"Test"
>>> test()       # Subsequent calls do nothing
```



<iframe src="https://medium.com/media/b24e748a10361799f97ba84ae68bff80" allowfullscreen="" frameborder="0" height="169" width="680" title="test.py" class="t u v hs aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 169px;"></iframe>

## Creating classes at runtime

We are now going to revisit the `type()` function mentioned earlier and use it to *create* a new class dynamically. To do this, we call `type()` with three arguments:

```python
type(name, bases, dict)
```

Where:

- `name` is the name of the class we are creating.
- `bases` is a tuple of base classes we inherit from.
- `dict` is a dictionary of attribute name, attribute value pairs.

So, in its simplest form, we can write:

```python
>>> A = type('A', (), {'x': 1})
>>> a = A()
>>> a.x
1
```

However, we can get much more advance that that. For example, to create a fully-fledged class:

```python
>>> exec("def init(self):\n\tprint(self.__class__.__name__ + \" created!\")")
>>> A = type('A', (), {'__init__' : init })
>>> a = A()
"A created!"
```

Notice that, here, we use Python’s built-in `exec` function to generate our class’s `__init__` method from a string. The method definition itself uses `self.__class__.__name__` to dynamically get the class name!

# A Final Word of Caution

Whilst the language capabilities outlined in this article are powerful, [with great power comes great responsibility](https://en.wikipedia.org/wiki/With_great_power_comes_great_responsibility)!

These techniques should be used very sparingly. Excessive use of dynamic programming can make code harder to read. In some cases, it can also introduce security vulnerabilities, especially if dynamic execution involves user input.

Dynamically modifying code at runtime is sometimes referred to as *monkey patching*. Further details about potential applications and pitfalls can be found [on Wikipedia](https://en.wikipedia.org/wiki/Monkey_patch).

------------------------------

## Type[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Reflection&action=edit&section=1)]

The type method enables to find out about the type of an object. The following tests return True:

- type(3) is int
- type(3.0) is float
- type(10**10) is long # Python 2
- type(1 + 1j) is complex
- type('Hello') is str
- type([1, 2]) is list
- type([1, [2, 'Hello']]) is list
- type({'city': 'Paris'}) is dict
- type((1,2)) is tuple
- type(set()) is set
- type(frozenset()) is frozenset
- \----
- type(3).__name__ == "int"
- type('Hello').__name__ == "str"
- \----
- import types, re, Tkinter # For the following examples
- type(re) is types.ModuleType
- type(re.sub) is types.FunctionType
- type(Tkinter.Frame) is types.ClassType
- type(Tkinter.Frame).__name__ == "classobj"
- type(Tkinter.Frame()).__name__ == "instance"
- type(re.compile('myregex')).__name__ == "SRE_Pattern"
- type(type(3)) is types.TypeType

The type function disregards class inheritance: "type(3) is object" yields False while "isinstance(3, object)" yields True.



## Isinstance[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Reflection&action=edit&section=2)]

Determines whether an object is an instance of a type or class.

The following tests return True:

- isinstance(3, int)
- isinstance([1, 2], list)
- isinstance(3, object)
- isinstance([1, 2], object)
- import Tkinter; isinstance(Tkinter.Frame(), Tkinter.Frame)
- import Tkinter; Tkinter.Frame().__class__.__name__ == "Frame"

Note that isinstance provides a weaker condition than a comparison using [#Type](https://en.wikibooks.org/wiki/Python_Programming/Reflection#Type).

Function isinstance and a user-defined class:

```python
class Plant: pass                        # Dummy class
class Tree(Plant): pass                  # Dummy class derived from Plant
tree = Tree()                            # A new instance of Tree class
print isinstance(tree, Tree)             # True
print isinstance(tree, Plant)            # True
print isinstance(tree, object)           # True
print type(tree) is Tree                 # False
print type(tree).__name__ == "instance"  # True
print tree.__class__.__name__ == "Tree"  # True
```



## Issubclass[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Reflection&action=edit&section=3)]

Determines whether a class is a subclass of another class. Pertains to classes, not their instances.

```python
class Plant: pass                        # Dummy class
class Tree(Plant): pass                  # Dummy class derived from Plant
tree = Tree()                            # A new instance of Tree class
print issubclass(Tree, Plant)            # True
print issubclass(Tree, object)           # False in Python 2
print issubclass(int, object)            # True
print issubclass(bool, int)              # True
print issubclass(int, int)               # True
print issubclass(tree, Plant)            # Error - tree is not a class
```



## Callable[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Reflection&action=edit&section=5)]

For an object, determines whether it can be called. A class can be made callable by providing a __call__() method.

Examples:

- callable(2)
  - Returns False. Ditto for callable("Hello") and callable([1, 2]).
- callable([1,2].pop)
  - Returns True, as pop without "()" returns a function object.
- callable([1,2].pop())
  - Returns False, as [1,2].pop() returns 2 rather than a function object.



## Dir[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Reflection&action=edit&section=6)]

Returns the list of names of attributes of an object, which includes methods. Is somewhat heuristic and possibly incomplete, as per python.org.

Examples:

- dir(3)
- dir("Hello")
- dir([1, 2])
- import re; dir(re)
  - Lists names of functions and other objects available in the re module for regular expressions.

## Getattr[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Reflection&action=edit&section=7)]

Returns the value of an attribute of an object, given the attribute name passed as a string.

An example:

- getattr(3, "imag")

The list of attributes of an object can be obtained using [#Dir](https://en.wikibooks.org/wiki/Python_Programming/Reflection#Dir).



## Keywords[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Reflection&action=edit&section=8)]

A list of Python keywords can be obtained from Python:

```python
import keyword
pykeywords = keyword.kwlist
print keyword.iskeyword("if")      # True
print keyword.iskeyword("True")    # False
```



## Built-ins[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Reflection&action=edit&section=9)]

A list of Python built-in objects and functions can be obtained from Python:

```python
print dir(__builtins__)           # Output the list
print type(__builtins__.list)     # = <type 'type'>
print type(__builtins__.open)     # = <type 'builtin_function_or_method'>
print list is __builtins__.list   # True
print open is __builtins__.open   # True
```

----------------

# [What are the differences between type() and isinstance()?](https://stackoverflow.com/questions/1549801/what-are-the-differences-between-type-and-isinstance)

`isinstance` caters for inheritance (an instance of a derived class *is an* instance of a base class, too), while checking for equality of `type` does not (it demands identity of types and rejects instances of subtypes, AKA subclasses).

Normally, in Python, you want your code to support inheritance, of course (since inheritance is so handy, it would be bad to stop code using yours from using it!), so `isinstance` is less bad than checking identity of `type`s because it seamlessly supports inheritance.

It's not that `isinstance` is *good*, mind you—it's just *less bad* than checking equality of types. The normal, Pythonic, preferred solution is almost invariably "duck typing": try using the argument *as if* it was of a certain desired type, do it in a `try`/`except` statement catching all exceptions that could arise if the argument was not in fact of that type (or any other type nicely duck-mimicking it;-), and in the `except` clause, try something else (using the argument "as if" it was of some other type).

`basestring` **is**, however, quite a special case—a builtin type that exists **only** to let you use `isinstance` (both `str` and `unicode` subclass `basestring`). Strings are sequences (you could loop over them, index them, slice them, ...), but you generally want to treat them as "scalar" types—it's somewhat incovenient (but a reasonably frequent use case) to treat all kinds of strings (and maybe other scalar types, i.e., ones you can't loop on) one way, all containers (lists, sets, dicts, ...) in another way, and `basestring` plus `isinstance` helps you do that—the overall structure of this idiom is something like:

```python
if isinstance(x, basestring)
  return treatasscalar(x)
try:
  return treatasiter(iter(x))
except TypeError:
  return treatasscalar(x)
```

You could say that `basestring` is an *Abstract Base Class* ("ABC")—it offers no concrete functionality to subclasses, but rather exists as a "marker", mainly for use with `isinstance`. The concept is obviously a growing one in Python, since [PEP 3119](http://www.python.org/dev/peps/pep-3119/), which introduces a generalization of it, was accepted and has been implemented starting with Python 2.6 and 3.0.

The PEP makes it clear that, while ABCs can often substitute for duck typing, there is generally no big pressure to do that (see [here](http://www.python.org/dev/peps/pep-3119/#abcs-vs-duck-typing)). ABCs as implemented in recent Python versions do however offer extra goodies: `isinstance` (and `issubclass`) can now mean more than just "[an instance of] a derived class" (in particular, any class can be "registered" with an ABC so that it will show as a subclass, and its instances as instances of the ABC); and ABCs can also offer extra convenience to actual subclasses in a very natural way via Template Method design pattern applications (see [here](http://en.wikipedia.org/wiki/Template_method_pattern) and [here](http://www.catonmat.net/blog/learning-python-design-patterns-through-video-lectures/) [[part II]] for more on the TM DP, in general and specifically in Python, independent of ABCs).

For the underlying mechanics of ABC support as offered in Python 2.6, see [here](http://docs.python.org/library/abc.html); for their 3.1 version, very similar, see [here](http://docs.python.org/3.1/library/abc.html). In both versions, standard library module [collections](http://docs.python.org/3.1/library/collections.html#module-collections) (that's the 3.1 version—for the very similar 2.6 version, see [here](http://docs.python.org/library/collections.html#module-collections)) offers several useful ABCs.

For the purpose of this answer, the key thing to retain about ABCs (beyond an arguably more natural placement for TM DP functionality, compared to the classic Python alternative of mixin classes such as [UserDict.DictMixin](http://docs.python.org/library/userdict.html?highlight=userdict#UserDict.DictMixin)) is that they make `isinstance` (and `issubclass`) much more attractive and pervasive (in Python 2.6 and going forward) than they used to be (in 2.5 and before), and therefore, by contrast, make checking type equality an even worse practice in recent Python versions than it already used to be.

---------------------

## Differences between `isinstance()` and `type()` in Python?

Allow me to demonstrate the difference:

### `type`

Say you need to ensure a certain behavior if your function gets a certain kind of argument (a common use-case for constructors). If you check for type like this:

```python
def foo(data):
    '''accepts a dict to construct something, string support in future'''
    if type(data) is not dict:
        # we're only going to test for dicts for now
        raise ValueError('only dicts are supported for now')
```

If we try to pass in a dict that is a subclass of `dict` (as we should be able to, if we're expecting our code to follow the principle of [Liskov Substitution](http://en.wikipedia.org/wiki/Liskov_substitution_principle), that subtypes can be substituted for types) our code breaks!:

```python
from collections import OrderedDict

foo(OrderedDict([('foo', 'bar'), ('fizz', 'buzz')]))
```

raises an error!

```python
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 3, in foo
ValueError: argument must be a dict
```

### `isinstance`

But if we use `isinstance`, we can support Liskov Substitution!:

```python
def foo(a_dict):
    if not isinstance(a_dict, dict):
        raise ValueError('argument must be a dict')
    return a_dict

foo(OrderedDict([('foo', 'bar'), ('fizz', 'buzz')]))
```

returns `OrderedDict([('foo', 'bar'), ('fizz', 'buzz')])`

## Abstract Base Classes

In fact, we can do even better. `collections` provides Abstract Base Classes that enforce minimal protocols for various types. In our case, if we only expect the `Mapping` protocol, we can do the following, and our code becomes even more flexible:

```python
from collections import Mapping

def foo(a_dict):
    if not isinstance(a_dict, Mapping):
        raise ValueError('argument must be a dict')
    return a_dict
```

## Response to comment:

> It should be noted that type can be used to check against multiple classes using `type(obj) in (A, B, C)`

Yes, you can test for equality of types, but instead of the above, use the multiple bases for control flow, unless you are specifically only allowing those types:

```python
isinstance(obj, (A, B, C))
```

The difference, again, is that `isinstance` supports subclasses that can be substituted for the parent without otherwise breaking the program, a property known as Liskov substitution.

Even better, though, invert your dependencies and don't check for specific types at all.

## Conclusion

So since we want to support substituting subclasses, in most cases, we want to avoid type-checking with `type` and prefer type-checking with `isinstance` - unless you really need to know the precise class of an instance.

------------------

A practical usage difference is how they handle `booleans`:

`True` and `False` are just keywords that mean `1` and `0` in python. Thus,

```python
isinstance(True, int)
```

and

```python
isinstance(False, int)
```

both return `True`. Both booleans are an instance of an integer. `type()`, however, is more clever:

```python
type(True) == int
```

returns `False`.

-----------------

### Python[[edit](https://en.wikipedia.org/w/index.php?title=Reflection_(computer_programming)&action=edit&section=13)]

The following is an example in [Python](https://en.wikipedia.org/wiki/Python_(programming_language)):

```python
# Without reflection
obj = Foo()
obj.hello()

# With reflection
obj = globals()['Foo']()
getattr(obj, 'hello')()

# With eval
eval('Foo().hello()')
```

-------------

`isinstance`(*object*, *classinfo*)

Return true if the *object* argument is an instance of the *classinfo* argument, or of a (direct, indirect or [virtual](https://docs.python.org/2/glossary.html#term-abstract-base-class)) subclass thereof. Also return true if *classinfo* is a type object (new-style class) and *object* is an object of that type or of a (direct, indirect or [virtual](https://docs.python.org/2/glossary.html#term-abstract-base-class)) subclass thereof. If *object* is not a class instance or an object of the given type, the function always returns false. If *classinfo* is a tuple of class or type objects (or recursively, other such tuples), return true if *object* is an instance of any of the classes or types. If *classinfo* is not a class, type, or tuple of classes, types, and such tuples, a [`TypeError`](https://docs.python.org/2/library/exceptions.html#exceptions.TypeError) exception is raised.



`issubclass`(*class*, *classinfo*)

Return true if *class* is a subclass (direct, indirect or [virtual](https://docs.python.org/2/glossary.html#term-abstract-base-class)) of *classinfo*. A class is considered a subclass of itself. *classinfo* may be a tuple of class objects, in which case every entry in *classinfo* will be checked. In any other case, a [`TypeError`](https://docs.python.org/2/library/exceptions.html#exceptions.TypeError) exception is raised.
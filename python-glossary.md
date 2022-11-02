# Python Glossary

```
>>>
```

The default Python prompt of the interactive shell. Often seen for code examples which can be executed interactively in the interpreter.

```
...
```

The default Python prompt of the interactive shell when entering code for an indented code block, when within a pair of matching left and right delimiters (parentheses, square brackets, curly braces or triple quotes), or after specifying a decorator.

2to3

A tool that tries to convert Python 2.x code to Python 3.x code by handling most of the incompatibilities which can be detected by parsing the source and traversing the parse tree.

2to3 is available in the standard library as [`lib2to3`](https://docs.python.org/2/library/2to3.html#module-lib2to3); a standalone entry point is provided as `Tools/scripts/2to3`. See [2to3 - Automated Python 2 to 3 code translation](https://docs.python.org/2/library/2to3.html#to3-reference).

abstract base class

Abstract base classes complement [duck-typing](https://docs.python.org/2/glossary.html#term-duck-typing) by providing a way to define interfaces when other techniques like [`hasattr()`](https://docs.python.org/2/library/functions.html#hasattr) would be clumsy or subtly wrong (for example with [magic methods](https://docs.python.org/2/reference/datamodel.html#new-style-special-lookup)). ABCs introduce virtual subclasses, which are classes that don’t inherit from a class but are still recognized by [`isinstance()`](https://docs.python.org/2/library/functions.html#isinstance) and [`issubclass()`](https://docs.python.org/2/library/functions.html#issubclass); see the [`abc`](https://docs.python.org/2/library/abc.html#module-abc) module documentation. Python comes with many built-in ABCs for data structures (in the [`collections`](https://docs.python.org/2/library/collections.html#module-collections) module), numbers (in the [`numbers`](https://docs.python.org/2/library/numbers.html#module-numbers) module), and streams (in the [`io`](https://docs.python.org/2/library/io.html#module-io) module). You can create your own ABCs with the [`abc`](https://docs.python.org/2/library/abc.html#module-abc) module.

argument

A value passed to a [function](https://docs.python.org/2/glossary.html#term-function) (or [method](https://docs.python.org/2/glossary.html#term-method)) when calling the function. There are two types of arguments:

- *keyword argument*: an argument preceded by an identifier (e.g. `name=`) in a function call or passed as a value in a dictionary preceded by `**`. For example, `3` and `5` are both keyword arguments in the following calls to [`complex()`](https://docs.python.org/2/library/functions.html#complex):

  ```
  complex(real=3, imag=5)
  complex(**{'real': 3, 'imag': 5})
  ```

- *positional argument*: an argument that is not a keyword argument. Positional arguments can appear at the beginning of an argument list and/or be passed as elements of an [iterable](https://docs.python.org/2/glossary.html#term-iterable) preceded by `*`. For example, `3` and `5` are both positional arguments in the following calls:

  ```
  complex(3, 5)
  complex(*(3, 5))
  ```

Arguments are assigned to the named local variables in a function body. See the [Calls](https://docs.python.org/2/reference/expressions.html#calls) section for the rules governing this assignment. Syntactically, any expression can be used to represent an argument; the evaluated value is assigned to the local variable.

See also the [parameter](https://docs.python.org/2/glossary.html#term-parameter) glossary entry and the FAQ question on [the difference between arguments and parameters](https://docs.python.org/2/faq/programming.html#faq-argument-vs-parameter).

attribute

A value associated with an object which is referenced by name using dotted expressions. For example, if an object *o* has an attribute *a* it would be referenced as *o.a*.

BDFL

Benevolent Dictator For Life, a.k.a. [Guido van Rossum](https://www.python.org/~guido/), Python’s creator.

bytes-like object

An object that supports the [buffer protocol](https://docs.python.org/2/c-api/buffer.html#bufferobjects), like [`str`](https://docs.python.org/2/library/functions.html#str), [`bytearray`](https://docs.python.org/2/library/functions.html#bytearray) or [`memoryview`](https://docs.python.org/2/library/stdtypes.html#memoryview). Bytes-like objects can be used for various operations that expect binary data, such as compression, saving to a binary file or sending over a socket. Some operations need the binary data to be mutable, in which case not all bytes-like objects can apply.

bytecode

Python source code is compiled into bytecode, the internal representation of a Python program in the CPython interpreter. The bytecode is also cached in `.pyc` and `.pyo` files so that executing the same file is faster the second time (recompilation from source to bytecode can be avoided). This “intermediate language” is said to run on a [virtual machine](https://docs.python.org/2/glossary.html#term-virtual-machine) that executes the machine code corresponding to each bytecode. Do note that bytecodes are not expected to work between different Python virtual machines, nor to be stable between Python releases.

A list of bytecode instructions can be found in the documentation for [the dis module](https://docs.python.org/2/library/dis.html#bytecodes).

class

A template for creating user-defined objects. Class definitions normally contain method definitions which operate on instances of the class.

classic class

Any class which does not inherit from [`object`](https://docs.python.org/2/library/functions.html#object). See [new-style class](https://docs.python.org/2/glossary.html#term-new-style-class). Classic classes have been removed in Python 3.

coercion

The implicit conversion of an instance of one type to another during an operation which involves two arguments of the same type. For example, `int(3.15)` converts the floating point number to the integer `3`, but in `3+4.5`, each argument is of a different type (one int, one float), and both must be converted to the same type before they can be added or it will raise a `TypeError`. Coercion between two operands can be performed with the `coerce` built-in function; thus, `3+4.5` is equivalent to calling `operator.add(*coerce(3, 4.5))` and results in `operator.add(3.0, 4.5)`. Without coercion, all arguments of even compatible types would have to be normalized to the same value by the programmer, e.g., `float(3)+4.5` rather than just `3+4.5`.

complex number

An extension of the familiar real number system in which all numbers are expressed as a sum of a real part and an imaginary part. Imaginary numbers are real multiples of the imaginary unit (the square root of `-1`), often written `i` in mathematics or `j` in engineering. Python has built-in support for complex numbers, which are written with this latter notation; the imaginary part is written with a `j` suffix, e.g., `3+1j`. To get access to complex equivalents of the [`math`](https://docs.python.org/2/library/math.html#module-math) module, use [`cmath`](https://docs.python.org/2/library/cmath.html#module-cmath). Use of complex numbers is a fairly advanced mathematical feature. If you’re not aware of a need for them, it’s almost certain you can safely ignore them.

context manager

An object which controls the environment seen in a [`with`](https://docs.python.org/2/reference/compound_stmts.html#with) statement by defining [`__enter__()`](https://docs.python.org/2/reference/datamodel.html#object.__enter__) and [`__exit__()`](https://docs.python.org/2/reference/datamodel.html#object.__exit__) methods. See [**PEP 343**](https://www.python.org/dev/peps/pep-0343).

CPython

The canonical implementation of the Python programming language, as distributed on [python.org](https://www.python.org/). The term “CPython” is used when necessary to distinguish this implementation from others such as Jython or IronPython.

decorator

A function returning another function, usually applied as a function transformation using the `@wrapper` syntax. Common examples for decorators are [`classmethod()`](https://docs.python.org/2/library/functions.html#classmethod) and [`staticmethod()`](https://docs.python.org/2/library/functions.html#staticmethod).

The decorator syntax is merely syntactic sugar, the following two function definitions are semantically equivalent:

```
def f(...):
    ...
f = staticmethod(f)

@staticmethod
def f(...):
    ...
```

The same concept exists for classes, but is less commonly used there. See the documentation for [function definitions](https://docs.python.org/2/reference/compound_stmts.html#function) and [class definitions](https://docs.python.org/2/reference/compound_stmts.html#class) for more about decorators.

descriptor

Any *new-style* object which defines the methods [`__get__()`](https://docs.python.org/2/reference/datamodel.html#object.__get__), [`__set__()`](https://docs.python.org/2/reference/datamodel.html#object.__set__), or [`__delete__()`](https://docs.python.org/2/reference/datamodel.html#object.__delete__). When a class attribute is a descriptor, its special binding behavior is triggered upon attribute lookup. Normally, using *a.b* to get, set or delete an attribute looks up the object named *b* in the class dictionary for *a*, but if *b* is a descriptor, the respective descriptor method gets called. Understanding descriptors is a key to a deep understanding of Python because they are the basis for many features including functions, methods, properties, class methods, static methods, and reference to super classes.

For more information about descriptors’ methods, see [Implementing Descriptors](https://docs.python.org/2/reference/datamodel.html#descriptors).

dictionary

An associative array, where arbitrary keys are mapped to values. The keys can be any object with [`__hash__()`](https://docs.python.org/2/reference/datamodel.html#object.__hash__) and [`__eq__()`](https://docs.python.org/2/reference/datamodel.html#object.__eq__) methods. Called a hash in Perl.

dictionary view

The objects returned from [`dict.viewkeys()`](https://docs.python.org/2/library/stdtypes.html#dict.viewkeys), [`dict.viewvalues()`](https://docs.python.org/2/library/stdtypes.html#dict.viewvalues), and [`dict.viewitems()`](https://docs.python.org/2/library/stdtypes.html#dict.viewitems) are called dictionary views. They provide a dynamic view on the dictionary’s entries, which means that when the dictionary changes, the view reflects these changes. To force the dictionary view to become a full list use `list(dictview)`. See [Dictionary view objects](https://docs.python.org/2/library/stdtypes.html#dict-views).

docstring

A string literal which appears as the first expression in a class, function or module. While ignored when the suite is executed, it is recognized by the compiler and put into the `__doc__` attribute of the enclosing class, function or module. Since it is available via introspection, it is the canonical place for documentation of the object.

duck-typing

A programming style which does not look at an object’s type to determine if it has the right interface; instead, the method or attribute is simply called or used (“If it looks like a duck and quacks like a duck, it must be a duck.”) By emphasizing interfaces rather than specific types, well-designed code improves its flexibility by allowing polymorphic substitution. Duck-typing avoids tests using [`type()`](https://docs.python.org/2/library/functions.html#type) or [`isinstance()`](https://docs.python.org/2/library/functions.html#isinstance). (Note, however, that duck-typing can be complemented with [abstract base classes](https://docs.python.org/2/glossary.html#term-abstract-base-class).) Instead, it typically employs [`hasattr()`](https://docs.python.org/2/library/functions.html#hasattr) tests or [EAFP](https://docs.python.org/2/glossary.html#term-eafp) programming.

EAFP

Easier to ask for forgiveness than permission. This common Python coding style assumes the existence of valid keys or attributes and catches exceptions if the assumption proves false. This clean and fast style is characterized by the presence of many [`try`](https://docs.python.org/2/reference/compound_stmts.html#try) and [`except`](https://docs.python.org/2/reference/compound_stmts.html#except) statements. The technique contrasts with the [LBYL](https://docs.python.org/2/glossary.html#term-lbyl) style common to many other languages such as C.

expression

A piece of syntax which can be evaluated to some value. In other words, an expression is an accumulation of expression elements like literals, names, attribute access, operators or function calls which all return a value. In contrast to many other languages, not all language constructs are expressions. There are also [statement](https://docs.python.org/2/glossary.html#term-statement)s which cannot be used as expressions, such as [`print`](https://docs.python.org/2/reference/simple_stmts.html#print) or [`if`](https://docs.python.org/2/reference/compound_stmts.html#if). Assignments are also statements, not expressions.

extension module

A module written in C or C++, using Python’s C API to interact with the core and with user code.

file object

An object exposing a file-oriented API (with methods such as `read()` or `write()`) to an underlying resource. Depending on the way it was created, a file object can mediate access to a real on-disk file or to another type of storage or communication device (for example standard input/output, in-memory buffers, sockets, pipes, etc.). File objects are also called *file-like objects* or *streams*.

There are actually three categories of file objects: raw binary files, buffered binary files and text files. Their interfaces are defined in the [`io`](https://docs.python.org/2/library/io.html#module-io) module. The canonical way to create a file object is by using the [`open()`](https://docs.python.org/2/library/functions.html#open) function.

file-like object

A synonym for [file object](https://docs.python.org/2/glossary.html#term-file-object).

finder

An object that tries to find the [loader](https://docs.python.org/2/glossary.html#term-loader) for a module. It must implement a method named `find_module()`. See [**PEP 302**](https://www.python.org/dev/peps/pep-0302) for details.

floor division

Mathematical division that rounds down to nearest integer. The floor division operator is `//`. For example, the expression `11 // 4` evaluates to `2` in contrast to the `2.75` returned by float true division. Note that `(-11) // 4` is `-3` because that is `-2.75` rounded *downward*. See [**PEP 238**](https://www.python.org/dev/peps/pep-0238).

function

A series of statements which returns some value to a caller. It can also be passed zero or more [arguments](https://docs.python.org/2/glossary.html#term-argument) which may be used in the execution of the body. See also [parameter](https://docs.python.org/2/glossary.html#term-parameter), [method](https://docs.python.org/2/glossary.html#term-method), and the [Function definitions](https://docs.python.org/2/reference/compound_stmts.html#function) section.

__future__

A pseudo-module which programmers can use to enable new language features which are not compatible with the current interpreter. For example, the expression `11/4` currently evaluates to `2`. If the module in which it is executed had enabled *true division* by executing:

```
from __future__ import division
```

the expression `11/4` would evaluate to `2.75`. By importing the [`__future__`](https://docs.python.org/2/library/__future__.html#module-__future__) module and evaluating its variables, you can see when a new feature was first added to the language and when it will become the default:

```
>>> import __future__
>>> __future__.division
_Feature((2, 2, 0, 'alpha', 2), (3, 0, 0, 'alpha', 0), 8192)
```

garbage collection

The process of freeing memory when it is not used anymore. Python performs garbage collection via reference counting and a cyclic garbage collector that is able to detect and break reference cycles.

generator

A function which returns an iterator. It looks like a normal function except that it contains [`yield`](https://docs.python.org/2/reference/simple_stmts.html#yield) statements for producing a series of values usable in a for-loop or that can be retrieved one at a time with the [`next()`](https://docs.python.org/2/library/functions.html#next) function. Each [`yield`](https://docs.python.org/2/reference/simple_stmts.html#yield) temporarily suspends processing, remembering the location execution state (including local variables and pending try-statements). When the generator resumes, it picks up where it left off (in contrast to functions which start fresh on every invocation).

generator expression

An expression that returns an iterator. It looks like a normal expression followed by a [`for`](https://docs.python.org/2/reference/compound_stmts.html#for) expression defining a loop variable, range, and an optional [`if`](https://docs.python.org/2/reference/compound_stmts.html#if) expression. The combined expression generates values for an enclosing function:

```
>>> sum(i*i for i in range(10))         # sum of squares 0, 1, 4, ... 81
285
```

GIL

See [global interpreter lock](https://docs.python.org/2/glossary.html#term-global-interpreter-lock).

global interpreter lock

The mechanism used by the [CPython](https://docs.python.org/2/glossary.html#term-cpython) interpreter to assure that only one thread executes Python [bytecode](https://docs.python.org/2/glossary.html#term-bytecode) at a time. This simplifies the CPython implementation by making the object model (including critical built-in types such as [`dict`](https://docs.python.org/2/library/stdtypes.html#dict)) implicitly safe against concurrent access. Locking the entire interpreter makes it easier for the interpreter to be multi-threaded, at the expense of much of the parallelism afforded by multi-processor machines.

However, some extension modules, either standard or third-party, are designed so as to release the GIL when doing computationally-intensive tasks such as compression or hashing. Also, the GIL is always released when doing I/O.

Past efforts to create a “free-threaded” interpreter (one which locks shared data at a much finer granularity) have not been successful because performance suffered in the common single-processor case. It is believed that overcoming this performance issue would make the implementation much more complicated and therefore costlier to maintain.

hashable

An object is *hashable* if it has a hash value which never changes during its lifetime (it needs a [`__hash__()`](https://docs.python.org/2/reference/datamodel.html#object.__hash__) method), and can be compared to other objects (it needs an [`__eq__()`](https://docs.python.org/2/reference/datamodel.html#object.__eq__) or [`__cmp__()`](https://docs.python.org/2/reference/datamodel.html#object.__cmp__) method). Hashable objects which compare equal must have the same hash value.

Hashability makes an object usable as a dictionary key and a set member, because these data structures use the hash value internally.

All of Python’s immutable built-in objects are hashable, while no mutable containers (such as lists or dictionaries) are. Objects which are instances of user-defined classes are hashable by default; they all compare unequal (except with themselves), and their hash value is derived from their [`id()`](https://docs.python.org/2/library/functions.html#id).

IDLE

An Integrated Development Environment for Python. IDLE is a basic editor and interpreter environment which ships with the standard distribution of Python.

immutable

An object with a fixed value. Immutable objects include numbers, strings and tuples. Such an object cannot be altered. A new object has to be created if a different value has to be stored. They play an important role in places where a constant hash value is needed, for example as a key in a dictionary.

integer division

Mathematical division discarding any remainder. For example, the expression `11/4` currently evaluates to `2` in contrast to the `2.75` returned by float division. Also called *floor division*. When dividing two integers the outcome will always be another integer (having the floor function applied to it). However, if one of the operands is another numeric type (such as a [`float`](https://docs.python.org/2/library/functions.html#float)), the result will be coerced (see [coercion](https://docs.python.org/2/glossary.html#term-coercion)) to a common type. For example, an integer divided by a float will result in a float value, possibly with a decimal fraction. Integer division can be forced by using the `//` operator instead of the `/` operator. See also [__future__](https://docs.python.org/2/glossary.html#term-future).

importing

The process by which Python code in one module is made available to Python code in another module.

importer

An object that both finds and loads a module; both a [finder](https://docs.python.org/2/glossary.html#term-finder) and [loader](https://docs.python.org/2/glossary.html#term-loader) object.

interactive

Python has an interactive interpreter which means you can enter statements and expressions at the interpreter prompt, immediately execute them and see their results. Just launch `python` with no arguments (possibly by selecting it from your computer’s main menu). It is a very powerful way to test out new ideas or inspect modules and packages (remember `help(x)`).

interpreted

Python is an interpreted language, as opposed to a compiled one, though the distinction can be blurry because of the presence of the bytecode compiler. This means that source files can be run directly without explicitly creating an executable which is then run. Interpreted languages typically have a shorter development/debug cycle than compiled ones, though their programs generally also run more slowly. See also [interactive](https://docs.python.org/2/glossary.html#term-interactive).

iterable

An object capable of returning its members one at a time. Examples of iterables include all sequence types (such as `list`, [`str`](https://docs.python.org/2/library/functions.html#str), and [`tuple`](https://docs.python.org/2/library/functions.html#tuple)) and some non-sequence types like [`dict`](https://docs.python.org/2/library/stdtypes.html#dict) and [`file`](https://docs.python.org/2/library/functions.html#file) and objects of any classes you define with an [`__iter__()`](https://docs.python.org/2/reference/datamodel.html#object.__iter__) or [`__getitem__()`](https://docs.python.org/2/reference/datamodel.html#object.__getitem__) method. Iterables can be used in a [`for`](https://docs.python.org/2/reference/compound_stmts.html#for) loop and in many other places where a sequence is needed ([`zip()`](https://docs.python.org/2/library/functions.html#zip), [`map()`](https://docs.python.org/2/library/functions.html#map), …). When an iterable object is passed as an argument to the built-in function [`iter()`](https://docs.python.org/2/library/functions.html#iter), it returns an iterator for the object. This iterator is good for one pass over the set of values. When using iterables, it is usually not necessary to call [`iter()`](https://docs.python.org/2/library/functions.html#iter) or deal with iterator objects yourself. The `for` statement does that automatically for you, creating a temporary unnamed variable to hold the iterator for the duration of the loop. See also [iterator](https://docs.python.org/2/glossary.html#term-iterator), [sequence](https://docs.python.org/2/glossary.html#term-sequence), and [generator](https://docs.python.org/2/glossary.html#term-generator).

iterator

An object representing a stream of data. Repeated calls to the iterator’s [`next()`](https://docs.python.org/2/reference/expressions.html#generator.next) method return successive items in the stream. When no more data are available a [`StopIteration`](https://docs.python.org/2/library/exceptions.html#exceptions.StopIteration) exception is raised instead. At this point, the iterator object is exhausted and any further calls to its [`next()`](https://docs.python.org/2/reference/expressions.html#generator.next) method just raise [`StopIteration`](https://docs.python.org/2/library/exceptions.html#exceptions.StopIteration) again. Iterators are required to have an [`__iter__()`](https://docs.python.org/2/reference/datamodel.html#object.__iter__) method that returns the iterator object itself so every iterator is also iterable and may be used in most places where other iterables are accepted. One notable exception is code which attempts multiple iteration passes. A container object (such as a `list`) produces a fresh new iterator each time you pass it to the [`iter()`](https://docs.python.org/2/library/functions.html#iter) function or use it in a [`for`](https://docs.python.org/2/reference/compound_stmts.html#for) loop. Attempting this with an iterator will just return the same exhausted iterator object used in the previous iteration pass, making it appear like an empty container.

More information can be found in [Iterator Types](https://docs.python.org/2/library/stdtypes.html#typeiter).

key function

A key function or collation function is a callable that returns a value used for sorting or ordering. For example, [`locale.strxfrm()`](https://docs.python.org/2/library/locale.html#locale.strxfrm) is used to produce a sort key that is aware of locale specific sort conventions.

A number of tools in Python accept key functions to control how elements are ordered or grouped. They include [`min()`](https://docs.python.org/2/library/functions.html#min), [`max()`](https://docs.python.org/2/library/functions.html#max), [`sorted()`](https://docs.python.org/2/library/functions.html#sorted), `list.sort()`, [`heapq.nsmallest()`](https://docs.python.org/2/library/heapq.html#heapq.nsmallest), [`heapq.nlargest()`](https://docs.python.org/2/library/heapq.html#heapq.nlargest), and [`itertools.groupby()`](https://docs.python.org/2/library/itertools.html#itertools.groupby).

There are several ways to create a key function. For example. the [`str.lower()`](https://docs.python.org/2/library/stdtypes.html#str.lower) method can serve as a key function for case insensitive sorts. Alternatively, an ad-hoc key function can be built from a [`lambda`](https://docs.python.org/2/reference/expressions.html#lambda) expression such as `lambda r: (r[0], r[2])`. Also, the [`operator`](https://docs.python.org/2/library/operator.html#module-operator) module provides three key function constructors: [`attrgetter()`](https://docs.python.org/2/library/operator.html#operator.attrgetter), [`itemgetter()`](https://docs.python.org/2/library/operator.html#operator.itemgetter), and [`methodcaller()`](https://docs.python.org/2/library/operator.html#operator.methodcaller). See the [Sorting HOW TO](https://docs.python.org/2/howto/sorting.html#sortinghowto) for examples of how to create and use key functions.

keyword argument

See [argument](https://docs.python.org/2/glossary.html#term-argument).

lambda

An anonymous inline function consisting of a single [expression](https://docs.python.org/2/glossary.html#term-expression) which is evaluated when the function is called. The syntax to create a lambda function is `lambda [parameters]: expression`

LBYL

Look before you leap. This coding style explicitly tests for pre-conditions before making calls or lookups. This style contrasts with the [EAFP](https://docs.python.org/2/glossary.html#term-eafp) approach and is characterized by the presence of many [`if`](https://docs.python.org/2/reference/compound_stmts.html#if) statements.

In a multi-threaded environment, the LBYL approach can risk introducing a race condition between “the looking” and “the leaping”. For example, the code, `if key in mapping: return mapping[key]` can fail if another thread removes *key* from *mapping* after the test, but before the lookup. This issue can be solved with locks or by using the EAFP approach.

list

A built-in Python [sequence](https://docs.python.org/2/glossary.html#term-sequence). Despite its name it is more akin to an array in other languages than to a linked list since access to elements is O(1).

list comprehension

A compact way to process all or part of the elements in a sequence and return a list with the results. `result = ["0x%02x" % x for x in range(256) if x % 2 == 0]` generates a list of strings containing even hex numbers (0x..) in the range from 0 to 255. The [`if`](https://docs.python.org/2/reference/compound_stmts.html#if) clause is optional. If omitted, all elements in `range(256)` are processed.

loader

An object that loads a module. It must define a method named `load_module()`. A loader is typically returned by a [finder](https://docs.python.org/2/glossary.html#term-finder). See [**PEP 302**](https://www.python.org/dev/peps/pep-0302) for details.

magic method

An informal synonym for [special method](https://docs.python.org/2/glossary.html#term-special-method).

mapping

A container object that supports arbitrary key lookups and implements the methods specified in the [`Mapping`](https://docs.python.org/2/library/collections.html#collections.Mapping) or [`MutableMapping`](https://docs.python.org/2/library/collections.html#collections.MutableMapping) [abstract base classes](https://docs.python.org/2/library/collections.html#collections-abstract-base-classes). Examples include [`dict`](https://docs.python.org/2/library/stdtypes.html#dict), [`collections.defaultdict`](https://docs.python.org/2/library/collections.html#collections.defaultdict), [`collections.OrderedDict`](https://docs.python.org/2/library/collections.html#collections.OrderedDict) and [`collections.Counter`](https://docs.python.org/2/library/collections.html#collections.Counter).

metaclass

The class of a class. Class definitions create a class name, a class dictionary, and a list of base classes. The metaclass is responsible for taking those three arguments and creating the class. Most object oriented programming languages provide a default implementation. What makes Python special is that it is possible to create custom metaclasses. Most users never need this tool, but when the need arises, metaclasses can provide powerful, elegant solutions. They have been used for logging attribute access, adding thread-safety, tracking object creation, implementing singletons, and many other tasks.

More information can be found in [Customizing class creation](https://docs.python.org/2/reference/datamodel.html#metaclasses).

method

A function which is defined inside a class body. If called as an attribute of an instance of that class, the method will get the instance object as its first [argument](https://docs.python.org/2/glossary.html#term-argument) (which is usually called `self`). See [function](https://docs.python.org/2/glossary.html#term-function) and [nested scope](https://docs.python.org/2/glossary.html#term-nested-scope).

method resolution order

Method Resolution Order is the order in which base classes are searched for a member during lookup. See [The Python 2.3 Method Resolution Order](https://www.python.org/download/releases/2.3/mro/) for details of the algorithm used by the Python interpreter since the 2.3 release.

module

An object that serves as an organizational unit of Python code. Modules have a namespace containing arbitrary Python objects. Modules are loaded into Python by the process of [importing](https://docs.python.org/2/glossary.html#term-importing).

See also [package](https://docs.python.org/2/glossary.html#term-package).

MRO

See [method resolution order](https://docs.python.org/2/glossary.html#term-method-resolution-order).

mutable

Mutable objects can change their value but keep their [`id()`](https://docs.python.org/2/library/functions.html#id). See also [immutable](https://docs.python.org/2/glossary.html#term-immutable).

named tuple

Any tuple-like class whose indexable elements are also accessible using named attributes (for example, [`time.localtime()`](https://docs.python.org/2/library/time.html#time.localtime) returns a tuple-like object where the *year* is accessible either with an index such as `t[0]` or with a named attribute like `t.tm_year`).

A named tuple can be a built-in type such as [`time.struct_time`](https://docs.python.org/2/library/time.html#time.struct_time), or it can be created with a regular class definition. A full featured named tuple can also be created with the factory function [`collections.namedtuple()`](https://docs.python.org/2/library/collections.html#collections.namedtuple). The latter approach automatically provides extra features such as a self-documenting representation like `Employee(name='jones', title='programmer')`.

namespace

The place where a variable is stored. Namespaces are implemented as dictionaries. There are the local, global and built-in namespaces as well as nested namespaces in objects (in methods). Namespaces support modularity by preventing naming conflicts. For instance, the functions `__builtin__.open()` and [`os.open()`](https://docs.python.org/2/library/os.html#os.open) are distinguished by their namespaces. Namespaces also aid readability and maintainability by making it clear which module implements a function. For instance, writing [`random.seed()`](https://docs.python.org/2/library/random.html#random.seed) or [`itertools.izip()`](https://docs.python.org/2/library/itertools.html#itertools.izip) makes it clear that those functions are implemented by the [`random`](https://docs.python.org/2/library/random.html#module-random) and [`itertools`](https://docs.python.org/2/library/itertools.html#module-itertools) modules, respectively.

nested scope

The ability to refer to a variable in an enclosing definition. For instance, a function defined inside another function can refer to variables in the outer function. Note that nested scopes work only for reference and not for assignment which will always write to the innermost scope. In contrast, local variables both read and write in the innermost scope. Likewise, global variables read and write to the global namespace.

new-style class

Any class which inherits from [`object`](https://docs.python.org/2/library/functions.html#object). This includes all built-in types like `list` and [`dict`](https://docs.python.org/2/library/stdtypes.html#dict). Only new-style classes can use Python’s newer, versatile features like `__slots__`, descriptors, properties, and [`__getattribute__()`](https://docs.python.org/2/reference/datamodel.html#object.__getattribute__).

More information can be found in [New-style and classic classes](https://docs.python.org/2/reference/datamodel.html#newstyle).

object

Any data with state (attributes or value) and defined behavior (methods). Also the ultimate base class of any [new-style class](https://docs.python.org/2/glossary.html#term-new-style-class).

package

A Python [module](https://docs.python.org/2/glossary.html#term-module) which can contain submodules or recursively, subpackages. Technically, a package is a Python module with an `__path__` attribute.

parameter

A named entity in a [function](https://docs.python.org/2/glossary.html#term-function) (or method) definition that specifies an [argument](https://docs.python.org/2/glossary.html#term-argument) (or in some cases, arguments) that the function can accept. There are four types of parameters:

- *positional-or-keyword*: specifies an argument that can be passed either [positionally](https://docs.python.org/2/glossary.html#term-argument) or as a [keyword argument](https://docs.python.org/2/glossary.html#term-argument). This is the default kind of parameter, for example *foo* and *bar* in the following:

  ```
  def func(foo, bar=None): ...
  ```

- *positional-only*: specifies an argument that can be supplied only by position. Python has no syntax for defining positional-only parameters. However, some built-in functions have positional-only parameters (e.g. [`abs()`](https://docs.python.org/2/library/functions.html#abs)).

- *var-positional*: specifies that an arbitrary sequence of positional arguments can be provided (in addition to any positional arguments already accepted by other parameters). Such a parameter can be defined by prepending the parameter name with `*`, for example *args* in the following:

  ```
  def func(*args, **kwargs): ...
  ```

- *var-keyword*: specifies that arbitrarily many keyword arguments can be provided (in addition to any keyword arguments already accepted by other parameters). Such a parameter can be defined by prepending the parameter name with `**`, for example *kwargs* in the example above.

Parameters can specify both optional and required arguments, as well as default values for some optional arguments.

See also the [argument](https://docs.python.org/2/glossary.html#term-argument) glossary entry, the FAQ question on [the difference between arguments and parameters](https://docs.python.org/2/faq/programming.html#faq-argument-vs-parameter), and the [Function definitions](https://docs.python.org/2/reference/compound_stmts.html#function) section.

PEP

Python Enhancement Proposal. A PEP is a design document providing information to the Python community, or describing a new feature for Python or its processes or environment. PEPs should provide a concise technical specification and a rationale for proposed features.

PEPs are intended to be the primary mechanisms for proposing major new features, for collecting community input on an issue, and for documenting the design decisions that have gone into Python. The PEP author is responsible for building consensus within the community and documenting dissenting opinions.

See [**PEP 1**](https://www.python.org/dev/peps/pep-0001).

positional argument

See [argument](https://docs.python.org/2/glossary.html#term-argument).

Python 3000

Nickname for the Python 3.x release line (coined long ago when the release of version 3 was something in the distant future.) This is also abbreviated “Py3k”.

Pythonic

An idea or piece of code which closely follows the most common idioms of the Python language, rather than implementing code using concepts common to other languages. For example, a common idiom in Python is to loop over all elements of an iterable using a [`for`](https://docs.python.org/2/reference/compound_stmts.html#for) statement. Many other languages don’t have this type of construct, so people unfamiliar with Python sometimes use a numerical counter instead:

```
for i in range(len(food)):
    print food[i]
```

As opposed to the cleaner, Pythonic method:

```
for piece in food:
    print piece
```

reference count

The number of references to an object. When the reference count of an object drops to zero, it is deallocated. Reference counting is generally not visible to Python code, but it is a key element of the [CPython](https://docs.python.org/2/glossary.html#term-cpython) implementation. The [`sys`](https://docs.python.org/2/library/sys.html#module-sys) module defines a [`getrefcount()`](https://docs.python.org/2/library/sys.html#sys.getrefcount) function that programmers can call to return the reference count for a particular object.

__slots__

A declaration inside a [new-style class](https://docs.python.org/2/glossary.html#term-new-style-class) that saves memory by pre-declaring space for instance attributes and eliminating instance dictionaries. Though popular, the technique is somewhat tricky to get right and is best reserved for rare cases where there are large numbers of instances in a memory-critical application.

sequence

An [iterable](https://docs.python.org/2/glossary.html#term-iterable) which supports efficient element access using integer indices via the [`__getitem__()`](https://docs.python.org/2/reference/datamodel.html#object.__getitem__) special method and defines a [`len()`](https://docs.python.org/2/library/functions.html#len) method that returns the length of the sequence. Some built-in sequence types are `list`, [`str`](https://docs.python.org/2/library/functions.html#str), [`tuple`](https://docs.python.org/2/library/functions.html#tuple), and [`unicode`](https://docs.python.org/2/library/functions.html#unicode). Note that [`dict`](https://docs.python.org/2/library/stdtypes.html#dict) also supports [`__getitem__()`](https://docs.python.org/2/reference/datamodel.html#object.__getitem__) and [`__len__()`](https://docs.python.org/2/reference/datamodel.html#object.__len__), but is considered a mapping rather than a sequence because the lookups use arbitrary [immutable](https://docs.python.org/2/glossary.html#term-immutable) keys rather than integers.

slice

An object usually containing a portion of a [sequence](https://docs.python.org/2/glossary.html#term-sequence). A slice is created using the subscript notation, `[]` with colons between numbers when several are given, such as in `variable_name[1:3:5]`. The bracket (subscript) notation uses [`slice`](https://docs.python.org/2/library/functions.html#slice) objects internally (or in older versions, [`__getslice__()`](https://docs.python.org/2/reference/datamodel.html#object.__getslice__) and [`__setslice__()`](https://docs.python.org/2/reference/datamodel.html#object.__setslice__)).

special method

A method that is called implicitly by Python to execute a certain operation on a type, such as addition. Such methods have names starting and ending with double underscores. Special methods are documented in [Special method names](https://docs.python.org/2/reference/datamodel.html#specialnames).

statement

A statement is part of a suite (a “block” of code). A statement is either an [expression](https://docs.python.org/2/glossary.html#term-expression) or one of several constructs with a keyword, such as [`if`](https://docs.python.org/2/reference/compound_stmts.html#if), [`while`](https://docs.python.org/2/reference/compound_stmts.html#while) or [`for`](https://docs.python.org/2/reference/compound_stmts.html#for).

struct sequence

A tuple with named elements. Struct sequences expose an interface similiar to [named tuple](https://docs.python.org/2/glossary.html#term-named-tuple) in that elements can be accessed either by index or as an attribute. However, they do not have any of the named tuple methods like [`_make()`](https://docs.python.org/2/library/collections.html#collections.somenamedtuple._make) or [`_asdict()`](https://docs.python.org/2/library/collections.html#collections.somenamedtuple._asdict). Examples of struct sequences include [`sys.float_info`](https://docs.python.org/2/library/sys.html#sys.float_info) and the return value of [`os.stat()`](https://docs.python.org/2/library/os.html#os.stat).

triple-quoted string

A string which is bound by three instances of either a quotation mark (“) or an apostrophe (‘). While they don’t provide any functionality not available with single-quoted strings, they are useful for a number of reasons. They allow you to include unescaped single and double quotes within a string and they can span multiple lines without the use of the continuation character, making them especially useful when writing docstrings.

type

The type of a Python object determines what kind of object it is; every object has a type. An object’s type is accessible as its [`__class__`](https://docs.python.org/2/library/stdtypes.html#instance.__class__) attribute or can be retrieved with `type(obj)`.

universal newlines

A manner of interpreting text streams in which all of the following are recognized as ending a line: the Unix end-of-line convention `'\n'`, the Windows convention `'\r\n'`, and the old Macintosh convention `'\r'`. See [**PEP 278**](https://www.python.org/dev/peps/pep-0278) and [**PEP 3116**](https://www.python.org/dev/peps/pep-3116), as well as [`str.splitlines()`](https://docs.python.org/2/library/stdtypes.html#str.splitlines) for an additional use.

virtual environment

A cooperatively isolated runtime environment that allows Python users and applications to install and upgrade Python distribution packages without interfering with the behaviour of other Python applications running on the same system.

virtual machine

A computer defined entirely in software. Python’s virtual machine executes the [bytecode](https://docs.python.org/2/glossary.html#term-bytecode) emitted by the bytecode compiler.

Zen of Python

Listing of Python design principles and philosophies that are helpful in understanding and using the language. The listing can be found by typing “`import this`” at the interactive prompt.
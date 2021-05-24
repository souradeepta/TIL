# Python [Programming FAQ](https://docs.python.org/2/faq/programming.html#id1)



### [Is there a tool to help find bugs or perform static analysis?](https://docs.python.org/2/faq/programming.html#id4)

Yes.

PyChecker is a static analysis tool that finds bugs in Python source code and warns about code complexity and style. You can get PyChecker from http://pychecker.sourceforge.net/.

[Pylint](https://www.pylint.org/) is another tool that checks if a module satisfies a coding standard, and also makes it possible to write plug-ins to add a custom feature. In addition to the bug checking that PyChecker performs, Pylint offers some additional features such as checking line length, whether variable names are well-formed according to your coding standard, whether declared interfaces are fully implemented, and more. https://docs.pylint.org/ provides a full list of Pylint’s features.



### [How can I create a stand-alone binary from a Python script?](https://docs.python.org/2/faq/programming.html#id5)

You don’t need the ability to compile Python to C code if all you want is a stand-alone program that users can download and run without having to install the Python distribution first. There are a number of tools that determine the set of modules required by a program and bind these modules together with a Python binary to produce a single executable.

One is to use the freeze tool, which is included in the Python source tree as `Tools/freeze`. It converts Python byte code to C arrays; a C compiler you can embed all your modules into a new program, which is then linked with the standard Python modules.

It works by scanning your source recursively for import statements (in both forms) and looking for the modules in the standard Python path as well as in the source directory (for built-in modules). It then turns the bytecode for modules written in Python into C code (array initializers that can be turned into code objects using the marshal module) and creates a custom-made config file that only contains those built-in modules which are actually used in the program. It then compiles the generated C code and links it with the rest of the Python interpreter to form a self-contained binary which acts exactly like your script.

Obviously, freeze requires a C compiler. There are several other utilities which don’t. One is Thomas Heller’s py2exe (Windows only) at

> http://www.py2exe.org/

### [Are there coding standards or a style guide for Python programs?](https://docs.python.org/2/faq/programming.html#id6)

Yes. The coding style required for standard library modules is documented as [**PEP 8**](https://www.python.org/dev/peps/pep-0008).

### [My program is too slow. How do I speed it up?](https://docs.python.org/2/faq/programming.html#id7)

That’s a tough one, in general. There are many tricks to speed up Python code; consider rewriting parts in C as a last resort.

In some cases it’s possible to automatically translate Python to C or x86 assembly language, meaning that you don’t have to modify your code to gain increased speed.

[Pyrex](http://www.cosc.canterbury.ac.nz/~greg/python/Pyrex/) can compile a slightly modified version of Python code into a C extension, and can be used on many different platforms.

[Psyco](http://psyco.sourceforge.net/) is a just-in-time compiler that translates Python code into x86 assembly language. If you can use it, Psyco can provide dramatic speedups for critical functions.

One thing to notice is that *function and (especially) method calls are rather expensive; i*f you have designed a purely OO interface with lots of tiny functions that don’t do much more than *get or set an instance variable or call another method,* you might consider using a more direct way such as directly accessing instance variables. Also see the standard module [`profile`](https://docs.python.org/2/library/profile.html#module-profile) which makes it possible to find out where your program is spending most of its time (if you have some patience – the profiling itself can slow your program down by an order of magnitude).

Remember that many standard optimization heuristics you may know from other programming experience may well apply to Python. For example it *may be faster to send output to output devices using larger writes rather than smaller ones* in order to reduce the overhead of kernel system calls. Thus CGI scripts that write all output in “one shot” may be faster than those that write lots of small pieces of output.

Also, be sure to use Python’s core features where appropriate. For example, slicing allows programs to chop up lists and other sequence objects in a single tick of the interpreter’s mainloop using highly optimized C implementations. Thus to get the same effect as:

```python
L2 = []
for i in range(3):
    L2.append(L1[i])
```

it is much shorter and far faster to use

```python
L2 = list(L1[:3])  # "list" is redundant if L1 is a list.
```

Note that the functionally-oriented built-in functions such as [`map()`](https://docs.python.org/2/library/functions.html#map), [`zip()`](https://docs.python.org/2/library/functions.html#zip), and friends can be a convenient accelerator for loops that perform a single task. For example to pair the elements of two lists together:

```python
>>> zip([1, 2, 3], [4, 5, 6])
[(1, 4), (2, 5), (3, 6)]
```

or to compute a number of sines:

```python
>>> map(math.sin, (1, 2, 3, 4))
[0.841470984808, 0.909297426826, 0.14112000806, -0.756802495308]
```

The operation completes very quickly in such cases.

Other examples include the `join()` and `split()` [methods of string objects](https://docs.python.org/2/library/stdtypes.html#string-methods). For example if s1..s7 are large (10K+) strings then `"".join([s1,s2,s3,s4,s5,s6,s7])` may be far faster than the more obvious `s1+s2+s3+s4+s5+s6+s7`, since the “summation” will compute many subexpressions, whereas `join()` does all the copying in one pass. For manipulating strings, use the `replace()` and the `format()` [methods on string objects](https://docs.python.org/2/library/stdtypes.html#string-methods). Use regular expressions only when you’re not dealing with constant string patterns. You may still use [the old % operations](https://docs.python.org/2/library/stdtypes.html#string-formatting) `string % tuple` and `string % dictionary`.

Be sure to use the `list.sort()` built-in method to do sorting, and see the [sorting mini-HOWTO](https://wiki.python.org/moin/HowTo/Sorting) for examples of moderately advanced usage. `list.sort()` beats other techniques for sorting in all but the most extreme circumstances.

Another common trick is to “push loops into functions or methods.” For example suppose you have a program that runs slowly and you use the profiler to determine that a Python function `ff()` is being called lots of times. If you notice that `ff()`:

```python
def ff(x):
    ... # do something with x computing result...
    return result
```

tends to be called in loops like:

```python
list = map(ff, oldlist)
```

or:

```python
for x in sequence:
    value = ff(x)
    ... # do something with value...
```

then you can often eliminate function call overhead by rewriting `ff()` to:

```python
def ffseq(seq):
    resultseq = []
    for x in seq:
        ... # do something with x computing result...
        resultseq.append(result)
    return resultseq
```

and rewrite the two examples to `list = ffseq(oldlist)` and to:

```python
for value in ffseq(sequence):
    ... # do something with value...
```

Single calls to `ff(x)` translate to `ffseq([x])[0]` with little penalty. Of course this technique is not always appropriate and there are other variants which you can figure out.



You can gain some performance by explicitly storing the results of a function or method lookup into a local variable. A loop like:

```python
for key in token:
    dict[key] = dict.get(key, 0) + 1
```

resolves `dict.get` every iteration. If the method isn’t going to change, a slightly faster implementation is:

```python
dict_get = dict.get  # look up the method once
for key in token:
    dict[key] = dict_get(key, 0) + 1
```

Default arguments can be used to determine values once, at compile time instead of at run time. This can only be done for functions or objects which will not be changed during program execution, such as replacing

```python
def degree_sin(deg):
    return math.sin(deg * math.pi / 180.0)
```

with

```python
def degree_sin(deg, factor=math.pi/180.0, sin=math.sin):
    return sin(deg * factor)
```

Because this trick uses default arguments for terms which should not be changed, it should only be used when you are not concerned with presenting a possibly confusing API to your users.

### [Why am I getting an UnboundLocalError when the variable has a value?](https://docs.python.org/2/faq/programming.html#id9)

It can be a surprise to get the UnboundLocalError in previously working code when it is modified by adding an assignment statement somewhere in the body of a function.

This code:

```python
>>> x = 10
>>> def bar():
...     print x
>>> bar()
10
```

works, but this code:

```python
>>> x = 10
>>> def foo():
...     print x
...     x += 1
```

results in an UnboundLocalError:

```python
>>> foo()
Traceback (most recent call last):
  ...
UnboundLocalError: local variable 'x' referenced before assignment
```

This is because when you make an assignment to a variable in a scope, that variable becomes local to that scope and shadows any similarly named variable in the outer scope. Since the last statement in foo assigns a new value to `x`, the compiler recognizes it as a local variable. Consequently when the earlier `print x` attempts to print the uninitialized local variable and an error results.

In the example above you can access the outer scope variable by declaring it global:

```python
>>> x = 10
>>> def foobar():
...     global x
...     print x
...     x += 1
>>> foobar()
10
```

This explicit declaration is required in order to remind you that (unlike the superficially analogous situation with class and instance variables) you are actually modifying the value of the variable in the outer scope:

```python
>>> print x
11
```

### [What are the rules for local and global variables in Python?](https://docs.python.org/2/faq/programming.html#id10)

In Python, variables that are only referenced inside a function are implicitly global. If a variable is assigned a value anywhere within the function’s body, it’s assumed to be a local unless explicitly declared as global.

Though a bit surprising at first, a moment’s consideration explains this. On one hand, requiring [`global`](https://docs.python.org/2/reference/simple_stmts.html#global) for assigned variables provides a bar against unintended side-effects. On the other hand, if `global` was required for all global references, you’d be using `global` all the time. You’d have to declare as global every reference to a built-in function or to a component of an imported module. This clutter would defeat the usefulness of the `global` declaration for identifying side-effects.

# Python locals()

#### The locals() method updates and returns a dictionary of the current local symbol table.

A symbol table is a data structure maintained by a compiler which contains all necessary information about the program.

These include variable names, methods, classes, etc.

There are mainly two kinds of symbol table.

1. Global symbol table
2. Local symbol table

A **Global** symbol table stores all information related to the global scope of the program, and is accessed in Python using [globals()](https://www.programiz.com/python-programming/methods/built-in/globals) method.



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

```python
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

### [What are the “best practices” for using import in a module?](https://docs.python.org/2/faq/programming.html#id13)

In general, don’t use `from modulename import *`. Doing so clutters the importer’s namespace, and makes it much harder for linters to detect undefined names.

Import modules at the top of a file. Doing so makes it clear what other modules your code requires and avoids questions of whether the module name is in scope. Using one import per line makes it easy to add and delete module imports, but using multiple imports per line uses less screen space.

It’s good practice if you import modules in the following order:

1. standard library modules – e.g. `sys`, `os`, `getopt`, `re`
2. third-party library modules (anything installed in Python’s site-packages directory) – e.g. mx.DateTime, ZODB, PIL.Image, etc.
3. locally-developed modules

Only use explicit relative package imports. If you’re writing code that’s in the `package.sub.m1` module and want to import `package.sub.m2`, do not just write `import m2`, even though it’s legal. Write `from package.sub import m2` or `from . import m2` instead.

It is sometimes necessary to move imports to a function or class to avoid problems with circular imports. Gordon McMillan says:

> Circular imports are fine where both modules use the “import <module>” form of import. They fail when the 2nd module wants to grab a name out of the first (“from module import name”) and the import is at the top level. That’s because names in the 1st are not yet available, because the first module is busy importing the 2nd.

In this case, if the second module is only used in one function, then the import can easily be moved into that function. By the time the import is called, the first module will have finished initializing, and the second module can do its import.

It may also be necessary to move imports out of the top level of code if some of the modules are platform-specific. In that case, it may not even be possible to import all of the modules at the top of the file. In this case, importing the correct modules in the corresponding platform-specific code is a good option.

*Only move imports into a local scope, such as inside a function definition, if it’s necessary to solve a problem such as avoiding a circular import or are trying to reduce the initialization time of a module.* This technique is especially helpful if many of the imports are unnecessary depending on how the program executes. You may also want to move imports into a function if the modules are only ever used in that function. Note that loading a module the first time may be expensive because of the one time initialization of the module, but loading a module multiple times is virtually free, costing only a couple of dictionary lookups. Even if the module name has gone out of scope, the module is probably available in [`sys.modules`](https://docs.python.org/2/library/sys.html#sys.modules).

### [Why are default values shared between objects?](https://docs.python.org/2/faq/programming.html#id14)

This type of bug commonly bites neophyte programmers. Consider this function:

```python
def foo(mydict={}):  # Danger: shared reference to one dict for all calls
    ... compute something ...
    mydict[key] = value
    return mydict
```

The first time you call this function, `mydict` contains a single item. The second time, `mydict` contains two items because when `foo()` begins executing, `mydict` starts out with an item already in it.

It is often expected that a function call creates new objects for default values. This is not what happens. *Default values are created exactly once, when the function is defined. If that object is changed, like the dictionary in this example, subsequent calls to the function will refer to this changed object.*

*By definition, immutable objects such as numbers, strings, tuples, and `None`, are safe from change. Changes to mutable objects such as dictionaries, lists, and class instances can lead to confusion.*

Because of this feature, it is good programming practice to not use mutable objects as default values. Instead, use `None` as the default value and inside the function, check if the parameter is `None` and create a new list/dictionary/whatever if it is. For example, don’t write:

```python
def foo(mydict={}):
    ...
```

but:

```python
def foo(mydict=None):
    if mydict is None:
        mydict = {}  # create a new dict for local namespace
```

This feature can be useful. When you have a function that’s time-consuming to compute, a common technique is to cache the parameters and the resulting value of each call to the function, and return the cached value if the same value is requested again. This is called “memoizing”, and can be implemented like this:

```python
# Callers will never provide a third parameter for this function.
def expensive(arg1, arg2, _cache={}):
    if (arg1, arg2) in _cache:
        return _cache[(arg1, arg2)]

    # Calculate the value
    result = ... expensive computation ...
    _cache[(arg1, arg2)] = result           # Store result in the cache
    return result
```

You could use a global variable containing a dictionary instead of the default value; it’s a matter of taste.

### [How can I pass optional or keyword parameters from one function to another?](https://docs.python.org/2/faq/programming.html#id15)

Collect the arguments using the `*` and `**` specifiers in the function’s parameter list; this gives you the positional arguments as a tuple and the keyword arguments as a dictionary. You can then pass these arguments when calling another function by using `*` and `**`:

```python
def f(x, *args, **kwargs):
    ...
    kwargs['width'] = '14.3c'
    ...
    g(x, *args, **kwargs)
```

In the unlikely case that you care about Python versions older than 2.0, use [`apply()`](https://docs.python.org/2/library/functions.html#apply):

```python
def f(x, *args, **kwargs):
    ...
    kwargs['width'] = '14.3c'
    ...
    apply(g, (x,)+args, kwargs)
```

### [Why did changing list ‘y’ also change list ‘x’?](https://docs.python.org/2/faq/programming.html#id17)

If you wrote code like:

```python
>>> x = []
>>> y = x
>>> y.append(10)
>>> y
[10]
>>> x
[10]
```

you might be wondering why appending an element to `y` changed `x` too.

There are two factors that produce this result:

1. Variables are simply names that refer to objects. Doing `y = x` doesn’t create a copy of the list – it creates a new variable `y` that refers to the same object `x` refers to. This means that there is only one object (the list), and both `x` and `y` refer to it.
2. Lists are [mutable](https://docs.python.org/2/glossary.html#term-mutable), which means that you can change their content.

After the call to `append()`, the content of the mutable object has changed from `[]` to `[10]`. Since both the variables refer to the same object, using either name accesses the modified value `[10]`.

If we instead assign an immutable object to `x`:

```python
>>> x = 5  # ints are immutable
>>> y = x
>>> x = x + 1  # 5 can't be mutated, we are creating a new object here
>>> x
6
>>> y
5
```

we can see that in this case `x` and `y` are not equal anymore. This is because integers are [immutable](https://docs.python.org/2/glossary.html#term-immutable), and when we do `x = x + 1` we are not mutating the int `5` by incrementing its value; instead, we are creating a new object (the int `6`) and assigning it to `x` (that is, changing which object `x` refers to). After this assignment we have two objects (the ints `6` and `5`) and two variables that refer to them (`x` now refers to `6` but `y` still refers to `5`).

Some operations (for example `y.append(10)` and `y.sort()`) mutate the object, whereas superficially similar operations (for example `y = y + [10]` and `sorted(y)`) create a new object. In general in Python (and in all cases in the standard library) a method that mutates an object will return `None` to help avoid getting the two types of operations confused. So if you mistakenly write `y.sort()` thinking it will give you a sorted copy of `y`, you’ll instead end up with `None`, which will likely cause your program to generate an easily diagnosed error.

However, there is one class of operations where the same operation sometimes has different behaviors with different types: the augmented assignment operators. For example, `+=` mutates lists but not tuples or ints (`a_list += [1, 2, 3]` is equivalent to `a_list.extend([1, 2, 3])` and mutates `a_list`, whereas `some_tuple += (1, 2, 3)` and `some_int += 1` create new objects).

In other words:

- If we have a mutable object (`list`, [`dict`](https://docs.python.org/2/library/stdtypes.html#dict), [`set`](https://docs.python.org/2/library/stdtypes.html#set), etc.), we can use some specific operations to mutate it and all the variables that refer to it will see the change.
- If we have an immutable object ([`str`](https://docs.python.org/2/library/functions.html#str), [`int`](https://docs.python.org/2/library/functions.html#int), [`tuple`](https://docs.python.org/2/library/functions.html#tuple), etc.), all the variables that refer to it will always see the same value, but operations that transform that value into a new value always return a new object.

If you want to know if two variables refer to the same object or not, you can use the [`is`](https://docs.python.org/2/reference/expressions.html#is) operator, or the built-in function [`id()`](https://docs.python.org/2/library/functions.html#id).



### [How do I write a function with output parameters (call by reference)?](https://docs.python.org/2/faq/programming.html#id18)

Remember that arguments are passed by assignment in Python. Since assignment just creates references to objects, there’s no alias between an argument name in the caller and callee, and so no call-by-reference per se. You can achieve the desired effect in a number of ways.

1. By returning a tuple of the results:

   ```python
   def func2(a, b):
       a = 'new-value'        # a and b are local names
       b = b + 1              # assigned to new objects
       return a, b            # return new values
   
   x, y = 'old-value', 99
   x, y = func2(x, y)
   print x, y                 # output: new-value 100
   ```

   This is almost always the clearest solution.

2. By using global variables. This isn’t thread-safe, and is not recommended.

3. By passing a mutable (changeable in-place) object:

   ```python
   def func1(a):
       a[0] = 'new-value'     # 'a' references a mutable list
       a[1] = a[1] + 1        # changes a shared object
   
   args = ['old-value', 99]
   func1(args)
   print args[0], args[1]     # output: new-value 100
   ```

4. By passing in a dictionary that gets mutated:

   ```python
   def func3(args):
       args['a'] = 'new-value'     # args is a mutable dictionary
       args['b'] = args['b'] + 1   # change it in-place
   
   args = {'a': 'old-value', 'b': 99}
   func3(args)
   print args['a'], args['b']
   ```

5. Or bundle up values in a class instance:

   ```python
   class callByRef:
       def __init__(self, **args):
           for (key, value) in args.items():
               setattr(self, key, value)
   
   def func4(args):
       args.a = 'new-value'        # args is a mutable callByRefpython
       args.b = args.b + 1         # change object in-place
   
   args = callByRef(a='old-value', b=99)
   func4(args)
   print args.a, args.b
   ```

   There’s almost never a good reason to get this complicated.

Your best choice is to return a tuple containing the multiple results.

### [How do you make a higher order function in Python?](https://docs.python.org/2/faq/programming.html#id19)

You have two choices: you can use nested scopes or you can use callable objects. For example, suppose you wanted to define `linear(a,b)` which returns a function `f(x)` that computes the value `a*x+b`. Using nested scopes:

```python
def linear(a, b):
    def result(x):
        return a * x + b
    return result
```

Or using a callable object:

```python
class linear:

    def __init__(self, a, b):
        self.a, self.b = a, b

    def __call__(self, x):
        return self.a * x + self.b
```

In both cases,

```python
taxes = linear(0.3, 2)
```

gives a callable object where `taxes(10e6) == 0.3 * 10e6 + 2`.

The callable object approach has the disadvantage that it is a bit slower and results in slightly longer code. However, note that a collection of callables can share their signature via inheritance:

```python
class exponential(linear):
    # __init__ inherited
    def __call__(self, x):
        return self.a * (x ** self.b)
```

Object can encapsulate state for several methods:

```python
class counter:

    value = 0

    def set(self, x):
        self.value = x

    def up(self):
        self.value = self.value + 1

    def down(self):
        self.value = self.value - 1

count = counter()
inc, dec, reset = count.up, count.down, count.set
```

Here `inc()`, `dec()` and `reset()` act like functions which share the same counting variable.

### [How do I copy an object in Python?](https://docs.python.org/2/faq/programming.html#id20)

In general, try [`copy.copy()`](https://docs.python.org/2/library/copy.html#copy.copy) or [`copy.deepcopy()`](https://docs.python.org/2/library/copy.html#copy.deepcopy) for the general case. Not all objects can be copied, but most can.

Some objects can be copied more easily. Dictionaries have a [`copy()`](https://docs.python.org/2/library/stdtypes.html#dict.copy) method:

```python
newdict = olddict.copy()
```

Sequences can be copied by slicing:

```python
new_l = l[:]
```

### [How can I find the methods or attributes of an object?](https://docs.python.org/2/faq/programming.html#id21)

For an instance x of a user-defined class, `dir(x)` returns an alphabetized list of the names containing the instance attributes and methods and attributes defined by its class.

### [How can my code discover the name of an object?](https://docs.python.org/2/faq/programming.html#id22)

Generally speaking, it can’t, because objects don’t really have names. Essentially, assignment always binds a name to a value; The same is true of `def` and `class` statements, but in that case the value is a callable. Consider the following code:

```python
>>> class A:
...     pass
...
>>> B = A
>>> a = B()
>>> b = a
>>> print b
<__main__.A instance at 0x16D07CC>
>>> print a
<__main__.A instance at 0x16D07CC>
```

Arguably the class has a name: even though it is bound to two names and invoked through the name B the created instance is still reported as an instance of class A. However, it is impossible to say whether the instance’s name is a or b, since both names are bound to the same value.

Generally speaking it should not be necessary for your code to “know the names” of particular values. Unless you are deliberately writing introspective programs, this is usually an indication that a change of approach might be beneficial.

In comp.lang.python, Fredrik Lundh once gave an excellent analogy in answer to this question:

> The same way as you get the name of that cat you found on your porch: the cat (object) itself cannot tell you its name, and it doesn’t really care – so the only way to find out what it’s called is to ask all your neighbours (namespaces) if it’s their cat (object)…
>
> ….and don’t be surprised if you’ll find that it’s known by many names, or no name at all!

### [What’s up with the comma operator’s precedence?](https://docs.python.org/2/faq/programming.html#id23)

Comma is not an operator in Python. Consider this session:

```python
>>> "a" in "b", "a"
(False, 'a')
```

Since the comma is not an operator, but a separator between expressions the above is evaluated as if you had entered:

```python
("a" in "b"), "a"
```

not:

```python
"a" in ("b", "a")
```

The same is true of the various assignment operators (`=`, `+=` etc). They are not truly operators but syntactic delimiters in assignment statements.

### [Is there an equivalent of C’s “?:” ternary operator?](https://docs.python.org/2/faq/programming.html#id24)

Yes, this feature was added in Python 2.5. The syntax would be as follows:

```python
[on_true] if [expression] else [on_false]

x, y = 50, 25

small = x if x < y else y
```

For versions previous to 2.5 the answer would be ‘No’.




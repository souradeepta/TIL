# Python Programming/Functions

## Function Calls[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Functions&action=edit&section=1)]

A *callable object* is an object that can accept some arguments (also called parameters) and possibly return an object (often a tuple containing multiple objects).

A function is the simplest callable object in Python, but there are others, such as [classes](https://en.wikibooks.org/wiki/Python_Programming/Classes) or certain class instances.

#### Defining Functions[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Functions&action=edit&section=2)]

A function is defined in Python by the following format:

```python
def functionname(arg1, arg2, ...):
    statement1
    statement2
    ...
>>> def functionname(arg1,arg2):
...     return arg1+arg2
...
>>> t = functionname(24,24) # Result: 48
```

If a function takes no arguments, it must still include the parentheses, but without anything in them:

```
def functionname():
    statement1
    statement2
    ...
```

The arguments in the function definition bind the arguments passed at function invocation (i.e. when the function is called), which are called actual parameters, to the names given when the function is defined, which are called formal parameters. The interior of the function has no knowledge of the names given to the actual parameters; the names of the actual parameters may not even be accessible (they could be inside another function).

A function can 'return' a value, for example:

```
def square(x):
    return x*x
```

A function can define variables within the function body, which are considered 'local' to the function. The locals together with the arguments comprise all the variables within the scope of the function. Any names within the function are unbound when the function returns or reaches the end of the function body.

You can **return multiple values** as follows:

```
def first2items(list1):
  return list1[0], list1[1]
a, b = first2items(["Hello", "world", "hi", "universe"])
print a + " " + b
```

Keywords: returning multiple values, multiple return values.

### Declaring Arguments[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Functions&action=edit&section=3)]

When calling a function that takes some values for further processing, we need to send some values as Function **Arguments**. For example:

```
>>> def find_max(a,b):
   if(a > b):
      return str(a) + " is greater than " + str(b)
   elif(b > a):
      return str(b) + " is greater than " + str(a)
>>> find_max(30, 45)  #Here (30, 45) are the arguments passing for finding max between this two numbers
The output will be: 45 is greater than 30
```

#### Default Argument Values[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Functions&action=edit&section=4)]

If any of the formal parameters in the function definition are declared with the format "arg = value," then you will have the option of not specifying a value for those arguments when calling the function. If you do not specify a value, then that parameter will have the default value given when the function executes.

```
>>> def display_message(message, truncate_after=4):
...     print message[:truncate_after]
...
>>> display_message("message")
mess
>>> display_message("message", 6)
messag
```

Links:

- [4.7.1. Default Argument Values](https://docs.python.org/2/tutorial/controlflow.html#default-argument-values), The Python Tutorial, docs.python.org

#### Variable-Length Argument Lists[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Functions&action=edit&section=5)]

Python allows you to declare two special arguments which allow you to create arbitrary-length argument lists. This means that each time you call the function, you can specify any number of arguments above a certain number.

```
def function(first,second,*remaining):
    statement1
    statement2
    ...
```

When calling the above function, you must provide value for each of the first two arguments. However, since the third parameter is marked with an asterisk, any actual parameters after the first two will be packed into a tuple and bound to "remaining."

```
>>> def print_tail(first,*tail):
...     print tail
...
>>> print_tail(1, 5, 2, "omega")
(5, 2, 'omega')
```

If we declare a formal parameter prefixed with *two* asterisks, then it will be bound to a dictionary containing any keyword arguments in the actual parameters which do not correspond to any formal parameters. For example, consider the function:

```
def make_dictionary(max_length=10, **entries):
    return dict([(key, entries[key]) for i, key in enumerate(entries.keys()) if i < max_length])
```

If we call this function with any keyword arguments other than max_length, they will be placed in the dictionary "entries." If we include the keyword argument of max_length, it will be bound to the formal parameter max_length, as usual.

```
>>> make_dictionary(max_length=2, key1=5, key2=7, key3=9)
{'key3': 9, 'key2': 7}
```

Links:

- [4.7.3. Arbitrary Argument Lists](https://docs.python.org/2/tutorial/controlflow.html#arbitrary-argument-lists), The Python Tutorial, docs.python.org

#### By Value and by Reference[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Functions&action=edit&section=6)]

Objects passed as arguments to functions are passed *by reference*; they are not being copied around. Thus, passing a large list as an argument does not involve copying all its members to a new location in memory. Note that even integers are objects. However, the distinction of *by value* and *by reference* present in some other programming languages often serves to distinguish whether the passed arguments can be *actually changed* by the called function and whether the *calling function can see the changes*.

Passed objects of *mutable* types such as lists and dictionaries can be changed by the called function and the changes are visible to the calling function. Passed objects of *immutable* types such as integers and strings cannot be changed by the called function; the calling function can be certain that the called function will not change them. For mutability, see also [Data Types](https://en.wikibooks.org/wiki/Python_Programming/Data_Types#Mutable_vs_Immutable_Objects) chapter.

An example:

```
def appendItem(ilist, item):
  ilist.append(item) # Modifies ilist in a way visible to the caller

def replaceItems(ilist, newcontentlist):
  del ilist[:]                 # Modification visible to the caller
  ilist.extend(newcontentlist) # Modification visible to the caller
  ilist = [5, 6] # No outside effect; lets the local ilist point to a new list object,
                 # losing the reference to the list object passed as an argument
def clearSet(iset):
  iset.clear()

def tryToTouchAnInteger(iint):
  iint += 1 # No outside effect; lets the local iint to point to a new int object,
            # losing the reference to the int object passed as an argument
  print "iint inside:",iint # 4 if iint was 3 on function entry 

list1 = [1, 2]
appendItem(list1, 3)
print list1 # [1, 2, 3]
replaceItems(list1, [3, 4])
print list1 # [3, 4]
set1 = set([1, 2])
clearSet(set1 )
print set1 # set([])
int1 = 3
tryToTouchAnInteger(int1)
print int1 # 3
```

### Preventing Argument Change[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Functions&action=edit&section=7)]

An argument cannot be declared to be constant, not to be changed by the called function. If an argument is of an immutable type, it cannot be changed anyway, but if it is of a mutable type such as list, the calling function is at the mercy of the called function. Thus, if the calling function wants to make sure a passed list does not get changed, it has to pass a copy of the list.

An example:

```
def evil_get_length(ilist):
  length = len(ilist)
  del ilist[:] # Muhaha: clear the list
  return length

list1 = [1, 2]
print evil_get_length(list1[:]) # Pass a copy of list1
print list1 # list1 = [1, 2]
print evil_get_length(list1) # list1 gets cleared
print list1 # list1 = []
```

### Calling Functions[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Functions&action=edit&section=8)]

A function can be called by appending the arguments in parentheses to the function name, or an empty matched set of parentheses if the function takes no arguments.

```
foo()
square(3)
bar(5, x)
```

A function's return value can be used by assigning it to a variable, like so:

```
x = foo()
y = bar(5,x)
```

As shown above, when calling a function you can specify the parameters by name and you can do so in any order

```
def display_message(message, start=0, end=4):
   print message[start:end]

display_message("message", end=3)
```

This above is valid and start will have the default value of 0. A restriction placed on this is after the first named argument then all arguments after it must also be named. The following is not valid

```
display_message(end=5, start=1, "my message")
```

because the third argument ("my message") is an unnamed argument.

## Nested functions[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Functions&action=edit&section=9)]

Nested functions are functions defined within other functions. Arbitrary level of nesting is possible.

Nested functions can read variables declared in the immediately outside function. For such variables that are mutable, nested functions can even modify them. For such variables that are immutable such as integers, attempt at modification in the nested function throws UnboundLocalError. In Python 3, an immutable immediately outside variable can be declared in the nested function to be *nonlocal*, in an analogy to *global*. Once this is done, the nested function can assign a new value to that variable and that modification is going to be seen outside of the nested function.

Nested functions can be used in [#Closures](https://en.wikibooks.org/wiki/Python_Programming/Functions#Closures), on which see below. Furthermore, they can be used to reduce repetion of code that pertains only to a single function, often with reduced argument list owing to seeing the immediately outside variables.

An example of a nested function that modifies an immediately outside variable that is a list and therefore mutable:

```
def outside():
  outsideList = [1, 2]
  def nested():
    outsideList.append(3)
  nested()
  print outsideList
```

An example in which the outside variable is first accessed *below* the nested function definition and it still works:

```
def outside():
  def nested():
    outsideList.append(3)
  outsideList = [1, 2]
  nested()
  print outsideList
```

Keywords: inner functions, internal functions, local functions.

Links:

- [Nested Function in Python](http://stackoverflow.com/questions/1589058/nested-function-in-python) stackoverflow.com
- [7. Simple statements # 7.13. The nonlocal statement](https://docs.python.org/3/reference/simple_stmts.html#nonlocal), docs.python.org/3
- [Python nonlocal statement](http://stackoverflow.com/questions/1261875/python-nonlocal-statement), stackoverflow.com

## Closures[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Functions&action=edit&section=10)]

A *closure* is a nested function with an after-return access to the data of the outer function, where the nested function is returned by the outer function as a function object. Thus, even when the outer function has finished its execution after being called, the closure function returned by it can refer to the values of the variables that the outer function had when it defined the closure function.

An example:

```
def adder(outer_argument): # outer function
  def adder_inner(inner_argument): # inner function, nested function
    return outer_argument + inner_argument # Notice outer_argument
  return adder_inner
add5 = adder(5) # a function that adds 5 to its argument
add7 = adder(7) # a function that adds 7 to its argument
print add5(3) # prints 8
print add7(3) # prints 10
```

Closures are possible in Python because functions are *first-class objects*. A function is merely an object of type function. Being an object means it is possible to pass a function object (an uncalled function) around as argument or as return value or to assign another name to the function object. A unique feature that makes closure useful is that the enclosed function may use the names defined in the parent function's scope.

## Lambda Expressions[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Functions&action=edit&section=11)]

A lambda is an anonymous (unnamed) function. It is used primarily to write very short functions that are a hassle to define in the normal way. A function like this:

```
>>> def add(a, b):
...    return a + b
...
>>> add(4, 3)
7
```

may also be defined using lambda

```
>>> print ((lambda a, b: a + b)(4, 3))
7
```

Lambda is often used as an argument to other functions that expects a function object, such as sorted()'s 'key' argument.

```
>>> sorted([[3, 4], [3, 5], [1, 2], [7, 3]], key=lambda x: x[1])
[[1, 2], [7, 3], [3, 4], [3, 5]]
```

The lambda form is often useful as a closure, such as illustrated in the following example:

```
>>> def attribution(name):
...    return lambda x: x + ' -- ' + name
...
>>> pp = attribution('John')
>>> pp('Dinner is in the fridge')
'Dinner is in the fridge -- John'
```

Note that the lambda function can use the values of variables from the [scope](https://en.wikibooks.org/wiki/Python_Programming/Scoping) in which it was created (like pre and post). This is the essence of closure.

Links:

- [4.7.5. Lambda Expressions](https://docs.python.org/2/tutorial/controlflow.html#lambda-expressions), The Python Tutorial, docs.python.org

## Generator Functions[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Functions&action=edit&section=12)]

When discussing loops, you came across the concept of an *iterator*. This yields in turn each element of some sequence, rather than the entire sequence at once, allowing you to deal with sequences much larger than might be able to fit in memory at once.

You can create your own iterators, by defining what is known as a *generator function*. To illustrate the usefulness of this, let us start by considering a simple function to return the *concatenation* of two lists:

```
def concat(a, b):
    return a + b

print concat([5, 4, 3], ["a", "b", "c"])
# prints [5, 4, 3, 'a', 'b', 'c']
```

Imagine wanting to do something like `concat(range(0, 1000000), range(1000000, 2000000))`

That would work, but it would consume a lot of memory.

Consider an alternative definition, which takes two iterators as arguments:

```
def concat(a, b):
    for i in a:
        yield i
    for i in b:
        yield i
```

Notice the use of the **yield** statement, instead of **return**. We can now use this something like

```
for i in concat(xrange(0, 1000000), xrange(1000000, 2000000))
    print i
```

and print out an awful lot of numbers, without using a lot of memory at all.

Note: You can still pass a list or other sequence type wherever Python expects an iterator (like to an argument of your `concat` function); this will still work, and makes it easy not to have to worry about the difference where you don’t need to.

## Generators

Generators are a special class of functions that simplify the task of writing iterators. Regular functions compute a value and return it, but generators return an iterator that returns a stream of values.

You’re doubtless familiar with how regular function calls work in Python or C. When you call a function, it gets a private namespace where its local variables are created. When the function reaches a `return` statement, the local variables are destroyed and the value is returned to the caller. A later call to the same function creates a new private namespace and a fresh set of local variables. But, what if the local variables weren’t thrown away on exiting a function? What if you could later resume the function where it left off? This is what generators provide; they can be thought of as resumable functions.

Here’s the simplest example of a generator function:

```
def generate_ints(N):
    for i in range(N):
        yield i
```

Any function containing a `yield` keyword is a generator function; this is detected by Python’s [bytecode](https://docs.python.org/2/glossary.html#term-bytecode) compiler which compiles the function specially as a result.

When you call a generator function, it doesn’t return a single value; instead it returns a generator object that supports the iterator protocol. On executing the `yield` expression, the generator outputs the value of `i`, similar to a `return` statement. The big difference between `yield` and a `return` statement is that on reaching a `yield` the generator’s state of execution is suspended and local variables are preserved. On the next call to the generator’s `.next()` method, the function will resume executing.

Here’s a sample usage of the `generate_ints()` generator:

```
>>> gen = generate_ints(3)
>>> gen
<generator object generate_ints at ...>
>>> gen.next()
0
>>> gen.next()
1
>>> gen.next()
2
>>> gen.next()
Traceback (most recent call last):
  File "stdin", line 1, in <module>
  File "stdin", line 2, in generate_ints
StopIteration
```

You could equally write `for i in generate_ints(5)`, or `a,b,c = generate_ints(3)`.

Inside a generator function, the `return` statement can only be used without a value, and signals the end of the procession of values; after executing a `return` the generator cannot return any further values. `return` with a value, such as `return 5`, is a syntax error inside a generator function. The end of the generator’s results can also be indicated by raising `StopIteration` manually, or by just letting the flow of execution fall off the bottom of the function.

You could achieve the effect of generators manually by writing your own class and storing all the local variables of the generator as instance variables. For example, returning a list of integers could be done by setting `self.count` to 0, and having the `next()` method increment `self.count` and return it. However, for a moderately complicated generator, writing a corresponding class can be much messier.

The test suite included with Python’s library, `test_generators.py`, contains a number of more interesting examples. Here’s one generator that implements an in-order traversal of a tree using generators recursively.

```
# A recursive generator that generates Tree leaves in in-order.
def inorder(t):
    if t:
        for x in inorder(t.left):
            yield x

        yield t.label

        for x in inorder(t.right):
            yield x
```

Two other examples in `test_generators.py` produce solutions for the N-Queens problem (placing N queens on an NxN chess board so that no queen threatens another) and the Knight’s Tour (finding a route that takes a knight to every square of an NxN chessboard without visiting any square twice).

### Passing values into a generator

In Python 2.4 and earlier, generators only produced output. Once a generator’s code was invoked to create an iterator, there was no way to pass any new information into the function when its execution is resumed. You could hack together this ability by making the generator look at a global variable or by passing in some mutable object that callers then modify, but these approaches are messy.

In Python 2.5 there’s a simple way to pass values into a generator. [`yield`](https://docs.python.org/2/reference/simple_stmts.html#yield) became an expression, returning a value that can be assigned to a variable or otherwise operated on:

```
val = (yield i)
```

I recommend that you **always** put parentheses around a `yield` expression when you’re doing something with the returned value, as in the above example. The parentheses aren’t always necessary, but it’s easier to always add them instead of having to remember when they’re needed.

(PEP 342 explains the exact rules, which are that a `yield`-expression must always be parenthesized except when it occurs at the top-level expression on the right-hand side of an assignment. This means you can write `val = yield i` but have to use parentheses when there’s an operation, as in `val = (yield i) + 12`.)

Values are sent into a generator by calling its `send(value)` method. This method resumes the generator’s code and the `yield` expression returns the specified value. If the regular `next()` method is called, the `yield` returns `None`.

Here’s a simple counter that increments by 1 and allows changing the value of the internal counter.

```
def counter (maximum):
    i = 0
    while i < maximum:
        val = (yield i)
        # If value provided, change counter
        if val is not None:
            i = val
        else:
            i += 1
```

And here’s an example of changing the counter:

```
>>> it = counter(10)
>>> print it.next()
0
>>> print it.next()
1
>>> print it.send(8)
8
>>> print it.next()
9
>>> print it.next()
Traceback (most recent call last):
  File "t.py", line 15, in <module>
    print it.next()
StopIteration
```

Because `yield` will often be returning `None`, you should always check for this case. Don’t just use its value in expressions unless you’re sure that the `send()` method will be the only method used to resume your generator function.

In addition to `send()`, there are two other new methods on generators:

- `throw(type, value=None, traceback=None)` is used to raise an exception inside the generator; the exception is raised by the `yield` expression where the generator’s execution is paused.

- `close()` raises a [`GeneratorExit`](https://docs.python.org/2/library/exceptions.html#exceptions.GeneratorExit) exception inside the generator to terminate the iteration. On receiving this exception, the generator’s code must either raise [`GeneratorExit`](https://docs.python.org/2/library/exceptions.html#exceptions.GeneratorExit) or [`StopIteration`](https://docs.python.org/2/library/exceptions.html#exceptions.StopIteration); catching the exception and doing anything else is illegal and will trigger a [`RuntimeError`](https://docs.python.org/2/library/exceptions.html#exceptions.RuntimeError). `close()` will also be called by Python’s garbage collector when the generator is garbage-collected.

  If you need to run cleanup code when a [`GeneratorExit`](https://docs.python.org/2/library/exceptions.html#exceptions.GeneratorExit) occurs, I suggest using a `try: ... finally:` suite instead of catching [`GeneratorExit`](https://docs.python.org/2/library/exceptions.html#exceptions.GeneratorExit).

The cumulative effect of these changes is to turn generators from one-way producers of information into both producers and consumers.

Generators also become **coroutines**, a more generalized form of subroutines. Subroutines are entered at one point and exited at another point (the top of the function, and a `return` statement), but coroutines can be entered, exited, and resumed at many different points (the `yield` statements).

## Built-in functions

Let’s look in more detail at built-in functions often used with iterators.

Two of Python’s built-in functions, [`map()`](https://docs.python.org/2/library/functions.html#map) and [`filter()`](https://docs.python.org/2/library/functions.html#filter), are somewhat obsolete; they duplicate the features of list comprehensions but return actual lists instead of iterators.

`map(f, iterA, iterB, ...)` returns a list containing `f(iterA[0], iterB[0]), f(iterA[1], iterB[1]), f(iterA[2], iterB[2]), ...`.

```
>>> def upper(s):
...     return s.upper()
>>> map(upper, ['sentence', 'fragment'])
['SENTENCE', 'FRAGMENT']
>>> [upper(s) for s in ['sentence', 'fragment']]
['SENTENCE', 'FRAGMENT']
```

As shown above, you can achieve the same effect with a list comprehension. The [`itertools.imap()`](https://docs.python.org/2/library/itertools.html#itertools.imap) function does the same thing but can handle infinite iterators; it’ll be discussed later, in the section on the [`itertools`](https://docs.python.org/2/library/itertools.html#module-itertools) module.

`filter(predicate, iter)` returns a list that contains all the sequence elements that meet a certain condition, and is similarly duplicated by list comprehensions. A **predicate** is a function that returns the truth value of some condition; for use with [`filter()`](https://docs.python.org/2/library/functions.html#filter), the predicate must take a single value.

```
>>> def is_even(x):
...     return (x % 2) == 0
>>> filter(is_even, range(10))
[0, 2, 4, 6, 8]
```

This can also be written as a list comprehension:

```
>>> [x for x in range(10) if is_even(x)]
[0, 2, 4, 6, 8]
```

[`filter()`](https://docs.python.org/2/library/functions.html#filter) also has a counterpart in the [`itertools`](https://docs.python.org/2/library/itertools.html#module-itertools) module, [`itertools.ifilter()`](https://docs.python.org/2/library/itertools.html#itertools.ifilter), that returns an iterator and can therefore handle infinite sequences just as [`itertools.imap()`](https://docs.python.org/2/library/itertools.html#itertools.imap) can.

`reduce(func, iter, [initial_value])` doesn’t have a counterpart in the [`itertools`](https://docs.python.org/2/library/itertools.html#module-itertools) module because it cumulatively performs an operation on all the iterable’s elements and therefore can’t be applied to infinite iterables. `func` must be a function that takes two elements and returns a single value. [`reduce()`](https://docs.python.org/2/library/functions.html#reduce) takes the first two elements A and B returned by the iterator and calculates `func(A, B)`. It then requests the third element, C, calculates `func(func(A, B), C)`, combines this result with the fourth element returned, and continues until the iterable is exhausted. If the iterable returns no values at all, a [`TypeError`](https://docs.python.org/2/library/exceptions.html#exceptions.TypeError) exception is raised. If the initial value is supplied, it’s used as a starting point and `func(initial_value, A)` is the first calculation.

```
>>> import operator
>>> reduce(operator.concat, ['A', 'BB', 'C'])
'ABBC'
>>> reduce(operator.concat, [])
Traceback (most recent call last):
  ...
TypeError: reduce() of empty sequence with no initial value
>>> reduce(operator.mul, [1,2,3], 1)
6
>>> reduce(operator.mul, [], 1)
1
```

If you use [`operator.add()`](https://docs.python.org/2/library/operator.html#operator.add) with [`reduce()`](https://docs.python.org/2/library/functions.html#reduce), you’ll add up all the elements of the iterable. This case is so common that there’s a special built-in called [`sum()`](https://docs.python.org/2/library/functions.html#sum) to compute it:

```
>>> reduce(operator.add, [1,2,3,4], 0)
10
>>> sum([1,2,3,4])
10
>>> sum([])
0
```

For many uses of [`reduce()`](https://docs.python.org/2/library/functions.html#reduce), though, it can be clearer to just write the obvious [`for`](https://docs.python.org/2/reference/compound_stmts.html#for) loop:

```
# Instead of:
product = reduce(operator.mul, [1,2,3], 1)

# You can write:
product = 1
for i in [1,2,3]:
    product *= i
```

`enumerate(iter)` counts off the elements in the iterable, returning 2-tuples containing the count and each element.

```
>>> for item in enumerate(['subject', 'verb', 'object']):
...     print item
(0, 'subject')
(1, 'verb')
(2, 'object')
```

[`enumerate()`](https://docs.python.org/2/library/functions.html#enumerate) is often used when looping through a list and recording the indexes at which certain conditions are met:

```
f = open('data.txt', 'r')
for i, line in enumerate(f):
    if line.strip() == '':
        print 'Blank line at line #%i' % i
```

`sorted(iterable, [cmp=None], [key=None], [reverse=False])` collects all the elements of the iterable into a list, sorts the list, and returns the sorted result. The `cmp`, `key`, and `reverse` arguments are passed through to the constructed list’s `.sort()` method.

```
>>> import random
>>> # Generate 8 random numbers between [0, 10000)
>>> rand_list = random.sample(range(10000), 8)
>>> rand_list
[769, 7953, 9828, 6431, 8442, 9878, 6213, 2207]
>>> sorted(rand_list)
[769, 2207, 6213, 6431, 7953, 8442, 9828, 9878]
>>> sorted(rand_list, reverse=True)
[9878, 9828, 8442, 7953, 6431, 6213, 2207, 769]
```

(For a more detailed discussion of sorting, see the Sorting mini-HOWTO in the Python wiki at https://wiki.python.org/moin/HowTo/Sorting.)

The `any(iter)` and `all(iter)` built-ins look at the truth values of an iterable’s contents. [`any()`](https://docs.python.org/2/library/functions.html#any) returns `True` if any element in the iterable is a true value, and [`all()`](https://docs.python.org/2/library/functions.html#all) returns `True` if all of the elements are true values:

```
>>> any([0,1,0])
True
>>> any([0,0,0])
False
>>> any([1,1,1])
True
>>> all([0,1,0])
False
>>> all([0,0,0])
False
>>> all([1,1,1])
True
```

## Small functions and the lambda expression

When writing functional-style programs, you’ll often need little functions that act as predicates or that combine elements in some way.

If there’s a Python built-in or a module function that’s suitable, you don’t need to define a new function at all:

```
stripped_lines = [line.strip() for line in lines]
existing_files = filter(os.path.exists, file_list)
```

If the function you need doesn’t exist, you need to write it. One way to write small functions is to use the `lambda` statement. `lambda` takes a number of parameters and an expression combining these parameters, and creates a small function that returns the value of the expression:

```
lowercase = lambda x: x.lower()

print_assign = lambda name, value: name + '=' + str(value)

adder = lambda x, y: x+y
```

An alternative is to just use the `def` statement and define a function in the usual way:

```
def lowercase(x):
    return x.lower()

def print_assign(name, value):
    return name + '=' + str(value)

def adder(x,y):
    return x + y
```

Which alternative is preferable? That’s a style question; my usual course is to avoid using `lambda`.

One reason for my preference is that `lambda` is quite limited in the functions it can define. The result has to be computable as a single expression, which means you can’t have multiway `if... elif... else` comparisons or `try... except` statements. If you try to do too much in a `lambda` statement, you’ll end up with an overly complicated expression that’s hard to read. Quick, what’s the following code doing?

```
total = reduce(lambda a, b: (0, a[1] + b[1]), items)[1]
```

You can figure it out, but it takes time to disentangle the expression to figure out what’s going on. Using a short nested `def` statements makes things a little bit better:

```
def combine (a, b):
    return 0, a[1] + b[1]

total = reduce(combine, items)[1]
```

But it would be best of all if I had simply used a `for` loop:

```
total = 0
for a, b in items:
    total += b
```

Or the [`sum()`](https://docs.python.org/2/library/functions.html#sum) built-in and a generator expression:

```
total = sum(b for a,b in items)
```

Many uses of [`reduce()`](https://docs.python.org/2/library/functions.html#reduce) are clearer when written as `for` loops.

Fredrik Lundh once suggested the following set of rules for refactoring uses of `lambda`:

1. Write a lambda function.
2. Write a comment explaining what the heck that lambda does.
3. Study the comment for a while, and think of a name that captures the essence of the comment.
4. Convert the lambda to a def statement, using that name.
5. Remove the comment.

I really like these rules, but you’re free to disagree about whether this lambda-free style is better.

## The itertools module

The [`itertools`](https://docs.python.org/2/library/itertools.html#module-itertools) module contains a number of commonly-used iterators as well as functions for combining several iterators. This section will introduce the module’s contents by showing small examples.

The module’s functions fall into a few broad classes:

- Functions that create a new iterator based on an existing iterator.
- Functions for treating an iterator’s elements as function arguments.
- Functions for selecting portions of an iterator’s output.
- A function for grouping an iterator’s output.

### Creating new iterators

`itertools.count(n)` returns an infinite stream of integers, increasing by 1 each time. You can optionally supply the starting number, which defaults to 0:

```
itertools.count() =>
  0, 1, 2, 3, 4, 5, 6, 7, 8, 9, ...
itertools.count(10) =>
  10, 11, 12, 13, 14, 15, 16, 17, 18, 19, ...
```

`itertools.cycle(iter)` saves a copy of the contents of a provided iterable and returns a new iterator that returns its elements from first to last. The new iterator will repeat these elements infinitely.

```
itertools.cycle([1,2,3,4,5]) =>
  1, 2, 3, 4, 5, 1, 2, 3, 4, 5, ...
```

`itertools.repeat(elem, [n])` returns the provided element `n` times, or returns the element endlessly if `n` is not provided.

```
itertools.repeat('abc') =>
  abc, abc, abc, abc, abc, abc, abc, abc, abc, abc, ...
itertools.repeat('abc', 5) =>
  abc, abc, abc, abc, abc
```

`itertools.chain(iterA, iterB, ...)` takes an arbitrary number of iterables as input, and returns all the elements of the first iterator, then all the elements of the second, and so on, until all of the iterables have been exhausted.

```
itertools.chain(['a', 'b', 'c'], (1, 2, 3)) =>
  a, b, c, 1, 2, 3
```

`itertools.izip(iterA, iterB, ...)` takes one element from each iterable and returns them in a tuple:

```
itertools.izip(['a', 'b', 'c'], (1, 2, 3)) =>
  ('a', 1), ('b', 2), ('c', 3)
```

It’s similar to the built-in [`zip()`](https://docs.python.org/2/library/functions.html#zip) function, but doesn’t construct an in-memory list and exhaust all the input iterators before returning; instead tuples are constructed and returned only if they’re requested. (The technical term for this behaviour is [lazy evaluation](http://en.wikipedia.org/wiki/Lazy_evaluation).)

This iterator is intended to be used with iterables that are all of the same length. If the iterables are of different lengths, the resulting stream will be the same length as the shortest iterable.

```
itertools.izip(['a', 'b'], (1, 2, 3)) =>
  ('a', 1), ('b', 2)
```

You should avoid doing this, though, because an element may be taken from the longer iterators and discarded. This means you can’t go on to use the iterators further because you risk skipping a discarded element.

`itertools.islice(iter, [start], stop, [step])` returns a stream that’s a slice of the iterator. With a single `stop` argument, it will return the first `stop` elements. If you supply a starting index, you’ll get `stop-start` elements, and if you supply a value for `step`, elements will be skipped accordingly. Unlike Python’s string and list slicing, you can’t use negative values for `start`, `stop`, or `step`.

```
itertools.islice(range(10), 8) =>
  0, 1, 2, 3, 4, 5, 6, 7
itertools.islice(range(10), 2, 8) =>
  2, 3, 4, 5, 6, 7
itertools.islice(range(10), 2, 8, 2) =>
  2, 4, 6
```

`itertools.tee(iter, [n])` replicates an iterator; it returns `n` independent iterators that will all return the contents of the source iterator. If you don’t supply a value for `n`, the default is 2. Replicating iterators requires saving some of the contents of the source iterator, so this can consume significant memory if the iterator is large and one of the new iterators is consumed more than the others.

```
itertools.tee( itertools.count() ) =>
   iterA, iterB

where iterA ->
   0, 1, 2, 3, 4, 5, 6, 7, 8, 9, ...

and   iterB ->
   0, 1, 2, 3, 4, 5, 6, 7, 8, 9, ...
```

### Calling functions on elements

Two functions are used for calling other functions on the contents of an iterable.

`itertools.imap(f, iterA, iterB, ...)` returns a stream containing `f(iterA[0], iterB[0]), f(iterA[1], iterB[1]), f(iterA[2], iterB[2]), ...`:

```
itertools.imap(operator.add, [5, 6, 5], [1, 2, 3]) =>
  6, 8, 8
```

The `operator` module contains a set of functions corresponding to Python’s operators. Some examples are `operator.add(a, b)` (adds two values), `operator.ne(a, b)` (same as `a!=b`), and `operator.attrgetter('id')` (returns a callable that fetches the `"id"` attribute).

`itertools.starmap(func, iter)` assumes that the iterable will return a stream of tuples, and calls `f()` using these tuples as the arguments:

```
itertools.starmap(os.path.join,
                  [('/usr', 'bin', 'java'), ('/bin', 'python'),
                   ('/usr', 'bin', 'perl'),('/usr', 'bin', 'ruby')])
=>
  /usr/bin/java, /bin/python, /usr/bin/perl, /usr/bin/ruby
```

### Selecting elements

Another group of functions chooses a subset of an iterator’s elements based on a predicate.

`itertools.ifilter(predicate, iter)` returns all the elements for which the predicate returns true:

```
def is_even(x):
    return (x % 2) == 0

itertools.ifilter(is_even, itertools.count()) =>
  0, 2, 4, 6, 8, 10, 12, 14, ...
```

`itertools.ifilterfalse(predicate, iter)` is the opposite, returning all elements for which the predicate returns false:

```
itertools.ifilterfalse(is_even, itertools.count()) =>
  1, 3, 5, 7, 9, 11, 13, 15, ...
```

`itertools.takewhile(predicate, iter)` returns elements for as long as the predicate returns true. Once the predicate returns false, the iterator will signal the end of its results.

```
def less_than_10(x):
    return (x < 10)

itertools.takewhile(less_than_10, itertools.count()) =>
  0, 1, 2, 3, 4, 5, 6, 7, 8, 9

itertools.takewhile(is_even, itertools.count()) =>
  0
```

`itertools.dropwhile(predicate, iter)` discards elements while the predicate returns true, and then returns the rest of the iterable’s results.

```
itertools.dropwhile(less_than_10, itertools.count()) =>
  10, 11, 12, 13, 14, 15, 16, 17, 18, 19, ...

itertools.dropwhile(is_even, itertools.count()) =>
  1, 2, 3, 4, 5, 6, 7, 8, 9, 10, ...
```

### Grouping elements

The last function I’ll discuss, `itertools.groupby(iter, key_func=None)`, is the most complicated. `key_func(elem)` is a function that can compute a key value for each element returned by the iterable. If you don’t supply a key function, the key is simply each element itself.

`groupby()` collects all the consecutive elements from the underlying iterable that have the same key value, and returns a stream of 2-tuples containing a key value and an iterator for the elements with that key.

```
city_list = [('Decatur', 'AL'), ('Huntsville', 'AL'), ('Selma', 'AL'),
             ('Anchorage', 'AK'), ('Nome', 'AK'),
             ('Flagstaff', 'AZ'), ('Phoenix', 'AZ'), ('Tucson', 'AZ'),
             ...
            ]

def get_state ((city, state)):
    return state

itertools.groupby(city_list, get_state) =>
  ('AL', iterator-1),
  ('AK', iterator-2),
  ('AZ', iterator-3), ...

where
iterator-1 =>
  ('Decatur', 'AL'), ('Huntsville', 'AL'), ('Selma', 'AL')
iterator-2 =>
  ('Anchorage', 'AK'), ('Nome', 'AK')
iterator-3 =>
  ('Flagstaff', 'AZ'), ('Phoenix', 'AZ'), ('Tucson', 'AZ')
```

`groupby()` assumes that the underlying iterable’s contents will already be sorted based on the key. Note that the returned iterators also use the underlying iterable, so you have to consume the results of iterator-1 before requesting iterator-2 and its corresponding key.

## The functools module

The [`functools`](https://docs.python.org/2/library/functools.html#module-functools) module in Python 2.5 contains some higher-order functions. A **higher-order function** takes one or more functions as input and returns a new function. The most useful tool in this module is the [`functools.partial()`](https://docs.python.org/2/library/functools.html#functools.partial) function.

For programs written in a functional style, you’ll sometimes want to construct variants of existing functions that have some of the parameters filled in. Consider a Python function `f(a, b, c)`; you may wish to create a new function `g(b, c)` that’s equivalent to `f(1, b, c)`; you’re filling in a value for one of `f()`’s parameters. This is called “partial function application”.

The constructor for `partial` takes the arguments `(function, arg1, arg2, ... kwarg1=value1, kwarg2=value2)`. The resulting object is callable, so you can just call it to invoke `function` with the filled-in arguments.

Here’s a small but realistic example:

```
import functools

def log (message, subsystem):
    "Write the contents of 'message' to the specified subsystem."
    print '%s: %s' % (subsystem, message)
    ...

server_log = functools.partial(log, subsystem='server')
server_log('Unable to open socket')
```

### The operator module

The [`operator`](https://docs.python.org/2/library/operator.html#module-operator) module was mentioned earlier. It contains a set of functions corresponding to Python’s operators. These functions are often useful in functional-style code because they save you from writing trivial functions that perform a single operation.

Some of the functions in this module are:

- Math operations: `add()`, `sub()`, `mul()`, `div()`, `floordiv()`, `abs()`, …
- Logical operations: `not_()`, `truth()`.
- Bitwise operations: `and_()`, `or_()`, `invert()`.
- Comparisons: `eq()`, `ne()`, `lt()`, `le()`, `gt()`, and `ge()`.
- Object identity: `is_()`, `is_not()`.

Consult the operator module’s documentation for a complete list.
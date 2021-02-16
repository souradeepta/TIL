[TOC]



# J. Classes and Objects I

## Python Classes and Methods

Python is an “object-oriented programming language.” This means that almost all the code is implemented using a special construct called classes. Programmers use classes to keep related things together. This is done using the keyword “class,” which is a grouping of object-oriented constructs.

By the end of this tutorial you will be able to:

1. Define what is a class
2. Describe how to create a class
3. Define what is a method
4. Describe how to do object instantiation
5. Describe how to create instance attributes in Python

## What is a class?

A class is a code template for creating objects. Objects have member variables and have behaviour associated with them. In python a class is created by the keyword `class`.

An object is created using the constructor of the class. This object will then be called the `instance` of the class. In Python we create instances in the following manner

```python
Instance = class(arguments)
```

## How to create a class

The simplest class can be created using the class keyword. For example, let's create a simple, empty class with no functionalities.

```python
>>> class Snake:
...     pass
... 
>>> snake = Snake()
>>> print(snake)
<__main__.Snake object at 0x7f315c573550>
```

## Attributes and Methods in class:

A class by itself is of no use unless there is some functionality associated with it. Functionalities are defined by setting attributes, which act as containers for data and functions related to those attributes. Those functions are called methods.

### Attributes:

You can define the following class with the name Snake. This class will have an attribute `name`.

```python
>>> class Snake:
...     name = "python" # set an attribute `name` of the class
...
```

You can assign the class to a variable. This is called object instantiation. You will then be able to access the attributes that are present inside the class using the dot `.` operator. For example, in the Snake example, you can access the attribute `name` of the class `Snake`.

```python
>>> # instantiate the class Snake and assign it to variable snake
>>> snake = Snake()

>>> # access the class attribute name inside the class Snake.
>>> print(snake.name)
python
```

### Methods

Once there are attributes that “belong” to the class, you can define functions that will access the class attribute. These functions are called methods. When you define methods, you will need to always provide the first argument to the method with a self keyword.

For example, you can define a class `Snake`, which has one attribute `name` and one method `change_name`. The method change name will take in an argument `new_name` along with the keyword `self`.

```python
>>> class Snake:
...     name = "python"
...     
...     def change_name(self, new_name): # note that the first argument is self
...         self.name = new_name # access the class attribute with the self keyword
...
```

Now, you can instantiate this class `Snake` with a variable `snake` and then change the name with the method `change_name`.

```python
>>> # instantiate the class
>>> snake = Snake()

>>> # print the current object name 
>>> print(snake.name)
python

>>> # change the name using the change_name method
>>> snake.change_name("anaconda")
>>> print(snake.name)
anaconda
```

### Instance attributes in python and the init method

You can also provide the values for the attributes at runtime. This is done by defining the attributes inside the init method. The following example illustrates this.

```python
class Snake:

    def __init__(self, name):
        self.name = name

    def change_name(self, new_name):
        self.name = new_name
```

Now you can directly define separate attribute values for separate objects. For example,

```python
>>> # two variables are instantiated
>>> python = Snake("python")
>>> anaconda = Snake("anaconda")

>>> # print the names of the two variables
>>> print(python.name)
python
>>> print(anaconda.name)
anaconda
```

# K .Classes and Objects II (Inheritance and Composition)

## Python classes and object object-oriented programming II

Classes are written to organize and structure code into meaningful blocks, which can then be used to implement the business logic. These implementations are used in such a way that more complex parts are abstracted away to provide for simpler interfaces which can then be used to build even simpler blocks. While doing this we will find that there are lots of times when we will need to establish relationships between the classes that we build. These relationships can then be established using either inheritance or composition. At this point it is best you take a look at our [Python Classes tutorial][1] to get in-depth knowledge on how classes are written in Python. Also, in case you are already doing object oriented programming in some other language, you may want to check out our [notes on design patterns](https://www.hackerearth.com/practice/notes/design-patterns-1/).

In this tutorial you will get to know how to build relationships between classes using inheritance and composition and the syntax that is needed.

## Python inheritance

### What is Inheritance

In inheritance an object is based on another object. When inheritance is implemented, the methods and attributes that were defined in the base class will also be present in the inherited class. This is generally done to abstract away similar code in multiple classes. The abstracted code will reside in the base class and the previous classes will now inherit from the base class.

### How to achieve Inheritance in Python

Python allows the classes to inherit commonly used attributes and methods from other classes through inheritance. We can define a base class in the following manner:

```python
class DerivedClassName(BaseClassName):
    pass
```

Let's look at an example of inheritance. In the following example, Rocket is the base class and MarsRover is the inherited class.

```python
class Rocket:
    def __init__(self, name, distance):
        self.name = name
        self.distance = distance

    def launch(self):
        return "%s has reached %s" % (self.name, self.distance)


class MarsRover(Rocket): # inheriting from the base class
    def __init__(self, name, distance, maker):
        Rocket.__init__(self, name, distance)
        self.maker = maker

    def get_maker(self):
        return "%s Launched by %s" % (self.name, self.maker)


if __name__ == "__main__":
    x = Rocket("simple rocket", "till stratosphere")
    y = MarsRover("mars_rover", "till Mars", "ISRO")
    print(x.launch())
    print(y.launch())
    print(y.get_maker())
```

The output of the code above is shown below:

```python
➜ Documents python rockets.py
simple rocket has reached till stratosphere
mars_rover has reached till Mars
mars_rover Launched by ISRO
```

## Python Composition:

### What is composition

In composition, we do not inherit from the base class but establish relationships between classes through the use of instance variables that are references to other objects. Talking in terms of pseudocode you may say that

```python
class GenericClass:
    define some attributes and methods

class ASpecificClass:
    Instance_variable_of_generic_class = GenericClass

# use this instance somewhere in the class
    some_method(Instance_variable_of_generic_class)
```

So you will instantiate the base class and then use the instance variable for any business logic.

### How to achieve composition in Python

To achieve composition you can instantiate other objects in the class and then use those instances. For example in the below example we instantiate the Rocket class using `self.rocket` and then using self.rocket in the method `get_maker`.

```python
class MarsRoverComp():
    def __init__(self, name, distance, maker):
        self.rocket = Rocket(name, distance) # instantiating the base

        self.maker = maker

    def get_maker(self):
        return "%s Launched by %s" % (self.rocket.name, self.maker)


if __name__ == "__main__":
    z = MarsRover("mars_rover2", "till Mars", "ISRO")
    print(z.launch())
    print(z.get_maker())
```

The output of the total code which has both inheritance and composition is shown below:

```python
➜ Documents python rockets.py
simple rocket has reached till stratosphere
mars_rover has reached till Mars
mars_rover Launched by ISRO
mars_rover2 has reached till Mars
mars_rover2 Launched by ISRO
```

# K. Errors and Exceptions



## Handling Exceptions with Try/Except/Finally

Errors and Exceptions in Python are handled with the `Try: Except: Finally` construct. You put the unsafe code in the `try:` block. You put the fall-back code in the `Except:` block. The final code is kept in the `Finally:` block.

For example, look at the code below.

```python
>>> try:
...     print("in the try block")
...     print(1/0)
... except:
...     print("In the except block")
... finally:
...     print("In the finally block")
...
in the try block
In the except block
In the finally block
```

## Raising exceptions for a predefined condition

Exceptions can also be raised if you want the code to behave within specific parameters. For example, if you want to limit the user-input to only positive integers, raise an exception.

```python
# exc.py
while True:
    try:
        user = int(input())
        if user < 0:
        raise ValueError("please give positive number")
        else:
        print("user input: %s" % user)
except ValueError as e:
        print(e)
```

So the output of the above program is:

```python
➜  python exc.py
4
user input: 4
3
user input: 3
2
user input: 2
1
user input: 1
-1
please give positive number
5
user input: 5
2
user input: 2
-5
please give positive number
^C
Traceback (most recent call last):
File "exc.py", line 3, in <module>
  user = int(input())
KeyboardInterrupt
```

# L. Iterators and Generators

## Python Iterators, generators, and the for loop

Iterators are containers for objects so that you can loop over the objects. In other words, you can run the "for" loop over the object. There are many iterators in the Python standard library. For example, list is an iterator and you can run a for loop over a list.

```python
>>> for lib in popular_python_libs:
...     print(lib)
...
requests
scrapy
pillow
SQLAlchemy
NumPy
```

In this tutorial you will get to know:

1. How to create a custom iterator
2. How to create a generator
3. How to run for loops on iterators and generators

## Python Iterators and the Iterator protocol

To create a Python iterator object, you will need to implement two methods in your iterator class.

`__iter__`: This returns the iterator object itself and is used while using the "for" and "in" keywords.

`__next__`: This returns the next value. This would return the StopIteration error once all the objects have been looped through.

Let us create a cool emoticon generator and l iterators.

```python
# iterator_example.py
"""
This should give an iterator with a emoticon.
"""

import random

class CoolEmoticonGenerator(object):
    """docstring for CoolEmoticonGenerator."""

    strings = "!@#$^*_-=+?/,.:;~"
    grouped_strings = [("(", ")"), ("<", ">"), ("[", "]"), ("{", "}")]

    def create_emoticon(self, grp):
        """actual method that creates the emoticon"""
        face_strings_list = [random.choice(self.strings) for _ in range(3)]
        face_strings = "".join(face_strings_list)
        emoticon = (grp[0], face_strings, grp[1])
        emoticon = "".join(emoticon)
        return emoticon

    def __iter__(self):
        """returns the self object to be accessed by the for loop"""
        return self

    def __next__(self):
        """returns the next emoticon indefinitely"""
        grp = random.choice(self.grouped_strings)
        return self.create_emoticon(grp)
```

Now you can call the above class as an iterator. Which means you can run the next function on it.

```python
from iterator_example import CoolEmoticonGenerator
g = CoolEmoticonGenerator()
print([next(g) for _ in range(5)])
```

Running the program above gives us the following output. The exact output may be different from what you get but it will be similar.

```python
➜  python3.5 iterator_example.py
['<,~!>', '<;_~>', '<!;@>', '[~=#]', '{?^-}']
```

You can use the KeyboardInterrupt to stop the execution.

## Python Generators

Python generator gives us an easier way to create python iterators. This is done by defining a function but instead of the return statement returning from the function, use the "yield" keyword. For example, see how you can get a simple vowel generator below.

```python
>>> def vowels():
...     yield "a"
...     yield "e"
...     yield "i"
...     yield "o"
...     yield "u"
...
>>> for i in vowels():
...     print(i)
...
a
e
i
o
u
```

Now let's try and create the CoolEmoticonGenerator.

```python
def create_emoticon_generator():
    while True:
        strings = "!@#$^*_-=+?/,.:;~"
        grouped_strings = [("(", ")"), ("<", ">"), ("[", "]"), ("{", "}")]
        grp = random.choice(grouped_strings)
        face_strings_list = [random.choice(strings) for _ in range(3)]
        face_strings = "".join(face_strings_list)
        emoticon = (grp[0], face_strings, grp[1])
        emoticon = "".join(emoticon)
        yield emoticon
```

Now, if you run the generator using the runner below

```python
from iterator_example import CoolEmoticonGenerator
g = create_emoticon_generator()
print([next(g) for _ in range(5)])
```

You should get the following output

```python
➜  python3.5 iterator_example.py
['(+~?)', '<**_>', '($?/)', '[#=+]', '{*=.}']
```

# M. Functional Programming

## Python and functional programming:

Functional Programming is a coding style that focuses on defining what to do, instead of performing some action. Functional programming is derived from the mathematical style of thinking where you define the kind of inputs that go into a function and the kind of outputs that we can expect from the function. In functional code, the output of the function depends only on the arguments that are passed. Calling the function f for the same value of x should return the same result f(x) no matter how many times you pass it. Thus, it calls for a radically different style of thinking where you are rarely changing state. Instead of moving through steps, you think of data as undergoing transformations with the desired result as the end state.

Python has many constructs that enable a programmer to dabble in functional programming. If you want to know more on functional programming, take a look at [our notes on the topic](https://www.hackerearth.com/practice/notes/functional-programming-a-new-paradigm/).

In this tutorial you will look at:

1. What are the characteristics of functional programming
2. How to achieve those characteristics.
3. What is the meaning of using functions as first class objects.
4. What is functional purity.
5. How to refactor procedural code to functional code.

## Characteristics of functional programming

A functionally pure language should support the following constructs:

- Functions as first class objects, which means that you should be able to apply all the constructs of using data, to functions as well.
- Pure functions; there should not be any side-effects in them
- Ways and constructs to limit the use of for loops
- Good support for recursion

### Functions as first class objects in python:

Using functions as first class objects means to use them in the same manner that you use data. So, You can pass them as parameters like passing a function to another function as an argument. For example, in the following example you can pass the int function as a parameter to map function.

```python
>>> list(map(int, ["1", "2", "3"]))
[1, 2, 3]
```

You can assign them to variables and return them. For example, in the following code, you can assign the function `hello_world`, and then the variable will be executed as a function.

```python
>>> def hello_world(h):
...     def world(w):
...         print(h, w)
...     return world # returning functions
...
>>> h = hello_world # assigning
>>> x = h("hello") # assigning
>>> x
<function world at 0x7fec47afc668>
>>> x("world")
('hello', 'world')
```

You can store functions in various data structures. For example, in the following code you can store multiple functions in a list.

```python
>>> function_list = [h, x]
>>> function_list
[<function hello_world at 0x7fec47afc5f0>, <function world at 0x7fec47afc668>]
```

### Python functional purity:

There are various built-in functions in Python that can help to avoid procedural code in functions. So something like this

```python
def naive_sum(list):
    s = 0
    for l in list:
        s += l
    return s
```

can be replaced with the following construct:

```python
sum(list)
```

Similarly, built-in functions such as map, reduce, and the itertools module in Python can be utilized to avoid side-effects in your code.

### Reducing the usage of loops in Python:

Loops come into the picture when you want to loop over a collection of objects and apply some kind of logic or function.

```python
for x in l:
    func(x)
```

The above construct stems from the traditional thinking of visualizing the whole program as a series of steps where you define how things need to be done. Making this more functional will need a change in the thinking pattern. You can replace the above for loop in Python in the following manner.

```python
map(func, l)
```

This is read as “map the function to the list,” which conforms to our idea of defining the question “what.”

If you take this idea and apply it to the sequential execution of functions, you get the following construct.

```python
def func1():
    pass

def func2():
    pass

def func3():
    pass

executing = lambda f: f()
map(executing, [func1, func2, func3])
```

Please note that this does not actually run the functions but returns a lazy map object. You need to pass this object to a `list` or any other eager function to have the code executed.

## Python Recursion:

### What is Recursion

Recursion is a method of breaking a problem into subproblems which are essentially of the same type as the original problem. You solve the base problems and then combine the results. Usually this involves the function calling itself.

An example for recursion may be something like:

eat the dumplings: 1. check how many dumplings on the plate 2. if no dumplings left stop eating 3. else eat one dumpling 4. "eat the dumplings"

### How to implement recursion in your code

Python functions support recursion and hence you can utilize the dynamic programming constructs in the code to optimize them. Recursion basically needs to fulfill two conditions. There should be a condition where the recursion should end, and it should call itself for all other conditions. The end condition should be limiting; i.e, the functions should call smaller versions of themselves.

Example: The following code generates Fibonacci numbers through recursion.

```python
def fib(n):
    if n == 0: return 0
    elif n == 1: return 1
    else: return fib(n-1)+fib(n-2)
```

A small example showing how to convert procedural code into functional code:

Let us go through a small example where you try to refactor procedural code into functional. In the below example, you have a starting number which gets squared, the result is incremented by 1 and the result of that is raised to the power of 3, the code then takes the decrement. Note that the code works in steps.

```python
# procedural code
starting_number = 96

# get the square of the number
square = starting_number ** 2

# increment the number by 1
increment = square + 1

# cube of the number
cube = increment ** 3

# decrease the cube by 1
decrement = cube - 1

# get the final result
result = print(decrement) # output 783012621312
```

The same procedural code can be written functionally. Note that unlike the code above instead of giving explicit instructions on how to do it, you are giving instructions on what to do. The functions below operate at a higher plane of abstraction.

```python
# define a function `call` where you provide the function and the arguments
def call(x, f):
    return f(x)

# define a function that returns the square
square = lambda x : x*x

# define a function that returns the increment
increment = lambda x : x+1

# define a function that returns the cube
cube = lambda x : x*x*x

# define a function that returns the decrement
decrement = lambda x : x-1

# put all the functions in a list in the order that you want to execute them
funcs = [square, increment, cube, decrement]

# bring it all together. Below is the non functional part. 
# in functional programming you separate the functional and the non functional parts.
from functools import reduce # reduce is in the functools library
print(reduce(call, funcs, 96)) # output 783012621312
```

# N. Higher Order Functions and Decorators

## Python Higher-Order Functions and Decorators

In Python, functions are treated as first class objects, allowing you to perform the following operations on functions.

- A function can take one or more functions as arguments
- A function can be returned as a result of another function

In this tutorial, you will learn:

```
1. Handling functions as arguments
2. Returning functions as the return value from other functions
3. Using closures and decorators
```

## Functions as arguments

You can pass functions as one of the arguments to another function. This is shown in the following example.

```python
def summation(nums): # normal function
    return sum(nums)

def main(f, *args): # function as an argument
    result = f(*args)
    print(result)

if __name__ == "__main__":
    main(summation, [1,2,3]) # output 6
```

The main function took in the function `summation` as an argument. The `main` function is a normal function which executes the supplied function with the arguments. You can see that the output reflects that. This opens up possibilities where you can pass different functions to a function, and the passed function only will be considered.

## Having a function as a return value

```python
def add_tw0_nums(x, y): # normal function which returns data
    return x + y

def add_three_nums(x, y, z): # normal function which returns data
    return x + y + z

def get_appropriate_function(num_len): # function which returns functions depending on the logic
    if num_len == 3:
        return add_three_nums
    else:
        return add_tw0_nums


if __name__ == "__main__":
    args = [1, 2, 3]
    num_len = len(args)
    res_function = get_appropriate_function(num_len)
    print(res_function)       # <function add_three_nums at 0x7f8f34173668>
    print(res_function(*args)) # unpack the args, output 6

    args = [1, 2]
    num_len = len(args)
    res_function = get_appropriate_function(num_len)
    print(res_function)       # <function add_tw0_nums at 0x7f1630955e18>
    print(res_function(*args)) # unpack the args, output 3
```

Here, you can see that different functions were returned depending on the argument in the “get_appropriate_function.”

## Python closures

Let’s now look at how closures are created in Python. A closure is a way of keeping alive a variable even when the function has returned. So, in a closure, a function is defined along with the environment. In Python, this is done by nesting a function inside the encapsulating function and then returning the underlying function.

```python
def add_5():
    five = 5

    def add(arg): # nesting functions
        return arg + five
    return add

if __name__ == '__main__':
    closure1 = add_5()
    print(closure1(1)) # output 6
    print(closure1(2)) # output 7
```

## Python decorators

Now, using the these ways of writing, Python code can be used to create decorators. Python decorators are convenient ways to make changes to the functionality of code without making changes to the code. A decorator is written as a function closure and implemented by giving the “@” operator on top of the function. The skeleton of a Python decorator is shown below.

```python
def my_decorator(f):
    # write your code here or your wrapping function
    # return the wrapping function
    pass

@my_decorator
def my_code(args):
    # original functionality
    pass

my_code(args)
```

Using this skeleton as an example, giving the @ operator on top of the function is the same as writing my_decorator(my_code(args)).

You can look at the following example where you have a function that returns a dictionary.

```python
def my_code(args):
    return {lang: args}
```

Executing this code will give the normal result.

```python
>>> def my_code(args):
...     return {"lang": args}
...
>>> print(my_code("python"))
{'lang': 'python'}
>>>
```

Say, you want to build the additional functionality of checking if the return value is a dict. In this case, you can use a decorator.

```python
>>> def check(f):
...     def wrapper(*args, **kwargs):
...         res = f(*args, **kwargs)
...         if isinstance(res, dict):
...             print("checked that the return value is dict")
...             return res
...     return wrapper
...
>>>
>>>
>>> @check
... def my_code(args):
...     return {"lang": args}
...
>>> print(my_code("python"))
checked that the return value is dict
{'lang': 'python'}
```

Here, the function “check” is used as a decorator, and it enforces the additional functionality of checking if the return of the function ”my_code” is a decorator or not.
# Python Programming/Classes

### Overview[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=1)]

Classes in Python at a glance:

```python
import math
class MyComplex:
  """A complex number"""       # Class documentation
  classvar = 0.0               # A class attribute, not an instance one
  def phase(self):             # A method
    return math.atan2(self.imaginary, self.real)
  def __init__(self):          # A constructor
    """A constructor"""
    self.real = 0.0            # An instance attribute
    self.imaginary = 0.0
c1 = MyComplex()
c1.real = 3.14                 # No access protection
c1.imaginary = 2.71
phase = c1.phase()             # Method call
c1.undeclared = 9.99           # Add an instance attribute
del c1.undeclared              # Delete an instance attribute

print vars(c1)                 # Attributes as a dictionary
vars(c1)["undeclared2"] = 7.77 # Write access to an attribute
print c1.undeclared2           # 7.77, indeed

MyComplex.classvar = 1         # Class attribute access
print c1.classvar == 1         # True; class attribute access, not an instance one
print "classvar" in vars(c1)   # False
c1.classvar = -1               # An instance attribute overshadowing the class one
MyComplex.classvar = 2         # Class attribute access
print c1.classvar == -1        # True; instance attribute acccess
print "classvar" in vars(c1)   # True

class MyComplex2(MyComplex):   # Class derivation or inheritance
  def __init__(self, re = 0, im = 0):
    self.real = re             # A constructor with multiple arguments with defaults
    self.imaginary = im
  def phase(self):
    print "Derived phase"
    return MyComplex.phase(self) # Call to a base class; "super"
c3 = MyComplex2()
c4 = MyComplex2(1, 1)
c4.phase()                     # Call to the method in the derived class

class Record: pass             # Class as a record/struct with arbitrary attributes
record = Record()
record.name = "Joe"
record.surname = "Hoe"
```

### Defining a Class[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=2)]

To define a class, use the following format:

```python
class ClassName:
    "Here is an explanation about your class"
    pass
```

The capitalization in this class definition is the convention, but is not required by the language. It's usually good to add at least a short explanation of what your class is supposed to do. The pass statement in the code above is just to say to the python interpreter just go on and do nothing. You can remove it as soon as you are adding your first statement.

### Instance Construction[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=3)]

The class is a callable object that constructs an instance of the class when called. Let's say we create a class Foo.

```python
class Foo:
    "Foo is our new toy."
    pass
```

To construct an instance of the class, Foo, "call" the class object:

```
f = Foo()
```

This constructs an instance of class Foo and creates a reference to it in f.

### Class Members[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=4)]

In order to access the member of an instance of a class, use the syntax <class instance>.<member>. It is also possible to access the members of the class definition with <class name>.<member>.

#### Methods[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=5)]

A method is a function within a class. The first argument (methods must always take at least one argument) is always the instance of the class on which the function is invoked. For example

```python
>>> class Foo:
...     def setx(self, x):
...         self.x = x
...     def bar(self):
...         print self.x
```

If this code were executed, nothing would happen, at least until an instance of Foo were constructed, and then bar were called on that instance.

##### Why a mandatory argument?[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=6)]

In a normal function, if you were to set a variable, such as `test = 23`, you could not access the test variable. Typing `test` would say it is not defined. This is true in class functions unless they use the `self` variable.

Basically, in the previous example, if we were to remove self.x, function bar could not do anything because it could not access x. The x in setx() would disappear. The self argument saves the variable into the class's "shared variables" database.

##### Why self?[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=7)]

You do not need to use self. However, it is a norm to use self.

#### Invoking Methods[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=8)]

Calling a method is much like calling a function, but instead of passing the instance as the first parameter like the list of formal parameters suggests, use the function as an attribute of the instance.

```python
>>> f = Foo()
>>> f.setx(5)
>>> f.bar()
```

This will output

```
5
```

It is possible to call the method on an arbitrary object, by using it as an attribute of the defining class instead of an instance of that class, like so:

```python
>>> Foo.setx(f,5)
>>> Foo.bar(f)
```

This will have the same output.

#### Dynamic Class Structure[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=9)]

As shown by the method setx above, the members of a Python class can change during runtime, not just their values, unlike classes in languages like C++ or Java. We can even delete f.x after running the code above.

```python
>>> del f.x
>>> f.bar()
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
  File "<stdin>", line 5, in bar
AttributeError: Foo instance has no attribute 'x'
```

Another effect of this is that we can change the definition of the Foo class during program execution. In the code below, we create a member of the Foo class definition named y. If we then create a new instance of Foo, it will now have this new member.

```python
>>> Foo.y = 10
>>> g = Foo()
>>> g.y
10
```

##### Viewing Class Dictionaries[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=10)]

At the heart of all this is a [dictionary](https://en.wikibooks.org/wiki/Python_Programming/Dictionaries) that can be accessed by "vars(ClassName)"

```python
>>> vars(g)
{}
```

At first, this output makes no sense. We just saw that g had the member y, so why isn't it in the member dictionary? If you remember, though, we put y in the class definition, Foo, not g.

```python
>>> vars(Foo)
{'y': 10, 'bar': <function bar at 0x4d6a3c>, '__module__': '__main__',
 'setx': <function setx at 0x4d6a04>, '__doc__': None}
```

And there we have all the members of the Foo class definition. When Python checks for g.member, it first checks g's vars dictionary for "member," then Foo. If we create a new member of g, it will be added to g's dictionary, but not Foo's.

```python
>>> g.setx(5)
>>> vars(g)
{'x': 5}
```

Note that if we now assign a value to g.y, we are not assigning that value to Foo.y. Foo.y will still be 10, but g.y will now override Foo.y

```python
>>> g.y = 9
>>> vars(g)
{'y': 9, 'x': 5}
>>> vars(Foo)
{'y': 10, 'bar': <function bar at 0x4d6a3c>, '__module__': '__main__',
 'setx': <function setx at 0x4d6a04>, '__doc__': None}
```

Sure enough, if we check the values:

```python
>>> g.y
9
>>> Foo.y
10
```

Note that f.y will also be 10, as Python won't find 'y' in vars(f), so it will get the value of 'y' from vars(Foo).

Some may have also noticed that the methods in Foo appear in the class dictionary along with the x and y. If you remember from the section on [lambda functions](https://en.wikibooks.org/wiki/Python_Programming/Functions#Lambda_expressions), we can treat functions just like variables. This means that we can assign methods to a class during runtime in the same way we assigned variables. If you do this, though, remember that if we call a method of a class instance, the first parameter passed to the method will always be the class instance itself.

##### Changing Class Dictionaries[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=11)]

We can also access the members dictionary of a class using the __dict__ member of the class.

```python
>>> g.__dict__
{'y': 9, 'x': 5}
```

If we add, remove, or change key-value pairs from g.__dict__, this has the same effect as if we had made those changes to the members of g.

```python
>>> g.__dict__['z'] = -4
>>> g.z
-4
```

### Why use classes?[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=12)]

Classes are special due to the fact once an instance is made, the instance is independent of all other instances. I could have two instances, each with a different x value, and they will not affect the other's x.

```python
f = Foo()
f.setx(324)
f.boo()
g = Foo()
g.setx(100)
g.boo()
```

`f.boo()` and `g.boo()` will print different values.

### New Style Classes[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=13)]

New style classes were introduced in python 2.2. A new-style class is a class that has a built-in as its base, most commonly object. At a low level, a major difference between old and new classes is their type. Old class instances were all of type `instance`. New style class instances will return the same thing as x.__class__ for their type. This puts user defined classes on a level playing field with built-ins. Old/Classic classes are slated to disappear in Python 3. With this in mind all development should use new style classes. New Style classes also add constructs like properties and static methods familiar to Java programmers.

Old/Classic Class

```python
>>> class ClassicFoo:
...     def __init__(self):
...         pass
```

New Style Class

```python
>>> class NewStyleFoo(object):
...     def __init__(self):
...         pass
```

#### Properties[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=14)]

Properties are attributes with getter and setter methods.

```python
>>> class SpamWithProperties(object):
...     def __init__(self):
...         self.__egg = "MyEgg"
...     def get_egg(self):
...         return self.__egg
...     def set_egg(self, egg):
...         self.__egg = egg
...     egg = property(get_egg, set_egg)

>>> sp = SpamWithProperties()
>>> sp.egg
'MyEgg'
>>> sp.egg = "Eggs With Spam"
>>> sp.egg
'Eggs With Spam'
>>>
```

and since Python 2.6, with @property decorator

```python
>>> class SpamWithProperties(object):
...     def __init__(self):
...         self.__egg = "MyEgg"
...     @property
...     def egg(self):
...         return self.__egg
...     @egg.setter
...     def egg(self, egg):
...         self.__egg = egg
```

#### Static Methods[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=15)]

Static methods in Python are just like their counterparts in C++ or Java. Static methods have no "self" argument and don't require you to instantiate the class before using them. They can be defined using staticmethod()

```python
>>> class StaticSpam(object):
...     def StaticNoSpam():
...         print "You can't have have the spam, spam, eggs and spam without any spam... that's disgusting"
...     NoSpam = staticmethod(StaticNoSpam)

>>> StaticSpam.NoSpam()
You can't have have the spam, spam, eggs and spam without any spam... that's disgusting
```

They can also be defined using the function decorator @staticmethod.

```python
>>> class StaticSpam(object):
...     @staticmethod
...     def StaticNoSpam():
...         print "You can't have have the spam, spam, eggs and spam without any spam... that's disgusting"
```

### Inheritance[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=16)]

Like all object oriented languages, Python provides support for inheritance. Inheritance is a simple concept by which a class can extend the facilities of another class, or in Python's case, multiple other classes. Use the following format for this:

```python
class ClassName(BaseClass1, BaseClass2, BaseClass3,...):
    ...
```

ClassName is what is known as the derived class, that is, derived from the base classes. The derived class will then have all the members of its base classes. If a method is defined in the derived class and in the base class, the member in the derived class will override the one in the base class. In order to use the method defined in the base class, it is necessary to call the method as an attribute on the defining class, as in Foo.setx(f,5) above:

```python
>>> class Foo:
...     def bar(self):
...         print "I'm doing Foo.bar()"
...     x = 10
...
>>> class Bar(Foo):
...     def bar(self):
...         print "I'm doing Bar.bar()"
...         Foo.bar(self)
...     y = 9
...
>>> g = Bar()
>>> Bar.bar(g)
I'm doing Bar.bar()
I'm doing Foo.bar()
>>> g.y
9
>>> g.x
10
```

Once again, we can see what's going on under the hood by looking at the class dictionaries.

```python
>>> vars(g)
{}
>>> vars(Bar)
{'y': 9, '__module__': '__main__', 'bar': <function bar at 0x4d6a04>,
 '__doc__': None}
>>> vars(Foo)
{'x': 10, '__module__': '__main__', 'bar': <function bar at 0x4d6994>,
 '__doc__': None}
```

When we call g.x, it first looks in the vars(g) dictionary, as usual. Also as above, it checks vars(Bar) next, since g is an instance of Bar. However, thanks to inheritance, Python will check vars(Foo) if it doesn't find x in vars(Bar).

### Multiple inheritance[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=17)]

As shown in section [#Inheritance](https://en.wikibooks.org/wiki/Python_Programming/Classes#Inheritance), a class can be derived from multiple classes:

```python
class ClassName(BaseClass1, BaseClass2, BaseClass3):
    pass
```

A tricky part about multiple inheritance is method resolution: upon a method call, if the method name is available from multiple base classes or their base classes, which base class method should be called.

The method resolution order depends on whether the class is an old-style class or a new-style class. For old-style classes, derived classes are considered from left to right, and base classes of base classes are considered before moving to the right. Thus, above, BaseClass1 is considered first, and if method is not found there, the base classes of BaseClass1 are considered. If that fails, BaseClass2 is considered, then its base classes, and so on. For new-style classes, see the Python documentation online.

Links:

- [9.5.1. Multiple Inheritance](https://docs.python.org/2/tutorial/classes.html), docs.python.org
- [The Python 2.3 Method Resolution Order](https://www.python.org/download/releases/2.3/mro/), python.org

### Special Methods[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=18)]

There are a number of methods which have reserved names which are used for special purposes like mimicking numerical or container operations, among other things. All of these names begin and end with two underscores. It is convention that methods beginning with a single underscore are 'private' to the scope they are introduced within.

#### Initialization and Deletion[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=19)]

##### __init__[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=20)]

One of these purposes is constructing an instance, and the special name for this is '__init__'. __init__() is called before an instance is returned (it is not necessary to return the instance manually). As an example,

```python
class A:
    def __init__(self):
        print 'A.__init__()'
a = A()
```

outputs

```python
A.__init__()
```

__init__() can take arguments, in which case it is necessary to pass arguments to the class in order to create an instance. For example,

```python
class Foo:
    def __init__ (self, printme):
        print printme
foo = Foo('Hi!')
```

outputs

```
Hi!
```

Here is an example showing the difference between using __init__() and not using __init__():

```python
class Foo:
    def __init__ (self, x):
         print x
foo = Foo('Hi!')
class Foo2:
    def setx(self, x):
        print x
f = Foo2()
Foo2.setx(f,'Hi!')
```

outputs

```
Hi!
Hi!
```

##### __del__[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=21)]

Similarly, '__del__' is called when an instance is destroyed; e.g. when it is no longer referenced.

##### __enter__ and __exit__[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=22)]

These methods are also a constructor and a destructor but they're only executed when the class is instantiated with `with`. Example:

```python
class ConstructorsDestructors:
    def __init__(self):
        print 'init'

    def __del__(self):
        print 'del'

    def __enter__(self):
        print 'enter'

    def __exit__(self, exc_type, exc_value, traceback):
        print 'exit'

with ConstructorsDestructors():
    pass
init
enter
exit
del
```

##### __new__[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=23)]

[Metaclass](https://en.wikipedia.org/wiki/Metaclass) constructor.

#### Representation[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=24)]

##### __str__[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=25)]

Converting an object to a string, as with the print statement or with the str() conversion function, can be overridden by overriding __str__. Usually, __str__ returns a formatted version of the objects content. This will NOT usually be something that can be executed.

For example:

```python
class Bar:
    def __init__ (self, iamthis):
        self.iamthis = iamthis
    def __str__ (self):
        return self.iamthis
bar = Bar('apple')
print bar
```

outputs

```
apple
```

##### __repr__[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=26)]

This function is much like __str__(). If __str__ is not present but this one is, this function's output is used instead for printing. __repr__ is used to return a representation of the object in string form. In general, it can be executed to get back the original object.

For example:

```python
class Bar:
    def __init__ (self, iamthis):
        self.iamthis = iamthis
    def __repr__(self):
        return "Bar('%s')" % self.iamthis
bar = Bar('apple')
bar
```

outputs (note the difference: it may not be necessary to put it inside a print, however in Python 2.7 it does)

```
Bar('apple')
```

**String Representation Override**

|  Function   |       Operator        |
| :---------: | :-------------------: |
|   __str__   |        str(A)         |
|  __repr__   |        repr(A)        |
| __unicode__ | unicode(x) (2.x only) |

#### Attributes[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=27)]

##### __setattr__[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=28)]

This is the function which is in charge of setting attributes of a class. It is provided with the name and value of the variables being assigned. Each class, of course, comes with a default __setattr__ which simply sets the value of the variable, but we can override it.

```python
>>> class Unchangable:
...    def __setattr__(self, name, value):
...        print "Nice try"
...
>>> u = Unchangable()
>>> u.x = 9
Nice try
>>> u.x
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
AttributeError: Unchangable instance has no attribute 'x'
```

##### __getattr___[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=29)]

Similar to __setattr__, except this function is called when we try to access a class member, and the default simply returns the value.

```python
>>> class HiddenMembers:
...     def __getattr__(self, name):
...         return "You don't get to see " + name
...
>>> h = HiddenMembers()
>>> h.anything
"You don't get to see anything"
```

##### __delattr__[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=30)]

This function is called to delete an attribute.

```python
>>> class Permanent:
...     def __delattr__(self, name):
...         print name, "cannot be deleted"
...
>>> p = Permanent()
>>> p.x = 9
>>> del p.x
x cannot be deleted
>>> p.x
9
```

**Attribute Override Functions**

|  Function   |  Indirect form   | Direct Form |
| :---------: | :--------------: | :---------: |
| __getattr__ |  getattr(A, B)   |     A.B     |
| __setattr__ | setattr(A, B, C) |   A.B = C   |
| __delattr__ |  delattr(A, B)   |   del A.B   |

#### Operator Overloading[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=31)]

Operator overloading allows us to use the built-in Python syntax and operators to call functions which we define.

### 	Binary Operators[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=32)]

If a class has the __add__ function, we can use the '+' operator to add instances of the class. This will call __add__ with the two instances of the class passed as parameters, and the return value will be the result of the addition.

```python
>>> class FakeNumber:
...     n = 5
...     def __add__(A,B):
...         return A.n + B.n
...
>>> c = FakeNumber()
>>> d = FakeNumber()
>>> d.n = 7
>>> c + d
12
```

To override the [augmented assignment](https://en.wikibooks.org/wiki/Python_Programming/Operators#Augmented_Assignment) operators, merely add 'i' in front of the normal binary operator, i.e. for '+=' use '__iadd__' instead of '__add__'. The function will be given one argument, which will be the object on the right side of the augmented assignment operator. The returned value of the function will then be assigned to the object on the left of the operator.

```python
>>> c.__imul__ = lambda B: B.n - 6
>>> c *= d
>>> c
1
```

It is important to note that the [augmented assignment](https://en.wikibooks.org/wiki/Python_Programming/Operators#Augmented_Assignment) operators will also use the normal operator functions if the augmented operator function hasn't been set directly. This will work as expected, with "__add__" being called for "+=" and so on.

```
>>> c = FakeNumber()
>>> c += d
>>> c
12
```

##### Item Operators[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=34)]

It is also possible in Python to override the [indexing and slicing](https://en.wikibooks.org/wiki/Python_Programming/Strings#Indexing_and_Slicing) operators. This allows us to use the class[i] and class[a:b] syntax on our own objects.The simplest form of item operator is __getitem__. This takes as a parameter the instance of the class, then the value of the index.

```python
>>> class FakeList:
...     def __getitem__(self,index):
...         return index * 2
...
>>> f = FakeList()
>>> f['a']
'aa'
```

We can also define a function for the syntax associated with assigning a value to an item. The parameters for this function include the value being assigned, in addition to the parameters from __getitem__

```python
>>> class FakeList:
...     def __setitem__(self,index,value):
...         self.string = index + " is now " + value
...
>>> f = FakeList()
>>> f['a'] = 'gone'
>>> f.string
'a is now gone'
```

We can do the same thing with slices. Once again, each syntax has a different parameter list associated with it.

```python
>>> class FakeList:
...     def __getslice___(self,start,end):
...         return str(start) + " to " + str(end)
...
>>> f = FakeList()
>>> f[1:4]
'1 to 4'
```

Keep in mind that one or both of the start and end parameters can be blank in slice syntax. Here, Python has default value for both the start and the end, as show below.

```
>> f[:]
'0 to 2147483647'
```

Note that the default value for the end of the slice shown here is simply the largest possible signed integer on a 32-bit system, and may vary depending on your system and C compiler.

- __setslice__ has the parameters (self,start,end,value)

We also have operators for deleting items and slices.

- __delitem__ has the parameters (self,index)
- __delslice__ has the parameters (self,start,end)

Note that these are the same as __getitem__ and __getslice__

### Programming Practices[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=36)]

The flexibility of python classes means that classes can adopt a varied set of behaviors. For the sake of understandability, however, it's best to use many of Python's tools sparingly. Try to declare all methods in the class definition, and always use the <class>.<member> syntax instead of __dict__ whenever possible. Look at classes in [C++](https://en.wikibooks.org/wiki/C%2B%2B_Programming/Classes) and [Java](https://en.wikipedia.org/wiki/Class_(computer_science)#Java) to see what most programmers will expect from a class.

#### Encapsulation[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=37)]

Since all python members of a python class are accessible by functions/methods outside the class, there is no way to enforce [encapsulation](https://en.wikipedia.org/wiki/Information_Hiding) short of overriding __getattr__, __setattr__ and __delattr__. General practice, however, is for the creator of a class or module to simply trust that users will use only the intended interface and avoid limiting access to the workings of the module for the sake of users who do need to access it. When using parts of a class or module other than the intended interface, keep in mind that the those parts may change in later versions of the module, and you may even cause errors or undefined behaviors in the module.since encapsulation is private.

#### Doc Strings[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=38)]

When defining a class, it is convention to document the class using a string literal at the start of the class definition. This string will then be placed in the __doc__ attribute of the class definition.

```python
>>> class Documented:
...     """This is a docstring"""
...     def explode(self):
...         """
...         This method is documented, too! The coder is really serious about
...         making this class usable by others who don't know the code as well
...         as he does.
...
...         """
...         print "boom"
>>> d = Documented()
>>> d.__doc__
'This is a docstring'
```

Docstrings are a very useful way to document your code. Even if you never write a single piece of separate documentation (and let's admit it, doing so is the lowest priority for many coders), including informative docstrings in your classes will go a long way toward making them usable.

Several tools exist for turning the docstrings in Python code into readable API documentation, *e.g.*, [EpyDoc](http://epydoc.sourceforge.net/using.html).

Don't just stop at documenting the class definition, either. Each method in the class should have its own docstring as well. Note that the docstring for the method *explode* in the example class *Documented* above has a fairly lengthy docstring that spans several lines. Its formatting is in accordance with the style suggestions of Python's creator, Guido van Rossum in [PEP 8](https://www.python.org/dev/peps/pep-0008/).

#### Adding methods at runtime[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=39)]

##### To a class[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=40)]

It is fairly easy to add methods to a class at runtime. Lets assume that we have a class called *Spam* and a function cook. We want to be able to use the function cook on all instances of the class Spam:

```python
class Spam:
  def __init__(self):
    self.myeggs = 5

def cook(self):
  print "cooking %s eggs" % self.myeggs

Spam.cook = cook   #add the function to the class Spam
eggs = Spam()      #NOW create a new instance of Spam
eggs.cook()        #and we are ready to cook!
```

This will output

```
cooking 5 eggs
```

##### To an instance of a class[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=41)]

It is a bit more tricky to add methods to an instance of a class that has already been created. Lets assume again that we have a class called *Spam* and we have already created eggs. But then we notice that we wanted to cook those eggs, but we do not want to create a new instance but rather use the already created one:

```python
class Spam:
  def __init__(self):
    self.myeggs = 5

eggs = Spam()

def cook(self):
  print "cooking %s eggs" % self.myeggs

import types
f = types.MethodType(cook, eggs, Spam)
eggs.cook = f

eggs.cook()
```

Now we can cook our eggs and the last statement will output:

```
cooking 5 eggs
```

##### Using a function[[edit](https://en.wikibooks.org/w/index.php?title=Python_Programming/Classes&action=edit&section=42)]

We can also write a function that will make the process of adding methods to an instance of a class easier.

```python
def attach_method(fxn, instance, myclass):
  f = types.MethodType(fxn, instance, myclass)
  setattr(instance, fxn.__name__, f)
```

All we now need to do is call the attach_method with the arguments of the function we want to attach, the instance we want to attach it to and the class the instance is derived from. Thus our function call might look like this:

```
attach_method(cook, eggs, Spam)
```

Note that in the function add_method we cannot write `instance.fxn = f` since this would add a function called fxn to the instance.
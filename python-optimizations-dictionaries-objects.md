# python-optimizations-dictionaries and objects



### [How can I get a dictionary to display its keys in a consistent order?](https://docs.python.org/2/faq/programming.html#id46)

You can’t. Dictionaries store their keys in an unpredictable order, so the display order of a dictionary’s elements will be similarly unpredictable.

This can be frustrating if you want to save a printable version to a file, make some changes and then compare it with some other printed dictionary. In this case, use the `pprint` module to pretty-print the dictionary; the items will be presented in order sorted by the key.

A more complicated solution is to subclass `dict` to create a `SortedDict` class that prints itself in a predictable order. Here’s one simpleminded implementation of such a class:

```python
class SortedDict(dict):
    def __repr__(self):
        keys = sorted(self.keys())
        result = ("{!r}: {!r}".format(k, self[k]) for k in keys)
        return "{{{}}}".format(", ".join(result))

    __str__ = __repr__
```

This will work for many common situations you might encounter, though it’s far from a perfect solution. The largest flaw is that if some values in the dictionary are also dictionaries, their values won’t be presented in any particular order.

### [I want to do a complicated sort: can you do a Schwartzian Transform in Python?](https://docs.python.org/2/faq/programming.html#id47)

The technique, attributed to Randal Schwartz of the Perl community, sorts the elements of a list by a metric which maps each element to its “sort value”. In Python, use the `key` argument for the `sort()` function:

```python
Isorted = L[:]
Isorted.sort(key=lambda s: int(s[10:15]))
```

### [How can I sort one list by values from another list?](https://docs.python.org/2/faq/programming.html#id48)

Merge them into a single list of tuples, sort the resulting list, and then pick out the element you want.

```python
>>> list1 = ["what", "I'm", "sorting", "by"]
>>> list2 = ["something", "else", "to", "sort"]
>>> pairs = zip(list1, list2)
>>> pairs
[('what', 'something'), ("I'm", 'else'), ('sorting', 'to'), ('by', 'sort')]
>>> pairs.sort()
>>> result = [ x[1] for x in pairs ]
>>> result
['else', 'sort', 'to', 'something']
```

An alternative for the last step is:

```python
>>> result = []
>>> for p in pairs: result.append(p[1])
```

If you find this more legible, you might prefer to use this instead of the final list comprehension. However, it is almost twice as slow for long lists. Why? First, the `append()` operation has to reallocate memory, and while it uses some tricks to avoid doing that each time, it still has to do it occasionally, and that costs quite a bit. Second, the expression “result.append” requires an extra attribute lookup, and third, there’s a speed reduction from having to make all those function calls.

## [Objects](https://docs.python.org/2/faq/programming.html#id49)

### [What is a class?](https://docs.python.org/2/faq/programming.html#id50)

A class is the particular object type created by executing a class statement. Class objects are used as templates to create instance objects, which embody both the data (attributes) and code (methods) specific to a datatype.

A class can be based on one or more other classes, called its base class(es). It then inherits the attributes and methods of its base classes. This allows an object model to be successively refined by inheritance. You might have a generic `Mailbox` class that provides basic accessor methods for a mailbox, and subclasses such as `MboxMailbox`, `MaildirMailbox`, `OutlookMailbox` that handle various specific mailbox formats.

### [What is a method?](https://docs.python.org/2/faq/programming.html#id51)

A method is a function on some object `x` that you normally call as `x.name(arguments...)`. Methods are defined as functions inside the class definition:

```python
class C:
    def meth(self, arg):
        return arg * 2 + self.attribute
```

### [What is self?](https://docs.python.org/2/faq/programming.html#id52)

Self is merely a conventional name for the first argument of a method. A method defined as `meth(self, a, b, c)` should be called as `x.meth(a, b, c)` for some instance `x` of the class in which the definition occurs; the called method will think it is called as `meth(x, a, b, c)`.

See also [Why must ‘self’ be used explicitly in method definitions and calls?](https://docs.python.org/2/faq/design.html#why-self).

### [How do I check if an object is an instance of a given class or of a subclass of it?](https://docs.python.org/2/faq/programming.html#id53)

Use the built-in function `isinstance(obj, cls)`. You can check if an object is an instance of any of a number of classes by providing a tuple instead of a single class, e.g. `isinstance(obj, (class1, class2, ...))`, and can also check whether an object is one of Python’s built-in types, e.g. `isinstance(obj, str)` or `isinstance(obj, (int, long, float, complex))`.

Note that most programs do not use [`isinstance()`](https://docs.python.org/2/library/functions.html#isinstance) on user-defined classes very often. If you are developing the classes yourself, a more proper object-oriented style is to define methods on the classes that encapsulate a particular behaviour, instead of checking the object’s class and doing a different thing based on what class it is. For example, if you have a function that does something:

```python
def search(obj):
    if isinstance(obj, Mailbox):
        ...  # code to search a mailbox
    elif isinstance(obj, Document):
        ...  # code to search a document
    elif ...
```

A better approach is to define a `search()` method on all the classes and just call it:

```python
class Mailbox:
    def search(self):
        ...  # code to search a mailbox

class Document:
    def search(self):
        ...  # code to search a document

obj.search()
```

### [What is delegation?](https://docs.python.org/2/faq/programming.html#id54)

Delegation is an object oriented technique (also called a design pattern). Let’s say you have an object `x` and want to change the behaviour of just one of its methods. You can create a new class that provides a new implementation of the method you’re interested in changing and delegates all other methods to the corresponding method of `x`.

Python programmers can easily implement delegation. For example, the following class implements a class that behaves like a file but converts all written data to uppercase:

```python
class UpperOut:

    def __init__(self, outfile):
        self._outfile = outfile

    def write(self, s):
        self._outfile.write(s.upper())

    def __getattr__(self, name):
        return getattr(self._outfile, name)
```

Here the `UpperOut` class redefines the `write()` method to convert the argument string to uppercase before calling the underlying `self.__outfile.write()` method. All other methods are delegated to the underlying `self.__outfile` object. The delegation is accomplished via the `__getattr__` method; consult [the language reference](https://docs.python.org/2/reference/datamodel.html#attribute-access) for more information about controlling attribute access.

Note that for more general cases delegation can get trickier. When attributes must be set as well as retrieved, the class must define a [`__setattr__()`](https://docs.python.org/2/reference/datamodel.html#object.__setattr__) method too, and it must do so carefully. The basic implementation of [`__setattr__()`](https://docs.python.org/2/reference/datamodel.html#object.__setattr__) is roughly equivalent to the following:

```python
class X:
    ...
    def __setattr__(self, name, value):
        self.__dict__[name] = value
    ...
```

Most [`__setattr__()`](https://docs.python.org/2/reference/datamodel.html#object.__setattr__) implementations must modify `self.__dict__` to store local state for self without causing an infinite recursion.

### [How do I call a method defined in a base class from a derived class that overrides it?](https://docs.python.org/2/faq/programming.html#id55)

If you’re using new-style classes, use the built-in [`super()`](https://docs.python.org/2/library/functions.html#super) function:

```python
class Derived(Base):
    def meth(self):
        super(Derived, self).meth()
```

If you’re using classic classes: For a class definition such as `class Derived(Base): ...` you can call method `meth()` defined in `Base` (or one of `Base`’s base classes) as `Base.meth(self, arguments...)`. Here, `Base.meth` is an unbound method, so you need to provide the `self` argument.

### [How can I organize my code to make it easier to change the base class?](https://docs.python.org/2/faq/programming.html#id56)

You could define an alias for the base class, assign the real base class to it before your class definition, and use the alias throughout your class. Then all you have to change is the value assigned to the alias. Incidentally, this trick is also handy if you want to decide dynamically (e.g. depending on availability of resources) which base class to use. Example:

```python
BaseAlias = <real base class>

class Derived(BaseAlias):
    def meth(self):
        BaseAlias.meth(self)
        ...
```

### [How do I create static class data and static class methods?](https://docs.python.org/2/faq/programming.html#id57)

Both static data and static methods (in the sense of C++ or Java) are supported in Python.

For static data, simply define a class attribute. To assign a new value to the attribute, you have to explicitly use the class name in the assignment:

```python
class C:
    count = 0   # number of times C.__init__ called

    def __init__(self):
        C.count = C.count + 1

    def getcount(self):
        return C.count  # or return self.count
```

`c.count` also refers to `C.count` for any `c` such that `isinstance(c, C)` holds, unless overridden by `c` itself or by some class on the base-class search path from `c.__class__` back to `C`.

Caution: within a method of C, an assignment like `self.count = 42` creates a new and unrelated instance named “count” in `self`’s own dict. Rebinding of a class-static data name must always specify the class whether inside a method or not:

```python
C.count = 314
```

Static methods are possible since Python 2.2:

```python
class C:
    def static(arg1, arg2, arg3):
        # No 'self' parameter!
        ...
    static = staticmethod(static)
```

With Python 2.4’s decorators, this can also be written as

```python
class C:
    @staticmethod
    def static(arg1, arg2, arg3):
        # No 'self' parameter!
        ...
```

However, a far more straightforward way to get the effect of a static method is via a simple module-level function:

```python
def getcount():
    return C.count
```

If your code is structured so as to define one class (or tightly related class hierarchy) per module, this supplies the desired encapsulation.

### [How can I overload constructors (or methods) in Python?](https://docs.python.org/2/faq/programming.html#id58)

This answer actually applies to all methods, but the question usually comes up first in the context of constructors.

In C++ you’d write

```python
class C {
    C() { cout << "No arguments\n"; }
    C(int i) { cout << "Argument is " << i << "\n"; }
}
```

In Python you have to write a single constructor that catches all cases using default arguments. For example:

```python
class C:
    def __init__(self, i=None):
        if i is None:
            print "No arguments"
        else:
            print "Argument is", i
```

This is not entirely equivalent, but close enough in practice.

You could also try a variable-length argument list, e.g.

```python
def __init__(self, *args):
    ...
```

The same approach works for all method definitions.

### [I try to use __spam and I get an error about _SomeClassName__spam.](https://docs.python.org/2/faq/programming.html#id59)

Variable names with double leading underscores are “mangled” to provide a simple but effective way to define class private variables. Any identifier of the form `__spam` (at least two leading underscores, at most one trailing underscore) is textually replaced with `_classname__spam`, where `classname` is the current class name with any leading underscores stripped.

This doesn’t guarantee privacy: an outside user can still deliberately access the “_classname__spam” attribute, and private values are visible in the object’s `__dict__`. Many Python programmers never bother to use private variable names at all.

### [My class defines __del__ but it is not called when I delete the object.](https://docs.python.org/2/faq/programming.html#id60)

There are several possible reasons for this.

The del statement does not necessarily call [`__del__()`](https://docs.python.org/2/reference/datamodel.html#object.__del__) – it simply decrements the object’s reference count, and if this reaches zero [`__del__()`](https://docs.python.org/2/reference/datamodel.html#object.__del__) is called.

If your data structures contain circular links (e.g. a tree where each child has a parent reference and each parent has a list of children) the reference counts will never go back to zero. Once in a while Python runs an algorithm to detect such cycles, but the garbage collector might run some time after the last reference to your data structure vanishes, so your [`__del__()`](https://docs.python.org/2/reference/datamodel.html#object.__del__) method may be called at an inconvenient and random time. This is inconvenient if you’re trying to reproduce a problem. Worse, the order in which object’s [`__del__()`](https://docs.python.org/2/reference/datamodel.html#object.__del__) methods are executed is arbitrary. You can run [`gc.collect()`](https://docs.python.org/2/library/gc.html#gc.collect) to force a collection, but there *are* pathological cases where objects will never be collected.

Despite the cycle collector, it’s still a good idea to define an explicit `close()` method on objects to be called whenever you’re done with them. The `close()` method can then remove attributes that refer to subobjecs. Don’t call [`__del__()`](https://docs.python.org/2/reference/datamodel.html#object.__del__) directly – [`__del__()`](https://docs.python.org/2/reference/datamodel.html#object.__del__) should call `close()` and `close()` should make sure that it can be called more than once for the same object.

Another way to avoid cyclical references is to use the [`weakref`](https://docs.python.org/2/library/weakref.html#module-weakref) module, which allows you to point to objects without incrementing their reference count. Tree data structures, for instance, should use weak references for their parent and sibling references (if they need them!).

If the object has ever been a local variable in a function that caught an expression in an except clause, chances are that a reference to the object still exists in that function’s stack frame as contained in the stack trace. Normally, calling [`sys.exc_clear()`](https://docs.python.org/2/library/sys.html#sys.exc_clear) will take care of this by clearing the last recorded exception.

Finally, if your [`__del__()`](https://docs.python.org/2/reference/datamodel.html#object.__del__) method raises an exception, a warning message is printed to [`sys.stderr`](https://docs.python.org/2/library/sys.html#sys.stderr).

### [How do I get a list of all instances of a given class?](https://docs.python.org/2/faq/programming.html#id61)

Python does not keep track of all instances of a class (or of a built-in type). You can program the class’s constructor to keep track of all instances by keeping a list of weak references to each instance.

### [Why does the result of `id()` appear to be not unique?](https://docs.python.org/2/faq/programming.html#id62)

The [`id()`](https://docs.python.org/2/library/functions.html#id) builtin returns an integer that is guaranteed to be unique during the lifetime of the object. Since in CPython, this is the object’s memory address, it happens frequently that after an object is deleted from memory, the next freshly created object is allocated at the same position in memory. This is illustrated by this example:

```python
>>> id(1000)
13901272
>>> id(2000)
13901272
```

The two ids belong to different integer objects that are created before, and deleted immediately after execution of the `id()` call. To be sure that objects whose id you want to examine are still alive, create another reference to the object:

```python
>>> a = 1000; b = 2000
>>> id(a)
13901272
>>> id(b)
13891296
```
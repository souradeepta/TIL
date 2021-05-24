# Python Metaclasses

#### 3.3.3.1. Metaclasses

By default, classes are constructed using [`type()`](https://docs.python.org/3.9/library/functions.html#type). The class body is executed in a new namespace and the class name is bound locally to the result of `type(name, bases, namespace)`.

The class creation process can be customized by passing the `metaclass` keyword argument in the class definition line, or by inheriting from an existing class that included such an argument. In the following example, both `MyClass` and `MySubclass` are instances of `Meta`:

```python
class Meta(type):
    pass

class MyClass(metaclass=Meta):
    pass

class MySubclass(MyClass):
    pass
```

Any other keyword arguments that are specified in the class definition are passed through to all metaclass operations described below.

When a class definition is executed, the following steps occur:

- MRO entries are resolved;
- the appropriate metaclass is determined;
- the class namespace is prepared;
- the class body is executed;
- the class object is created.

#### 3.3.3.2. Resolving MRO entries

If a base that appears in class definition is not an instance of [`type`](https://docs.python.org/3.9/library/functions.html#type), then an `__mro_entries__` method is searched on it. If found, it is called with the original bases tuple. This method must return a tuple of classes that will be used instead of this base. The tuple may be empty, in such case the original base is ignored.



#### 3.3.3.3. Determining the appropriate metaclass

The appropriate metaclass for a class definition is determined as follows:

- if no bases and no explicit metaclass are given, then [`type()`](https://docs.python.org/3.9/library/functions.html#type) is used;
- if an explicit metaclass is given and it is *not* an instance of [`type()`](https://docs.python.org/3.9/library/functions.html#type), then it is used directly as the metaclass;
- if an instance of [`type()`](https://docs.python.org/3.9/library/functions.html#type) is given as the explicit metaclass, or bases are defined, then the most derived metaclass is used.

The most derived metaclass is selected from the explicitly specified metaclass (if any) and the metaclasses (i.e. `type(cls)`) of all specified base classes. The most derived metaclass is one which is a subtype of *all* of these candidate metaclasses. If none of the candidate metaclasses meets that criterion, then the class definition will fail with `TypeError`.



#### 3.3.3.4. Preparing the class namespace

Once the appropriate metaclass has been identified, then the class namespace is prepared. If the metaclass has a `__prepare__` attribute, it is called as `namespace = metaclass.__prepare__(name, bases, **kwds)` (where the additional keyword arguments, if any, come from the class definition). The `__prepare__` method should be implemented as a [`classmethod()`](https://docs.python.org/3.9/library/functions.html#classmethod). The namespace returned by `__prepare__` is passed in to `__new__`, but when the final class object is created the namespace is copied into a new `dict`.

If the metaclass has no `__prepare__` attribute, then the class namespace is initialised as an empty ordered mapping.



#### 3.3.3.5. Executing the class body

The class body is executed (approximately) as `exec(body, globals(), namespace)`. The key difference from a normal call to [`exec()`](https://docs.python.org/3.9/library/functions.html#exec) is that lexical scoping allows the class body (including any methods) to reference names from the current and outer scopes when the class definition occurs inside a function.

However, even when the class definition occurs inside the function, methods defined inside the class still cannot see names defined at the class scope. Class variables must be accessed through the first parameter of instance or class methods, or through the implicit lexically scoped `__class__` reference described in the next section.



#### 3.3.3.6. Creating the class object

Once the class namespace has been populated by executing the class body, the class object is created by calling `metaclass(name, bases, namespace, **kwds)` (the additional keywords passed here are the same as those passed to `__prepare__`).

This class object is the one that will be referenced by the zero-argument form of [`super()`](https://docs.python.org/3.9/library/functions.html#super). `__class__` is an implicit closure reference created by the compiler if any methods in a class body refer to either `__class__` or `super`. This allows the zero argument form of [`super()`](https://docs.python.org/3.9/library/functions.html#super) to correctly identify the class being defined based on lexical scoping, while the class or instance that was used to make the current call is identified based on the first argument passed to the method.

**CPython implementation detail:** In CPython 3.6 and later, the `__class__` cell is passed to the metaclass as a `__classcell__` entry in the class namespace. If present, this must be propagated up to the `type.__new__` call in order for the class to be initialised correctly. Failing to do so will result in a [`RuntimeError`](https://docs.python.org/3.9/library/exceptions.html#RuntimeError) in Python 3.8.

When using the default metaclass [`type`](https://docs.python.org/3.9/library/functions.html#type), or any metaclass that ultimately calls `type.__new__`, the following additional customisation steps are invoked after creating the class object:

- first, `type.__new__` collects all of the descriptors in the class namespace that define a [`__set_name__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__set_name__) method;
- second, all of these `__set_name__` methods are called with the class being defined and the assigned name of that particular descriptor;
- finally, the [`__init_subclass__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__init_subclass__) hook is called on the immediate parent of the new class in its method resolution order.

After the class object is created, it is passed to the class decorators included in the class definition (if any) and the resulting object is bound in the local namespace as the defined class.

When a new class is created by `type.__new__`, the object provided as the namespace parameter is copied to a new ordered mapping and the original object is discarded. The new copy is wrapped in a read-only proxy, which becomes the [`__dict__`](https://docs.python.org/3.9/library/stdtypes.html#object.__dict__) attribute of the class object.

See also

- [**PEP 3135**](https://www.python.org/dev/peps/pep-3135) - New super

  Describes the implicit `__class__` closure reference

#### 3.3.3.7. Uses for metaclasses

The potential uses for metaclasses are boundless. Some ideas that have been explored include enum, logging, interface checking, automatic delegation, automatic property creation, proxies, frameworks, and automatic resource locking/synchronization.

### 3.3.4. Customizing instance and subclass checks

The following methods are used to override the default behavior of the [`isinstance()`](https://docs.python.org/3.9/library/functions.html#isinstance) and [`issubclass()`](https://docs.python.org/3.9/library/functions.html#issubclass) built-in functions.

In particular, the metaclass [`abc.ABCMeta`](https://docs.python.org/3.9/library/abc.html#abc.ABCMeta) implements these methods in order to allow the addition of Abstract Base Classes (ABCs) as “virtual base classes” to any class or type (including built-in types), including other ABCs.

- `class.``__instancecheck__`(*self*, *instance*)

  Return true if *instance* should be considered a (direct or indirect) instance of *class*. If defined, called to implement `isinstance(instance, class)`.

- `class.``__subclasscheck__`(*self*, *subclass*)

  Return true if *subclass* should be considered a (direct or indirect) subclass of *class*. If defined, called to implement `issubclass(subclass, class)`.

Note that these methods are looked up on the type (metaclass) of a class. They cannot be defined as class methods in the actual class. This is consistent with the lookup of special methods that are called on instances, only in this case the instance is itself a class.

See also

- [**PEP 3119**](https://www.python.org/dev/peps/pep-3119) - Introducing Abstract Base Classes

  Includes the specification for customizing [`isinstance()`](https://docs.python.org/3.9/library/functions.html#isinstance) and [`issubclass()`](https://docs.python.org/3.9/library/functions.html#issubclass) behavior through [`__instancecheck__()`](https://docs.python.org/3.9/reference/datamodel.html#class.__instancecheck__) and [`__subclasscheck__()`](https://docs.python.org/3.9/reference/datamodel.html#class.__subclasscheck__), with motivation for this functionality in the context of adding Abstract Base Classes (see the [`abc`](https://docs.python.org/3.9/library/abc.html#module-abc) module) to the language.

### 3.3.5. Emulating generic types

One can implement the generic class syntax as specified by [**PEP 484**](https://www.python.org/dev/peps/pep-0484) (for example `List[int]`) by defining a special method:

- *classmethod* `object.``__class_getitem__`(*cls*, *key*)

  Return an object representing the specialization of a generic class by type arguments found in *key*.

This method is looked up on the class object itself, and when defined in the class body, this method is implicitly a class method. Note, this mechanism is primarily reserved for use with static type hints, other usage is discouraged.

See also

 

[**PEP 560**](https://www.python.org/dev/peps/pep-0560) - Core support for typing module and generic types



### 3.3.6. Emulating callable objects

- `object.``__call__`(*self*[, *args...*])

  Called when the instance is “called” as a function; if this method is defined, `x(arg1, arg2, ...)` is a shorthand for `x.__call__(arg1, arg2, ...)`.



### 3.3.7. Emulating container types

The following methods can be defined to implement container objects. Containers usually are sequences (such as lists or tuples) or mappings (like dictionaries), but can represent other containers as well. The first set of methods is used either to emulate a sequence or to emulate a mapping; the difference is that for a sequence, the allowable keys should be the integers *k* for which `0 <= k < N` where *N* is the length of the sequence, or slice objects, which define a range of items. It is also recommended that mappings provide the methods `keys()`, `values()`, `items()`, `get()`, `clear()`, `setdefault()`, `pop()`, `popitem()`, `copy()`, and `update()` behaving similar to those for Python’s standard dictionary objects. The [`collections.abc`](https://docs.python.org/3.9/library/collections.abc.html#module-collections.abc) module provides a [`MutableMapping`](https://docs.python.org/3.9/library/collections.abc.html#collections.abc.MutableMapping) abstract base class to help create those methods from a base set of [`__getitem__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__getitem__), [`__setitem__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__setitem__), [`__delitem__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__delitem__), and `keys()`. Mutable sequences should provide methods `append()`, `count()`, `index()`, `extend()`, `insert()`, `pop()`, `remove()`, `reverse()` and `sort()`, like Python standard list objects. Finally, sequence types should implement addition (meaning concatenation) and multiplication (meaning repetition) by defining the methods [`__add__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__add__), [`__radd__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__radd__), [`__iadd__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__iadd__), [`__mul__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__mul__), [`__rmul__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__rmul__) and [`__imul__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__imul__) described below; they should not define other numerical operators. It is recommended that both mappings and sequences implement the [`__contains__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__contains__) method to allow efficient use of the `in` operator; for mappings, `in` should search the mapping’s keys; for sequences, it should search through the values. It is further recommended that both mappings and sequences implement the [`__iter__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__iter__) method to allow efficient iteration through the container; for mappings, [`__iter__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__iter__) should iterate through the object’s keys; for sequences, it should iterate through the values.

- `object.``__len__`(*self*)

  Called to implement the built-in function [`len()`](https://docs.python.org/3.9/library/functions.html#len). Should return the length of the object, an integer `>=` 0. Also, an object that doesn’t define a [`__bool__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__bool__) method and whose [`__len__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__len__) method returns zero is considered to be false in a Boolean context.**CPython implementation detail:** In CPython, the length is required to be at most [`sys.maxsize`](https://docs.python.org/3.9/library/sys.html#sys.maxsize). If the length is larger than `sys.maxsize` some features (such as [`len()`](https://docs.python.org/3.9/library/functions.html#len)) may raise [`OverflowError`](https://docs.python.org/3.9/library/exceptions.html#OverflowError). To prevent raising `OverflowError` by truth value testing, an object must define a [`__bool__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__bool__) method.

- `object.``__length_hint__`(*self*)

  Called to implement [`operator.length_hint()`](https://docs.python.org/3.9/library/operator.html#operator.length_hint). Should return an estimated length for the object (which may be greater or less than the actual length). The length must be an integer `>=` 0. The return value may also be [`NotImplemented`](https://docs.python.org/3.9/library/constants.html#NotImplemented), which is treated the same as if the `__length_hint__` method didn’t exist at all. This method is purely an optimization and is never required for correctness.*New in version 3.4.*

Note

 

Slicing is done exclusively with the following three methods. A call like

```
a[1:2] = b
```

is translated to

```
a[slice(1, 2, None)] = b
```

and so forth. Missing slice items are always filled in with `None`.

- `object.``__getitem__`(*self*, *key*)

  Called to implement evaluation of `self[key]`. For sequence types, the accepted keys should be integers and slice objects. Note that the special interpretation of negative indexes (if the class wishes to emulate a sequence type) is up to the [`__getitem__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__getitem__) method. If *key* is of an inappropriate type, [`TypeError`](https://docs.python.org/3.9/library/exceptions.html#TypeError) may be raised; if of a value outside the set of indexes for the sequence (after any special interpretation of negative values), [`IndexError`](https://docs.python.org/3.9/library/exceptions.html#IndexError) should be raised. For mapping types, if *key* is missing (not in the container), [`KeyError`](https://docs.python.org/3.9/library/exceptions.html#KeyError) should be raised.Note [`for`](https://docs.python.org/3.9/reference/compound_stmts.html#for) loops expect that an [`IndexError`](https://docs.python.org/3.9/library/exceptions.html#IndexError) will be raised for illegal indexes to allow proper detection of the end of the sequence.

- `object.``__setitem__`(*self*, *key*, *value*)

  Called to implement assignment to `self[key]`. Same note as for [`__getitem__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__getitem__). This should only be implemented for mappings if the objects support changes to the values for keys, or if new keys can be added, or for sequences if elements can be replaced. The same exceptions should be raised for improper *key* values as for the [`__getitem__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__getitem__) method.

- `object.``__delitem__`(*self*, *key*)

  Called to implement deletion of `self[key]`. Same note as for [`__getitem__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__getitem__). This should only be implemented for mappings if the objects support removal of keys, or for sequences if elements can be removed from the sequence. The same exceptions should be raised for improper *key* values as for the [`__getitem__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__getitem__) method.

- `object.``__missing__`(*self*, *key*)

  Called by [`dict`](https://docs.python.org/3.9/library/stdtypes.html#dict).[`__getitem__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__getitem__) to implement `self[key]` for dict subclasses when key is not in the dictionary.

- `object.``__iter__`(*self*)

  This method is called when an iterator is required for a container. This method should return a new iterator object that can iterate over all the objects in the container. For mappings, it should iterate over the keys of the container.Iterator objects also need to implement this method; they are required to return themselves. For more information on iterator objects, see [Iterator Types](https://docs.python.org/3.9/library/stdtypes.html#typeiter).

- `object.``__reversed__`(*self*)

  Called (if present) by the [`reversed()`](https://docs.python.org/3.9/library/functions.html#reversed) built-in to implement reverse iteration. It should return a new iterator object that iterates over all the objects in the container in reverse order.If the [`__reversed__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__reversed__) method is not provided, the [`reversed()`](https://docs.python.org/3.9/library/functions.html#reversed) built-in will fall back to using the sequence protocol ([`__len__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__len__) and [`__getitem__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__getitem__)). Objects that support the sequence protocol should only provide [`__reversed__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__reversed__) if they can provide an implementation that is more efficient than the one provided by [`reversed()`](https://docs.python.org/3.9/library/functions.html#reversed).

The membership test operators ([`in`](https://docs.python.org/3.9/reference/expressions.html#in) and [`not in`](https://docs.python.org/3.9/reference/expressions.html#not-in)) are normally implemented as an iteration through a container. However, container objects can supply the following special method with a more efficient implementation, which also does not require the object be iterable.

- `object.``__contains__`(*self*, *item*)

  Called to implement membership test operators. Should return true if *item* is in *self*, false otherwise. For mapping objects, this should consider the keys of the mapping rather than the values or the key-item pairs.For objects that don’t define [`__contains__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__contains__), the membership test first tries iteration via [`__iter__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__iter__), then the old sequence iteration protocol via [`__getitem__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__getitem__), see [this section in the language reference](https://docs.python.org/3.9/reference/expressions.html#membership-test-details).
# Python List Optimizations

> ## What is the difference between the list methods append and extend?

- `append` adds its argument as a single element to the end of a list. The length of the list itself will increase by one.
- `extend` iterates over its argument adding each element to the list, extending the list. The length of the list will increase by however many elements were in the iterable argument.

## `append`

The `list.append` method appends an object to the end of the list.

```python
my_list.append(object) 
```

Whatever the object is, whether a number, a string, another list, or something else, it gets added onto the end of `my_list` as a single entry on the list.

```python
>>> my_list
['foo', 'bar']
>>> my_list.append('baz')
>>> my_list
['foo', 'bar', 'baz']
```

So keep in mind that a list is an object. If you append another list onto a list, the first list will be a single object at the end of the list (which may not be what you want):

```python
>>> another_list = [1, 2, 3]
>>> my_list.append(another_list)
>>> my_list
['foo', 'bar', 'baz', [1, 2, 3]]
                     #^^^^^^^^^--- single item at the end of the list.
```

## `extend`

The `list.extend` method extends a list by appending elements from an iterable:

```python
my_list.extend(iterable)
```

So with extend, each element of the iterable gets appended onto the list. For example:

```python
>>> my_list
['foo', 'bar']
>>> another_list = [1, 2, 3]
>>> my_list.extend(another_list)
>>> my_list
['foo', 'bar', 1, 2, 3]
```

Keep in mind that a string is an iterable, so if you extend a list with a string, you'll append each character as you iterate over the string (which may not be what you want):

```python
>>> my_list.extend('baz')
>>> my_list
['foo', 'bar', 1, 2, 3, 'b', 'a', 'z']
```

## Operator Overload, `__add__` (`+`) and `__iadd__` (`+=`)

Both `+` and `+=` operators are defined for `list`. They are semantically similar to extend.

`my_list + another_list` creates a third list in memory, so you can return the result of it, but it requires that the second iterable be a list.

`my_list += another_list` modifies the list in-place (it *is* the in-place operator, and lists are mutable objects, as we've seen) so it does not create a new list. It also works like extend, in that the second iterable can be any kind of iterable.

Don't get confused - `my_list = my_list + another_list` is not equivalent to `+=` - it gives you a brand new list assigned to my_list.

## Time Complexity

Append has [constant time complexity](https://wiki.python.org/moin/TimeComplexity), O(1).

Extend has time complexity, O(k).

Iterating through the multiple calls to `append` adds to the complexity, making it equivalent to that of extend, and since extend's iteration is implemented in C, it will always be faster if you intend to append successive items from an iterable onto a list.

## Performance

You may wonder what is more performant, since append can be used to achieve the same outcome as extend. The following functions do the same thing:

```python
def append(alist, iterable):
    for item in iterable:
        alist.append(item)

def extend(alist, iterable):
    alist.extend(iterable)
```

So let's time them:

```python
import timeit

>>> min(timeit.repeat(lambda: append([], "abcdefghijklmnopqrstuvwxyz")))
2.867846965789795
>>> min(timeit.repeat(lambda: extend([], "abcdefghijklmnopqrstuvwxyz")))
0.8060121536254883
```

### Addressing a comment on timings

A commenter said:

> Perfect answer, I just miss the timing of comparing adding only one element

Do the semantically correct thing. If you want to append all elements in an iterable, use `extend`. If you're just adding one element, use `append`.

Ok, so let's create an experiment to see how this works out in time:

```python
def append_one(a_list, element):
    a_list.append(element)

def extend_one(a_list, element):
    """creating a new list is semantically the most direct
    way to create an iterable to give to extend"""
    a_list.extend([element])

import timeit
```

And we see that going out of our way to create an iterable just to use extend is a (minor) waste of time:

```python
>>> min(timeit.repeat(lambda: append_one([], 0)))
0.2082819009956438
>>> min(timeit.repeat(lambda: extend_one([], 0)))
0.2397019260097295
```

We learn from this that there's nothing gained from using `extend` when we have only *one* element to append.

Also, these timings are not that important. I am just showing them to make the point that, in Python, doing the semantically correct thing is doing things the *Right* Way™.

It's conceivable that you might test timings on two comparable operations and get an ambiguous or inverse result. Just focus on doing the semantically correct thing.

## Conclusion

We see that `extend` is semantically clearer, and that it can run much faster than `append`, *when you intend to append each element in an iterable to a list.*

If you only have a single element (not in an iterable) to add to the list, use `append`.





### [How do I convert between tuples and lists?](https://docs.python.org/2/faq/programming.html#id37)

The type constructor `tuple(seq)` converts any sequence (actually, any iterable) into a tuple with the same items in the same order.

For example, `tuple([1, 2, 3])` yields `(1, 2, 3)` and `tuple('abc')` yields `('a', 'b', 'c')`. If the argument is a tuple, it does not make a copy but returns the same object, so it is cheap to call [`tuple()`](https://docs.python.org/2/library/functions.html#tuple) when you aren’t sure that an object is already a tuple.

The type constructor `list(seq)` converts any sequence or iterable into a list with the same items in the same order. For example, `list((1, 2, 3))` yields `[1, 2, 3]` and `list('abc')` yields `['a', 'b', 'c']`. If the argument is a list, it makes a copy just like `seq[:]` would.

### [What’s a negative index?](https://docs.python.org/2/faq/programming.html#id38)

Python sequences are indexed with positive numbers and negative numbers. For positive numbers 0 is the first index 1 is the second index and so forth. For negative indices -1 is the last index and -2 is the penultimate (next to last) index and so forth. Think of `seq[-n]` as the same as `seq[len(seq)-n]`.

Using negative indices can be very convenient. For example `S[:-1]` is all of the string except for its last character, which is useful for removing the trailing newline from a string.

### [How do I iterate over a sequence in reverse order?](https://docs.python.org/2/faq/programming.html#id39)

Use the [`reversed()`](https://docs.python.org/2/library/functions.html#reversed) built-in function, which is new in Python 2.4:

```python
for x in reversed(sequence):
    ...  # do something with x ...
```

This won’t touch your original sequence, but build a new copy with reversed order to iterate over.

With Python 2.3, you can use an extended slice syntax:

```python
for x in sequence[::-1]:
    ...  # do something with x ...
```

### [How do you remove duplicates from a list?](https://docs.python.org/2/faq/programming.html#id40)

See the Python Cookbook for a long discussion of many ways to do this:

> https://code.activestate.com/recipes/52560/

If you don’t mind reordering the list, sort it and then scan from the end of the list, deleting duplicates as you go:

```python
if mylist:
    mylist.sort()
    last = mylist[-1]
    for i in range(len(mylist)-2, -1, -1):
        if last == mylist[i]:
            del mylist[i]
        else:
            last = mylist[i]
```

If all elements of the list may be used as dictionary keys (i.e. they are all hashable) this is often faster

```python
d = {}
for x in mylist:
    d[x] = 1
mylist = list(d.keys())
```

In Python 2.5 and later, the following is possible instead:

```python
mylist = list(set(mylist))
```

This converts the list into a set, thereby removing duplicates, and then back into a list.

### [How do I create a multidimensional list?](https://docs.python.org/2/faq/programming.html#id42)

You probably tried to make a multidimensional array like this:

```python
>>> A = [[None] * 2] * 3
```

This looks correct if you print it:

```python
>>> A
[[None, None], [None, None], [None, None]]
```

But when you assign a value, it shows up in multiple places:

```python
>>> A[0][0] = 5
>>> A
[[5, None], [5, None], [5, None]]
```

The reason is that replicating a list with `*` doesn’t create copies, it only creates references to the existing objects. The `*3` creates a list containing 3 references to the same list of length two. Changes to one row will show in all rows, which is almost certainly not what you want.

The suggested approach is to create a list of the desired length first and then fill in each element with a newly created list:

```python
A = [None] * 3
for i in range(3):
    A[i] = [None] * 2
```

This generates a list containing 3 different lists of length two. You can also use a list comprehension:

```python
w, h = 2, 3
A = [[None] * w for i in range(h)]
```

Or, you can use an extension that provides a matrix datatype; [NumPy](http://www.numpy.org/) is the best known.

### [How do I apply a method to a sequence of objects?](https://docs.python.org/2/faq/programming.html#id43)

Use a list comprehension:

```python
result = [obj.method() for obj in mylist]
```

More generically, you can try the following function:

```python
def method_map(objects, method, arguments):
    """method_map([a,b], "meth", (1,2)) gives [a.meth(1,2), b.meth(1,2)]"""
    nobjects = len(objects)
    methods = map(getattr, objects, [method]*nobjects)
    return map(apply, methods, [arguments]*nobjects)
```



### [Why does a_tuple[i\] += [‘item’] raise an exception when the addition works?](https://docs.python.org/2/faq/programming.html#id44)

This is because of a combination of the fact that augmented assignment operators are *assignment* operators, and the difference between mutable and immutable objects in Python.

This discussion applies in general when augmented assignment operators are applied to elements of a tuple that point to mutable objects, but we’ll use a `list` and `+=` as our exemplar.

If you wrote:

```python
>>> a_tuple = (1, 2)
>>> a_tuple[0] += 1
Traceback (most recent call last):
   ...
TypeError: 'tuple' object does not support item assignment
```

The reason for the exception should be immediately clear: `1` is added to the object `a_tuple[0]` points to (`1`), producing the result object, `2`, but when we attempt to assign the result of the computation, `2`, to element `0` of the tuple, we get an error because we can’t change what an element of a tuple points to.

Under the covers, what this augmented assignment statement is doing is approximately this:

```python
>>> result = a_tuple[0] + 1
>>> a_tuple[0] = result
Traceback (most recent call last):
  ...
TypeError: 'tuple' object does not support item assignment
```

It is the assignment part of the operation that produces the error, since a tuple is immutable.

When you write something like:

```python
>>> a_tuple = (['foo'], 'bar')
>>> a_tuple[0] += ['item']
Traceback (most recent call last):
  ...
TypeError: 'tuple' object does not support item assignment
```

The exception is a bit more surprising, and even more surprising is the fact that even though there was an error, the append worked:

```python
>>> a_tuple[0]
['foo', 'item']
```

To see why this happens, you need to know that (a) if an object implements an `__iadd__` magic method, it gets called when the `+=` augmented assignment is executed, and its return value is what gets used in the assignment statement; and (b) for lists, `__iadd__` is equivalent to calling `extend` on the list and returning the list. That’s why we say that for lists, `+=` is a “shorthand” for `list.extend`:

```python
>>> a_list = []
>>> a_list += [1]
>>> a_list
[1]
```

This is equivalent to:

```python
>>> result = a_list.__iadd__([1])
>>> a_list = result
```

The object pointed to by a_list has been mutated, and the pointer to the mutated object is assigned back to `a_list`. The end result of the assignment is a no-op, since it is a pointer to the same object that `a_list` was previously pointing to, but the assignment still happens.

Thus, in our tuple example what is happening is equivalent to:

```python
>>> result = a_tuple[0].__iadd__(['item'])
>>> a_tuple[0] = result
Traceback (most recent call last):
  ...
TypeError: 'tuple' object does not support item assignment
```

The `__iadd__` succeeds, and thus the list is extended, but even though `result` points to the same object that `a_tuple[0]` already points to, that final assignment still results in an error, because tuples are immutable.
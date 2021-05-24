# [python max function using 'key' and lambda expression](https://stackoverflow.com/questions/18296755/python-max-function-using-key-and-lambda-expression)

[Ask Question](https://stackoverflow.com/questions/ask)

I come from OOP background and trying to learn python. I am using the `max` function which uses a lambda expression to return the instance of type `Player` having maximum `totalScore` among the list `players`.

```py
def winner():
    w = max(players, key=lambda p: p.totalScore)
```

The function correctly returns instance of type `Player` having maximum `totalScore`. I am confused about the following three things:

1. How does the `max` function work? What are the arguments it is taking? I looked at the documentation but failed to understand.
2. What is use of the keyword `key` in max function? I know it is also used in context of `sort` function
3. Meaning of the lambda expression? How to read them? How do they work?

These are all very noobish conceptual questions but will help me understand the language. It would help if you could give examples to explain. Thanks

`lambda` is an anonymous function, it is equivalent to:

```python
def func(p):
   return p.totalScore     
```

Now `max` becomes:

```python
max(players, key=func)
```

But as `def` statements are compound statements they can't be used where an expression is required, that's why sometimes `lambda`'s are used.

Note that `lambda` is equivalent to what you'd put in a return statement of a `def`. Thus, you can't use statements inside a `lambda`, only expressions are allowed.

------

**What does `max` do?**

> max(a, b, c, ...[, key=func]) -> value
>
> With a single iterable argument, return its largest item. With two or more arguments, return the largest argument.

So, it simply returns the object that is the largest.

------

**How does `key` work?**

By default in Python 2 `key` compares items based on a [set of rules](http://docs.python.org/2/reference/expressions.html#not-in) based on the type of the objects (for example a string is always greater than an integer).

To modify the object before comparison, or to compare based on a particular attribute/index, you've to use the `key` argument.

Example 1:

A simple example, suppose you have a list of numbers in string form, but you want to compare those items by their integer value.

```python
>>> lis = ['1', '100', '111', '2']
```

Here `max` compares the items using their original values (strings are compared lexicographically so you'd get `'2'` as output) :

```python
>>> max(lis)
'2'
```

To compare the items by their integer value use `key` with a simple `lambda`:

```python
>>> max(lis, key=lambda x:int(x))  # compare `int` version of each item
'111'
```

Example 2: Applying `max` to a list of tuples.

```python
>>> lis = [(1,'a'), (3,'c'), (4,'e'), (-1,'z')]
```

By default `max` will compare the items by the first index. If the first index is the same then it'll compare the second index. As in my example, all items have a unique first index, so you'd get this as the answer:

```python
>>> max(lis)
(4, 'e')
```

But, what if you wanted to compare each item by the value at index 1? Simple: use `lambda`:

```python
>>> max(lis, key = lambda x: x[1])
(-1, 'z')
```

------

**Comparing items in an iterable that contains objects of different type**:

List with mixed items:

```python
lis = ['1','100','111','2', 2, 2.57]
```

[In Python 2 it is possible to compare items of two different types](http://docs.python.org/2/reference/expressions.html#not-in):

```python
>>> max(lis)  # works in Python 2
'2'
>>> max(lis, key=lambda x: int(x))  # compare integer version of each item
'111'
```

[But in Python 3 you can't do that any more](http://docs.python.org/3.0/whatsnew/3.0.html#ordering-comparisons):

```python
>>> lis = ['1', '100', '111', '2', 2, 2.57]
>>> max(lis)
Traceback (most recent call last):
  File "<ipython-input-2-0ce0a02693e4>", line 1, in <module>
    max(lis)
TypeError: unorderable types: int() > str()
```

But this works, as we are comparing integer version of each object:

```python
>>> max(lis, key=lambda x: int(x))  # or simply `max(lis, key=int)`
'111'
```



[share](https://stackoverflow.com/a/18296814) [improve this answer](https://stackoverflow.com/posts/18296814/edit) follow 
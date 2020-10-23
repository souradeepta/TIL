# Python-optimizations-numbers-strings



### [How do I specify hexadecimal and octal integers?](https://docs.python.org/2/faq/programming.html#id27)

To specify an octal digit, precede the octal value with a zero, and then a lower or uppercase “o”. For example, to set the variable “a” to the octal value “10” (8 in decimal), type:

```python
>>> a = 0o10
>>> a
8
```

Hexadecimal is just as easy. Simply precede the hexadecimal number with a zero, and then a lower or uppercase “x”. Hexadecimal digits can be specified in lower or uppercase. For example, in the Python interpreter:

```python
>>> a = 0xa5
>>> a
165
>>> b = 0XB2
>>> b
178
```

### [How do I convert a string to a number?](https://docs.python.org/2/faq/programming.html#id29)

For integers, use the built-in [`int()`](https://docs.python.org/2/library/functions.html#int) type constructor, e.g. `int('144') == 144`. Similarly, [`float()`](https://docs.python.org/2/library/functions.html#float) converts to floating-point, e.g. `float('144') == 144.0`.

By default, these interpret the number as decimal, so that `int('0144') == 144` and `int('0x144')` raises [`ValueError`](https://docs.python.org/2/library/exceptions.html#exceptions.ValueError). `int(string, base)` takes the base to convert from as a second optional argument, so `int('0x144', 16) == 324`. If the base is specified as 0, the number is interpreted using Python’s rules: a leading ‘0’ indicates octal, and ‘0x’ indicates a hex number.

Do not use the built-in function [`eval()`](https://docs.python.org/2/library/functions.html#eval) if all you need is to convert strings to numbers. [`eval()`](https://docs.python.org/2/library/functions.html#eval) will be significantly slower and it presents a security risk: someone could pass you a Python expression that might have unwanted side effects. For example, someone could pass `__import__('os').system("rm -rf $HOME")` which would erase your home directory.

[`eval()`](https://docs.python.org/2/library/functions.html#eval) also has the effect of interpreting numbers as Python expressions, so that e.g. `eval('09')` gives a syntax error because Python regards numbers starting with ‘0’ as octal (base 8).



### [How do I convert a number to a string?](https://docs.python.org/2/faq/programming.html#id30)

To convert, e.g., the number 144 to the string ‘144’, use the built-in type constructor [`str()`](https://docs.python.org/2/library/functions.html#str). If you want a hexadecimal or octal representation, use the built-in functions [`hex()`](https://docs.python.org/2/library/functions.html#hex) or [`oct()`](https://docs.python.org/2/library/functions.html#oct). For fancy formatting, see the [Format String Syntax](https://docs.python.org/2/library/string.html#formatstrings) section, e.g. `"{:04d}".format(144)` yields `'0144'` and `"{:.3f}".format(1.0/3.0)` yields `'0.333'`. In Python 2, the division (/) operator returns the floor of the mathematical result of division if the arguments are ints or longs, but it returns a reasonable approximation of the division result if the arguments are floats or complex:

```python
>>> print('{:.3f}'.format(1/3))
0.000
>>> print('{:.3f}'.format(1.0/3))
0.333
```

In Python 3, the default behaviour of the division operator (see [**PEP 238**](https://www.python.org/dev/peps/pep-0238)) has been changed but you can have the same behaviour in Python 2 if you import `division` from [`__future__`](https://docs.python.org/2/library/__future__.html#module-__future__):

```python
>>> from __future__ import division
>>> print('{:.3f}'.format(1/3))
0.333
```

You may also use [the % operator](https://docs.python.org/2/library/stdtypes.html#string-formatting) on strings. See the library reference manual for details.

### [How do I modify a string in place?](https://docs.python.org/2/faq/programming.html#id31)

You can’t, because strings are immutable. If you need an object with this ability, try converting the string to a list or use the array module:

```python
>>> import io
>>> s = "Hello, world"
>>> a = list(s)
>>> print a
['H', 'e', 'l', 'l', 'o', ',', ' ', 'w', 'o', 'r', 'l', 'd']
>>> a[7:] = list("there!")
>>> ''.join(a)
'Hello, there!'

>>> import array
>>> a = array.array('c', s)
>>> print a
array('c', 'Hello, world')
>>> a[0] = 'y'; print a
array('c', 'yello, world')
>>> a.tostring()
'yello, world'
```

### [How do I use strings to call functions/methods?](https://docs.python.org/2/faq/programming.html#id32)

There are various techniques.

- The best is to use a dictionary that maps strings to functions. The primary advantage of this technique is that the strings do not need to match the names of the functions. This is also the primary technique used to emulate a case construct:

  ```python
  def a():
      pass
  
  def b():
      pass
  
  dispatch = {'go': a, 'stop': b}  # Note lack of parens for funcs
  
  dispatch[get_input()]()  # Note trailing parens to call function
  ```

- Use the built-in function [`getattr()`](https://docs.python.org/2/library/functions.html#getattr):

  ```python
  import foo
  getattr(foo, 'bar')()
  ```

  Note that [`getattr()`](https://docs.python.org/2/library/functions.html#getattr) works on any object, including classes, class instances, modules, and so on.

  This is used in several places in the standard library, like this:

  ```python
  class Foo:
      def do_foo(self):
          ...
  
      def do_bar(self):
          ...
  
  f = getattr(foo_instance, 'do_' + opname)
  f()
  ```

- Use [`locals()`](https://docs.python.org/2/library/functions.html#locals) or [`eval()`](https://docs.python.org/2/library/functions.html#eval) to resolve the function name:

  ```python
  def myFunc():
      print "hello"
  
  fname = "myFunc"
  
  f = locals()[fname]
  f()
  
  f = eval(fname)
  f()
  ```

  Note: Using [`eval()`](https://docs.python.org/2/library/functions.html#eval) is slow and dangerous. If you don’t have absolute control over the contents of the string, someone could pass a string that resulted in an arbitrary function being executed.

### [Is there an equivalent to Perl’s chomp() for removing trailing newlines from strings?](https://docs.python.org/2/faq/programming.html#id33)

Starting with Python 2.2, you can use `S.rstrip("\r\n")` to remove all occurrences of any line terminator from the end of the string `S` without removing other trailing whitespace. If the string `S` represents more than one line, with several empty lines at the end, the line terminators for all the blank lines will be removed:

```python
>>> lines = ("line 1 \r\n"
...          "\r\n"
...          "\r\n")
>>> lines.rstrip("\n\r")
'line 1 '
```

Since this is typically only desired when reading text one line at a time, using `S.rstrip()` this way works well.

For older versions of Python, there are two partial substitutes:

- If you want to remove all trailing whitespace, use the `rstrip()` method of string objects. This removes all trailing whitespace, not just a single newline.
- Otherwise, if there is only one line in the string `S`, use `S.splitlines()[0]`.
[TOC]



# A. Python Input and Output



## Input using the `input( )` function

A function is defined as a block of organized, reusable code used to perform a single, related action. Python has many built-in functions; you can also create your own. Python has an input function which lets you ask a user for some text input. You call this function to tell the program to stop and wait for the user to key in the data. In Python 2, you have a built-in function `raw_input()`, whereas in Python 3, you have `input()`. The program will resume once the user presses the ENTER or RETURN key. Look at this example to get input from the keyboard using Python 2 in the interactive mode. Your output is displayed in quotes once you hit the ENTER key.

```python
>>>raw_input()
I am learning at hackerearth   (This is where you type in)
'I am learning at hackerearth' (The interpreter showing you how the input is captured.)
```

![raw_input in python2](https://he-s3.s3.amazonaws.com/media/uploads/005c16d.gif)

In Python 3.x, you need to use input().

```python
>>> input()
I am learning at hackerearth.
'I am learning at hackerearth.'
```

![input in python3](https://he-s3.s3.amazonaws.com/media/uploads/6fd5746.gif)

You can always tell your user what to input by printing a prompt. There is no difference between `input` in Python 3 and `raw_input` in Python 2 except for the keywords.

## Output using the `print()` function

To output your data to the screen, use the `print()` function. You can write `print(argument)` and this will print the `argument` in the next line when you press the `ENTER` key.

> **Definitions to remember:** An argument is a value you pass to a function when calling it. A value is a letter or a number. A variable is a name that refers to a value. It begins with a letter. An assignment statement creates new variables and gives them values.

This syntax is valid in both Python 3.x and Python 2.x. For example, if your data is "Guido," you can put "Guido" inside the parentheses `( )` after `print`.

```python
>>> print("Guido")
Guido
```

## More on using input

To capture the input in your program, you will need a variable. A variable is a container to hold data. (You will learn more on variables in a later tutorial.) You can take the input and assign it to a variable. This is done using the `=` operator before the input keyword and then putting the variable name before the `=` operator. For example, when you give a sentence "generic input" as the input, this gets assigned to a variable, say, `my_var`. You can then print the value stored in `my_var`. Let us understand this with the following example:

```python
>>> # take an input and assign it to a variable 
>>> beautiful_number = input() # The data you key in the next line which is 6 will be assigned to beautiful_number 
6
>>> print(beautiful_number) # the next line will print the value in beautiful_number after you press enter or return
'6'
```

## Give a helpful hint during the prompt

It is often a good idea to tell the user what to input. You can do this by putting the hint in quotes inside the input parentheses. The hint will come in the next line and will wait for the user input. You can then type the input and when you hit the ENTER key, it will capture the input. In this example, "tell me a beautiful number" is the hint. This gets printed in the next line when asking for the input. If you type 6, this will be assigned to the variable `beautiful_number` which we can print later.

```python
    >>> beautiful_number = input("tell me a beautiful number ")
    tell me a beautiful number 6
    >>> print(beautiful_number)
    '6'
```

## More on using print

Say you want to print a specific string (a sequence of characters such as letters, punctuation marks, numbers, and letters) N number of times. The (asterisk) * operator performs repetition on strings. You can print "5" six times. Inside the print parentheses, put â€œ5â€ followed by `*` and the number of times you want â€œ5â€ repeated.

```python
>>> print("5"*6)
555555
```

You can separate the output using the comma delimiter. By default, this adds a space between the output items. For example, the sequence of numbers `5,6,7` separated by comma `,` gets printed with a space between one number and the next.

```
>>> print(5,6,7)
5 6 7
```

To change the output to what you want, use the keyword arguments sep and end to print ( ). When separating the output with a comma delimiter, you can also define the separation format using the `sep` keyword.

```python
>>> print('LOVE', 30, 82.2)
LOVE 30 82.2
>>> print('LOVE', 30, 82.2, sep=',')
'LOVE', 30, 82.2
```

By default, `print` goes to a new line at the end. You can change this by using the keyword `end` as shown in the example below.

```python
>>> print('LOVE', 30, 82.2, sep=',', end='!!\n')
'LOVE', 30, 82.2!!
```

For example, you can print the letters in the word "python" and all the letters will come in a new line.

```
>>> for i in "python":
...     print(i)
...
p
y
t
h
o
n
```

You can change this default implementation. You can have a colon `:` between the letters instead of a new line.

```python
>>> for i in "python":
...     print(i, end=":")
...
p:y:t:h:o:n:
```

## Printing the result of a calculation

Say you can assign the number `7` to a variable `population` and if you write the logic `population * 7` inside the parentheses of `print`, it will just do the calculation up front and print the result.

```python
>>> population = 7
>>> print("Population in 2050: ", population * 1.28) # making the calculation in place
Population in 2050: 8.96
```

# B. Python Variables

## Assignment

In Python, assignment can be done by writing the variable name followed by the value separated by an equal `=` symbol. The skeleton or pseudo-code is

```python
“Variable name” = “ value or information ”
```

In the following examples, you assign various numbers and strings to variables.

```python
>>> # assign the value 299792458 to the variable speed_of_light
>>> speed_of_light = 299792458
>>> print(speed_of_light)
299792458

>>> # assign a decimal number 3.14 to the variable pi
>>> pi = 3.14
>>> print(pi)
3.14

>>> # assign a string
>>> fav_lang = "python"
>>> print(fav_lang)
'python'
```

## Valid and invalid ways of assigning variables

Multiple words Assignment only works when the variable is a single word.

```python
>>> multiple word = "multiple word"
  File "<stdin>", line 1
    multiple word = "multiple word"
                ^
SyntaxError: invalid syntax
```

So, if you want to have more than one word in the name, the convention is to use underscore "_" in the name.

```python
>>> multiple_word = "multiple word" # note the variable name has an underscore _
>>> print(multiple_word)
multiple word
```

Do not start with a number

You cannot start a variable name with a number. The rest of the variable name can contain a number.

For example, 1var is wrong.

```python
>>> 1var = 1
  File "<stdin>", line 1
         1var = 1
       ^
SyntaxError: invalid syntax
```

But var1 is fine.

```python
>>> var1 = 1
>>> print(var1)
1
```

More points to remember while deciding a variable name

You can only include a-z, A-Z, _, and 0-9 in your variable names. Other special characters are not permitted.

For example, you cannot have hash key `#` in your variable names.

```python
>>> # a_var_containing_# will not work as it has # in the name
>>> a_var_containing_# = 1
 Traceback (most recent call last):
   File "<stdin>", line 1, in <module>
NameError: name 'a_var_containing_' is not defined

>>> # but if we remove the # then it works    
>>> a_var_containing_ = 1
>>> print(a_var_containing_)
1
```

Interestingly, you can have a variable name in your local language.

```python
 >>> 零 = 0 # chinese
>>> print(零)
0

 >>> ශුන්ය = 0 # sinhala
 >>> print(ශුන්ය)
 0
```

## More on Assignments

Python supports assigning all data structures to variables.

For example, we can assign a list to a variable like you see in the following example, where we assign the list of names, denoted by `[...]`, to the variable `fav_writers`.

```python
>>> # assigning list
>>> fav_writers = ["Mark Twain", "Fyodor Dostoyevsky"]
>>> print(fav_writers)
['Mark Twain', 'Fyodor Dostoyevsky']
```

Here is another example where you can assign dicts, shown by `{...}`, to a variable `birthdays`.

```python
>>> # assign dicts
...
>>> birthdays = {"mom": "9Jan", "daughter": "24Dec"}
>>> print(birthdays)
{'mom': '9Jan', 'daughter': '24Dec'}
```

Data structures such as lists and dicts will be discussed in later tutorials.

You can also assign functions and classes to variables.

You will know more about functions and classes in later tutorials.

```python
>>> # assigning functions
...
>>> import functools
>>> memoize = functools.lru_cache
>>> print(memoize)
<function lru_cache at 0x7fb2a6b42f28>

>>> # class assignment
...
>>> class MyClass:
...     pass
...
>>> give_me_more = MyClass()
>>> print(give_me_more)
<__main__.MyClass object at 0x7f512e65bfd0>
```

## Working with variables

Variables will support any method the underlying type supports. For example, if an integer value is stored in a variable, then the variable will support integer functions such as addition.

In the following example, you assign the number 2 to the variable `var` and then add `3` to `var`. This will print `5`, the result of 3 being added to the value stored in `var` which is `2`.

```python
>>> var = 2
>>> print(var + 3)
5
```

You can make a change in a variable and assign it to the same variable. This is done generally when some kind of data type change is done.

For example, you can take a number as input. This will take in the digit as a string. You can then take the string number and convert it to int and assign it to the same number.

```python
>>> number = input()
2
>>> type(number)
<class 'str'>
>>> number = int(number)
>>> type(number)
<class 'int'>
```

We will use a function `range(3)` which returns three values.

```python
>>> print(range(3))
[0, 1, 2]
```

Something that returns three values can be unpacked to three variables. This is like saying take whatever is in `range(3)` and instead of assigning it to a single variable, break it up and assign individual values to the three variables. This is done using a comma between the variables.

```python
>>> id1, id2, id3 = range(3)
>>> print(id1)
0
>>> print(id2)
1
>>> print(id3)
2
```

# C. Numbers

## Numbers:

**What are numbers in Python?** All numbers that are handled will belong to some type. A type is a way to define various data structures and containers and define the functionality associated with them. Types are implemented in Python as classes. If you want to verify if an integer belongs to the class `int` you can use `isinstance`. You will need to pass number as the first argument and class name as the second argument, and it will check if the object is an instance of the class. This is done by the following construct.

```python
isinstance(object, class)	
```

This checks if the object belongs to the class. We will throw more light on objects and how they belong to a class, or “object instantiation”, in the objects and class tutorial. For now, just remember that an integer number is categorized into `int` class and hence if you check whether the number `2` is an instance of the class `int`, the interpreter will return `True`.

```python
>>> isinstance(2, int)
True
```

**Numeric types and operations using numbers** There are three numeric types in Python: `int` for integers, `float` for decimal numbers, and `complex` for complex numbers. General mathematical operations such as addition, subtraction, and multiplication can be done on them.

Operations when the type are the same: When the type of the input numbers are the same the result will be a number of the same class that the input numbers belong. This is true except the case of division as will be shown in the following examples.

For example, you can assign the integers `2` and `3` to variables `int_var` and `int_var1`and then print the sum of the two.

```python
>>> # assign the integer 2 to a var int_var
>>>  int_var = 2

>>> # assign the integer 3 to a var int_var1
>>>  int_var1 = 3

>>> # print the sum of the two variables int_var and int_var1
>>>  print(int_var + int_var1)
5
```

Similarly, assign two decimal numbers to two variables, say, `float_var1` and `float_var2`, and print the sum of the two.

```python
>>> # assign a decimal number to a variable float_var
>>>  float_var = 5.6

>>> # assign another decimal number to a variable float_var1
>>>  float_var1 = 6.7

>>> # print the sum of the two decimal numbers
>>>  print(float_var + float_var1)
12.3
```

You can finally check if the same operation can be done on complex numbers. Assign two complex numbers to two variables. say, `complex_var` and `complex_var1` and print the sum of the two.

```python
>>> # assign a complex number to a variable complex_var
>>>  complex_var = (1 + 2j)

>>> # assign another complex number to a variable complex_var1
>>>  complex_var1 = (2 + 3j)

>>> # print the sum of the two complex numbers
>>>  print(complex_var + complex_var1)
(3+5j)
```

In case of division, dividing two integers will always result in a float.

```python
>>> # divide two integers and assign it to a variable result
>>> result = 3 / 2

>>> # print the result
>>> print(result)
```

1.5

```python
>>> # check if the result belongs to the class float
>>> print(isinstance(result, float))
```

True

Operations between numbers of different types: The operations work when operate of variables that are of different numeric types as well.

For example you can add two numbers, one with class `int` and another with class `float`. The result will be in float. This is done by changing the integer to a float and then adding them.

```python
>>> # add two numbers, one is int and the other is float
>>> result = 2 + 5.1

>>> # print the result, note that the result is a float
>>> print(result)
7.1

>>> # check that the result is an instance of float
>>> print(isinstance(result, float))
True
```

Other operations like adding int to complex or float to complex works in similar manner. The results of both the operations belong to the complex class.

```python
>>> # add two numbers belonging to complex and int class
>>> result = (2 + 3j) + 4

>>> # print the result
>>> print(result)
(6+3j)

>>> # check if the result is of class complex
>>> print(isinstance(result, complex))
True

>>> # add two numbers belonging to complex and float class
>>> result = (2 + 3j) + 4.12

>>> # print the result
>>> print(result)
(6.12+3j)

>>> # check if the result is of class complex
>>> print(isinstance(result, complex))
True
```

# D. String

## How to create a string and assign it to a variable

To create a string, put the sequence of characters inside either single quotes, double quotes, or triple quotes and then assign it to a variable. You can look into how variables work in Python in the Python variables tutorial.

For example, you can assign a character ‘a’ to a variable `single_quote_character`. Note that the string is a single character and it is “enclosed” by single quotes.

```python
>>> single_quote_character = 'a'
>>> print(single_quote_character)
a
>>> print(type(single_quote_character)) # check the type of the variable.
<class 'str'>
```

Similarly, you can assign a single character to a variable `double_quote_character`. Note that the string is a single character but it is “enclosed” by double quotes.

```python
>>> double_quote_character = "b"
>>> print(double_quote_character)
b
>>> print(type(double_quote_character))
<class 'str'>
```

Also check out if you can assign a sequence of characters or multiple characters to a variable. You can assign both single quote sequences and double quote sequences.

```python
>>> double_quote_multiple_characters = "aeiou"
>>> single_quote_multiple_characters = 'aeiou'
>>> print(type(double_quote_multiple_characters), type(single_quote_multiple_characters))
<class 'str'> <class 'str'>
```

Interestingly if you check the equivalence of one to the other using the keyword `is`, it returns True.

```python
>>> print(double_quote_multiple_characters is double_quote_multiple_characters)
True
```

Take a look at assignment of strings using triple quotes and check if they belong to the class `str` as well.

```python
>>> triple_quote_example = """this is a sentence written in triple quotes"""
>>> print(type(triple_quote_example))
<class 'str'>
```

In the examples above, the function `type` is used to show the underlying class that the object will belong to. Please note that all the variables that have been initiated with single, double, or triple quotes are taken as string. You can use single and double quotes for a single line of characters. Multiple lines are generally put in triple quotes.

## String common methods

- Get the index of a substring in a string.

  ```python
  # find the index of a "c" in a string "abcde"
  >>>  "abcde".index("c")
  2
  ```

2 is returned because the position of the individual letters in the strings is 0-indexed. So, index of "a" in "abcde" is 0, that of "b" is 1, and so on.

- Test if a substring is a member of a larger string. This is done using the keyword `in` and writing the test. The skeleton is shown below.

  substring in string

  ```python
  >>> # for example, test if string "i" is present in string "pythonic" at least once. "i" is present in the string. Therefore, the result should be true.
  >>>  "i" in "pythonic"
  True
  >>> # as "x" is not present in the string "pythonic" the below test should return false
  >>> "x" in "pythonic" # "x" is not present in "pythonic"
  False
  ```

- Join a list of strings using the join method. A list of strings is written by delimiting the sequence with a comma `,`, and enclosing the whole group with brackets `[...]`. For a more detailed tutorial on lists head over to the python lists tutorial. You can join a list of strings by giving the delimiter as the object on which the method `join` will act and the list of strings as the argument.

  ```python
  >>> # join a list of strings 1, 2, 3 with a space as a delimiter and 1,2,3 as the list of strings. So, the result will be the strings with spaces between them.
  >>>  combined_string = " ".join(["1", "2", "3"])
  '1 2 3'
  ```

- Break a string based on some rule. This takes in the string as the object on which the method `split` is passed using the dot operator. Splitting takes a space as the default parameter.

For example you can split a string based on the spaces between the individual values.

```python
    >>> # split the string "1 2 3" and return a list of the numbers.
    >>>  "1 2 3".split() # splitting
    ['1', '2', '3']
```

Or you can split a string based on a delimiter like `:`.

```python
     >>> “1:2:3”.split(“:”)
     [‘1’, ‘2’, ‘3’]
```



```python
>>>  lang = "python"
>>>  print(lang[0])
>>>  print(lang[2]) # access the 3rd letter
't'
>>>  print(lang[-3]) # access the third letter from the end.
'h'
```

Formatting in String:

String object can be formatted. You can use `%s` as a formatter which will enable you to insert different values into a string at runtime and thus format the string. The `%s` symbol is replaced by whatever is passed to the string.

```python
    >>>  print("I love %s in %s" % ("programming", "Python")) # templating strings
    'I love programming in Python'
```

You can also use the keyword `format`. This will enable you to set your own formatters instead of `%s`.

```python
>>> print("I love {programming} in {python}".format(programming="programming", python="Python"))
'I love programming in Python'
```

## Truth value testing of String

A string is considered to be true in Python if it is not an empty string. So, we get the following:

```python
# Test truth value of empty string
>>>  print(bool(""))
False

# Test truth value of non-empty string "x"
>>>  print(bool("x"))
True
```

# E. Conditionals

## Python Control Structures - Loops and Conditionals

You can control the flow of logic in your code through various methods.

Basic control flows

- Selection (if statements)
- Iteration (for loops)

More advanced control flows

- Procedural Abstraction (functions)
- Recursion
- Concurrency
- Exception Handling and Speculation
- Nondeterminacy

In this tutorial you will come to know:

How to have sequential, selective and iterative flows in your code. This can be achieved using the for loop. How to achieve procedural abstraction. This can be done by the use of functions.

Other topics like Recursion, Exception Handling, Concurrency will be discussed in later tutorials.

## Loops

### Working on items of the iterable

If you want to run an operation on a collection of items, then you can do it using for loops. The skeleton for using the for loop is shown below.Note that the for statement line will end with a colon `:` and the rest of the code block must be indented with a spacing of 4 spaces. An iterable is any object that can be looped on such as list, tuple, string etc.

```python
for item in iterable: # you can place any list or tuple or string in place of iterable
    # write your code here.
    pass
```

If you want to print an element of a list of fruits, you can write the following code to achieve that.

```python
>>> fruits = ["apples", "oranges", "mangoes"]
>>> for fruit in fruits:
...     print(fruit)
...
apples
oranges
mangoes
```

In the example above, note that items in the iterable (i.e fruits) will be assigned to the for loop variable (i.e fruit) during the iteration process. So, we can access the item directly.

```python
>>> fruits = ["apples", "oranges", "mangoes"]
>>> for fruit in fruits:
...     string_size = 0
...     for alphabet in fruit:
...         string_size += 1
...     print("name of fruit: %s is has length %s" % (fruit, string_size))
...
name of fruit: apples is has length 6
name of fruit: oranges is has length 7
name of fruit: mangoes is has length 7
```

### Looping on both indexes and items

In the previous section, index or the place value of the item in the iterable was not considered. However, if you are interested in working with the index, then you can call the `enumerate` function which returns a tuple of the index and the item. Taking the example above, you can print the name of the fruit and the index of the list of fruits.

```python
>>> fruits = ["apples", "oranges", "mangoes"]
>>> for index, fruit in enumerate(fruits):
...     print("index is %s" % index)
...     print("fruit is %s" % fruit)
...     print("###########################")
...
index is 0
fruit is apples
###########################
index is 1
fruit is oranges
###########################
index is 2
fruit is mangoes
###########################
```

### While statement

The while statement will execute a block of code as long as the condition is true. The skeleton of a while block is shown below.

```python
while condition:
    code_block
```

Note that similar to the for loop, the `while` statement ends with a colon `:` and the remaining code block is indented by 4 spaces. We can implement the fruit example in the while block as well, although the logic becomes a bit complicated than the for block.

```python
>>> fruits = ["apples", "oranges", "mangoes"] # get the list
>>> length = len(fruits) # get the length that will be needed for the while condition
>>> i = 0 # initialise a counter
>>> while i < length: # give the condition
...         print(fruits[i]) # the code block
...         i += 1 # increment the counter
...
apples
oranges
mangoes
```

### Nested for loops

You can have one or more nested for loops. For example, look at the following example where you can print the multiplication table. The table is shown only for 1 and 2 to save space. You can try for the remaining digits.

```python
>>> for i in range(1,3):
...         for j in range(1,3):
...                 print('%d x %d = %d' % (i, j, i*j))
...
1 x 1 = 1 
1 x 2 = 2
2 x 1 = 2
2 x 2 = 4
```

## Selection and Python If statements

### Creating if blocks

As a programmer, you will continually feel the need to control the flow of your program and let it make runtime decisions based on some condition. The is done using the if syntax. To implement this you can look at the if .. elif .. else syntax.

```python
if condition1:
        code_block1
elif condition2:
        code_block2
else:
        code_block3
```

You can try the following example to understand better .

```python
>>> num = 42
>>> if num == 42: # condition
...     print("number is 42") # direction 1
...
number is 42
```

Adding an else block:

```python
>>> num = 43
>>> if num == 42:
...         print("number is 42")
...      else:
...         print("number if not 42")
...
number if not 42
```

Now, let us add an elif block to it as well and see what happens:

```python
>>> num = 44
>>> if num == 42:
...         print("number is 42")
...      elif num == 44:
...         print("num is 44")
...      else:
...         print("num is neither 42 nor 44")
...
num is 44
```

### Nested if statements

You can have one or more nested if blocks inside if statements.

```python
>>> num = 42
>>> if num > 20:
...         if num < 50:
...                 print("num between 20 and 50")
...
num between 20 and 50
```

# F. Functions

## How to create a function:

In Python to create a function, you need to write it in the following manner. Please note that the body of the function is indented by 4 spaces.

```python
def name_of_the_function(arguments):
    '''
    doctring of the function
    note that the function block is indented by 4 spaces
    '''
    body of the function
    return the return value or expression
```

You can look at the example below where a function returns the sum of two numbers.

```python
def add_two_numbers(num1, num2):
    '''returns the sum of num1 and num2'''
    result = num1 + num2
    return result
```

Here are all the parts of the function:

**Keyword `def`**: This is the keyword used to say that a function will be defined now, and the next word that is there, is the function name.

**Function name**: This is the name that is used to identify the function. The function name comes after the `def` keyword. Function names have to be a single word. PEP8, which is a style guide for Python, recommends that in case multiple words are used, they should be in lowercase and they should be separated with an underscore. In the example above, `add_two_numbers` is the parameter name.

**Parameter list**: Parameter list are place holders that define the parameters that go into the function. The parameters help to generalise the transformation/computation/task that is needed to be done. In Python, parameters are enclosed in parentheses. In the example above, the parameters are `num1`and `num2`. You can pass as many parameters as needed to a function.

**Function docstrings**: These are optional constructs that provide a convenient way for associated documentation to the corresponding function. Docstrings are enclosed by triple quotes `'''you will write the docstring here'''`

**Function returns**: Python functions returns a value. You can define what to return by the `return` keyword. In the example above, the function returns `result`. In case you do not define a return value, the function will return None.

## How to call a function

### Call a function with a return value

To call a function means that you are telling the program to execute the function. If there is a return value defined, the function would return the value, else the function would return None. To call the function, you write the name of the function followed by parentheses. In case you need to pass parameters/arguments to the function, you write them inside the parentheses.

For example, if you had a function that added two numbers

```python
def add_two_numbers(num1, num2):
    '''returns the sum of num1 and num2'''
    result = num1 + num2
    return result
```

You would call the function like this:

```python
add_two_numbers(1, 2)
```

Note that arguments 1 and 2 have been passed. Hence, the return value will be 3. You can put any two numbers in place of 1 and 2, and it will return the corresponding sum of the two numbers. But calling a function and not doing anything with the result is meaningless, isn’t it? So you can now assign it to a variable which may be used later on. In the following example, can just printing it.

```python
>>> def add_two_numbers(num1, num2):
...     '''returns the sum of num1 and num2'''
...     result = num1 + num2
...     return result
...
>>> # call the function add_two_numbers with arguments 4 and 5 and assign it
>>> # to a variable sum_of_4_and_5
>>> sum_of_4_and_5 = add_two_numbers(4, 5)
>>>
>>> # show the value stored in sum_of_4_and_5
>>> print(sum_of_4_and_5)
9
```

### Call a function that performs a task and has no return value

In case the function is not meant to return anything and just performs some task, like committing something to a database or changing the text of some button in the user interface, then you do not need to assign the function to a variable. You can just call the function.

For example, if you had a function that prints a string

```python
def printing_side_effects():
    '''a function with side effects'''
    print('this is a function with side effects and performs some task')
```

You can just call the function and it will get executed.

```python
>>> printing_side_effects()
```

this is a function with side effects and performs some task

### How to call a function with arguments

Note that in this case you pass parameters in the order in which they are supposed to be processed. For example, if you had a function that duplicates a string by the number of times, where both the string and the number needs to be provided by the function, such as:

```python
>>> def string_multiplier(string_arg, number):
...     '''takes the string_arg and multiplies it with one more than the number'''
...     return string_arg * (number + 1)
...

>>> # passing string_arg and number and in that order...
>>> print(string_multiplier('a', 5))aaaaaa

>>> # below code will return error as the arguments are not in order...
>>> print(string_multiplier(5, 'a'))Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 3, in string_multiplier
TypeError: must be str, not int
```
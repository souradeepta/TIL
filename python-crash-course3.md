[TOC]



# G. Lists

## How to create a list:

To create a list, you separate the elements with a comma and enclose them with a bracket “[]”.

For example, you can create a list of company names containing “hackerearth”, “google”, “facebook”. This will preserve the order of the names.

```python
>>> companies = ["hackerearth", "google", "facebook"]
>>> # get the first company name
>>> print(companies[0])
'hackerearth'
>>> # get the second company name
>>> print(companies[1])
'google'
>>> # get the third company name
>>> print(companies[2])
'facebook'
>>> # try to get the fourth company name
>>> # but this will return an error as only three names
>>> # have been defined.
>>> print(companies[3])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```

Trying to access elements outside the range will give an error. You can create a two-dimensional list. This is done by nesting a list inside another list. For example, you can group “hackerearth” and “paytm” into one list and “tcs” and “cts” into another and group both the lists into another “master” list.

```python
>>> companies = [["hackerearth", "paytm"], ["tcs", "cts"]]
>>> print(companies)
[['hackerearth', 'paytm'], ['tcs', 'cts']]
```

## Methods over Python Lists

Python lists support common methods that are commonly required while working with lists. The methods change the list in place. (More on methods in the classes and objects tutorial). In case you want to make some changes in the list and keep both the old list and the changed list, take a look at the functions that are described after the methods.

### How to add elements to the list:

- list.append(elem) - will add another element to the list at the end.

  ```python
  >>> # create an empty list
  >>> companies = []
  
  >>> # add “hackerearth” to companies
  >>> companies.append(“hackerearth”)
  
  >>> # add "google" to companies
  >>> companies.append("google")
  
  >>> # add "facebook" to companies
  >>> companies.append("facebook")
  
  >>> # print the items stored in companies
  >>> print(companies)
  ['hackerearth', 'google', 'facebook']
  ```

Note the items are printed in the order in which they youre inserted.

- list.insert(index, element) - will add another element to the list at the given index, shifting the elements greater than the index one step to the right. In other words, the elements with the index greater than the provided index will increase by one.

For example, you can create a list of companies `['hackerearth', 'google', 'facebook']` and insert “airbnb” in third position which is held by “facebook”.

```python
    >>> # initialise a preliminary list of companies
    >>> companies = ['hackerearth', 'google', 'facebook']

    >>> # check what is there in position 2
    >>> print(companies[2])
    facebook

    >>> # insert “airbnb” at position 2
    >>> companies.insert(2, "airbnb")
    >>> # print the new companies list
    >>> print(companies)
    ['hackerearth', 'google', 'airbnb', 'facebook']
    >>> # print the company name at position 2
    >>> print(companies[2])
    airbnb
```

- list.extend(another_list) - will add the elements in list 2 at the end of list.

For example, you can concatenate two lists `["haskell", "clojure", "apl"]` and `["scala", "F#"]` to the same list `langs`.

```python
    >>> langs = ["haskell", "clojure", "apl"]
    >>> langs.extend(["scala", "F#"])
    >>> print(langs)
    ['haskell', 'clojure', 'apl', 'scala', 'F#']
```

- list.index(elem) - will give the index number of the element in the list.

For example, if you have a list of languages with elements `['haskell', 'clojure', 'apl', 'scala', 'F#']` and you want the index of `“scala”`, you can use the index method.

```python
    >>> index_of_scala = langs.index("scala")
    >>> print(index_of_scala)
    3
```

### How to remove elements from the list:

- list.remove(elem) - will search for the first occurrence of the element in the list and will then remove it.

For example, if you have a list of languages with elements `['haskell', 'clojure', 'apl', 'scala', 'F#']` and you want to remove scala, you can use the remove method.

```python
    >>> langs.remove("scala")
    >>> print(langs)
    ['haskell', 'clojure', 'apl', 'F#']
```

- list.pop() - will remove the last element of the list. If the index is provided, then it will remove the element at the particular index. For example, if you have a list

   Try to pop an element from a random index that exists in the list.

```python
    >>> # pop the element at index 1
    >>> some_numbers.pop(1)
    4
    >>> # check the present list
    >>> print(some_numbers)
    [5, 3]
```

### Other useful list methods

- list.sort() - will sort the list in-place.

For example, if you have an unsorted list `[4,3,5,1]`, you can sort it using the `sort` method.

```python
    >>> # initialise an unsorted list some_numbers
    >>> some_numbers = [4,3,5,1]

    >>> # sort the list
    >>> some_numbers.sort()

    >>> # print the list to see if it is sorted.
    >>> some_numbers
    [1, 3, 4, 5]
```

- list.reverse() - will reverse the list in place

For example, if you have a list `[1, 3, 4, 5]` and you need to reverse it, you can call the `reverse` method.

```python
    >>> # initialise a list of numbers that
    >>> some_numbers = [1, 3, 4, 5]

    >>> # Try to reverse the list now
    >>> some_numbers.reverse()

    >>> # print the list to check if it is really reversed.
    >>> print(some_numbers)
    [5, 4, 3, 1]
```

## Functions over Python Lists:

- You use the function “len” to get the length of the list.

For example, if you have a list of companies `['hackerearth', 'google', 'facebook']` and you want the list length, you can use the `len` function.

```python
    >>> # you have a list of companies
    >>> companies = ['hackerearth', 'google', 'facebook']

    >>> # you want the length of the list
    >>> print(len(companies))
    3
```

- If you use another function “enumerate” over a list, it gives us a nice construct to get both the index and the value of the element in the list.

For example, you have the list of companies `['hackerearth', 'google', 'facebook']` and you want the index, along with the items in the list, you can use the `enumerate` function.

```python
    >>> # loop over the companies and print both the index as youll as the name.
    >>> for indx, name in enumerate(companies):
    ...     print("Index is %s for company: %s" % (indx, name))
    ...
    Index is 0 for company: hackerearth
    Index is 1 for company: google
    Index is 2 for company: facebook
```

In this example, you use the for loop. For loops are pretty common in all programming languages that support procedural constructs. You can head over to A complete theoretical reference to loops in C to have a deeper understanding of for loops. Also look at the tutorial on loops in Python in Python Control Structures tutorial.

- sorted function will sort over the list

Similar to the sort method, you can also use the sorted function which also sorts the list. The difference is that it returns the sorted list, while the sort method sorts the list in place. So this function can be used when you want to preserve the original list as well.

```python
    >>> # initialise a list
    >>> some_numbers = [4,3,5,1]
    >>> # get the sorted list
    >>> print(sorted(some_numbers))
    [1, 3, 4, 5]
    >>> # the original list remains unchanged
    >>> print(some_numbers)
    [4, 3, 5, 1]
```

# H. Dictionary

## Creating a Dictionary

Let’s try to build a profile of three people using dictionaries. To do that you separate the key-value pairs by a colon(“:”). The keys would need to be of an immutable type, i.e., data-types for which the keys cannot be changed at runtime such as int, string, tuple, etc. The values can be of any type. Individual pairs will be separated by a comma(“,”) and the whole thing will be enclosed in curly braces(`{...}`).

For example, you can have the fields “city”, “name,” and “food” for keys in a dictionary and assign the key,value pairs to the dictionary variable person1_information.

```python
>>> person_information = {'city': 'San Francisco', 'name': 'Sam', "food": "shrimps"}
>>> type(person1_information)
<class 'dict'>
>>> print(person1_information)
{'city': 'San Francisco', 'name': 'Sam', 'food': 'shrimps'}
```

## Get the values in a Dictionary

To get the values of a dictionary from the keys, you can directly reference the keys. To do this, you enclose the key in brackets `[...]` after writing the variable name of the dictionary.

So, in the following example, a dictionary is initialized with keys “city”, “name,” and “food” and you can retrieve the value corresponding to the key “city.”

```python
>>> create a dictionary person1_information
>>> person1_information = {'city': 'San Francisco', 'name': 'Sam', "food":         "shrimps"}
>>> print the dictionary
>>> print(person1_information["city"])
San Francisco
```

You can also use the get method to retrieve the values in a dict. The only difference is that in the get method, you can set a default value. In direct referencing, if the key is not present, the interpreter throws KeyError.

```python
>>> # create a small dictionary
>>> alphabets = {1: ‘a’}
>>> # get the value with key 1
>>> print(alphabets.get(1))
'a'
>>> # get the value with key 2. Pass “default” as default. Since key 2 does not exist, you get “default” as the return value.
>>> print(alphabets.get(2, "default"))
'default'
>>> # get the value with key 2 through direct referencing 
>>> print(alphabets[2])
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
KeyError: 2
```

## Looping over dictionary

Say, you got a dictionary, and you want to print the keys and values in it. Note that the key-words `for` and `in` are used which are used when you try to loop over something. To learn more about looping please look into tutorial on looping.

```python
>>> person1_information = {'city': 'San Francisco', 'name': 'Sam', "food": "shrimps"}
>>> for k, v in person1_information.items():
...     print("key is: %s" % k)
...     print("value is: %s" % v)
...     print("###########################")
...
key is: food
value is: shrimps
###########################
key is: city
value is: San Francisco
###########################
key is: name
value is: Sam
###########################
```

## Add elements to a dictionary

You can add elements by updating the dictionary with a new key and then assigning the value to a new key.

```python
>>> # initialize an empty dictionary
>>> person1_information = {}

>>> # add the key, value information with key “city”
>>> person1_information["city"] = "San Francisco"
>>> # print the present person1_information
>>> print(person1_information)
{'city': 'San Francisco'}

>>> # add another key, value information with key “name”
>>> person1_information["name"] = "Sam"
>>> # print the present dictionary
>>> print(person1_information)
{'city': 'San Francisco', 'name': 'Sam'}

>>> # add another key, value information with key “food”
>>> person1_information["food"] = "shrimps"
>>> # print the present dictionary
>>> print(person1_information)
{'city': 'San Francisco', 'name': 'Sam', 'food': 'shrimps'}
```

Or you can combine two dictionaries to get a larger dictionary using the update method.

```python
>>> # create a small dictionary
>>> person1_information = {'city': 'San Francisco'}

>>> # print it and check the present elements in the dictionary
>>> print(person1_information) 
{'city': 'San Francisco'}

>>> # have a different dictionary
>>> remaining_information = {'name': 'Sam', "food": "shrimps"}

>>> # add the second dictionary remaining_information to personal1_information using the update method
>>> person1_information.update(remaining_information)

>>> # print the current dictionary
>>> print(person1_information)
{'city': 'San Francisco', 'name': 'Sam', 'food': 'shrimps'}
```

## Delete elements of a dictionary

To delete a key, value pair in a dictionary, you can use the `del` method.

```python
>>> # initialise a dictionary with the keys “city”, “name”, “food”
>>> person1_information = {'city': 'San Francisco', 'name': 'Sam', "food": "shrimps"}

>>> # delete the key, value pair with the key “food”
>>> del person1_information["food"]

>>> # print the present personal1_information. Note that the key, value pair “food”: “shrimps” is not there anymore.
>>> print(person1_information)
{'city': 'San Francisco', 'name': 'Sam'}
```

A disadvantage is that it gives KeyError if you try to delete a nonexistent key.

```python
>>> # initialise a dictionary with the keys “city”, “name”, “food”
>>> person1_information = {'city': 'San Francisco', 'name': 'Sam', "food": "shrimps"}

>>> # deleting a non existent key gives key error.
>>> del person1_information["non_existent_key"]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'non_existent_key'
```

So, instead of the `del` statement you can use the `pop` method. This method takes in the key as the parameter. As a second argument, you can pass the default value if the key is not present.

```python
>>> # initialise a dictionary with key, value pairs
>>> person1_information = {'city': 'San Francisco', 'name': 'Sam', "food": "shrimps"}

>>> # remove a key, value pair with key “food” and default value None
>>> print(person1_information.pop("food", None))
'Shrimps'

>>> # print the updated dictionary. Note that the key “food” is not present anymore
>>> print(person1_information)
{'city': 'San Francisco', 'name': 'Sam'}

>>> # try to delete a nonexistent key. This will return None as None is given as the default value.
>>> print(person1_information.pop("food", None))
None
```

## More facts about the Python dictionary

You can test the presence of a key using the `has_key` method.

```python
>>> alphabets = {1: ‘a’}
>>> alphabets.has_key(1)
True
>>> alphabets.has_key(2)
False
```

A dictionary in Python doesn't preserve the order. Hence, we get the following:

```python
>>> call = {'sachin': 4098, 'guido': 4139}
>>> call["snape"] = 7663
>>> call
{'snape': 7663, 'sachin': 4098, 'guido': 4139}
```

You see that even though "snape" was given later, it appeared at the top.

# I. Set

## How to create Sets

Sets can be created by calling the built-in set() function with a sequence or another iterable object.

```python
>>> #creating an empty set
>>> setA = set()
>>> print(setA)
set()

>>> # creating a set with a string.
>>> # since a string is an iterable, this will succeed.
>>> setA = set("HackerEarth")
>>> print(setA)
{'h', 'H', 't', 'k', 'e', 'c', 'E', 'a', 'r'}

>>> # creating a set with a list
>>> setA = set(["C", “C++”, “Python”])
>>> print(setA)
{'C', 'Python', 'C++'}

>>> # creating a set with a list of numbers
>>> # there are some duplicates in it.
>>> setA = set([1, 2, 3, 4, 5, 6, 7, 7, 7])
>>> print(setA)
{1, 2, 3, 4, 5, 6, 7}

>>> # creating a set with a string. The string has some repeated characters.
>>> myString = 'foobar'
>>> setA = set(myString)
>>> print(setA)
{'r', 'a', 'b', 'f', 'o'}
```

set(object) iterates over the elements present in object and adds all the unique elements to the set.

Next you will learn about different operations available for Python Sets.

For all set operations, the set created below which is a set of integers. There are some integers that are repeated here. :

```python
>>> setA = set([1, 2, 3, 4, 5, 6, 7, 7, 7])
>>> print(setA)
{1, 2, 3, 4, 5, 6, 7}
```

## Methods to change a set

### How to add elements to a set

- Python set add(element)

This will add element to a set:

For example, you can add the element 8 to the set 8

```python
    >>> setA.add(8)
    >>> print(setA)
    {1, 2, 3, 4, 5, 6, 7, 8}
```

Or you can add a tuple (9, 10) to the setA and the new set will consist of the tuple as well.

```python
    >>> setA.add((9, 10))
    >>> print(setA)
    {1, 2, 3, 4, 5, 6, 7, 8, (9, 10)}
```

- Python set update(element)

Adds element to list; it is an in-place set union operation.

For example you can pass a list to the update method and this will update the setA with the elements.

```python
    >>> # pass a list with elements 11 and 12 
    >>> setA.update([11, 12])
    >>> # check if setA is updated with the elements.
    >>> print(setA)
    {1, 2, 3, 4, 5, 6, 7, 8, 11, 12, (9, 10)}
```

Similarly you can update with a list and a new set as shown below

```python
    >>> setA.update([12, 14], {15, 16})
    >>> print(setA)
    {1, 2, 3, 4, 5, 6, 7, 8, 11, 12, 14, 15, (9, 10), 16}
```

Using add, elements can be added but not another iterable like set, list, or tuple. Update can be used to add iterable or iterables of hashable elements.

### Methods to remove elements from a set

Python set discard(element) and remove(element) Used to remove element from the set

```python
>>> # removes element 7 from set
>>> setA.discard(7)
>>> print(setA)
{1, 2, 3, 4, 5, 6, 8, 11, 12, 14, 15, (9, 10), 16}

>>> # removes element 8 from set
>>> setA.remove(8)
>>> print(setA)
{1, 2, 3, 4, 5, 6, 11, 12, 14, 15, (9, 10), 16}
```

Both discard and remove take a single argument, the element to be deleted from the set. If the value is not present, discard() does not do anything. Whereas, remove will raise a KeyError exception.

```python
>>> # discard doesn’t do anything is value to be discarded is not present
>>> setA.discard(19)
>>> print(setA)
{1, 2, 3, 4, 5, 6, 11, 12, 14, 15, (9, 10), 16}

>>> # this operation fails with an exception being raised
>>> setA.remove(19)
Traceback (most recent call last):
   File "python", line 1, in <module>
KeyError: 19
```

### Other useful set methods

- Python set copy() Creates a shallow copy of the set with which it is called

  ```python
  >>> shallow_copy_of_setA = setA.copy()
  >>> print(shallow_copy_of_setA)
  {1, 2, 3, 4, 5, 6, 11, 12, 14, 15, (9, 10), 16}
  ```

Using assignment here instead of copy() will create a pointer to the already existing set.

- Python set clear() Will remove all elements from set

  ```python
  >>> # clear the set shallow_copy_of_setA created before using copy() operation
  >>> shallow_copy_of_setA.clear()
  >>> print(shallow_copy_of_setA)
  set()
  ```

- Python set pop() Removes an arbitrary set element

  ```python
  >>> # popping an element from setA
  >>> setA.pop()
  1
  >>> # pop raises a KeyError exception if the set is empty
  >>> shallow_copy_of_setA.pop()
  Traceback (most recent call last):
    File "python", line 1, in <module>
  KeyError: 'pop from an empty set'
  ```

## Set Operations

- Set Intersection using intersection(s) Returns element present in both sets; this can also be achieved using the ampersand operator (&).

  ```python
  >>> # create a new set setB
  >>> setB= set()
  
  >>> # update setB with values
  >>> setB.update([1, 2, 3, 4, 5, 10, 15, 22])
  >>> print(setB)
  {1, 2, 3, 4, 5, 10, 15, 22}
  
  >>> # print a new set with the values present in both setA and setB
  >>> print(setA & setB)
  {2, 3, 4, 5, 15}
  
  >>> # above operation and using method name intersection shows same results
  >>> setA.intersection(setB)
  {2, 3, 4, 5, 15}
  ```

- Set Difference using difference() Returns the difference of two sets; “-” operator can also be used to find the set difference.

  ```python
  >>> # print a new set with values present in setA but not in setB
  >>> setA.difference(setB)
  {6, 11, 12, 14, (9, 10), 16}
  
  >>> # this returns empty set 
  >>> setB.difference(setA)
  set()
  ```

setB is a proper subset of setA to setB - setA is empty set.

### Other Set Operations

- Python set isdisjoint() Returns true if intersection of sets is empty otherwise false

  ```python
  >>> # returns false as both have common elements
  >>> setA.isdisjoint(setB)
  False
  
  >>> # create a new empty set setC
  >>> setC = set()
  >>> # update setC with values
  >>> setC.update([100, 99])
  
  >>> # returns true as setA and setC has no elements in common
  >>> setA.disjoint(setC)
  True
  ```

- Python set difference_update() setA.difference_update(setB) removes all elements of y from setA; ‘-=’ can be used in place of the difference_update method.

  ```python
  >>> # update setA by removing elements present in setB from setA
  >>> setA.difference_update(setB)
  >>> # check the result set
  >>> print(setA)
  {6, 11, 12, 14, (9, 10), 16}
  ```

Similarly, setA.intersection_update(setB) removes elements from setA which are not present in the intersection set of setA and setB. ‘&=’ can be used in place of the intersection_update method.

- Python set issubset() and issuperset() setA.issubset(setB) returns True if setA is subset of setB, False if not. “<=” operator can be used to test for issubset. To check for proper subset “<” is used.

  ```python
  >>> # check if setA is a subset of setB
  >>> setA.issubset(setB)
  False
  >>> # check if set B is a subset of setA
  >>> setB.issubset(setA)
  False
  ```

Let’s make setB a subset of setA by removing values 1, 10, and 22.

```python
    >>> # remove few elements to make setB a subset of setA
    >>> setB.remove(1)
    >>> setB.remove(10)
    >>> setB.remove(22)

    >>> # check the values present in setB now
    >>> print(setB)
    {2, 3, 4, 5, 15}

    >>> # issubset now returns true
    >>> setB.issubset(setA)
    True
    >>> setB < setA
    True

    >>> #setA now becomes a superset of setB
    >>> setA.issuperset(setB)
    True
```

# J. Expressions

## How to create an expressions

Python expressions only contain identifiers, literals, and operators. So, what are these?

**Identifiers**: Any name that is used to define a class, function, variable module, or object is an identifier. **Literals**: These are language-independent terms in Python and should exist independently in any programming language. In Python, there are the string literals, byte literals, integer literals, floating point literals, and imaginary literals. **Operators**: In Python you can implement the following operations using the corresponding tokens.

| Operator                 | Token |
| ------------------------ | ----- |
| add                      | +     |
| subtract                 | -     |
| multiply                 | *     |
| power                    | **    |
| Integer Division         | /     |
| remainder                | %     |
| decorator                | @     |
| Binary left shift        | <<    |
| Binary right shift       | >>    |
| and                      | &     |
| or                       | \     |
| Binary Xor               | ^     |
| Binary ones complement   | ~     |
| Less than                | <     |
| Greater than             | >     |
| Less than or equal to    | <=    |
| Greater than or equal to | >=    |
| Check equality           | ==    |
| Check not equal          | !=    |

Following are a few types of python expressions:

### List comprehension

The syntax for list comprehension is shown below:

```python
[ compute(var) for var in iterable ]
```

For example, the following code will get all the number within 10 and put them in a list.

```
>>> [x for x in range(10)]
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### Dictionary comprehension

This is the same as list comprehension but will use curly braces:

```python
{ k, v for k in iterable }
```

For example, the following code will get all the numbers within 5 as the keys and will keep the corresponding squares of those numbers as the values.

```
>>> {x:x**2 for x in range(5)}
{0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

### Generator expression

The syntax for generator expression is shown below:

```python
( compute(var) for var in iterable )
```

For example, the following code will initialize a generator object that returns the values within 10 when the object is called.

```python
>>> (x for x in range(10))
<generator object <genexpr> at 0x7fec47aee870>
>>> list(x for x in range(10))
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### Conditional Expressions

You can use the following construct for one-liner conditions:

```python
true_value if Condition else false_value
```

Example:

```
>>> x = "1" if True else "2"
>>> x
'1'
```
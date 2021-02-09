# Top 100 Python Interview Questions You Must Prepare In 2021

## **Basic Python Interview Questions**

### **Q1. What is the difference between list and tuples in Python?**

| **LIST**                                  | **TUPLES**                                                   |
| ----------------------------------------- | ------------------------------------------------------------ |
| Lists are mutable i.e they can be edited. | Tuples are immutable (tuples are lists which can’t be edited). |
| Lists are slower than tuples.             | Tuples are faster than list.                                 |
| Syntax: list_1 = [10, ‘Chelsea’, 20]      | Syntax: tup_1 = (10, ‘Chelsea’ , 20)                         |

### **Q2. What are the key features of Python?**

- Python is an **interpreted** language. That means that, unlike languages like *C* and its variants, Python does not need to be compiled before it is run. Other interpreted languages include *PHP* and *Ruby*.
- Python is **dynamically typed**, this means that you don’t need to state the types of variables when you declare them or anything like that. You can do things like `x=111 `and then `x="I'm a string"` without error
- Python is well suited to [**object orientated programming**](https://www.edureka.co/blog/python-class/) in that it allows the definition of classes along with composition and inheritance. Python does not have access specifiers (like C++’s `public`, `private`).
- In Python, [**functions**](https://www.edureka.co/blog/python-functions) are **first-class objects**. This means that they can be assigned to variables, returned from other functions and passed into functions. Classes are also first class objects
- **Writing Python code is quick** but running it is often slower than compiled languages. Fortunately，Python allows the inclusion of C-based extensions so bottlenecks can be optimized away and often are. The [numpy](https://www.edureka.co/blog/python-numpy-tutorial/) package is a good example of this, it’s really quite quick because a lot of the number-crunching it does isn’t actually done by Python
- Python finds **use in many spheres** – web applications, automation, scientific modeling, big data applications and many more. It’s also often used as “glue” code to get other languages and components to play nice.

### **Q3. What type of language is python? Programming or scripting?**

***Ans:*** Python is capable of scripting, but in general sense, it is considered as a general-purpose programming language. To know more about Scripting, you can refer to the [Python Scripting Tutorial](https://youtu.be/9F6zAuYtuFw).

### **Q4.Python an interpreted language. Explain.**

***Ans:*** An interpreted language is any programming language which is not in machine-level code before runtime. Therefore, Python is an interpreted language.

### **Q5.What is pep 8?**

***Ans:*** PEP stands for **Python Enhancement Proposal.** It is a set of rules that specify how to format Python code for maximum readability.

### **Q6. How is memory managed in Python?**

**Ans:** Memory is managed in Python in the following ways:

1. Memory management in python is managed by ***Python private heap space\***. All Python objects and data structures are located in a private heap. The programmer does not have access to this private heap. The python interpreter takes care of this instead.
2. The allocation of heap space for Python objects is done by Python’s memory manager. The core API gives access to some tools for the programmer to code.
3. Python also has an inbuilt garbage collector, which recycles all the unused memory and so that it can be made available to the heap space.

### **Q7. What is namespace in Python?**

***Ans:*** A namespace is a naming system used to make sure that names are unique to avoid naming conflicts.

### **Q8. What is PYTHONPATH?**

***Ans:*** It is an environment variable which is used when a module is imported. Whenever a module is imported, PYTHONPATH is also looked up to check for the presence of the imported modules in various directories. The interpreter uses it to determine which module to load.

### **Q9. What are python modules? Name some commonly used built-in modules in Python?**

***Ans:*** Python modules are files containing Python code. This code can either be functions classes or variables. A Python module is a .py file containing executable code.

Some of the commonly used built-in modules are:

- os
- sys
- math
- random
- data time
- JSON

### **Q10.What are local variables and global variables in Python?**

**Global Variables:**

Variables declared outside a function or in global space are called global variables. These variables can be accessed by any function in the program.

**Local Variables:**

Any variable declared inside a function is known as a local variable. This variable is present in the local space and not in the global space.

**Example:**

```python
a=2
def add():
    b=3
    c=a+b
print(c)
add()
```

**Output:** 5

When you try to access the local variable outside the function add(), it will throw an error.

### **Q11. Is python case sensitive?**

***Ans:*** Yes. Python is a case sensitive language.

### **Q12.What is type conversion in Python?**

***Ans:*** Type conversion refers to the conversion of one data type iinto another.

**int()** – converts any data type into integer type

**float()** – converts any data type into float type

**ord()** – converts characters into integer

**hex(**) – converts integers to hexadecimal

**oct()** – converts integer to octal

**tuple() –** This function is used to convert to a tuple.

**set() –** This function returns the type after converting to set.

**list() –** This function is used to convert any data type to a list type.

**dict() –** This function is used to convert a tuple of order (key,value) into a dictionary.

**str() –** Used to convert integer into a string.

**complex(real,imag) –** This functionconverts real numbers to complex(real,imag) number.

**Q13. How to install Python on Windows and set path variable?**

***Ans:*** To install Python on Windows, follow the below steps:

- Install python from this link: https://www.python.org/downloads/
- After this, install it on your PC. Look for the location where PYTHON has been installed on your PC using the following command on your command prompt: cmd python. 
- Then go to advanced system settings and add a new variable and name it as PYTHON_NAME and paste the copied path.
- Look for the path variable, select its value and select ‘edit’.
- Add a semicolon towards the end of the value if it’s not present and then type %PYTHON_HOME% 

### **Q14. Is indentation required in python?**

***Ans:*** Indentation is necessary for Python. It specifies a block of code. All code within loops, classes, functions, etc is specified within an indented block. It is usually done using four space characters. If your code is not indented necessarily, it will not execute accurately and will throw errors as well.

### **Q15. What is the difference between Python Arrays and lists?**

***Ans:*** Arrays and lists, in Python, have the same way of storing data. But, arrays can hold only a single data type elements whereas lists can hold any data type elements.

**Example:**

```python
import array as arr
My_Array=arr.array('i',[1,2,3,4])
My_list=[1,'abc',1.20]
print(My_Array)
print(My_list)
```

**Output:**

array(‘i’, [1, 2, 3, 4]) [1, ‘abc’, 1.2]

### **Q16. What are functions in Python?**

***Ans:*** A function is a block of code which is executed only when it is called. To define a [Python function](https://www.edureka.co/blog/python-functions), the **def** keyword is used.

**Example:**

```python
def Newfunc():
	print("Hi, Welcome to Edureka")
Newfunc(); #calling the function
```

**Output:** Hi, Welcome to Edureka

### **Q17.What is __init__?**

***Ans:*** __init__ is a method or constructor in [Python](https://www.edureka.co/blog/python-programming-language). This method is automatically called to allocate memory when a new object/ instance of a class is created. All classes have the __init__ method.

Here is an example of how to use it.

```python
class Employee:
    def __init__(self, name, age,salary):
        self.name = name
        self.age = age
        self.salary = 20000
E1 = Employee("XYZ", 23, 20000)
# E1 is the instance of class Employee.
#__init__ allocates memory for E1. 
print(E1.name)
print(E1.age)
print(E1.salary)
```

**Output:**

XYZ

23

20000

### **Q18.What is a lambda function?**

***Ans:*** An anonymous function is known as a lambda function. This function can have any number of parameters but, can have just one statement.

**Example:**

```python
a = lambda x,y : x+y
print(a(5, 6))
```

**Output:** 11

### **Q19. What is self in Python?** 

***Ans:*** Self is an instance or an object of a class. In Python, this is explicitly included as the first parameter. However, this is not the case in Java where it’s optional. It helps to differentiate between the methods and attributes of a class with local variables.

The self variable in the init method refers to the newly created object while in other methods, it refers to the object whose method was called.

### **Q20.** **How does break, continue and pass work?**

| Break    | Allows loop termination when some condition is met and the control is transferred to the next statement. |
| -------- | ------------------------------------------------------------ |
| Continue | Allows skipping some part of a loop when some specific condition is met and the control is transferred to the beginning of the loop |
| Pass     | Used when you need some block of code syntactically, but you want to skip its execution. This is basically a null operation. Nothing happens when this is executed. |

### **Q21. What does [::-1} do?**

***Ans:*** [::-1] is used to reverse the order of an array or a sequence.

*For example:*

```python
import array as arr
My_Array=arr.array('i',[1,2,3,4,5])
My_Array[::-1]
```

**Output**: array(‘i’, [5, 4, 3, 2, 1])

[::-1] reprints a reversed copy of ordered data structures such as an array or a list. the original array or list remains unchanged.

 

### **Q22. How can you randomize the items of a list in place in Python?**

**Ans:** Consider the example shown below:

```python
from random import shuffle
x = ['Keep', 'The', 'Blue', 'Flag', 'Flying', 'High']
shuffle(x)
print(x)
```

``The output of the following code is as below.

```
['Flying', 'Keep', 'Blue', 'High', 'The', 'Flag']
```

### **Q23. What are python iterators?**

***Ans:*** Iterators are objects which can be traversed though or iterated upon.

### **Q24. How can you generate random numbers in Python?**

**Ans:** Random module is the standard module that is used to generate a random number. The method is defined as:

```python
import random
random.random
```

The statement random.random() method return the floating point number that is in the range of [0, 1). The function generates random float numbers. The methods that are used with the random class are the bound methods of the hidden instances. The instances of the Random can be done to show the multi-threading programs that creates a different instance of individual threads. The other random generators that are used in this are:

1. `randrange(a, b)`: it chooses an integer and define the range in-between [a, b). It returns the elements by selecting it randomly from the range that is specified. It doesn’t build a range object.
2. `uniform(a, b)`: it chooses a floating point number that is defined in the range of [a,b).It returns the floating point number
3. `normalvariate(mean, sdev)`: it is used for the normal distribution where the mu is a mean and the sdev is a sigma that is used for standard deviation.
4. The Random class that is used and instantiated creates independent multiple random number generators.

## **Python Interview Questions**

### **Q25. What is the difference between range & xrange?**

**Ans:** For the most part, xrange and range are the exact same in terms of functionality. They both provide a way to generate a list of integers for you to use, however you please. The only difference is that range returns a Python list object and x range returns an xrange object.

This means that xrange doesn’t actually generate a static list at run-time like range does. It creates the values as you need them with a special technique called yielding. This technique is used with a type of object known as generators. That means that if you have a really gigantic range you’d like to generate a list for, say one billion, xrange is the function to use.

This is especially true if you have a really memory sensitive system such as a cell phone that you are working with, as range will use as much memory as it can to create your array of integers, which can result in a Memory Error and crash your program. It’s a memory hungry beast.

### **Q26. How do you write comments in python?**

***Ans:*** Comments in Python start with a # character. However, alternatively at times, commenting is done using docstrings(strings enclosed within triple quotes).

**Example:**

```python
#Comments in Python start like this
print("Comments in Python start with a #")
```

**Output:** Comments in Python start with a #

### **Q27. What is pickling and unpickling?**

**Ans:** Pickle module accepts any Python object and converts it into a string representation and dumps it into a file by using dump function, this process is called pickling. While the process of retrieving original Python objects from the stored string representation is called unpickling.

### **Q28. What are the generators in python?**

***Ans:*** Functions that return an iterable set of items are called generators.

### **Q29. How will you capitalize the first letter of string?**

***Ans:*** In Python, the capitalize() method capitalizes the first letter of a string. If the string already consists of a capital letter at the beginning, then, it returns the original string.

### **Q30. How will you convert a string to all lowercase?**

***Ans:*** To convert a string to lowercase, lower() function can be used.

**Example:**

```python
stg='ABCD'
print(stg.lower())
```

**Output:** abcd

### **Q31. How to comment multiple lines in python?**

***Ans:*** Multi-line comments appear in more than one line. All the lines to be commented are to be prefixed by a #. You can also a very good **shortcut method to comment multiple lines**. All you need to do is hold the ctrl key and **left click** in every place wherever you want to include a # character and type a # just once. This will comment all the lines where you introduced your cursor.

### **Q32.What are docstrings in Python?**

***Ans:*** Docstrings are not actually comments, but, they are ***documentation strings***. These docstrings are within triple quotes. They are not assigned to any variable and therefore, at times, serve the purpose of comments as well.

**Example:**

```python
"""
Using docstring as a comment.
This code divides 2 numbers
"""
x=8
y=4
z=x/y
print(z)
```

**Output:** 2.0

### **Q33. What is the purpose of is, not and in operators?**

***Ans:*** Operators are special functions. They take one or more values and produce a corresponding result.

is: returns true when 2 operands are true (Example: “a” is ‘a’)

not: returns the inverse of the boolean value

in: checks if some element is present in some sequence

### **Q34. What is the usage of help() and dir() function in Python?**

**Ans:** Help() and dir() both functions are accessible from the Python interpreter and used for viewing a consolidated dump of built-in functions. 

1. Help() function: The help() function is used to display the documentation string and also facilitates you to see the help related to modules, keywords, attributes, etc.
2. Dir() function: The dir() function is used to display the defined symbols.

### **Q35. Whenever Python exits, why isn’t all the memory de-allocated?**

**Ans:** 

1. Whenever Python exits, especially those Python modules which are having circular references to other objects or the objects that are referenced from the global namespaces are not always de-allocated or freed.
2. It is impossible to de-allocate those portions of memory that are reserved by the C library.
3. On exit, because of having its own efficient clean up mechanism, Python would try to de-allocate/destroy every other object.

### **Q36. What is a dictionary in Python?**

**Ans:** The built-in datatypes in Python is called dictionary. It defines one-to-one relationship between keys and values. Dictionaries contain pair of keys and their corresponding values. Dictionaries are indexed by keys.

Let’s take an example:

The following example contains some keys. Country, Capital & PM. Their corresponding values are India, Delhi and Modi respectively.

```python
dict={'Country':'India','Capital':'Delhi','PM':'Modi'}
```

### **Q37.** **How can the ternary operators be used in python?**

**Ans:** The Ternary operator is the operator that is used to show the conditional statements. This consists of the true or false values with a statement that has to be evaluated for it.

**Syntax**:

The Ternary operator will be given as:
[on_true] if [expression] else [on_false]

```python
x, y = 25, 50 
big = x if x < y else y
```

**Example:**

The expression gets evaluated like if x<y else y, in this case if x<y is true then the value is returned as big=x and if it is incorrect then big=y will be sent as a result.

### **Q38. What does this mean: \*args, \**kwargs? And why would we use it?**

**Ans:** We use *args when we aren’t sure how many arguments are going to be passed to a function, or if we want to pass a stored list or tuple of arguments to a function. \*\*kwargs is used when we don’t know how many keyword arguments will be passed to a function, or it can be used to pass the values of a dictionary as keyword arguments. The identifiers args and kwargs are a convention, you could also use *bob and **billy but that would not be wise.

### **Q39. What does len() do?**

***Ans:*** It is used to determine the length of a string, a list, an array, etc.

**Example:**

```python
stg='ABCD'
len(stg) 
```



## **Python Interview Questions**

### **Q40. Explain split(), sub(), subn() methods of “re” module in Python.**

**Ans:** To modify the strings, Python’s “re” module is providing 3 methods. They are:

- split() – uses a regex pattern to “split” a given string into a list.
- sub() – finds all substrings where the regex pattern matches and then replace them with a different string
- subn() – it is similar to sub() and also returns the new string along with the no. of replacements.

### **Q41.** **What are negative indexes and why are they used?**

**Ans:** The sequences in Python are indexed and it consists of the positive as well as negative numbers. The numbers that are positive uses ‘0’ that is uses as first index and ‘1’ as the second index and the process goes on like that.

The index for the negative number starts from ‘-1’ that represents the last index in the sequence and ‘-2’ as the penultimate index and the sequence carries forward like the positive number.

The negative index is used to remove any new-line spaces from the string and allow the string to except the last character that is given as S[:-1]. The negative index is also used to show the index to represent the string in correct order.

### **Q42.** **What are Python packages?**

***Ans:*** Python packages are namespaces containing multiple modules.

### **Q43.How can files be deleted in Python?**

***Ans:*** To delete a file in Python, you need to import the OS Module. After that, you need to use the os.remove() function.

**Example:**

```python
import os
os.remove("xyz.txt")
```

### **Q44. What are the built-in types of python?**

***Ans:*** Built-in types in Python are as follows –

- Integers
- Floating-point
- Complex numbers
- Strings
- Boolean
- Built-in functions

### **Q45. What advantages do NumPy arrays offer over (nested) Python lists?**

**Ans:** 

1. Python’s lists are efficient general-purpose containers. They support (fairly) efficient insertion, deletion, appending, and concatenation, and Python’s list comprehensions make them easy to construct and manipulate.
2. They have certain limitations: they don’t support “vectorized” operations like elementwise addition and multiplication, and the fact that they can contain objects of differing types mean that Python must store type information for every element, and must execute type dispatching code when operating on each element.
3. [NumPy](https://www.edureka.co/blog/python-numpy-tutorial/) is not just more efficient; it is also more convenient. You get a lot of vector and matrix operations for free, which sometimes allow one to avoid unnecessary work. And they are also efficiently implemented.
4. NumPy array is faster and You get a lot built in with NumPy, FFTs, convolutions, fast searching, basic statistics, linear algebra, [histograms](https://www.edureka.co/blog/python-matplotlib-tutorial/#Histogram), etc. 

### **Q46.** **How to add values to a python array?**

***Ans:*** Elements can be added to an array using the **append()**, **extend()** and the **insert (i,x)** functions.

**Example:**

```python
a=arr.array('d', [1.1 , 2.1 ,3.1] )
a.append(3.4)
print(a)
a.extend([4.5,6.3,6.8])
print(a)
a.insert(2,3.8)
print(a)
```

**Output:**

array(‘d’, [1.1, 2.1, 3.1, 3.4])

array(‘d’, [1.1, 2.1, 3.1, 3.4, 4.5, 6.3, 6.8])

array(‘d’, [1.1, 2.1, 3.8, 3.1, 3.4, 4.5, 6.3, 6.8])

### **Q47. How to remove values to a python array?**

***Ans:*** Array elements can be removed using **pop()** or **remove()** method. The difference between these two functions is that the former returns the deleted value whereas the latter does not.

**Example:**

```python
a=arr.array('d', [1.1, 2.2, 3.8, 3.1, 3.7, 1.2, 4.6])
print(a.pop())
print(a.pop(3))
a.remove(1.1)
print(a)
```

**Output:**

4.6

3.1

array(‘d’, [2.2, 3.8, 3.7, 1.2])

### **Q48.** **Does Python have OOps concepts?**

***Ans:*** Python is an object-oriented programming language. This means that any program can be solved in python by creating an object model. However, Python can be treated as procedural as well as structural language.

### **Q49. What is the difference between deep and shallow copy?**

***Ans:** Shallow copy* is used when a new instance type gets created and it keeps the values that are copied in the new instance. Shallow copy is used to copy the reference pointers just like it copies the values. These references point to the original objects and the changes made in any member of the class will also affect the original copy of it. Shallow copy allows faster execution of the program and it depends on the size of the data that is used.

*Deep copy* is used to store the values that are already copied. Deep copy doesn’t copy the reference pointers to the objects. It makes the reference to an object and the new object that is pointed by some other object gets stored. The changes made in the original copy won’t affect any other copy that uses the object. Deep copy makes execution of the program slower due to making certain copies for each object that is been called.

### **Q50. How is Multithreading achieved in Python?**

**Ans:** 

1. Python has a multi-threading package but if you want to multi-thread to speed your code up, then it’s usually not a good idea to use it.
2. Python has a construct called the Global Interpreter Lock (GIL). The GIL makes sure that only one of your ‘threads’ can execute at any one time. A thread acquires the GIL, does a little work, then passes the GIL onto the next thread.
3. This happens very quickly so to the human eye it may seem like your threads are executing in parallel, but they are really just taking turns using the same CPU core.
4. All this GIL passing adds overhead to execution. This means that if you want to make your code run faster then using the threading package often isn’t a good idea.

### **Q51.** **What is the process of compilation and linking in python?**

**Ans:** The compiling and linking allows the new extensions to be compiled properly without any error and the linking can be done only when it passes the compiled procedure. If the dynamic loading is used then it depends on the style that is being provided with the system. The python interpreter can be used to provide the dynamic loading of the configuration setup files and will rebuild the interpreter.

The steps that are required in this as:

1. Create a file with any name and in any language that is supported by the compiler of your system. For example file.c or file.cpp
2. Place this file in the Modules/ directory of the distribution which is getting used.
3. Add a line in the file Setup.local that is present in the Modules/ directory.
4. Run the file using spam file.o
5. After a successful run of this rebuild the interpreter by using the make command on the top-level directory.
6. If the file is changed then run rebuildMakefile by using the command as ‘make Makefile’.

### **Q52.** **What are Python libraries? Name a few of them. **

Python libraries are a collection of Python packages. Some of the majorly used python libraries are – [Numpy](https://www.edureka.co/blog/python-numpy-tutorial/), [Pandas](https://www.edureka.co/blog/python-pandas-tutorial/), [Matplotlib](https://www.edureka.co/blog/python-matplotlib-tutorial/), [Scikit-learn](https://www.edureka.co/blog/scikit-learn-machine-learning/) and many more.

### **Q53.** **What is split used for?**

The split() method is used to separate a given string in Python.

**Example:**

```python
a="edureka python"
print(a.split())
```

**Output:** [‘edureka’, ‘python’]

**Q54.** **How to import modules in python?**

Modules can be imported using the **import** keyword. You can import modules in three ways-

**Example:**

```python
import array           #importing using the original module name
import array as arr    # importing using an alias name
from array import *    #imports everything present in the array module
```

## **OOPS** **Python Interview Questions**

### **Q55. Explain Inheritance in Python with an example.**

**Ans:** Inheritance allows One class to gain all the members(say attributes and methods) of another class. Inheritance provides code reusability, makes it easier to create and maintain an application. The class from which we are inheriting is called super-class and the class that is inherited is called a derived / child class.

They are different types of inheritance supported by Python:

1. Single Inheritance – where a derived class acquires the members of a single super class.
2. Multi-level inheritance – a derived class d1 in inherited from base class base1, and d2 are inherited from base2.
3. Hierarchical inheritance – from one base class you can inherit any number of child classes
4. Multiple inheritance – a derived class is inherited from more than one base class.

### **Q56. How are classes created in Python?** 

**Ans:** Class in Python is created using the **class** keyword.

**Example:**

```python
class Employee:
def __init__(self, name):
self.name = name
E1=Employee("abc")
print(E1.name)
```

**Output:** abc



### **Q57.** **What is monkey patching in Python?**

**Ans:** In Python, the term monkey patch only refers to dynamic modifications of a class or module at run-time.

Consider the below example:

```python
# m.py
class MyClass:
    def f(self):
    print "f()"
```

We can then run the monkey-patch testing like this:

```python
import m
def monkey_f(self):
	print "monkey_f()"
 
m.MyClass.f = monkey_f
obj = m.MyClass()
obj.f()
```

The output will be as below:

```
monkey_f()
```

As we can see, we did make some changes in the behavior of *f()* in *MyClass* using the function we defined, *monkey_f()*, outside of the module *m*.

### **Q58. Does python support multiple inheritance?**

**Ans:** Multiple inheritance means that a class can be derived from more than one parent classes. Python does support multiple inheritance, unlike Java.

### **Q****59. What is Polymorphism in Python?**

**Ans:** Polymorphism means the ability to take multiple forms. So, for instance, if the parent class has a method named ABC then the child class also can have a method with the same name ABC having its own parameters and variables. Python allows polymorphism.

### **Q60. Define encapsulation in Python?**

**Ans:** Encapsulation means binding the code and the data together. A Python class in an example of encapsulation.

### **Q61. How do you do data abstraction in Python?**

**Ans:** Data Abstraction is providing only the required details and hiding the implementation from the world. It can be achieved in Python by using interfaces and abstract classes.

### **Q62.Does python make use of access specifiers?**

**Ans:** Python does not deprive access to an instance variable or function. Python lays down the concept of prefixing the name of the variable, function or method with a single or double underscore to imitate the behavior of protected and private access specifiers. 

### **Q63. How to create an empty class in Python?** 

**Ans:** An empty class is a class that does not have any code defined within its block. It can be created using the *pass* keyword. However, you can create objects of this class outside the class itself. IN PYTHON THE PASS command does nothing when its executed. it’s a null statement. 

**For example-**

```python
class a:
  pass
obj=a()
obj.name="xyz"
print("Name = ",obj.name)
```

**Output:** 

```
Name =  xyz
```

### **Q64. What does an object() do?**

**Ans:** It returns a featureless object that is a base for all classes. Also, it does not take any parameters.



## **Basic Python Programs – Python Interview Questions**

### 

### **Q65. Write a program in Python to execute the Bubble sort algorithm.**

```python
def bubbleSort(arr): 
    n = len(arr) 
  
    # Traverse through all array elements 
    for i in range(n-1): 
    # range(n) also work but outer loop will repeat one time more than needed. 
  
        # Last i elements are already in place 
        for j in range(0, n-i-1): 
  
            # traverse the array from 0 to n-i-1 
            # Swap if the element found is greater 
            # than the next element 
            if arr[j] > arr[j+1] : 
                arr[j], arr[j+1] = arr[j+1], arr[j] 
  
# Driver code to test above 
arr = [64, 34, 25, 12, 22, 11, 90] 
  
bubbleSort(arr) 
  
print ("Sorted array is:") 
for i in range(len(arr)): 
    print ("%d" %arr[i]),  
```

**Output:** [3, 5, 6, 7, 32, 54, 87]

## **Python Interview Questions**

### **Q66. Write a program in Python to produce Star triangle.**

```python

def pyfunc(r):
    for x in range(r):
        print(' '*(r-x-1)+'*'*(2*x+1))    
pyfunc(9)
```

**Output:
**

```
        *
       ***
      *****
     *******
    *********
   ***********
  *************
 ***************
*****************
```

### **Q67. Write a program to produce Fibonacci series in Python.**

```python
ef fibonacci(n):
    a = 0
    b = 1
     
    # Check is n is less
    # than 0
    if n < 0:
        print("Incorrect input")
         
    # Check is n is equal
    # to 0
    elif n == 0:
        return 0
       
    # Check if n is equal to 1
    elif n == 1:
        return b
    else:
        for i in range(1, n):
            c = a + b
            a = b
            b = c
        return b
```

 

**Output:** Enter the terms 5 0 1 1 2 3

### **Q68. Write a program in Python to check if a number is prime.**

```python
# A optimized school method based 
# Python3 program to check 
# if a number is prime 
 
 
def isPrime(n) : 
 
    # Corner cases 
    if (n <= 1) : 
        return False
    if (n <= 3) : 
        return True
 
    # This is checked so that we can skip 
    # middle five numbers in below loop 
    if (n % 2 == 0 or n % 3 == 0) : 
        return False
 
    i = 5
    while(i * i <= n) : 
        if (n % i == 0 or n % (i + 2) == 0) : 
            return False
        i = i + 6
 
    return True
```

**Output:**

enter number 3

Prime

### **Q69. Write a program in Python to check if a sequence is a Palindrome.**

```python
a=input("enter sequence")
b=a[::-1]
if a==b:
  print("palindrome")
else:
  print("Not a Palindrome")
```

**Output:**

enter sequence 323 palindrome

### **Q70. Write a one-liner that will count the number of capital letters in a file. Your code should work even if the file is too big to fit in memory.**

**Ans:** Let us first write a multiple line solution and then convert it to one-liner code.

```python
with open(SOME_LARGE_FILE) as fh:
count = 0
text = fh.read()
for character in text:
    if character.isupper():
count += 1
```

We will now try to transform this into a single line.

```python
count sum(1 for line in fh for character in line if character.isupper())
```

### **Q71. Write a sorting algorithm for a numerical dataset in Python.**

**Ans:** The following code can be used to sort a list in Python:

```python
list = ["1", "4", "0", "6", "9"]
list = [int(i) for i in list]
list.sort()
print (list)
```

### **Q72. Looking at the below code, write down the final values of A0, A1, …An.**

```python
A0 = dict(zip(('a','b','c','d','e'),(1,2,3,4,5)))
A1 = range(10)
A2 = sorted([i for i in A1 if i in A0])
A3 = sorted([A0[s] for s in A0])
A4 = [i for i in A1 if i in A3]
A5 = {i:i*i for i in A1}
A6 = [[i,i*i] for i in A1]
print(A0,A1,A2,A3,A4,A5,A6)
```

**Ans:** The following will be the final outputs of A0, A1, … A6



```python
A0 = {'a': 1, 'c': 3, 'b': 2, 'e': 5, 'd': 4} # the order may vary
A1 = range(0, 10) 
A2 = []
A3 = [1, 2, 3, 4, 5]
A4 = [1, 2, 3, 4, 5]
A5 = {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25, 6: 36, 7: 49, 8: 64, 9: 81}
A6 = [[0, 0], [1, 1], [2, 4], [3, 9], [4, 16], [5, 25], [6, 36], [7, 49], [8, 64], [9, 81]]
```

## **Python Libraries – Python Interview Questions**

### **Q73. Explain what Flask is and its benefits?**

**Ans:** Flask is a web microframework for Python based on “Werkzeug, Jinja2 and good intentions” BSD license. Werkzeug and Jinja2 are two of its dependencies. This means it will have little to no dependencies on external libraries. It makes the framework light while there is a little dependency to update and fewer security bugs.

A session basically allows you to remember information from one request to another. In a flask, a session uses a signed cookie so the user can look at the session contents and modify. The user can modify the session if only it has the secret key Flask.secret_key.

### **Q74. Is Django better than Flask?**

**Ans:** Django and Flask map the URL’s or addresses typed in the web browsers to functions in Python. 

Flask is much simpler compared to Django but, Flask does not do a lot for you meaning you will need to specify the details, whereas Django does a lot for you wherein you would not need to do much work. [Django](https://www.edureka.co/blog/django-tutorial/) consists of prewritten code, which the user will need to analyze whereas Flask gives the users to create their own code, therefore, making it simpler to understand the code. Technically both are equally good and both contain their own pros and cons.

### **Q75. Mention the differences between Django, Pyramid and Flask.**

**Ans:** 

- Flask is a “microframework” primarily build for a small application with simpler requirements. In flask, you have to use external libraries. Flask is ready to use.
- Pyramid is built for larger applications. It provides flexibility and lets the developer use the right tools for their project. The developer can choose the database, URL structure, templating style and more. Pyramid is heavy configurable.
- Django can also be used for larger applications just like Pyramid. It includes an ORM.

### **Q76****. Discuss Django architecture.**

**Ans:** Django MVT Pattern:

![Django Architecture - Python Interview Questions - Edureka](https://www.edureka.co/blog/wp-content/uploads/2017/06/Django-Architecture-Python-Interview-Questions-Edureka.png)**Figure:** *Python Interview Questions – Django Architecture*

The developer provides the Model, the view and the template then just maps it to a URL and Django does the magic to serve it to the user.

### **Q77. Explain how you can set up the Database in Django.**

**Ans:** You can use the command edit mysite/setting.py, it is a normal python module with module level representing Django settings.

Django uses SQLite by default; it is easy for Django users as such it won’t require any other type of installation. In the case your database choice is different that you have to the following keys in the DATABASE ‘default’ item to match your database connection settings.

- **Engines**: you can change the database by using ‘django.db.backends.sqlite3’ , ‘django.db.backeneds.mysql’, ‘django.db.backends.postgresql_psycopg2’, ‘django.db.backends.oracle’ and so on
- **Name**: The name of your database. In the case if you are using SQLite as your database, in that case, database will be a file on your computer, Name should be a full absolute path, including the file name of that file.
- If you are not choosing SQLite as your database then settings like Password, Host, User, etc. must be added.

Django uses SQLite as a default database, it stores data as a single file in the filesystem. If you do have a database server—PostgreSQL, MySQL, Oracle, MSSQL—and want to use it rather than SQLite, then use your database’s administration tools to create a new database for your Django project. Either way, with your (empty) database in place, all that remains is to tell Django how to use it. This is where your project’s settings.py file comes in.

We will add the following lines of code to the *setting.py* file:

```python
DATABASES = {
     'default': {
          'ENGINE' : 'django.db.backends.sqlite3',
          'NAME' : os.path.join(BASE_DIR, 'db.sqlite3'),
     }
}
```

### **Q78. Give an example how you can write a VIEW in Django?**

**Ans:** This is how we can use write a view in Django:

```python
from django.http import HttpResponse
import datetime
 
def Current_datetime(request):
     now = datetime.datetime.now()
     html = "&amp;amp;lt;html&amp;amp;gt;&amp;amp;lt;body&amp;amp;gt;It is now %s&amp;amp;lt;/body&amp;amp;gt;&amp;amp;lt;/html&amp;amp;gt; % now
     return HttpResponse(html)
```

*Returns the current date and time, as an HTML document*

### **Q79. Mention what the Django templates consist of.**

**Ans:** The template is a simple text file. It can create any text-based format like XML, CSV, HTML, etc. A template contains variables that get replaced with values when the template is evaluated and tags (% tag %) that control the logic of the template.

![Django Template - Python Interview Questions - Edureka](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2017/06/Django-Template-Python-Interview-Questions-Edureka.png)

![Django Template - Python Interview Questions - Edureka](data:image/gif;base64,R0lGODlhAQABAIAAAMLCwgAAACH5BAAAAAAALAAAAAABAAEAAAICRAEAOw==)**Figure:** *Python Interview Questions – Django Template*

### **Q80. Explain the use of session in Django framework?**

**Ans:** Django provides a session that lets you store and retrieve data on a per-site-visitor basis. Django abstracts the process of sending and receiving cookies, by placing a session ID cookie on the client side, and storing all the related data on the server side.

![Django Framework - Python Interview Questions - Edureka](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2017/06/Django-Framework-Python-Interview-Questions-Edureka.png)

![Django Framework - Python Interview Questions - Edureka](data:image/gif;base64,R0lGODlhAQABAIAAAMLCwgAAACH5BAAAAAAALAAAAAABAAEAAAICRAEAOw==)**Figure:** *Python Interview Questions – Django Framework*

So the data itself is not stored client side. This is nice from a security perspective.

**Q81.** **List out the inheritance styles in Django.**

**Ans:** In Django, there are three possible inheritance styles:

1. Abstract Base Classes: This style is used when you only want parent’s class to hold information that you don’t want to type out for each child model.
2. Multi-table Inheritance: This style is used If you are sub-classing an existing model and need each model to have its own database table.
3. Proxy models: You can use this model, If you only want to modify the Python level behavior of the model, without changing the model’s fields.

## **Web Scraping – Python Interview Questions**

### **Q82. How To Save An Image Locally Using Python Whose URL Address I Already Know?**

**Ans:** We will use the following code to save an image locally from an URL address

```python
import urllib.request
urllib.request.urlretrieve("URL", "local-filename.jpg")
```

### **Q83. How can you Get the Google cache age of any URL or web page?**

**Ans:** Use the following URL format:

```
http://webcache.googleusercontent.com/search?q=cache:URLGOESHERE
```

Be sure to replace “URLGOESHERE” with the proper web address of the page or site whose cache you want to retrieve and see the time for. For example, to check the Google Webcache age of edureka.co you’d use the following URL:

```
http://webcache.googleusercontent.com/search?q=cache:edureka.co
```

### ``**Q84. You are required to scrap data from IMDb top 250 movies page. It should only have fields movie name, year, and rating.**

**Ans:** We will use the following lines of code:



```python
from bs4 import BeautifulSoup
 
import requests
import sys
 
url = '<a href="http://www.imdb.com/chart/top">http://www.imdb.com/chart/top</a>'
response = requests.get(url)
soup = BeautifulSoup(response.text)
tr = soup.findChildren("tr")
tr = iter(tr)
next(tr)
 
for movie in tr:
    title = movie.find('td', {'class': 'titleColumn'} ).find('a').contents[0]
    year = movie.find('td', {'class': 'titleColumn'} ).find('span', {'class': 'secondaryInfo'}).contents[0]
    rating = movie.find('td', {'class': 'ratingColumn imdbRating'} ).find('strong').contents[0]
    row = title + ' - ' + year + ' ' + ' ' + rating
 
print(row)
```

The above code will help scrap data from IMDb’s top 250 list

## **Data Analysis – Python Interview Questions**

### **Q85. What is map function in Python?**

**Ans:** *map* function executes the function given as the first argument on all the elements of the iterable given as the second argument. If the function given takes in more than 1 arguments, then many iterables are given. 

### **Q86. Is python numpy better than lists?**

**Ans:** We use python numpy array instead of a list because of the below three reasons:

1. Less Memory
2. Fast
3. Convenient

For more information on these parameters, you can refer to this section – [Numpy Vs List](https://www.edureka.co/blog/python-numpy-tutorial/#NumpyVsList).

### **Q87. How to get indices of N maximum values in a NumPy array?**

**Ans:** We can get the indices of N maximum values in a NumPy array using the below code:

```python
import numpy as np
arr = np.array([1, 3, 2, 4, 5])
print(arr.argsort()[-3:][::-1])
```

Output

```
[ 4 3 1 ]
```

### **Q88. How do you calculate percentiles with Python/ NumPy?**

**Ans:** We can calculate percentiles with the following code

```python
import numpy as np
a = np.array([1,2,3,4,5])
p = np.percentile(a, 50) #Returns 50th percentile, e.g. median
print(p)
```

``Output

```
3
```

### **Q89. What is the difference between NumPy and SciPy?**

**Ans:** 

1. In an ideal world, NumPy would contain nothing but the array data type and the most basic operations: indexing, sorting, reshaping, basic elementwise functions, et cetera.
2. All numerical code would reside in SciPy. However, one of NumPy’s important goals is compatibility, so NumPy tries to retain all features supported by either of its predecessors.
3. Thus NumPy contains some linear algebra functions, even though these more properly belong in SciPy. In any case, SciPy contains more fully-featured versions of the linear algebra modules, as well as many other numerical algorithms.
4. If you are doing scientific computing with python, you should probably install both NumPy and SciPy. Most new features belong in SciPy rather than NumPy.

### **Q90. How do you make 3D plots/visualizations using NumPy/SciPy?**



**Ans:** Like 2D plotting, 3D graphics is beyond the scope of NumPy and SciPy, but just as in the 2D case, packages exist that integrate with NumPy. Matplotlib provides basic 3D plotting in the mplot3d subpackage, whereas Mayavi provides a wide range of high-quality 3D visualization features, utilizing the powerful VTK engine.

## **Multiple Choice Questions (MCQ) –** **Python Interview Questions**

### **Q91. Which of the following statements create a dictionary? (Multiple Correct Answers Possible)**

### a) d = {} 

### b) d = {“john”:40, “peter”:45} 

### c) d = {40:”john”, 45:”peter”} d) 

### d = (40:”john”, 45:”50”)

**Answer:** a, b, c & d. 



Dictionaries are created by specifying keys and values.

### **Q92. Which one of these is floor division?** 

### a) / 

### b) // 

### c) % 

### d) None of the mentioned

**Answer:** b) //

When both of the operands are integer then python chops out the fraction part and gives you the round off value, to get the accurate answer use floor division. For ex, 5//2 = 2.5 but both of the operands are integer so answer of this expression in python is 2. To get the 2.5 as the answer, use floor division using //. So, 5/2 = 2.5

### **Q93. What is the maximum possible length of an identifier?** 

#### a) 31 characters b) 63 characters c) 79 characters d) None of the above

**Answer:** d) None of the above

Identifiers can be of any length.

### **Q94. Why are local variable names beginning with an underscore discouraged?** 

#### a) they are used to indicate a private variables of a class 

#### b) they confuse the interpreter 

#### c) they are used to indicate global variables 

#### d) they slow down execution

**Answer:** a) they are used to indicate a private variable of a class

As Python has no concept of private variables, leading underscores are used to indicate variables that must not be accessed from outside the class.

### **Q95. Which of the following is an invalid statement?** 

#### a) abc = 1,000,000 

#### b) a b c = 1000 2000 3000 

#### c) a,b,c = 1000, 2000, 3000 

#### d) a_b_c = 1,000,000

**Answer:** b) a b c = 1000 2000 3000

Spaces are not allowed in variable names.

### **Q96. What is the output of the following?**

```python
try:
    if '1' != 1:
        raise "someError"
    else:
        print("someError has not occured")
except "someError":
    print ("someError has occured")
```

### a) someError has occured 

### b) someError has not occured 

### c) invalid code 

### d) none of the above 

**Answer:** c) invalid code

A new exception class must inherit from a BaseException. There is no such inheritance here.

### **Q97. Suppose list1 is [2, 33, 222, 14, 25], What is list1[-1] ?** 

#### a) Error b) None c) 25 d) 2 

**Answer:** c) 25

The index -1 corresponds to the last index in the list.

### **Q98. To open a file c:scores.txt for writing, we use** 

#### a) outfile = open(“c:scores.txt”, “r”) b) outfile = open(“c:scores.txt”, “w”) c) outfile = open(file = “c:scores.txt”, “r”) d) outfile = open(file = “c:scores.txt”, “o”)

**Answer:** b) The location contains double slashes ( ) and w is used to indicate that file is being written to.

### **Q99. What is the output of the following?**

```python
f = None
 
for i in range (5):
    with open("data.txt", "w") as f:
        if i > 2:
            break
 
print f.closed
```

### a) True b) False c) None d) Error 

**Answer:** a) True 

The WITH statement when used with open file guarantees that the file object is closed when the with block exits.

### **Q100. When will the else part of try-except-else be executed?** 

### a) always b) when an exception occurs c) when no exception occurs d) when an exception occurs into except block 

**Answer:** c) when no exception occurs

The else part is executed when no exception occurs.

-----------------

## Python Basic Interview Questions & Answers for Freshers & Experienced

**1) What is Python? What are the benefits of using Python?**

Python is a programming language with objects, modules, threads, exceptions and automatic memory management. The benefits of pythons are that it is simple and easy, portable, extensible, build-in data structure and it is an open source.

**2) What is PEP 8?**

PEP 8 is a coding convention, a set of recommendation, about how to write your Python code more readable.

**3) What is pickling and unpickling?**

Pickle module accepts any Python object and converts it into a string representation and dumps it into a file by using dump function, this process is called pickling. While the process of retrieving original Python objects from the stored string representation is called unpickling.

**4) How Python is interpreted?**

Python language is an interpreted language. Python program runs directly from the source code. It converts the source code that is written by the programmer into an intermediate language, which is again translated into machine language that has to be executed.

**5) How memory is managed in Python?**

- Python memory is managed by Python private heap space. All Python objects and data structures are located in a private heap. The programmer does not have an access to this private heap and interpreter takes care of this Python private heap.
- The allocation of Python heap space for Python objects is done by Python memory manager. The core API gives access to some tools for the programmer to code.
- Python also have an inbuilt garbage collector, which recycle all the unused memory and frees the memory and makes it available to the heap space.

**6) What are the tools that help to find bugs or perform static analysis?**

PyChecker is a static analysis tool that detects the bugs in Python source code and warns about the style and complexity of the bug. Pylint is another tool that verifies whether the module meets the coding standard.

**7) What are Python decorators?**

A Python decorator is a specific change that we make in Python syntax to alter functions easily.

**8) What is the difference between list and tuple?**

The difference between list and tuple is that list is mutable while tuple is not. Tuple can be hashed for e.g as a key for dictionaries.

**9) How are arguments passed by value or by reference?**

Everything in Python is an object and [all variables](https://www.guru99.com/variables-in-python.html) hold references to the objects. The references values are according to the functions; as a result you cannot change the value of the references. However, you can change the objects if it is mutable.

**10) What is Dict and List comprehensions are?**

They are syntax constructions to ease the creation of a Dictionary or List based on existing iterable.

**11) What are the built-in type does python provides?**

There are mutable and Immutable types of Pythons built in types Mutable built-in types

- List
- Sets
- Dictionaries

Immutable built-in types

- Strings
- Tuples
- Numbers

**12) What is namespace in Python?**

In Python, every name introduced has a place where it lives and can be hooked for. This is known as namespace. It is like a box where a variable name is mapped to the object placed. Whenever the variable is searched out, this box will be searched, to get corresponding object.

**13) What is lambda in Python?**

It is a single expression anonymous function often used as inline function.

**14) Why lambda forms in python does not have statements?**

A lambda form in python does not have statements as it is used to make new function object and then return them at runtime.

**15) What is pass in Python?**

Pass means, no-operation Python statement, or in other words it is a place holder in compound statement, where there should be a blank left and nothing has to be written there.

**16) In Python what are iterators?**

In Python, iterators are used to iterate a group of elements, containers like list.

**17) What is unittest in Python?**

A unit testing framework in Python is known as unittest. It supports sharing of setups, automation testing, shutdown code for tests, aggregation of tests into collections etc.

**18) In Python what is slicing?**

A mechanism to select a range of items from sequence types like list, tuple, strings etc. is known as slicing.

**19) What are generators in Python?**

The way of implementing iterators are known as generators. It is a normal function except that it yields expression in the function.

**20) What is docstring in Python?**

A Python documentation string is known as docstring, it is a way of documenting Python functions, modules and classes.

**21) How can you copy an object in Python?**

To copy an object in Python, you can try copy.copy () or copy.deepcopy() for the general case. You cannot copy all objects but most of them.

**22) What is negative index in Python?**

Python sequences can be index in positive and negative numbers. For positive index, 0 is the first index, 1 is the second index and so forth. For negative index, (-1) is the last index and (-2) is the second last index and so forth.

**23) How you can convert a number to a string?**

In order to convert a number into a string, use the inbuilt function str(). If you want a octal or hexadecimal representation, use the inbuilt function oct() or hex().

**24) What is the difference between Xrange and range?**

Xrange returns the xrange object while range returns the list, and uses the same memory and no matter what the range size is.

**25) What is module and package in Python?**

In Python, module is the way to structure program. Each Python program file is a module, which imports other modules like objects and attributes.

The folder of Python program is a package of modules. A package can have modules or subfolders.

**26) Mention what are the rules for local and global variables in Python?**

**Local variables**: If a variable is assigned a new value anywhere within the function's body, it's assumed to be local.

**Global variables**: Those variables that are only referenced inside a function are implicitly global.

**27) How can you share global variables across modules?**

To share global variables across modules within a single program, create a special module. Import the config module in all modules of your application. The module will be available as a global variable across modules.

**28) Explain how can you make a Python Script executable on Unix?**

To make a Python Script executable on Unix, you need to do two things,

- Script file's mode must be executable and
- the first line must begin with # ( #!/usr/local/bin/python)

**29) Explain how to delete a file in Python?**

By using a command os.remove (filename) or os.unlink(filename)

**30) Explain how can you generate random numbers in Python?**

To generate random numbers in Python, you need to import command as

import random

random.random()

This returns a random floating point number in the range [0,1)

**31) Explain how can you access a module written in Python from C?**

You can access a module written in Python from C by following method,

Module = =PyImport_ImportModule("<modulename>");

**32) Mention the use of // operator in Python?**

It is a Floor Divisionoperator , which is used for dividing two operands with the result as quotient showing only digits before the decimal point. For instance, 10//5 = 2 and 10.0//5.0 = 2.0.

**33) Mention five benefits of using Python?**

- Python comprises of a huge standard library for most Internet platforms like Email, HTML, etc.
- Python does not require explicit [memory management](https://www.guru99.com/os-memory-management.html) as the interpreter itself allocates the memory to new variables and free them automatically
- Provide easy readability due to use of square brackets
- Easy-to-learn for beginners
- Having the built-in data types saves programming time and effort from declaring variables

**34) Mention the use of the split function** **in Python**?

The use of the split function in Python is that it breaks a string into shorter strings using the defined separator. It gives a list of all words present in the string.

**35) Explain what is Flask & its benefits**?

Flask is a web micro framework for Python based on "Werkzeug, Jinja 2 and good intentions" BSD licensed. Werkzeug and jingja are two of its dependencies.

Flask is part of the micro-framework. Which means it will have little to no dependencies on external libraries. It makes the framework light while there is little dependency to update and less security bugs.

**36) Mention what is the difference between Django, Pyramid, and Flask?**

Flask is a "microframework" primarily build for a small application with simpler requirements. In flask, you don't have to use external libraries. Flask is ready to use.

Pyramid are build for larger applications. It provides flexibility and lets the developer use the right tools for their project. The developer can choose the database, URL structure, templating style and more. Pyramid is heavy configurable.

Like Pyramid, Django can also used for larger applications. It includes an ORM.



**37) Mention what is Flask-WTF and what are their features?**

Flask-WTF offers simple integration with WTForms. Features include for Flask WTF are

- Integration with wtforms
- Secure form with csrf token
- Global csrf protection
- Internationalization integration
- Recaptcha supporting
- File upload that works with Flask Uploads

**38) Explain what is the common way for the Flask script to work?**

The common way for the flask script to work is

- Either it should be the import path for your application
- Or the path to a Python file



**39) Explain how you can access sessions in Flask?**

A session basically allows you to remember information from one request to another. In a flask, it uses a signed cookie so the user can look at the session contents and modify. The user can modify the session if only it has the secret key Flask.secret_key.

**40) Is Flask an MVC model and if yes give an example showing MVC pattern for your application?**

Basically, Flask is a minimalistic framework which behaves same as MVC framework. So MVC is a perfect fit for Flask, and the pattern for MVC we will consider for the following example

| from flask import Flask<br />app = Flask(_name_)<br /><br />@app.route("/")<br />Def hello():<br />return "Hello World"<br /><br />app.run(debug = True) | In this code your,Configuration part will be from flask import Flaskapp = Flask(_name_) View part will be@app.route("/")Def hello():return "Hello World"While you model or main part will be app.run(debug = True) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
|                                                              |                                                              |

**41) Explain database connection in Python Flask?**

Flask supports database powered application (RDBS). Such system requires creating a schema, which requires piping the shema.sql file into a sqlite3 command. So you need to install sqlite3 command in order to create or initiate the database in Flask.

Flask allows to request database in three ways

- before_request() : They are called before a request and pass no arguments
- after_request() : They are called after a request and pass the response that will be sent to the client
- teardown_request(): They are called in situation when exception is raised, and response are not guaranteed. They are called after the response been constructed. They are not allowed to modify the request, and their values are ignored.

**42) You are having multiple Memcache servers running Python, in which one of the memcacher server fails, and it has your data, will it ever try to get key data from that one failed server?**

The data in the failed server won't get removed, but there is a provision for auto-failure, which you can configure for multiple nodes. Fail-over can be triggered during any kind of socket or Memcached server level errors and not during normal client errors like adding an existing key, etc.

**43) Explain how you can minimize the Memcached server outages in your Python Development?**

- When one instance fails, several of them goes down, this will put larger load on the database server when lost data is reloaded as client make a request. To avoid this, if your code has been written to minimize cache stampedes then it will leave a minimal impact
- Another way is to bring up an instance of Memcached on a new machine using the lost machines IP address
- Code is another option to minimize server outages as it gives you the liberty to change the Memcached server list with minimal work
- Setting timeout value is another option that some Memcached clients implement for Memcached server outage. When your Memcached server goes down, the client will keep trying to send a request till the time-out limit is reached

**44) Explain what is Dogpile effect? How can you prevent this effect?**

Dogpile effect is referred to the event when cache expires, and websites are hit by the multiple requests made by the client at the same time. This effect can be prevented by using semaphore lock. In this system when value expires, first process acquires the lock and starts generating new value.

**45) Explain how Memcached should not be used in your Python project?**

- Memcached common misuse is to use it as a data store, and not as a cache
- Never use Memcached as the only source of the information you need to run your application. Data should always be available through another source as well
- Memcached is just a key or value store and cannot perform query over the data or iterate over the contents to extract information
- Memcached does not offer any form of security either in encryption or authentication


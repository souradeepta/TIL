# Object-Oriented Programming in Python vs Java



## Sample Classes in Python vs Java

To begin, you’ll implement the same small class in both Python and Java to illustrate the differences between them. You’ll make modifications to them as the article progresses.

First, assume you have the following `Car` class definition in Java:

```java
 1 public class Car {
 2     private String color;
 3     private String model;
 4     private int year;
 5 
 6     public Car(String color, String model, int year) {
 7         this.color = color;
 8         this.model = model;
 9         this.year = year;
10     }
11 
12     public String getColor() {
13         return color;
14     }
15 
16     public String getModel() {
17         return model;
18     }
19 
20     public int getYear() {
21         return year;
22     }
23 }
```



Java classes are defined in files with the same name as the class. So, you have to save this class in a file named `Car.java`. Only one class can be defined in each file.

A similar small `Car` class is written in Python as follows:

```python
 1 class Car:
 2     def __init__(self, color, model, year):
 3         self.color = color
 4         self.model = model
 5         self.year = year
```



In Python you can declare a class anywhere, in any file, at any time. Save this class in the file `car.py`.

Using these classes as your base, you can explore the basic components of classes and objects.



## Object Attributes

All object-oriented languages have some way to store data about the object. In Java and Python, data is stored in **attributes**, which are variables associated with specific objects.

One of the most significant differences between Python vs Java is how they define and manage class and object attributes. Some of these differences come from constraints imposed by the languages, while others come from best practices.

### Declaration and Initialization

In Java, you declare attributes in the class body, outside of any methods, with a definite type. You must define class attributes before they are used:

```java
 1 public class Car {
 2     private String color;
 3     private String model;
 4     private int year;
```

In Python, you both declare and define attributes inside the class `__init__()`, which is the equivalent of Java’s constructor:

```python
 1 def __init__(self, color, model, year):
 2     self.color = color
 3     self.model = model
 4     self.year = year
```

By prefixing the variable names with `self`, you tell Python these are attributes. Each instance of the class will have a copy. All variables in Python are [loosely typed](https://realpython.com/python-type-checking/#duck-typing), and these attributes are no exception.

You can also create instance variables outside of `.__init__()`, but it’s not a best practice as their scope is often confusing. If not used properly, instance variables created outside of `.__init__()` can lead to subtle bugs that are hard to find. For example, you can add a new attribute `.wheels` to a `Car` object like this:

\>>>

```python
 1 >>> import car
 2 >>> my_car = car.Car("yellow", "beetle", 1967)
 3 >>> print(f"My car is {my_car.color}")
 4 My car is yellow
 5 
 6 >>> my_car.wheels = 5
 7 >>> print(f"Wheels: {my_car.wheels}")
 8 Wheels: 5
```

However, if you forget the `my_car.wheels = 5` on line 6, then Python displays an error:

\>>>

```python
 1 >>> import car
 2 >>> my_car = car.Car("yellow", "beetle", 1967)
 3 >>> print(f"My car is {my_car.color}")
 4 My car is yellow
 5 
 6 >>> print(f"Wheels: {my_car.wheels}")
 7 Traceback (most recent call last):
 8   File "<stdin>", line 1, in <module>
 9 AttributeError: 'Car' object has no attribute 'wheels'
```

In Python, when you declare a variable outside of a method, it’s treated as a class variable. Update the `Car` class as follows:

```python
 1 class Car:
 2 
 3     wheels = 0
 4 
 5     def __init__(self, color, model, year):
 6         self.color = color
 7         self.model = model
 8         self.year = year
```

This changes how you use the variable `wheels`. Instead of referring to it using an object, you refer to it using the class name:

\>>>

```python
 1 >>> import car
 2 >>> my_car = car.Car("yellow", "beetle", 1967)
 3 >>> print(f"My car is {my_car.color}")
 4 My car is yellow
 5 
 6 >>> print(f"It has {car.Car.wheels} wheels")
 7 It has 0 wheels
 8 
 9 >>> print(f"It has {my_car.wheels} wheels")
10 It has 0 wheels
```

**Note:** In Python, you refer to a class variable using the following syntax:

1. The name of the file containing the class, without the `.py` extension
2. A dot
3. The name of the class
4. A dot
5. The name of the variable

Since you saved the `Car` class in the file `car.py`, you refer to the class variable `wheels` on line 6 as `car.Car.wheels`.



You can refer to `my_car.wheels` or `car.Car.wheels`, but be careful. Changing the value of the instance variable `my_car.wheels` will not change the value of the class variable `car.Car.wheels`:

\>>>

```python
 1 >>> from car import *
 2 >>> my_car = car.Car("yellow", "Beetle", "1966")
 3 >>> my_other_car = car.Car("red", "corvette", "1999")
 4 
 5 >>> print(f"My car is {my_car.color}")
 6 My car is yellow
 7 >>> print(f"It has {my_car.wheels} wheels")
 8 It has 0 wheels
 9 
10 >>> print(f"My other car is {my_other_car.color}")
11 My other car is red
12 >>> print(f"It has {my_other_car.wheels} wheels")
13 It has 0 wheels
14 
15 >>> # Change the class variable value
16 ... car.Car.wheels = 4
17 
18 >>> print(f"My car has {my_car.wheels} wheels")
19 My car has 4 wheels
20 >>> print(f"My other car has {my_other_car.wheels} wheels")
21 My other car has 4 wheels
22 
23 >>> # Change the instance variable value for my_car
24 ... my_car.wheels = 5
25 
26 >>> print(f"My car has {my_car.wheels} wheels")
27 My car has 5 wheels
28 >>> print(f"My other car has {my_other_car.wheels} wheels")
29 My other car has 4 wheels
```



You define two `Car` objects on lines 2 and 3:

1. `my_car`
2. `my_other_car`

At first, both of them have zero wheels. When you set the class variable using `car.Car.wheels = 4` on line 16, both objects now have four wheels. However, when you set the instance variable using `my_car.wheels = 5` on line 24, only that object is affected.

This means that there are now two different copies of the `wheels` attribute:

1. A class variable that applies to all `Car` objects
2. A specific instance variable applicable to the `my_car` object only

It isn’t difficult to accidentally refer to the wrong one and introduce subtle bugs.

Java’s equivalent to a class attribute is a `static` attribute:

```java
public class Car {
    private String color;
    private String model;
    private int year;
    private static int wheels;

    public Car(String color, String model, int year) {
        this.color = color;
        this.model = model;
        this.year = year;
    }

    public static int getWheels() {
        return wheels;
    }

    public static void setWheels(int count) {
        wheels = count;
    }
}
```



Normally, you refer to static variables using the Java class name. You can refer to a static variable through a class instance like Python, but it’s not a best practice.

Your Java class is getting long. One of the reasons why Java is more verbose than Python is the notion of public and private methods and attributes.



### Public and Private

Java controls access to methods and attributes by differentiating between **public** data and **private** data.

In Java, it is expected that attributes are declared as `private`, or `protected` if subclasses need direct access to them. This limits access to these attributes from code outside the class. To provide access to `private` attributes, you declare `public` methods which set and retrieve data in a controlled manner (more on that later).

Recall from your Java class above that the `color` variable was declared as `private`. Therefore, this Java code will show a compilation error at the highlighted line:

```java
Car myCar = new Car("blue", "Ford", 1972);

// Paint the car
myCar.color = "red";
```



If you don’t specify an access level, then the attribute defaults to **package protected**, which limits access to classes in the same package. You have to mark the attribute as `public` if you want this code to work.

However, declaring public attributes is not considered a best practice in Java. You’re expected to declare attributes as `private`, and use `public` access methods, such as the `.getColor()` and `.getModel()` shown in the code.

Python doesn’t have the same notion of `private` or `protected` data that Java does. Everything in Python is `public`. This code works with your existing Python class just fine:

\>>>

```python
>>> my_car = car.Car("blue", "Ford", 1972)

>>> # Paint the car
... my_car.color = "red"
```



Instead of `private`, Python has a notion of a **non-public** instance variable. Any variable which starts with an underscore character is defined to be non-public. This naming convention makes it harder to access a variable, but it’s only a naming convention, and you can still access the variable directly.

Add the following line to your Python `Car` class:

```python
class Car:

    wheels = 0

    def __init__(self, color, model, year):
        self.color = color
        self.model = model
        self.year = year
        self._cupholders = 6
```



You can access the `._cupholders` variable directly:

\>>>

```python
>>> import car
>>> my_car = car.Car("yellow", "Beetle", "1969")
>>> print(f"It was built in {my_car.year}")
It was built in 1969
>>> my_car.year = 1966
>>> print(f"It was built in {my_car.year}")
It was built in 1966
>>> print(f"It has {my_car._cupholders} cupholders.")
It has 6 cupholders.
```



Python lets you access `._cupholders`, but IDEs such as [VS Code](https://realpython.com/courses/python-development-visual-studio-code-setup-guide/) may issue a warning through linters that support PEP 8. For more on PEP 8, read [How to Write Beautiful Python Code With PEP 8](https://realpython.com/python-pep8/).

Here’s the code in VS Code, with a warning highlighted and displayed:

[![Linter highlighting a PEP8 issue in Python.](https://files.realpython.com/media/oop_linting_vscode.b7a1cd7fd5ca.png)](https://files.realpython.com/media/oop_linting_vscode.b7a1cd7fd5ca.png)

Python further recognizes using double underscore characters in front of a variable to conceal an attribute in Python. When Python sees a double underscore variable, it changes the variable name internally to make it difficult to access directly. This mechanism [avoids accidents](https://www.python.org/dev/peps/pep-0008/#method-names-and-instance-variables) but still doesn’t make data impossible to access.

Python further recognizes using double underscore characters in front of a variable to conceal an attribute in Python. When Python sees a double underscore variable, it changes the variable name internally to make it difficult to access directly. This mechanism [avoids accidents](https://www.python.org/dev/peps/pep-0008/#method-names-and-instance-variables) but still doesn’t make data impossible to access.

To show this mechanism in action, change the Python `Car` class again:

```python
class Car:

    wheels = 0

    def __init__(self, color, model, year):
        self.color = color
        self.model = model
        self.year = year
        self.__cupholders = 6
```

Now, when you try to access the `.__cupholders` variable, you see the following error:

\>>>

```python
>>> import car
>>> my_car = car.Car("yellow", "Beetle", "1969")
>>> print(f"It was built in {my_car.year}")
It was built in 1969
>>> my_car.year = 1966
>>> print(f"It was built in {my_car.year}")
It was built in 1966
>>> print(f"It has {my_car.__cupholders} cupholders.")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Car' object has no attribute '__cupholders'
```

So why doesn’t the `.__cupholders` attribute exist?

When Python sees an attribute with double underscores, it changes the attribute by prefixing the original name of the attribute with an underscore, followed by the class name. To use the attribute directly, you need to change the name you use as well:

\>>>

```python
>>> print(f"It has {my_car._Car__cupholders} cupholders")
It has 6 cupholders
```

When you use double underscores to conceal an attribute from the user, Python changes the name in a well-documented manner. This means that a determined developer can still access the attribute directly.

So if your Java attributes are declared `private`, and your Python attributes are prefaced with double underscores, then how do you provide and control access to the data they store?

### Access Control

In Java, you access `private` attributes using **setters** and **getters**. To allow users to paint their cars, add the following code to your Java class:

```java
public String getColor() {
    return color;
}

public void setColor(String color) {
    this.color = color;
}
```

Since `.getColor()` and `.setColor()` are `public`, anyone can call them to change or retrieve the car’s color. Java’s best practices of using `private` attributes accessed with `public` **getters** and **setters** is one of the reasons why Java code tends to be more verbose than Python.

As you saw above, you access attributes directly in Python. Since everything is `public`, you can access anything at any time from anywhere. You set and get attribute values directly by referring to their names. You can even delete attributes in Python, which isn’t possible in Java:

\>>>

```python
>>> my_car = Car("yellow", "beetle", 1969)
>>> print(f"My car was built in {my_car.year}")
My car was built in 1969
>>> my_car.year = 1966
>>> print(f"It was built in {my_car.year}")
It was built in 1966
>>> del my_car.year
>>> print(f"It was built in {my_car.year}")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Car' object has no attribute 'year'
```

However, there are times you may want to control access to an attribute. In that case, you can use Python properties.

In Python, **properties** provide controllable access to class attributes using Python decorator syntax. (You can learn about decorators in the video course [Python Decorators 101](https://realpython.com/courses/python-decorators-101/).) Properties allow functions to be declared in Python classes that are analogous to Java getter and setter methods, with the added bonus of allowing you to delete attributes as well.

You can see how properties work by adding one to your `Car` class:

```python
 1 class Car:
 2     def __init__(self, color, model, year):
 3         self.color = color
 4         self.model = model
 5         self.year = year
 6         self._voltage = 12
 7 
 8     @property
 9     def voltage(self):
10         return self._voltage
11 
12     @voltage.setter
13     def voltage(self, volts):
14         print("Warning: this can cause problems!")
15         self._voltage = volts
16 
17     @voltage.deleter
18     def voltage(self):
19         print("Warning: the radio will stop working!")
20         del self._voltage
```



Here, you expand the notion of the `Car` to include electric vehicles. You declare the `._voltage` attribute to hold the battery voltage on line 6.

To provide controlled access, you [define a function](https://realpython.com/defining-your-own-python-function/) called `voltage()` to return the private value on lines 9 and 10. By using the `@property` decoration, you mark it as a getter that anyone can access directly.

Similarly, you define a setter function on lines 13 to 15, also called `voltage()`. However, you decorate this function with `@voltage.setter`. Lastly, you use `@voltage.deleter` to decorate a third `voltage()` on lines 18 to 20, which allows controlled deletion of the attribute.

The names of the decorated functions are all the same, indicating they control access to the same attribute. The function names also become the name of the attribute you use to access the value. Here’s how these properties work in practice:

\>>>

```python
 1 >>> from car import *
 2 >>> my_car = Car("yellow", "beetle", 1969)
 3 
 4 >>> print(f"My car uses {my_car.voltage} volts")
 5 My car uses 12 volts
 6 
 7 >>> my_car.voltage = 6
 8 Warning: this can cause problems!
 9 
10 >>> print(f"My car now uses {my_car.voltage} volts")
11 My car now uses 6 volts
12 
13 >>> del my_car.voltage
14 Warning: the radio will stop working!
```



Note that you use `.voltage` in the highlighted lines above, not `._voltage`. This tells Python to use the property functions you defined:

- When you print the value of `my_car.voltage` on line 4, Python calls `.voltage()` decorated with `@property`.
- When you assign a value to `my_car.voltage` on line 7, Python calls `.voltage()` decorated with `@voltage.setter`.
- When you delete `my_car.voltage` on line 13, Python calls `.voltage()` decorated with `@voltage.deleter`.

The `@property`, `@.setter`, and `@.deleter` decorations make it possible to control access to attributes without requiring users to use different methods. You can even make attributes appear to be read-only properties by omitting the `@.setter` and `@.deleter` decorated functions.

### 

### `self` and `this`

In Java, a class refers to itself with the `this` reference:

```java
public void setColor(String color) {
    this.color = color;
}
```

`this` is implicit in Java code: it doesn’t normally need to be written, unless there may be confusion between two variables with the same name.

You can write the same setter this way:

```java
public void setColor(String newColor) {
    color = newColor;
}
```



Since `Car` has an attribute named `.color`, and there isn’t another variable in scope with the same name, a reference to that name works. We used `this` in the first example to differentiate between the attribute and parameter both named `color`.

In Python, the [keyword](https://realpython.com/python-keywords/) `self` serves a similar purpose. It’s how you refer to member variables, but unlike Java’s `this`, it’s *required* if you want to create or refer to a member attribute:

```python
class Car:
    def __init__(self, color, model, year):
        self.color = color
        self.model = model
        self.year = year
        self._voltage = 12

    @property
    def voltage(self):
        return self._voltage
```



Python requires each `self` in the code above. Each one either creates or refers to the attributes. If you omit them, then Python will create a local variable instead of an attribute.

The difference between how you use `self` and `this` in Python and Java is due to underlying differences between the two languages and how they name variables and attributes.



## Methods and Functions

This difference between Python vs Java is, simply put, that Python has functions, and Java doesn’t.

In Python, the following code is perfectly fine (and very common):

\>>>

```python
>>> def say_hi():
...     print("Hi!")
... 
>>> say_hi()
Hi!
```



You can call `say_hi()` from anywhere it’s visible. This function has no reference to `self`, which indicates that it’s a global function, not a class function. It can’t alter or store any data in any classes but can use local and global variables.

In contrast, every line of Java code you write belongs to a class. Functions can’t exist outside of a class, and by definition, all Java functions are methods. In Java, the closest you can get to a pure function is by using a static method:

```java
public class Utils {
    static void SayHi() {
        System.out.println("Hi!");
    }
}
```



`Utils.SayHi()` is callable from anywhere without first creating an instance of `Utils`. Since you can call `SayHi()` without creating an object first, the `this` reference doesn’t exist. However, this is still not a function in the sense that `say_hi()` is in Python.



## Inheritance and Polymorphism

[Inheritance](https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) and [polymorphism](https://en.wikipedia.org/wiki/Polymorphism_(computer_science)) are two fundamental concepts in object-oriented programming.

[Inheritance](https://realpython.com/inheritance-composition-python/) allows objects to derive attributes and functionality from other objects, creating a hierarchy moving from more general objects to more specific. For example, a `Car` and a `Boat` are both specific types of `Vehicles`. Objects can inherit their behavior from a single parent object or multiple parent objects, and are referred to as child objects when they do.

[Polymorphism](https://en.wikipedia.org/wiki/Polymorphism_(computer_science)) allows two or more objects to behave like one another, which allows them to be used interchangeably. For example, if a method or function knows how to paint a `Vehicle` object, then it can also paint a `Car` or `Boat` object, since they inherit their data and behavior from the `Vehicle`.

These fundamental OOP concepts are implemented quite differently in Python vs Java.

### Inheritance

Python supports multiple inheritance, or creating classes that inherit behavior from more than one parent class.

To see how this works, update the `Car` class by breaking it into two categories, one for vehicles, and one for devices that use electricity:

```python
class Vehicle:
    def __init__(self, color, model):
        self.color = color
        self.model = model

class Device:
    def __init__(self):
        self._voltage = 12

class Car(Vehicle, Device):
    def __init__(self, color, model, year):
        Vehicle.__init__(self, color, model)
        Device.__init__(self)
        self.year = year

    @property
    def voltage(self):
        return self._voltage

    @voltage.setter
    def voltage(self, volts):
        print("Warning: this can cause problems!")
        self._voltage = volts

    @voltage.deleter
    def voltage(self):
        print("Warning: the radio will stop working!")
        del self._voltage
```

A `Vehicle` is defined as having `.color` and `.model` attributes. Then, a `Device` is defined to have a `._voltage` attribute. Since the original `Car` object had these three attributes, it can be redefined to inherit both the `Vehicle` and `Device` classes. The `color`, `model`, and `_voltage` attributes will be part of the new `Car` class.

In the `.__init__()` for `Car`, you call the `.__init__()` methods for both of the parent classes to make sure everything is initialized properly. Once done, you can add any other functionality you want to your `Car`. In this case, you add a `.year` attribute specific to `Car` objects, and getter and setter methods for `.voltage`.

Functionally, the new `Car` class behaves as it always has. You create and use `Car` objects just as before:



```python
>>> from car import *
>>> my_car = Car("yellow", "beetle", 1969)

>>> print(f"My car is {my_car.color}")
My car is yellow

>>> print(f"My car uses {my_car.voltage} volts")
My car uses 12 volts

>>> my_car.voltage = 6
Warning: this can cause problems!

>>> print(f"My car now uses {my_car.voltage} volts")
My car now uses 6 volts
```



Java, on the other hand, only supports single inheritance, which means classes in Java can inherit data and behavior from only a single parent class. However, Java objects can inherit behavior from many different [interfaces](https://docs.oracle.com/javase/tutorial/java/concepts/interface.html). Interfaces provide a group of related methods an object must implement, and allow multiple child classes to behave similarly.

To see this in action, split the Java `Car` class into a parent class and an `interface`:

```java
public class Vehicle {

    private String color;
    private String model;

    public Vehicle(String color, String model) {
        this.color = color;
        this.model = model;
    }

    public String getColor() {
        return color;
    }

    public String getModel() {
        return model;
    }
}

public interface Device {
    int getVoltage();
}

public class Car extends Vehicle implements Device {

    private int voltage;
    private int year;

    public Car(String color, String model, int year) {
        super(color, model);
        this.year = year;
        this.voltage = 12;
    }

    @Override
    public int getVoltage() {
        return voltage;
    }

    public int getYear() {
        return year;
    }
}
```



Remember that each `class` and `interface` needs to live in its own file.

As you did with Python, you create a new class called `Vehicle` to hold the more general vehicle related data and functionality. However, to add the `Device` functionality, you need to create an `interface` instead. This `interface` defines a single method to return the voltage of the `Device`.

Redefining the `Car` class requires you to inherit from `Vehicle` using `extend`, and implement the `Device` interface using `implements`. In the constructor, you call the parent class constructor using the built-in `super()`. Since there is only one parent class, it can only refer to the `Vehicle` constructor. To implement the `interface`, you write `getVoltage()` using the `@Override` annotation.

Rather than getting code reuse from `Device` as Python did, Java requires you to implement the same functionality in every class that implements the `interface`. Interfaces only define the methods—they cannot define instance data or implementation details.

So why is this the case for Java? It all comes down to types.

### Types and Polymorphism

Java’s strict type checking is what drives its `interface` design.

Every `class` and `interface` in Java is a type. Therefore, if two Java objects implement the same `interface`, then they are considered to be the same type with respect to that `interface`. This mechanism allows different classes to be used interchangeably, which is the definition of polymorphism.

You can implement device charging for your Java objects by creating a `.charge()` that takes a `Device` to charge. Any object that implements the `Device` interface can be passed to `.charge()`. This also means that classes that do not implement `Device` will generate a compilation error.

Create the following class in a file called `Rhino.java`:

```java
public class Rhino {
}
```



Now you can create a new `Main.java` to implement `.charge()` and explore how `Car` and `Rhino` objects differ:

```java
public class Main{
    public static void charge(Device device) {
       device.getVoltage();
    }

    public static void main(String[] args) throws Exception {
        Car car = new Car("yellow", "beetle", 1969);
        Rhino rhino = new Rhino();
        charge(car);
        charge(rhino);
    }
}
```



Here is what you should see when you try to build this code:

```java
Information:2019-02-02 15:20 - Compilation completed with 
    1 error and 0 warnings in 4 s 395 ms
Main.java
Error:(43, 11) java: incompatible types: Rhino cannot be converted to Device
```



Since the `Rhino` class doesn’t implement the `Device` interface, it can’t be passed into `.charge()`.

In contrast to Java’s strict variable typing, Python uses a concept called **duck typing**, which in basic terms means that if a variable “walks like a duck and quacks like a duck, then it’s a duck.” Instead of identifying objects by type, Python examines their behavior. You can learn more about the Python type system, and duck typing, in [The Ultimate Guide to Python Type Checking](https://realpython.com/python-type-checking).

You can explore duck typing by implementing similar device charging capabilities for your Python `Device` class:

\>>>

```python
>>> def charge(device):
...     if hasattr(device, '_voltage'):
...         print(f"Charging a {device._voltage} volt device")
...     else:
...         print(f"I can't charge a {device.__class__.__name__}")
... 
>>> class Phone(Device):
...     pass
... 
>>> class Rhino:
...     pass
... 
>>> my_car = Car("yellow", "Beetle", "1966")
>>> my_phone = Phone()
>>> my_rhino = Rhino()

>>> charge(my_car)
Charging a 12 volt device
>>> charge(my_phone)
Charging a 12 volt device
>>> charge(my_rhino)
I can't charge a Rhino
```



`charge()` must check for the existence of the `._voltage` attribute in the object it’s passed. Since the `Device` class defines this attribute, any class that inherits from it (such as `Car` and `Phone`) will have this attribute, and will therefore show they are charging properly. Classes that do not inherit from `Device` (like `Rhino`) may not have this attribute, and will not be able to charge (which is good, since charging rhinos can be dangerous).



### Default Methods

All Java classes descend from the `Object` class, which contains a set of methods every other class inherits. Subclasses can either override them or keep the defaults. The `Object` class defines the following methods:

```java
class Object {
    boolean equals(Object obj) { ... }    
    int hashCode() { ... }    
    String toString() { ... }    
}
```



By default, [`equals()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#equals-java.lang.Object-) compares the addresses of the current `Object` with a second `Object` passed in, and [`hashcode()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#hashCode--) computes a unique identifier that also uses the address of the current `Object`. These methods are used in many different contexts in Java. For example, utility classes, such as collections that sort objects based on value, need both of them.

[`toString()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#toString--) returns a `String` representation of the `Object`. By default, this is the name of the class and the address. This method is called automatically when an `Object` is passed to a method that requires a `String` argument, such as `System.out.println()`:

```java
Car car = new Car("yellow", "Beetle", 1969);
System.out.println(car);
```



Running this code will use the default `.toString()` to show the `car` object:

```java
Car@61bbe9ba
```



Not very useful, right? You can improve this by overriding the default `.toString()`. Add this method to your Java `Car` class:

```java
public String toString() {
    return "Car: " + getColor() + " : " + getModel() + " : " + getYear();
}
```



Now, when you run the same sample code, you’ll see the following:

```java
Car: yellow : Beetle : 1969
```



Python provides similar functionality with a set of common [dunder (short for “double underscore”) methods](https://dbader.org/blog/python-dunder-methods). Every Python class inherits these methods, and you can override them to modify their behavior.

For string representations of an object, Python provides `__repr__()` and `__str__()`, which you can learn about in [Pythonic OOP String Conversion: `__repr__` vs `__str__`](https://realpython.com/courses/pythonic-oop-string-conversion-__repr__-vs-__str__/). The unambiguous representation of an object is returned by `__repr__()`, while `__str__()` returns a human readable representation. These are roughly analogous to `.hashcode()` and `.toString()` in Java.

Like Java, Python provides default implementations of these dunder methods:

\>>>

```python
>>> my_car = Car("yellow", "Beetle", "1966")

>>> print(repr(my_car))
<car.Car object at 0x7fe4ca154f98>
>>> print(str(my_car))
<car.Car object at 0x7fe4ca154f98>
```



You can improve this output by overriding `.__str__()`, adding this to your Python `Car` class:

```python
def __str__(self):
    return f'Car {self.color} : {self.model} : {self.year}'
```



This gives you a much nicer result:

\>>>

```python
>>> my_car = Car("yellow", "Beetle", "1966")

>>> print(repr(my_car))
<car.Car object at 0x7f09e9a7b630>
>>> print(str(my_car))
Car yellow : Beetle : 1966
```



Overriding the dunder method gave us a more readable representation of your `Car`. You may want to override the `.__repr__()` as well, as it is often useful for debugging.

Python offers [a lot more dunder methods](https://docs.python.org/3/reference/datamodel.html#special-method-names). Using dunder methods, you can define your object’s behavior during iteration, comparison, addition, or making an object callable directly, among other things.



### Operator Overloading

Operator overloading refers to redefining how Python operators work when operating on user-defined objects. Python’s dunder methods allow you to implement operator overloading, something that Java doesn’t offer at all.

Modify your Python `Car` class with the following additional dunder methods:

```python
class Car:
    def __init__(self, color, model, year):
        self.color = color
        self.model = model
        self.year = year

    def __str__(self):
        return f'Car {self.color} : {self.model} : {self.year}'

    def __eq__(self, other):
        return self.year == other.year

    def __lt__(self, other):
        return self.year < other.year

    def __add__(self, other):
        return Car(self.color + other.color, 
                   self.model + other.model, 
                   int(self.year) + int(other.year))
```



The table below shows the relationship between these dunder methods and the Python operators they represent:

| Dunder Method | Operator | Purpose                                    |
| ------------- | -------- | ------------------------------------------ |
| `__eq__`      | `==`     | Do these `Car` objects have the same year? |
| `__lt__`      | `<`      | Which `Car` is an earlier model?           |
| `__add__`     | `+`      | Add two `Car` objects in a nonsensical way |

When Python sees an expression containing objects, it calls any dunder methods defined that correspond to operators in the expression. The code below uses these new overloaded arithmetic operators on a couple of `Car` objects:

\>>>

```python
>>> my_car = Car("yellow", "Beetle", "1966")
>>> your_car = Car("red", "Corvette", "1967")

>>> print (my_car < your_car)
True
>>> print (my_car > your_car)
False
>>> print (my_car == your_car)
False
>>> print (my_car + your_car)
Car yellowred : BeetleCorvette : 3933 
```



There are many more operators you can overload using [dunder methods](https://docs.python.org/3/reference/datamodel.html#special-method-names). They offer a way to enrich your object’s behavior in a way that Java’s common base class default methods don’t.

## Reflection

[Reflection](https://en.wikipedia.org/wiki/Reflection_(computer_programming)) refers to examining an object or class from within the object or class. Both Java and Python offer ways to explore and examine the attributes and methods in a class.

### Examining an Object’s Type

Both languages have ways to test or check an object’s type.

In Python, you use `type()` to display the type of a variable, and `isinstance()` to determine if a given variable is an instance or child of a specific class:

\>>>

```python
>>> my_car = Car("yellow", "Beetle", "1966")

>>> print(type(my_car))
<class 'car.Car'>
>>> print(isinstance(my_car, Car))
True
>>> print(isinstance(my_car, Device))
True
```

In Java, you query the object for its type using `.getClass()`, and use the `instanceof` operator to check for a specific class:

```java
Car car = new Car("yellow", "beetle", 1969);

System.out.println(car.getClass());
System.out.println(car instanceof Car);
```

This code outputs the following:

```java
class com.realpython.Car
true
```

### Examining an Object’s Attributes

In Python, you can view every attribute and function contained in any object (including all the dunder methods) using `dir()`. To get the specific details of a given attribute or function, use `getattr()`:

\>>>

```python
>>> print(dir(my_car))
['_Car__cupholders', '__add__', '__class__', '__delattr__', '__dict__', 
 '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__',
 '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__',
 '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__',
 '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__',
 '_voltage', 'color', 'model', 'voltage', 'wheels', 'year']

>>> print(getattr(my_car, "__format__"))
<built-in method __format__ of Car object at 0x7fb4c10f5438>
```



Java has similar capabilities, but the language’s access control and type safety make it more complicated to retrieve.

`.getFields()` retrieves a list of all publicly accessible attributes. However, since none of the attributes of `Car` are `public`, this code returns an empty array:

```java
Field[] fields = car.getClass().getFields();
```



Java treats attributes and methods as separate entities, so `public` methods are retrieved using `.getDeclaredMethods()`. Since `public` attributes will have a corresponding `.get` method, one way to discover if a class contains a specific property might look like this:

- Use `.getDeclaredMethods()` to generate an array of all the methods.
- Loop through all the methods returned:
  - For each method discovered, return true if the method:
    - Begins with the word `get` OR accepts zero arguments
    - AND doesn’t return `void`
    - AND includes the name of the property
  - Otherwise, return false.

Here’s a quick-and-dirty example:

```java
 1 public static boolean getProperty(String name, Object object) throws Exception {
 2 
 3     Method[] declaredMethods = object.getClass().getDeclaredMethods();
 4     for (Method method : declaredMethods) {
 5         if (isGetter(method) && 
 6             method.getName().toUpperCase().contains(name.toUpperCase())) {
 7               return true;
 8         }
 9     }
10     return false;
11 }
12 
13 // Helper function to get if the method is a getter method
14 public static boolean isGetter(Method method) {
15     if ((method.getName().startsWith("get") || 
16          method.getParameterCount() == 0 ) && 
17         !method.getReturnType().equals(void.class)) {
18           return true;
19     }
20     return false;
21 }
```



`getProperty()` is your entry point. Call this with the name of an attribute and an object. It returns `true` if the property is found, and `false` if not.

### Calling Methods Through Reflection

Both Java and Python provide mechanisms to call methods through reflection.

In the Java example above, instead of simply returning `true` if the property was found, you could call the method directly. Recall that `getDeclaredMethods()` returns an array of `Method` objects. The `Method` object itself has a method called `.invoke()`, which will call the `Method`. Instead of returning `true` when the correct method is found on line 7 above, you can return `method.invoke(object)` instead.

This capability exists in Python as well. However, since Python doesn’t differentiate between functions and attributes, you have to look specifically for entries that are `callable`:

\>>>

```python
>>> for method_name in dir(my_car):
...     if callable(getattr(my_car, method_name)):
...         print(method_name)
... 
__add__
__class__
__delattr__
__dir__
__eq__
__format__
__ge__
__getattribute__
__gt__
__init__
__init_subclass__
__le__
__lt__
__ne__
__new__
__reduce__
__reduce_ex__
__repr__
__setattr__
__sizeof__
__str__
__subclasshook__
```

Python methods are simpler to manage and call than in Java. Adding the `()` operator (and any required arguments) is all you need to do.

The code below will find an object’s `.__str__()` and call it through reflection:

\>>>

```python
>>> for method_name in dir(my_car):
...     attr = getattr(my_car, method_name)
...     if callable(attr):
...         if method_name == '__str__':
...             print(attr())
... 
Car yellow : Beetle : 1966
```

Here, every attribute returned by `dir()` is checked. You get the actual attribute object using `getattr()`, and check if it’s a callable function using `callable()`. If so, you then check if its name is `__str__()`, and then call it.

## Conclusion

Throughout the course of this article, you learned how object-oriented principles differ in Python vs Java. As you read, you:

- Built a basic class in both Java and Python
- Explored how object attributes work in Python vs Java
- Compared and contrasted Java methods and Python functions
- Discovered inheritance and polymorphism mechanisms in both languages
- Investigated reflection across Python vs Java
- Applied everything in a complete class implementation in both languages
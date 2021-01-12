# Common Java Exceptions

## **1. Introduction**

This tutorial focuses on some common Java exceptions.

We'll start by discussing what an exception basically is. Later, we'll discuss different types of checked and unchecked exceptions in detail.

## **2. Exceptions**

**An exception is an abnormal condition that occurs in a code sequence during the execution of a program.** This abnormal condition arises when a program violates certain constraints at runtime.

All exception types are subclasses of the class *Exception*. This class is then subclassed into checked exceptions and unchecked exceptions. We'll consider them in detail in the subsequent sections.

## **3. Checked Exceptions**

**Checked exceptions are mandatory to handle.** They are direct subclasses of the class *Exception*.

There's a [debate](https://www.ibm.com/developerworks/library/j-jtp05254/index.html) on their importance that's worth taking a look.

Let's define some checked exceptions in detail.

### **3.1. \*IOException\***

**A method throws an \*IOException\* or a direct subclass of it when any Input/Output operation fails.** 

Typical uses of these I/O operations include:

- Working with the file system or data streams using *java.io* package
- Creating network applications using *java.net* package

***FileNotFoundException***

[*FileNotFoundException*](https://www.baeldung.com/java-filenotfound-exception) is a common type of *IOException* while working with the file system:

```java
try {
    new FileReader(new File("/invalid/file/location"));
} catch (FileNotFoundException e) {
    LOGGER.info("FileNotFoundException caught!");
}
```

***MalformedURLException***

When working with URLs, we might encounter with *MalformedURLException –* if our URLs are invalid.

```java
try {
    new URL("malformedurl");
} catch (MalformedURLException e) {
    LOGGER.error("MalformedURLException caught!");
}
```

### **3.2. \*ParseException\***

Java uses text parsing to create an object based on a given *String.* **If parsing causes an error, it throws a \*ParseException\*.**

For instance, we could represent *Date* in different ways e.g. *dd/mm/yyyy* or *dd,mm,yyyy,* but try to parse a *string* with a different format:

```java
try {
    new SimpleDateFormat("MM, dd, yyyy").parse("invalid-date");
} catch (ParseException e) {
    LOGGER.error("ParseException caught!");
}
```

Here, the *String* is malformed and causes a *ParseException*.

### **3.3. \*InterruptedException\***

Whenever a Java thread calls *join(), sleep()* or *wait()* it goes into either the *WAITING* state or the *TIMED_WAITING* state.

In addition, a thread can interrupt another thread by calling another thread's *interrupt()* method.

Consequently, **the** **thread throws an \*InterruptedException\* if another thread interrupts it while it is in the \*WAITING\* or in the \*TIMED_WAITING\* state.**

Consider the following example with two threads:

- The main thread starts the child thread and interrupts it
- The child thread starts and calls *sleep()*

This scenario results in an *InterruptedException:*

```java
class ChildThread extends Thread {

    public void run() {
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            LOGGER.error("InterruptedException caught!");
        }
    }
}

public class MainThread {

    public static void main(String[] args) 
      throws InterruptedException {
        ChildThread childThread = new ChildThread();
        childThread.start();
        childThread.interrupt();
    }
}
```

## **4. Unchecked Exceptions**

**For Unchecked Exceptions, the compiler doesn't check during the compilation process.** Hence, it isn't mandatory for the method to handle these exceptions.

All unchecked exceptions extend the class *RuntimeException.*

Let's discuss some unchecked exceptions in detail.

### **4.1. \*NullPointerException\***

**If an application attempts to use \*null\* where it actually requires an object instance, the method will throw a \*NullPointerException\*.** 

There are different scenarios where illegal uses of *null* causes *NullPointerException.* Let's consider some of them.

Calling a method of the class that has no object instance:

```java
String strObj = null;
strObj.equals("Hello World"); // throws NullPointerException.
```

Also, if an application tries to access or modify an instance variable with a *null* reference, we get a *NullPointerException:*

```java
Person personObj = null;
String name = personObj.personName; // Accessing the field of a null object
personObj.personName = "Jon Doe"; // Modifying the field of a null object
```

### **4.2. \*ArrayIndexOutOfBoundsException\***

An array stores its elements in contiguous fashion. Thus, we can access its elements via indices.

However**, if a piece of code tries to access an illegal index of an array, the respective method throws an** ***ArrayIndexOutOfBoundException.***

Let's see a few examples that throw *ArrayIndexOutOfBoundException*:

```java
int[] nums = new int[] {1, 2, 3};
int numFromNegativeIndex = nums[-1]; // Trying to access at negative index
int numFromGreaterIndex = nums[4];   // Trying to access at greater index
int numFromLengthIndex = nums[3];    // Trying to access at index equal to size of the array
```

### **4.3. \*StringIndexOutOfBoundsException\***

The *String* class in Java provides the methods to access a particular character of the string or to slice out a character array out of the *String.* When we use these methods, internally it converts the *String* into a character array.

Again, there could be an illegal use of indexes on this array. In such cases, these methods of the *String* class throws the *StringIndexOutOfBoundsException*.

This exception **indicates that the index is either greater than or equal to the size of the \*String.\*** *StringIndexOutOfBoundsException* extends *IndexOutOfBoundsException*.

The method *charAt(index)* of the class *String* throws this exception when we try to access a character at the index equal to the *String's* length or some other illegal index:

```java
String str = "Hello World";
char charAtNegativeIndex = str.charAt(-1); // Trying to access at negative index
char charAtLengthIndex = str.charAt(11);   // Trying to access at index equal to size of the string		
```

### **4.4. \*NumberFormatException\***

Quite often an application ends up with numeric data in a *String*. In order to interpret this data as numeric, Java allows the conversion of *String* to numeric types. The wrapper classes such as *Integer, Float, etc.* contains utility methods for this purpose.

However, **if the \*String\* doesn't have an appropriate format during the conversion, the method throws a \*NumberFormatException.\***

Let's consider the following snippet.

Here, we declare a *String* with an alphanumeric data. Further, we try to use the methods of the *Integer* wrapper class to interpret this data as numeric.

Consequently, this results in *NumberFormatException:*

```java
String str = "100ABCD";
int x = Integer.parseInt(str); // Throws NumberFormatException
int y = Integer.valueOf(str); //Throws NumberFormatException
```

### **4.5. \*ArithmeticException\***

**When a program evaluates an arithmetic operation and it results in some exceptional condition, it throws \*ArithmeticException\*.** In addition, *ArithmeticException* applies to only *int* and *long* data types.

For instance, if we try to divide an integer by zero, we get an *ArithmeticException*:

```java
int illegalOperation = 30/0; // Throws ArithmeticException
```

### **4.6. \*ClassCastException\***

Java allows [typecasting](https://www.baeldung.com/java-type-casting) between the objects in order to support inheritance and polymorphism. We can either upcast an object or downcast it.

In upcasting, we cast an object to its supertype. And in downcasting, we cast an object to one of its subtypes.

However, **at runtime, if the code attempts to downcast an object to a subtype of which it isn't an instance, the method throws a \*ClassCastException\*.**

The runtime instance is what actually matters in typecasting. Consider the following inheritance between *Animal*, *Dog, and Lion*:

```java
class Animal {}

class Dog extends Animal {}

class Lion extends Animal {}
```

Further, in the driver class, we cast the *Animal* reference containing an instance of *Lion* into a *Dog*.

However, at the runtime, the JVM notices that instance *Lion* isn't compatible with the subtype of the class *Dog*.

This results in *ClassCastException:*

```java
Animal animal = new Lion(); // At runtime the instance is Lion
Dog tommy = (Dog) animal; // Throws ClassCastException
```

### **4.7. \*IllegalArgumentException\***

**A method throws an \*IllegalArgumentException\* if we call it with some illegal or inappropriate arguments.**

For instance, the *sleep()* method of the *Thread* class expects positive time and we pass a negative time interval as an argument. This results in *IllegalArgumentException*:

```java
Thread.currentThread().sleep(-10000); // Throws IllegalArgumentException
```

### **4.8. \*IllegalStateException\***

***IllegalStateException\* signals that a method's been invoked at an illegal or inappropriate time.**

Every Java object has a state (instance variables) and some behavior (methods). Thus, *IllegalStateException* means it's illegal to invoke the behavior of this object with the current state variables.

However, with some different state variables, it might be legal.

For example, we use an iterator to iterate a list. Whenever we initialize one, it internally sets its state variable *lastRet* to -1.

With this context, the program tries to call the *remove* method on the list:

```java
//Initialized with index at -1
Iterator<Integer> intListIterator = new ArrayList<>().iterator(); 

intListIterator.remove(); // IllegalStateException
```

Internally, the *remove* method checks the state variable *lastRet* and if it is less than 0, it throws *IllegalStateException.* Here, the variable is still pointing to the value -1.

As a result, we get an *IllegalStateException*.

## **5. Conclusion**

In this article, we first discussed what are exceptions. An *exception* is an event, which occurs during the execution of a program, that disrupts the normal flow of the program's instructions.

Then, we categorized the exceptions into the Checked Exceptions and the Unchecked Exceptions.

Next, we discussed different types of exceptions that can come up during the compile time or at the runtime.

We can find the code for this article [over on GitHub](https://github.com/eugenp/tutorials/tree/master/core-java-modules/core-java-exceptions).
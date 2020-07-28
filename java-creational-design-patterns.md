# JAVA STRUCTURAL DESIGN PATTERNS

[TOC]



## 1. Singleton Pattern

**Java Singleton Pattern** is one of the **Gangs of Four Design patterns** and comes in the **Creational Design Pattern** category.

- Singleton pattern restricts the  instantiation of a class and ensures that only one instance of the class exists in the JVM.
- The singleton class must provide a global access point to get the instance of the class.
- Singleton pattern is used for [logging](https://www.journaldev.com/977/logger-in-java-logging-example), drivers objects, caching and [thread pool](https://www.journaldev.com/1069/threadpoolexecutor-java-thread-pool-example-executorservice).
- Singleton design pattern is also used in other design patterns like [Abstract Factory](https://www.journaldev.com/1418/abstract-factory-design-pattern-in-java), [Builder](https://www.journaldev.com/1425/builder-design-pattern-in-java), [Prototype](https://www.journaldev.com/1440/prototype-design-pattern-in-java), [Facade](https://www.journaldev.com/1557/facade-design-pattern-in-java) etc.
- Singleton design pattern is used in core java classes also, for example `java.lang.Runtime`, `java.awt.Desktop`.

### Implementation

To implement a Singleton pattern, we have different approaches but all of them have the following common concepts.

- Private constructor to restrict instantiation of the class from other classes.
- Private static variable of the same class that is the only instance of the class.
- Public static method that returns the instance of the class, this is the  global access point for outer world to get the instance of the singleton class.

### A. Eager initialization

In eager initialization, the instance of Singleton Class is created at the time of class loading, this is the easiest method to create a singleton class but it has a drawback that  instance is created even though client application might not be using  it.

Here is the implementation of the static initialization singleton class.

```java
package com.journaldev.singleton;

public class EagerInitializedSingleton {
    
    private static final EagerInitializedSingleton instance = new EagerInitializedSingleton();
    
    //private constructor to avoid client applications to use constructor
    private EagerInitializedSingleton(){}

    public static EagerInitializedSingleton getInstance(){
        return instance;
    }
}
```

If your singleton class is not using a lot of resources, this is the approach to use. But in most of the scenarios, Singleton classes are created for resources such as File System, Database  connections, etc. We should avoid the instantiation until unless client  calls the `getInstance` method. Also, this method doesn’t provide any options for exception handling.

### B. Static block initialization

[Static block](https://www.journaldev.com/1365/static-keyword-in-java) initialization implementation is similar to eager initialization, except that instance of class is created in the static block that  provides option for [exception handling](https://www.journaldev.com/1696/exception-handling-in-java).

```java
package com.journaldev.singleton;

public class StaticBlockSingleton {

    private static StaticBlockSingleton instance;
    
    private StaticBlockSingleton(){}
    
    //static block initialization for exception handling
    static{
        try{
            instance = new StaticBlockSingleton();
        }catch(Exception e){
            throw new RuntimeException("Exception occured in creating singleton instance");
        }
    }
    
    public static StaticBlockSingleton getInstance(){
        return instance;
    }
}
```

Both eager initialization and static block  initialization creates the instance even before it’s being used and that is not the best practice to use. So in further sections, we will learn  how to create a Singleton class that supports lazy initialization.

**Read**: [Java static](https://www.journaldev.com/1365/static-keyword-in-java)

### C. Lazy Initialization

Lazy initialization method to implement Singleton pattern creates the  instance in the global access method. Here is the sample code for  creating Singleton class with this approach.

```java
package com.journaldev.singleton;

public class LazyInitializedSingleton {

    private static LazyInitializedSingleton instance;
    
    private LazyInitializedSingleton(){}
    
    public static LazyInitializedSingleton getInstance(){
        if(instance == null){
            instance = new LazyInitializedSingleton();
        }
        return instance;
    }
}
```

The above implementation works fine in case of the  single-threaded environment but when it comes to multithreaded systems,  it can cause issues if multiple threads are inside the if condition at  the same time. It will destroy the singleton pattern and both threads  will get the different instances of the singleton class.

### D. Thread Safe Singleton

The easier way to create a thread-safe singleton class is to make the global access method [synchronized](https://www.journaldev.com/1061/thread-safety-in-java), so that only one thread can execute this method at a time. General implementation of this approach is like the below class.

```java
package com.journaldev.singleton;

public class ThreadSafeSingleton {

    private static ThreadSafeSingleton instance;
    
    private ThreadSafeSingleton(){}
    
    public static synchronized ThreadSafeSingleton getInstance(){
        if(instance == null){
            instance = new ThreadSafeSingleton();
        }
        return instance;
    }
    
}
```

Above implementation works fine and provides  thread-safety but it reduces the performance because of the cost  associated with the synchronized method, although we need it only for  the first few threads who might create the separate instances (Read: [Java Synchronization](https://www.journaldev.com/1061/thread-safety-in-java)). To avoid this extra overhead every time, **double checked locking** principle is used. In this approach, the synchronized block is used  inside the if condition with an additional check to ensure that only one instance of a singleton class is created.

The following code snippet provides the double-checked locking implementation.

```java
public static ThreadSafeSingleton getInstanceUsingDoubleLocking(){
    if(instance == null){
        synchronized (ThreadSafeSingleton.class) {
            if(instance == null){
                instance = new ThreadSafeSingleton();
            }
        }
    }
    return instance;
}
```

### E. Serialization and Singleton

Sometimes in distributed  systems, we need to implement Serializable interface in Singleton class  so that we can store its state in the file system and retrieve it at a  later point of time. Here is a small singleton class that implements  Serializable interface also.

```java
package com.journaldev.singleton;

import java.io.Serializable;

public class SerializedSingleton implements Serializable{

    private static final long serialVersionUID = -7604766932017737115L;

    private SerializedSingleton(){}
    
    private static class SingletonHelper{
        private static final SerializedSingleton instance = new SerializedSingleton();
    }
    
    public static SerializedSingleton getInstance(){
        return SingletonHelper.instance;
    }
    
}
```

The problem with serialized singleton class is that  whenever we deserialize it, it will create a new instance of the class.  Let’s see it with a simple program.

Let’s see it with a simple program.

```java
package com.journaldev.singleton;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectInput;
import java.io.ObjectInputStream;
import java.io.ObjectOutput;
import java.io.ObjectOutputStream;

public class SingletonSerializedTest {

    public static void main(String[] args) throws FileNotFoundException, IOException, ClassNotFoundException {
        SerializedSingleton instanceOne = SerializedSingleton.getInstance();
        ObjectOutput out = new ObjectOutputStream(new FileOutputStream(
                "filename.ser"));
        out.writeObject(instanceOne);
        out.close();
        
        //deserailize from file to object
        ObjectInput in = new ObjectInputStream(new FileInputStream(
                "filename.ser"));
        SerializedSingleton instanceTwo = (SerializedSingleton) in.readObject();
        in.close();
        
        System.out.println("instanceOne hashCode="+instanceOne.hashCode());
        System.out.println("instanceTwo hashCode="+instanceTwo.hashCode());
        
    }

}
```

The output of the above program is;

```java
instanceOne hashCode=2011117821
instanceTwo hashCode=109647522
```

So it destroys the singleton pattern, to overcome this scenario all we need to do it provide the implementation of `readResolve()` method.

```java
protected Object readResolve() {
    return getInstance();
}
```

After this, you will notice that hashCode of both the instances is same in the test program.

## 2. Factory Pattern

The factory design pattern is used when we have a superclass with  multiple sub-classes and based on input, we need to return one of the  sub-class. This pattern takes out the responsibility of the  instantiation of a class from the client program to the factory class.

### Factory Design Pattern Super Class

Super class in factory design pattern can be an interface, **[abstract class](https://www.journaldev.com/1582/abstract-class-in-java)** or a normal java class. For our factory design pattern example, we have abstract super class with [overridden](https://www.journaldev.com/817/java-override-method-overriding) `toString()` method for testing purpose.

```java
package com.journaldev.design.model;

public abstract class Computer {
	
	public abstract String getRAM();
	public abstract String getHDD();
	public abstract String getCPU();
	
	@Override
	public String toString(){
		return "RAM= "+this.getRAM()+", HDD="+this.getHDD()+", CPU="+this.getCPU();
	}
}
```

### Factory Design Pattern Sub Classes

Let’s say we have two sub-classes PC and Server with below implementation.

```java
package com.journaldev.design.model;

public class PC extends Computer {

	private String ram;
	private String hdd;
	private String cpu;
	
	public PC(String ram, String hdd, String cpu){
		this.ram=ram;
		this.hdd=hdd;
		this.cpu=cpu;
	}
	@Override
	public String getRAM() {
		return this.ram;
	}

	@Override
	public String getHDD() {
		return this.hdd;
	}

	@Override
	public String getCPU() {
		return this.cpu;
	}

}
```

Notice that both the classes are extending `Computer` super class.

```java
package com.journaldev.design.model;

public class Server extends Computer {

	private String ram;
	private String hdd;
	private String cpu;
	
	public Server(String ram, String hdd, String cpu){
		this.ram=ram;
		this.hdd=hdd;
		this.cpu=cpu;
	}
	@Override
	public String getRAM() {
		return this.ram;
	}

	@Override
	public String getHDD() {
		return this.hdd;
	}

	@Override
	public String getCPU() {
		return this.cpu;
	}

}
```

### Factory Class

Now that we have super classes and sub-classes ready, we can write our factory class. Here is the basic implementation.

```java
package com.journaldev.design.factory;

import com.journaldev.design.model.Computer;
import com.journaldev.design.model.PC;
import com.journaldev.design.model.Server;

public class ComputerFactory {

	public static Computer getComputer(String type, String ram, String hdd, String cpu){
		if("PC".equalsIgnoreCase(type)) return new PC(ram, hdd, cpu);
		else if("Server".equalsIgnoreCase(type)) return new Server(ram, hdd, cpu);
		
		return null;
	}
}
```

Some important points about Factory Design Pattern method are;

1. We can keep Factory class Singleton or we can keep the method that returns the subclass as [static](https://www.journaldev.com/1365/static-keyword-in-java).
2. Notice that based on the input parameter, different subclass is created and returned. `getComputer` is the factory method.

[![factory pattern java, factory pattern, factory design pattern, factory pattern class diagram](https://cdn.journaldev.com/wp-content/uploads/2013/05/factory-pattern-java-450x327.png)](https://cdn.journaldev.com/wp-content/uploads/2013/05/factory-pattern-java.png)

Here is a simple test client program that uses above factory design pattern implementation.

```java
package com.journaldev.design.test;

import com.journaldev.design.factory.ComputerFactory;
import com.journaldev.design.model.Computer;

public class TestFactory {

	public static void main(String[] args) {
		Computer pc = ComputerFactory.getComputer("pc","2 GB","500 GB","2.4 GHz");
		Computer server = ComputerFactory.getComputer("server","16 GB","1 TB","2.9 GHz");
		System.out.println("Factory PC Config::"+pc);
		System.out.println("Factory Server Config::"+server);
	}

}
```

Output of above program is:

```java
Factory PC Config::RAM= 2 GB, HDD=500 GB, CPU=2.4 GHz
Factory Server Config::RAM= 16 GB, HDD=1 TB, CPU=2.9 GHz
```

### Factory Design Pattern Advantages

1. Factory design pattern provides approach to code for interface rather than implementation.
2. Factory pattern removes the instantiation of actual implementation classes from client code. Factory pattern makes our code more robust, less coupled  and easy to extend. For example, we can easily change PC class  implementation because client program is unaware of this.
3. Factory pattern provides abstraction between implementation and client classes through inheritance.

### Factory Design Pattern Examples in JDK

1. java.util.Calendar, ResourceBundle and NumberFormat `getInstance()` methods uses Factory pattern.
2. `valueOf()` method in wrapper classes like Boolean, Integer etc.

## 3. Abstract Factory Pattern

If you are familiar with [**factory design pattern in java**](https://www.journaldev.com/1392/factory-design-pattern-in-java), you will notice that we have a single Factory class. This factory class returns different subclasses based on the input provided and factory  class uses if-else or switch statement to achieve this.

In the  Abstract Factory pattern, we get rid of if-else block and have a factory class for each sub-class. Then an Abstract Factory class that will  return the sub-class based on the input factory class. At first, it  seems confusing but once you see the implementation, it’s really easy to grasp and understand the minor difference between Factory and Abstract  Factory pattern.

### Abstract Factory Design Pattern Super Class and Subclasses

```java
Computer.java
package com.journaldev.design.model;
 
public abstract class Computer {
     
    public abstract String getRAM();
    public abstract String getHDD();
    public abstract String getCPU();
     
    @Override
    public String toString(){
        return "RAM= "+this.getRAM()+", HDD="+this.getHDD()+", CPU="+this.getCPU();
    }
}
PC.java
package com.journaldev.design.model;
 
public class PC extends Computer {
 
    private String ram;
    private String hdd;
    private String cpu;
     
    public PC(String ram, String hdd, String cpu){
        this.ram=ram;
        this.hdd=hdd;
        this.cpu=cpu;
    }
    @Override
    public String getRAM() {
        return this.ram;
    }
 
    @Override
    public String getHDD() {
        return this.hdd;
    }
 
    @Override
    public String getCPU() {
        return this.cpu;
    }
 
}
Server.java
package com.journaldev.design.model;
 
 
public class Server extends Computer {
 
    private String ram;
    private String hdd;
    private String cpu;
     
    public Server(String ram, String hdd, String cpu){
        this.ram=ram;
        this.hdd=hdd;
        this.cpu=cpu;
    }
    @Override
    public String getRAM() {
        return this.ram;
    }
 
    @Override
    public String getHDD() {
        return this.hdd;
    }
 
    @Override
    public String getCPU() {
        return this.cpu;
    }
 
}
```

### Factory Class for Each subclass

First of all we need to create a Abstract Factory interface or [**abstract class**](https://www.journaldev.com/1582/abstract-class-in-java).

```java
ComputerAbstractFactory.java
package com.journaldev.design.abstractfactory;

import com.journaldev.design.model.Computer;

public interface ComputerAbstractFactory {

	public Computer createComputer();

}
```

Notice that `createComputer()` method is returning an instance of super class `Computer`. Now our factory classes will implement this interface and return their respective sub-class.

```java
PCFactory.java
package com.journaldev.design.abstractfactory;

import com.journaldev.design.model.Computer;
import com.journaldev.design.model.PC;

public class PCFactory implements ComputerAbstractFactory {

	private String ram;
	private String hdd;
	private String cpu;
	
	public PCFactory(String ram, String hdd, String cpu){
		this.ram=ram;
		this.hdd=hdd;
		this.cpu=cpu;
	}
	@Override
	public Computer createComputer() {
		return new PC(ram,hdd,cpu);
	}

}
```

Similarly we will have a factory class for `Server` subclass.

```java
ServerFactory.java
package com.journaldev.design.abstractfactory;

import com.journaldev.design.model.Computer;
import com.journaldev.design.model.Server;

public class ServerFactory implements ComputerAbstractFactory {

	private String ram;
	private String hdd;
	private String cpu;
	
	public ServerFactory(String ram, String hdd, String cpu){
		this.ram=ram;
		this.hdd=hdd;
		this.cpu=cpu;
	}
	
	@Override
	public Computer createComputer() {
		return new Server(ram,hdd,cpu);
	}

}
```

Now we will create a consumer class that will provide the entry point for the client classes to create sub-classes.

```java
ComputerFactory.java
package com.journaldev.design.abstractfactory;

import com.journaldev.design.model.Computer;

public class ComputerFactory {

	public static Computer getComputer(ComputerAbstractFactory factory){
		return factory.createComputer();
	}
}
```

Notice that its a simple class and `getComputer` method is accepting `ComputerAbstractFactory` argument and returning `Computer` object. At this point the implementation must be getting clear.

Let’s write a simple test method and see how to use the abstract factory to get the instance of sub-classes.

```java
TestDesignPatterns.java
package com.journaldev.design.test;

import com.journaldev.design.abstractfactory.PCFactory;
import com.journaldev.design.abstractfactory.ServerFactory;
import com.journaldev.design.factory.ComputerFactory;
import com.journaldev.design.model.Computer;

public class TestDesignPatterns {

	public static void main(String[] args) {
		testAbstractFactory();
	}

	private static void testAbstractFactory() {
		Computer pc = com.journaldev.design.abstractfactory.ComputerFactory.getComputer(new PCFactory("2 GB","500 GB","2.4 GHz"));
		Computer server = com.journaldev.design.abstractfactory.ComputerFactory.getComputer(new ServerFactory("16 GB","1 TB","2.9 GHz"));
		System.out.println("AbstractFactory PC Config::"+pc);
		System.out.println("AbstractFactory Server Config::"+server);
	}
}
```

Output of the above program will be:

```
AbstractFactory PC Config::RAM= 2 GB, HDD=500 GB, CPU=2.4 GHz
AbstractFactory Server Config::RAM= 16 GB, HDD=1 TB, CPU=2.9 GHz
```

Here is the class diagram of abstract factory design pattern implementation.

[![Abstract Factory Pattern](https://cdn.journaldev.com/wp-content/uploads/2013/06/Abstract-Factory-Pattern-450x354.png)](https://cdn.journaldev.com/wp-content/uploads/2013/06/Abstract-Factory-Pattern.png)

### Abstract Factory Design Pattern Benefits

- Abstract Factory design pattern provides approach to code for interface rather than implementation.
- Abstract Factory pattern is “factory of factories” and can be easily extended to accommodate more products, for example we can add another sub-class  Laptop and a factory LaptopFactory.
- Abstract Factory pattern is robust and avoid conditional logic of Factory pattern.

### Abstract Factory Design Pattern Examples in JDK

- javax.xml.parsers.DocumentBuilderFactory#newInstance()
- javax.xml.transform.TransformerFactory#newInstance()
- javax.xml.xpath.XPathFactory#newInstance()

## 5. Builder Pattern

Builder pattern was introduced to solve some of the problems with  Factory and Abstract Factory design patterns when the Object contains a  lot of attributes.

There are three major issues with Factory and Abstract Factory design patterns when the Object contains a lot of attributes.

1. Too Many arguments to pass from client program to the Factory class that  can be error prone because most of the time, the type of arguments are  same and from client side its hard to maintain the order of the  argument.
2. Some of the parameters might be optional but in  Factory pattern, we are forced to send all the parameters and optional  parameters need to send as NULL.
3. If the object is heavy and its creation is complex, then all that complexity will be part of Factory classes that is confusing.

We can solve the issues with large number of parameters by providing a  constructor with required parameters and then different setter methods  to set the optional parameters. The problem with this approach is that  the Object state will be **inconsistent** until unless all the attributes are set explicitly.

### Implementation

1. First of all you need to create a [static nested class](https://www.journaldev.com/996/java-inner-class) and then copy all the arguments from the outer class to the Builder  class. We should follow the naming convention and if the class name is `Computer` then builder class should be named as `ComputerBuilder`
2. Java Builder class should have a public constructor with all the required attributes as parameters.
3. Java Builder class should have methods to set the optional parameters and it should return the same Builder object after setting the optional  attribute.
4. The final step is to provide a `build()`  method in the builder class that will return the Object needed by client program. For this we need to have a private constructor in the Class  with Builder class as argument.

Here is the sample builder pattern example code where we have a Computer class and ComputerBuilder class to build it.

```java
package com.journaldev.design.builder;

public class Computer {
    
	//required parameters
	private String HDD;
	private String RAM;
	
	//optional parameters
	private boolean isGraphicsCardEnabled;
	private boolean isBluetoothEnabled;
	

	public String getHDD() {
		return HDD;
	}

	public String getRAM() {
		return RAM;
	}

	public boolean isGraphicsCardEnabled() {
		return isGraphicsCardEnabled;
	}

	public boolean isBluetoothEnabled() {
		return isBluetoothEnabled;
	}
	
	private Computer(ComputerBuilder builder) {
		this.HDD=builder.HDD;
		this.RAM=builder.RAM;
		this.isGraphicsCardEnabled=builder.isGraphicsCardEnabled;
		this.isBluetoothEnabled=builder.isBluetoothEnabled;
	}
	
	//Builder Class
	public static class ComputerBuilder{

		// required parameters
		private String HDD;
		private String RAM;

		// optional parameters
		private boolean isGraphicsCardEnabled;
		private boolean isBluetoothEnabled;
		
		public ComputerBuilder(String hdd, String ram){
			this.HDD=hdd;
			this.RAM=ram;
		}

		public ComputerBuilder setGraphicsCardEnabled(boolean isGraphicsCardEnabled) {
			this.isGraphicsCardEnabled = isGraphicsCardEnabled;
			return this;
		}

		public ComputerBuilder setBluetoothEnabled(boolean isBluetoothEnabled) {
			this.isBluetoothEnabled = isBluetoothEnabled;
			return this;
		}
		
		public Computer build(){
			return new Computer(this);
		}

	}

}
```

Notice that Computer class has only getter methods and  no public constructor. So the only way to get a Computer object is  through the ComputerBuilder class.

Here is a builder pattern example test program showing how to use Builder class to get the object.

```java
package com.journaldev.design.test;

import com.journaldev.design.builder.Computer;

public class TestBuilderPattern {

	public static void main(String[] args) {
		//Using builder to get the object in a single line of code and 
                //without any inconsistent state or arguments management issues		
		Computer comp = new Computer.ComputerBuilder(
				"500 GB", "2 GB").setBluetoothEnabled(true)
				.setGraphicsCardEnabled(true).build();
	}

}
```



## 6. Prototype pattern

Prototype design pattern is used when the Object creation is a costly affair and requires a lot of time and resources and you have a similar  object already existing.

Prototype pattern provides a mechanism to copy the original object to a new object and then modify it according  to our needs. Prototype design pattern uses java cloning to copy the  object.

### Prototype Design Pattern Example

It would be easy to understand prototype design pattern with an example.  Suppose we have an Object that loads data from database. Now we need to  modify this data in our program multiple times, so it’s not a good idea  to create the Object using `new` keyword and load all the data again from database.

The better approach would be to clone the existing object into a new object and then do the data manipulation.

Prototype design pattern mandates that the Object which you are copying should provide the copying feature. It should not be done by any other class.  However whether to use shallow or deep copy of the Object properties  depends on the requirements and its a design decision.

Here is a sample program showing Prototype design pattern example in java.

```
Employees.java
```

```java
package com.journaldev.design.prototype;

import java.util.ArrayList;
import java.util.List;

public class Employees implements Cloneable{

	private List<String> empList;
	
	public Employees(){
		empList = new ArrayList<String>();
	}
	
	public Employees(List<String> list){
		this.empList=list;
	}
	public void loadData(){
		//read all employees from database and put into the list
		empList.add("Pankaj");
		empList.add("Raj");
		empList.add("David");
		empList.add("Lisa");
	}
	
	public List<String> getEmpList() {
		return empList;
	}

	@Override
	public Object clone() throws CloneNotSupportedException{
			List<String> temp = new ArrayList<String>();
			for(String s : this.getEmpList()){
				temp.add(s);
			}
			return new Employees(temp);
	}
	
}
```

Notice that the `clone` method is overridden to provide a deep copy of the employees list.

Here is the prototype design pattern example test program that will show the benefit of prototype pattern.

```java
PrototypePatternTest.java
package com.journaldev.design.test;

import java.util.List;

import com.journaldev.design.prototype.Employees;

public class PrototypePatternTest {

	public static void main(String[] args) throws CloneNotSupportedException {
		Employees emps = new Employees();
		emps.loadData();
		
		//Use the clone method to get the Employee object
		Employees empsNew = (Employees) emps.clone();
		Employees empsNew1 = (Employees) emps.clone();
		List<String> list = empsNew.getEmpList();
		list.add("John");
		List<String> list1 = empsNew1.getEmpList();
		list1.remove("Pankaj");
		
		System.out.println("emps List: "+emps.getEmpList());
		System.out.println("empsNew List: "+list);
		System.out.println("empsNew1 List: "+list1);
	}

}
```

Output of the above prototype design pattern example program is:

```java
emps List: [Pankaj, Raj, David, Lisa]
empsNew List: [Pankaj, Raj, David, Lisa, John]
empsNew1 List: [Raj, David, Lisa]
```


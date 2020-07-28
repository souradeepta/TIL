# JAVA Clone

If you want to use **Java Object clone()** method, you have to implement the `java.lang.Cloneable`  marker interface. Otherwise, it will throw `CloneNotSupportedException` at runtime.

Also Object clone is a [protected method](https://www.journaldev.com/2345/java-access-modifiers), so you will have to [override](https://www.journaldev.com/817/java-override-method-overriding) it.

Let’s look at Object cloning in Java with an example program.

```java
package com.journaldev.cloning;

import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;

public class Employee implements Cloneable {

	private int id;

	private String name;

	private Map<String, String> props;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public Map<String, String> getProps() {
		return props;
	}

	public void setProps(Map<String, String> p) {
		this.props = p;
	}

	 @Override
	 public Object clone() throws CloneNotSupportedException {
	 return super.clone();
	 }package com.journaldev.cloning;

import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;

public class Employee implements Cloneable {

	private int id;

	private String name;

	private Map<String, String> props;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public Map<String, String> getProps() {
		return props;
	}

	public void setProps(Map<String, String> p) {
		this.props = p;
	}

	 @Override
	 public Object clone() throws CloneNotSupportedException {
	 return super.clone();
	 }

}

We are using Object clone() method, so we have implemented the Cloneable interface. We are calling the superclass clone() method i.e. Object clone() method.
Using Object clone() Method

Let’s create a test program to use the object clone() method to create a copy of the instance.

package com.journaldev.cloning;

import java.util.HashMap;
import java.util.Map;

public class CloningTest {

	public static void main(String[] args) throws CloneNotSupportedException {

		Employee emp = new Employee();

		emp.setId(1);
		emp.setName("Pankaj");
		Map<String, String> props = new HashMap<>();
		props.put("salary", "10000");
		props.put("city", "Bangalore");
		emp.setProps(props);

		Employee clonedEmp = (Employee) emp.clone();

		// Check whether the emp and clonedEmp attributes are same or different
		System.out.println("emp and clonedEmp == test: " + (emp == clonedEmp));
		
		System.out.println("emp and clonedEmp HashMap == test: " + (emp.getProps() == clonedEmp.getProps()));
		
		// Let's see the effect of using default cloning
		
		// change emp props
		emp.getProps().put("title", "CEO");
		emp.getProps().put("city", "New York");
		System.out.println("clonedEmp props:" + clonedEmp.getProps());

		// change emp name
		emp.setName("new");
		System.out.println("clonedEmp name:" + clonedEmp.getName());

	}

}

Output:

emp and clonedEmp == test: false
emp and clonedEmp HashMap == test: true
clonedEmp props:{city=New York, salary=10000, title=CEO}
clonedEmp name:Pankaj

CloneNotSupportedException at Runtime

If our Employee class won’t implement Cloneable interface, the above program will throw CloneNotSupportedException runtime exception.

Exception in thread "main" java.lang.CloneNotSupportedException: com.journaldev.cloning.Employee
	at java.lang.Object.clone(Native Method)
	at com.journaldev.cloning.Employee.clone(Employee.java:41)
	at com.journaldev.cloning.CloningTest.main(CloningTest.java:19)


}
```

We are using Object clone() method, so we have implemented the `Cloneable` [interface](https://www.journaldev.com/1601/interface-in-java). We are calling the superclass clone() method i.e. Object clone() method.

## Using Object clone() Method

Let’s create a test program to use the object clone() method to create a copy of the instance.

```java
package com.journaldev.cloning;

import java.util.HashMap;
import java.util.Map;

public class CloningTest {

	public static void main(String[] args) throws CloneNotSupportedException {

		Employee emp = new Employee();

		emp.setId(1);
		emp.setName("Pankaj");
		Map<String, String> props = new HashMap<>();
		props.put("salary", "10000");
		props.put("city", "Bangalore");
		emp.setProps(props);

		Employee clonedEmp = (Employee) emp.clone();

		// Check whether the emp and clonedEmp attributes are same or different
		System.out.println("emp and clonedEmp == test: " + (emp == clonedEmp));
		
		System.out.println("emp and clonedEmp HashMap == test: " + (emp.getProps() == clonedEmp.getProps()));
		
		// Let's see the effect of using default cloning
		
		// change emp props
		emp.getProps().put("title", "CEO");
		emp.getProps().put("city", "New York");
		System.out.println("clonedEmp props:" + clonedEmp.getProps());

		// change emp name
		emp.setName("new");
		System.out.println("clonedEmp name:" + clonedEmp.getName());

	}

}
```

Output:

```java
emp and clonedEmp == test: false
emp and clonedEmp HashMap == test: true
clonedEmp props:{city=New York, salary=10000, title=CEO}
clonedEmp name:Pankaj
```

## CloneNotSupportedException at Runtime

If our Employee class won’t implement `Cloneable` interface, the above program will throw `CloneNotSupportedException` runtime [exception](https://www.journaldev.com/1696/exception-handling-in-java).

```java
Exception in thread "main" java.lang.CloneNotSupportedException: com.journaldev.cloning.Employee
	at java.lang.Object.clone(Native Method)
	at com.journaldev.cloning.Employee.clone(Employee.java:41)
	at com.journaldev.cloning.CloningTest.main(CloningTest.java:19)
```



## Understanding Object Cloning

Let’s look into the above output and understand what’s happening with Object `clone()` method.

1. `emp and clonedEmp == test: false`: It means that emp and clonedEmp are two different objects, not  referring to the same object. This is in agreement with the java object  cloning requirement.
2. `emp and clonedEmp HashMap == test: true`: So both emp and clonedEmp object variables refer to the same object.  This can be a serious data integrity issue if we change the underlying  object value. Any change in the value might get reflected to the cloned  instance too.
3. `clonedEmp props:{city=New York, salary=10000, title=CEO}`: We didn’t make any change in clonedEmp properties, but still, they got  changed because both emp and clonedEmp variables are referring to the  same object.This is happening because the default Object clone() method  creates a shallow copy. It can be a problem when you want to create  totally detached objects through cloning process. This can lead to  unwanted results, hence the need to properly override the Object clone() method.
4. `clonedEmp name:Pankaj`: What happened here?We changed the emp name but clonedEmp name didn’t change. It’s because [String is immutable](https://www.journaldev.com/802/string-immutable-final-java). So when we are setting emp name, a new string is created and emp name reference is changed in `this.name = name;`.Hence clonedEmp name remains unchanged. You will find similar behavior for  any primitive variable types too. So we are good with java object  default cloning as long as we have only primitive and immutable  variables in the object.

## Object Cloning Types

There are two types of object cloning – shallow cloning, and deep cloning.  Let’s understand each of them and find out the best way to implement  cloning in our Java programs.

### 1. Shallow Cloning

The default implementation of Java Object clone() method is using shallow copy. It’s using [reflection](https://www.journaldev.com/1789/java-reflection-example-tutorial) API to create the copy of the instance. The below code snippet showcase the shallow cloning implementation.

```java
@Override
 public Object clone() throws CloneNotSupportedException {
 
	 Employee e = new Employee();
	 e.setId(this.id);
	 e.setName(this.name);
	 e.setProps(this.props);
	 return e;
}
```

### 2. Deep Cloning

In deep cloning, we have to copy fields one by one. If we have a field  with nested objects such as List, Map, etc. then we have to write the  code to copy them too one by one. That’s why it’s called deep cloning or deep copy.

We can override the Employee clone method like the following code for deep cloning.

```java
public Object clone() throws CloneNotSupportedException {

	Object obj = super.clone(); //utilize clone Object method

	Employee emp = (Employee) obj;

	// deep cloning for immutable fields
	emp.setProps(null);
	Map<String, String> hm = new HashMap<>();
	String key;
	Iterator<String> it = this.props.keySet().iterator();
	// Deep Copy of field by field
	while (it.hasNext()) {
		key = it.next();
		hm.put(key, this.props.get(key));
	}
	emp.setProps(hm);
	
	return emp;
}

```

With this clone() method implementation, our test program will produce the following output.

```java
emp and clonedEmp == test: false
emp and clonedEmp HashMap == test: false
clonedEmp props:{city=Bangalore, salary=10000}
clonedEmp name:Pankaj
```

In most of the cases, this is what we want. The clone()  method should return a new object totally detached from the original  instance.

So if you are thinking to use Object clone and cloning  in your program, do it wisely and override it properly by taking care of mutable fields.

It could be a daunting task if your class extends other class that in turn extends other class and so on. You will have  to go all the way in the Object inheritance hierarchy to take care of  the deep copy of all the mutable fields.

## Cloning using Serialization?

One way to easily perform deep cloning is through [serialization](https://www.journaldev.com/2452/serialization-in-java). But serialization is an expensive procedure and your class should implement `Serializable` interface. All the fields and superclasses must implement Serializable too.

## Using Apache Commons Util

If you are already using Apache Commons Util classes in your project and your class is serializable, then use the below method.

```java
Employee clonedEmp = org.apache.commons.lang3.SerializationUtils.clone(emp);
```

## Copy Constructor for Cloning

We can define a [copy constructor](https://www.journaldev.com/21078/copy-constructor-in-java) to create a copy of the object. Why to depend on the Object clone() method at all?

For example, we can have an Employee copy constructor like the following code.

```java
public Employee(Employee emp) {
	
	this.setId(emp.getId());
	this.setName(emp.getName());
	
	Map<String, String> hm = new HashMap<>();
	String key;
	Iterator<String> it = emp.getProps().keySet().iterator();
	// Deep Copy of field by field
	while (it.hasNext()) {
		key = it.next();
		hm.put(key, emp.getProps().get(key));
	}
	this.setProps(hm);

}
```
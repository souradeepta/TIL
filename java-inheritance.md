# JAVA Inheritance vs Composition

[TOC]

**Inheritance and composition** are two programming techniques developers use to establish relationships between classes and objects. Whereas  inheritance derives one class from another, composition defines a class  as the sum of its parts.

Classes and objects created through inheritance are *tightly coupled* because changing the parent or superclass in an inheritance  relationship risks breaking your code. Classes and objects created  through composition are *loosely coupled*, meaning that you can more easily change the component parts without breaking your code.

Because loosely coupled code offers more flexibility, many developers have  learned that composition is a better technique than inheritance, but the truth is more complex. 

## When to use inheritance in Java

In object-oriented  programming, we can use inheritance when we know there is an "is a"  relationship between a child and its parent class

In each case, the child or subclass is a *specialized* version  of the parent or superclass. Inheriting from the superclass is an  example of code reuse. To better understand this relationship, take a  moment to study the `Car` class, which inherits from `Vehicle`:

 

```java
class Vehicle {

    String brand;
    String color;
    double weight;
    double speed;

    void move() {
        System.out.println("The vehicle is moving");
    }

}

public class Car extends Vehicle {

    String licensePlateNumber;
    String owner;
    String bodyStyle;

    public static void main(String... inheritanceExample) {
        System.out.println(new Vehicle().brand);
        System.out.println(new Car().brand);
        new Car().move();
    }

}
```

## When to use composition in Java

In object-oriented programming, we can use composition in cases where one object "has" (or is part of) another object

```java
public class CompositionExample {

    public static void main(String... houseComposition) {
        new House(new Bedroom(), new LivingRoom());
        // The house now is composed with a Bedroom and a LivingRoom
    }

    static class House {

        Bedroom bedroom;
        LivingRoom livingRoom;

        House(Bedroom bedroom, LivingRoom livingRoom) {
            this.bedroom = bedroom;
            this.livingRoom = livingRoom;
        }

    }

    static class Bedroom { }

    static class LivingRoom { }

}
```



​		Unlike some languages, such as C++, Java does not allow multiple  inheritance with classes. You can use multiple inheritance with  		interfaces, however. The difference between a class and an interface, in this case, is that interfaces don't keep state.

​	If you attempt multiple inheritance like I have below, the code won't compile:	

```java
class Animal {}
class Mammal {}
class Dog extends Animal, Mammal {}
```

A solution using classes would be to inherit one-by-one:

```java
class Animal {}
class Mammal extends Animal {}
class Dog extends Mammal {}
```

Another solution is to search the classes with interfaces:

```java
interface Animal {}
interface Mammal {}
class Dog implements Animal, Mammal {}
```

In inheritance, we can assign the child class to the parent class  without casting but we can't assign a parent class to the child class  without using casting.

Consider the following example:

```java
public class CastingExample {

    public static void main(String... castingExample) {
        Animal animal = new Animal();
        Dog dogAnimal = (Dog) animal; // We will get ClassCastException
        Dog dog = new Dog();
        Animal dogWithAnimalType = new Dog();
        Dog specificDog = (Dog) dogWithAnimalType;
        specificDog.bark();
        Animal anotherDog = dog; // It's fine here, no need for casting
        System.out.println(((Dog)anotherDog)); // This is another way to cast the object
    }

}

class Animal { }
class Dog extends Animal { void bark() { System.out.println("Au au"); } }
```

### Using ‘super' to access parent classes methods

When two classes are related through inheritance, the child class must be  able to access every accessible field, method, or constructor of its  parent class. In Java, we use the reserved word `super` to ensure the child class can still access its parent's overridden method:

```java
public class SuperWordExample {

    class Character {

        Character() {
            System.out.println("A Character has been created");
        }

        void move() {
            System.out.println("Character walking...");
        }

    }

    class Moe extends Character {

        Moe() {
            super();
        }

        void giveBeer() {
            super.move();
            System.out.println("Give beer");
        }
    }

}
```

In this example, `Character` is the parent class for Moe. Using `super`, we are able to access `Character`'s `move()` method in order to give Moe a beer.

## Using constructors with inheritance

When one class inherits from another, the superclass's constructor always  will be loaded first, before loading its subclass. In most cases, the  reserved word `super` will be added automatically to the  constructor. However, if the superclass has a parameter in its  constructor, we will have to deliberately invoke the `super` constructor, as shown below:

```java
public class ConstructorSuper {

    class Character {

        Character() {
            System.out.println("The super constructor was invoked");
        }

    }

    class Barney extends Character {

        // No need to declare the constructor or to invoke the super constructor
        // The JVM will to that

    }

}
```

If the parent class has a constructor with at least one parameter, then we must declare the constructor in the subclass and use `super` to explicitly invoke the parent constructor. The `super` reserved word won't be added automatically and the code won't compile without it. For example:

```java
public class CustomizedConstructorSuper {

    class Character {

        Character(String name) {
            System.out.println(name + "was invoked");
        }

    }

    class Barney extends Character {

        // We will have compilation error if we don't invoke the constructor explicitly
        // We need to add it
        Barney() {
            super("Barney Gumble");
        }

    }

}
```
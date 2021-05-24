# What Is a Monad? Basic Theory for a Java Developer

As you can guess from the title, the main topic of this article will be monads. I will try to dive deep in their structure and inner workings. With some help of **Java** **Optional**, I will try to describe all of this in a more detailed way. At the end of this article, I will implement a log monad — a type of a writer monad —and then describe the role of each major code fragment and provide a simple example of its usage.

## Why Learn How Monads Work?

First of all, it is always good to have a basic understanding of how things that we use work. If you are a Java developer, you probably use monads without even knowing about it. It may surprise you, but two of the most commonly known **Java 8** features are monad implementations, namely **Stream** and **Optional**. In addition, functional programming becomes more and more popular nowadays so it is possible that we will have more similar monadic structures. Then, this knowledge will be even more very valuable.

Let’s start with describing what a monad is – more or less accurately. In my opinion, the matter here is fairly straightforward.**
**

> ***Monad is just a monoid in the category of endofunctors***
>
> **Based on** **a** **quote** **from** **"**[**Categories for the Working Mathematician**](https://link.springer.com/book/10.1007/978-1-4612-9839-7)**" by Saunders Mac Lane.** 

Back to being serious...

Monad is a concept, not a class or trait. Of course, it can be further implemented as a class or trait. It is possible in almost any statically typed language with generics support. Moreover, we can view it as a wrapper which puts our value in some context and allows us to perform operations on the value. In this context, the output of an operation at any step is the input to the operation at the next step.

Examples of monads in modern day programming languages:

- Stream (Java).
- Optional/Option (Java/Scala).
-  Either (Scala).
- Try (Scala).
- IO Monad (Haskell).

The last thing that needs mentioning while speaking of monads are their laws. If we want to consider our implementation a real monad, we must obey them. There are three laws: **left identity**, **right identity** and **associativity**. In my opinion, it can be somewhat hard to understand what they actually mean.
Now with the help of Optional I will try to explain above laws in a more detailed way.

But first a few assumptions:

- F is a function with the signature: `(T -> Optional<U>) = Optional<U>`.
- G is a function with the signature `(A -> Optional<B>) = Optional<B>`.
- `FG = F.apply(value).flatMap(G)` with the signature: `(T -> Optional<B>) = Optional<B>`.

**Left identity**: If we create a new monad and bind it to the function, the result should be the same as applying the function to the value: `Optional.of(value).flatMap(F).equals(F.apply(value))`

**Right identity**: The result of binding a unit function to a monad should be the same as the creation of a new monad: `Optional.of(value).flatMap(Optional::of).equals(Optional.of(value))`

**Associativity** - in the chain of function applications, it should not matter how functions are nested:

- `Optional<B> leftSide= Optional.of(value).flatMap(F).flatMap(G)`
- `Optional<B> rightSide= Optional.of(value).flatMap(F.apply(value)`
  `.flatMap(G))`
- `leftSide.equals(rightSide).`

Now, when we know the basics, we can focus on implementation.

The first thing we need is a parameterized type **M<T>**, which is a wrapper for our value of type **T**. Our type must implement two functions:

- `unit` which is used to wrap our value and has a following signature `(T) = M<T>`.
- `bind` responsible for performing operations. Here we pass a function which operates on value in our context and returns it with other type already wrapped in context. This method should have the following signature `(T -> M<U>) = M<U>`.

To make it more understandable, I will use Optional one more time and show what the above structure looks like in its case.

Here, the first condition is met right away because Optional is a parameterized type. The role of the `unit` function is fulfilled by `ofNullable` and `of` methods. `FlatMap` plays the role of the `bind` function. Of course, in the case of Optional, type boundaries allow us to use more complex types than in the definition above.

### **Done With the Theory, Let’s Implement**

Java

1

```
//imports
```

2

```
public final class LogMonad<T> {
```

3

4

```
    private final T value;
```

5

6

```
    private static final Logger LOGGER = 
```

7

```
      Logger.getLogger(LogMonad.class.getName());
```

8

9

```
    private LogMonad() {
```

10

```
        this.value = null;
```

11

```
    }
```

12

13

```
    private LogMonad(T value) {
```

14

```
        this.value = value;
```

15

```
    }
```

16

17

```
    static <T> LogMonad<T> of(T value) {
```

18

```
        Objects.requireNonNull(value, "value is null");
```

19

```
        return new LogMonad<>(value);
```

20

```
    }
```

21

22

```
    <U> LogMonad<U> flatMap(Function<T, LogMonad<U>> function) {
```

23

```
        Objects.requireNonNull(function, "function is null");
```

24

```
        try {
```

25

```
            return function.apply(value);
```

26

```
        } catch (Throwable t) {
```

27

```
            return new LogMonad<>();
```

28

```
        }
```

29

```
    }
```

30

31

```
    LogMonad<T> log() {
```

32

```
        LOGGER.log(Level.INFO, "{0}", value);
```

33

```
        return this;
```

34

```
    }
```

35

36

```
    LogMonad<T> log(Level loggingLevel) {
```

37

```
        Objects.requireNonNull(loggingLevel, "logging level is null");
```

38

```
        LOGGER.log(loggingLevel, "{0}", value);
```

39

```
        return this;
```

40

```
    }
```

41

```
//equals, hashCode & toString
```

42

```
}
```


Et voila, monad implemented. Let’s describe in detail what exactly I have done here.

The base of our implementation is the parameterized class with the immutable field named "value", which is responsible for storing our value. The second field is "Logger", which will be responsible for the key effect of our monad. Then, we have a private constructor, which makes it impossible to create an object in any other way than through our wrapping method. 

Next, we have two basic monad functions, namely **of (equivalent of unit)** and **flatMap (equivalent of bind)**, which will guarantee that our implementation fulfills the required conditions in form of monad laws. The last two methods are responsible for our monad effect, i.e. they are responsible for logging the current value to standard output. One of them allows to pass the logging level, the other one uses “INFO” level.

With the functionalities described it is time for a usage example. So here it is.

Java

1

```
//imports
```

2

```
public class Example {
```

3

```
  
```

4

```
    private static final Logger LOGGER = Logger.getLogger(Example.class.getName());
```

5

6

```
    public static void main(String[] args) {
```

7

```
        String value = "5";
```

8

9

```
        Function<String, LogMonad<Integer>> hashCodeMonadic =
```

10

```
          input -> LogMonad.of(input.hashCode());
```

11

```
        Function<Integer, LogMonad<List<Integer>>> toListMonadic = 
```

12

```
          input -> LogMonad.of(List.of(input));
```

13

```
      
```

14

```
         LogMonad.of(value)
```

15

```
                .flatMap(hashCodeMonadic)
```

16

```
                .log()
```

17

```
                .flatMap(toListMonadic)
```

18

```
                .log(Level.WARNING);
```

19

20

```
        Function<String, Integer> hashCode = String::hashCode;
```

21

```
        Function<Integer, List<Integer>> toList = List::of;
```

22

```
        Integer valueHashCode = hashCode.apply(value);
```

23

```
        LOGGER.info(valueHashCode.toString());
```

24

```
        List<Integer> hashCodeList = toList.apply(valueHashCode);
```

25

```
        LOGGER.log(Level.WARNING, "{0}", hashCodeList);
```

26

```
    }
```

27

```
}
```


In the code above, other than seeing how monads work, we can also see a few pros of using them.

First of all we encapsulate our logging side-effect within a monadic context. That provided an abstraction layer over our logic. Through this abstraction we were able to reduce the amount of boilerplate. Our code became more declarative, easier to read and understand. Finally, we combined all operations into a single pipeline.

On the other hand, in the non monadic part of example, we can see all the implementation details. The code is less descriptive and reusable. What is more, we exposed our side-effect that may cause some problems in the future and made our code less readable. If we ever decide to handle an error from a function call, we will need a lot of boilerplate.

Moreover, in my opinion, the monadic code is easier to read and understand than its non monadic counterpart.

## **Conclusion**

Monad is a very useful concept. Probably many of us use it in our day to day life. In this article I tried to provide a clear and descriptive explanation of its theoretical basis and logic behind it. I implemented LogMonad to show that it is not rocket science and in fact it is fairly simple to implement one. In the example I showed what the use of monad may look like, what the potential pros of such an action are and how it can differ from normal methods call.
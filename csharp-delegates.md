# Delegates (C# Programming Guide)

A [delegate](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/reference-types) is a type that represents references to methods with a particular parameter list and return type. When you instantiate a delegate, you can associate its instance with any method with a compatible signature and return type. You can invoke (or call) the method through the delegate instance.

Delegates are used to pass methods as arguments to other methods. Event handlers are nothing more than methods that are invoked through delegates. You create a custom method, and a class such as a windows control can call your method when a certain event occurs. The following example shows a delegate declaration:

```csharp
public delegate int PerformCalculation(int x, int y);
```

Any method from any accessible class or struct that matches the delegate type can be assigned to the delegate. The method can be either static or an instance method. This makes it possible to programmatically change method calls, and also plug new code into existing classes.

 Note

In the context of method overloading, the signature of a method does not include the return value. But in the context of delegates, the signature does include the return value. In other words, a method must have the same return type as the delegate.

This ability to refer to a method as a parameter makes delegates ideal for defining callback methods. For example, a reference to a method that compares two objects could be passed as an argument to a sort algorithm. Because the comparison code is in a separate procedure, the sort algorithm can be written in a more general way.

## Delegates Overview

Delegates have the following properties:

- Delegates are similar to C++ function pointers, but delegates are fully object-oriented, and unlike C++ pointers to member functions, delegates encapsulate both an object instance and a method.
- Delegates allow methods to be passed as parameters.
- Delegates can be used to define callback methods.
- Delegates can be chained together; for example, multiple methods can be called on a single event.
- Methods do not have to match the delegate type exactly. For more information, see [Using Variance in Delegates](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/covariance-contravariance/using-variance-in-delegates).
- C# version 2.0 introduced the concept of [anonymous methods](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/delegate-operator), which allow code blocks to be passed as parameters in place of a separately defined method. C# 3.0 introduced lambda expressions as a more concise way of writing inline code blocks. Both anonymous methods and lambda expressions (in certain contexts) are compiled to delegate types. Together, these features are now known as anonymous functions. For more information about lambda expressions, see [Lambda expressions](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-expressions).



# Using Delegates (C# Programming Guide)

A [delegate](https://docs.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2010/900fyy8e(v=vs.100)) is a type that safely encapsulates a method, similar to a function pointer in C and C++. Unlike C function pointers, delegates are object-oriented, type safe, and secure. The type of a delegate is defined by the name of the delegate. The following example declares a delegate named Del that can encapsulate a method that takes a [string](https://docs.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2010/362314fe(v=vs.100)) as an argument and returns [void](https://docs.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2010/yah0tteb(v=vs.100)):

```csharp
public delegate void Del(string message);
```

A delegate object is normally constructed by providing the name of the method the delegate will wrap, or with an [anonymous Method](https://docs.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2010/0yw3tz5k(v=vs.100)). Once a delegate is instantiated, a method call made to the delegate will be passed by the delegate to that method. The parameters passed to the delegate by the caller are passed to the method, and the return value, if any, from the method is returned to the caller by the delegate. This is known as invoking the delegate. An instantiated delegate can be invoked as if it were the wrapped method itself. For example:

```csharp
// Create a method for a delegate.
public static void DelegateMethod(string message)
{
    System.Console.WriteLine(message);
}
```

```csharp
// Instantiate the delegate.
Del handler = DelegateMethod;

// Call the delegate.
handler("Hello World");
```

Delegate types are derived from the [Delegate](https://msdn.microsoft.com/en-us/library/y22acf51(v=vs.100)) class in the .NET Framework. Delegate types are [sealed](https://docs.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2010/88c54tsw(v=vs.100))—they cannot be derived from— and it is not possible to derive custom classes from [Delegate](https://msdn.microsoft.com/en-us/library/y22acf51(v=vs.100)). Because the instantiated delegate is an object, it can be passed as a parameter, or assigned to a property. This allows a method to accept a delegate as a parameter, and call the delegate at some later time. This is known as an asynchronous callback, and is a common method of notifying a caller when a long process has completed. When a delegate is used in this fashion, the code using the delegate does not need any knowledge of the implementation of the method being used. The functionality is similar to the encapsulation interfaces provide. For more information, see [When to Use Delegates Instead of Interfaces](https://docs.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2010/ms173173(v=vs.100)).

Another common use of callbacks is defining a custom comparison method and passing that delegate to a sort method. It allows the caller's code to become part of the sort algorithm. The following example method uses the Del type as a parameter:

C#Copy

```csharp
public void MethodWithCallback(int param1, int param2, Del callback)
{
    callback("The number is: " + (param1 + param2).ToString());
}
```

You can then pass the delegate created above to that method:

C#Copy

```csharp
MethodWithCallback(1, 2, handler);
```

and receive the following output to the console:

The number is: 3

Using the delegate as an abstraction, MethodWithCallback does not need to call the console directly—it does not have to be designed with a console in mind. What MethodWithCallback does is simply prepare a string and pass the string to another method. This is especially powerful since a delegated method can use any number of parameters.

When a delegate is constructed to wrap an instance method, the delegate references both the instance and the method. A delegate has no knowledge of the instance type aside from the method it wraps, so a delegate can refer to any type of object as long as there is a method on that object that matches the delegate signature. When a delegate is constructed to wrap a static method, it only references the method. Consider the following declarations:

C#Copy

```csharp
public class MethodClass
{
    public void Method1(string message) { }
    public void Method2(string message) { }
}
```

Along with the static DelegateMethod shown previously, we now have three methods that can be wrapped by a Del instance.

A delegate can call more than one method when invoked. This is referred to as multicasting. To add an extra method to the delegate's list of methods—the invocation list—simply requires adding two delegates using the addition or addition assignment operators ('+' or '+='). For example:

C#Copy

```csharp
MethodClass obj = new MethodClass();
Del d1 = obj.Method1;
Del d2 = obj.Method2;
Del d3 = DelegateMethod;

//Both types of assignment are valid.
Del allMethodsDelegate = d1 + d2;
allMethodsDelegate += d3;
```

At this point allMethodsDelegate contains three methods in its invocation list—Method1, Method2, and DelegateMethod. The original three delegates, d1, d2, and d3, remain unchanged. When allMethodsDelegate is invoked, all three methods are called in order. If the delegate uses reference parameters, the reference is passed sequentially to each of the three methods in turn, and any changes by one method are visible to the next method. When any of the methods throws an exception that is not caught within the method, that exception is passed to the caller of the delegate and no subsequent methods in the invocation list are called. If the delegate has a return value and/or out parameters, it returns the return value and parameters of the last method invoked. To remove a method from the invocation list, use the decrement or decrement assignment operator ('-' or '-='). For example:

C#Copy

```csharp
//remove Method1
allMethodsDelegate -= d1;

// copy AllMethodsDelegate while removing d2
Del oneMethodDelegate = allMethodsDelegate - d2;
```

Because delegate types are derived from System.Delegate, the methods and properties defined by that class can be called on the delegate. For example, to find the number of methods in a delegate's invocation list, you may write:

C#Copy

```csharp
int invocationCount = d1.GetInvocationList().GetLength(0);
```

Delegates with more than one method in their invocation list derive from [MulticastDelegate](https://msdn.microsoft.com/en-us/library/1kswf507(v=vs.100)), which is a subclass of System.Delegate. The above code works in either case because both classes support GetInvocationList.

Multicast delegates are used extensively in event handling. Event source objects send event notifications to recipient objects that have registered to receive that event. To register for an event, the recipient creates a method designed to handle the event, then creates a delegate for that method and passes the delegate to the event source. The source calls the delegate when the event occurs. The delegate then calls the event handling method on the recipient, delivering the event data. The delegate type for a given event is defined by the event source. For more, see [Events (C# Programming Guide)](https://docs.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2010/awbftdfh(v=vs.100)).

Comparing delegates of two different types assigned at compile-time will result in a compilation error. If the delegate instances are statically of the type System.Delegate, then the comparison is allowed, but will return false at run time. For example:

C#Copy

```csharp
delegate void Delegate1();
delegate void Delegate2();

static void method(Delegate1 d, Delegate2 e, System.Delegate f)
{
    // Compile-time error.
    //Console.WriteLine(d == e);

    // OK at compile-time. False if the run-time type of f 
    // is not the same as that of d.
    System.Console.WriteLine(d == f);
}
```



# When to Use Delegates Instead of Interfaces (C# Programming Guide)

Both delegates and interfaces enable a class designer to separate type declarations and implementation. A given [interface](https://docs.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2010/87d83y5b(v=vs.100)) can be inherited and implemented by any [class](https://docs.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2010/0b0thckt(v=vs.100)) or [struct](https://docs.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2010/ah19swz4(v=vs.100)). A [delegate](https://docs.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2010/900fyy8e(v=vs.100)) can be created for a method on any class, as long as the method fits the method signature for the delegate. An interface reference or a delegate can be used by an object that has no knowledge of the class that implements the interface or delegate method. Given these similarities, when should a class designer use a delegate and when should it use an interface?

Use a delegate in the following circumstances:

- An eventing design pattern is used.
- It is desirable to encapsulate a static method.
- The caller has no need to access other properties, methods, or interfaces on the object implementing the method.
- Easy composition is desired.
- A class may need more than one implementation of the method.

Use an interface in the following circumstances:

- There is a group of related methods that may be called.
- A class only needs one implementation of the method.
- The class using the interface will want to cast that interface to other interface or class types.
- The method being implemented is linked to the type or identity of the class: for example, comparison methods.

One good example of using a single-method interface instead of a delegate is [IComparable](https://msdn.microsoft.com/en-us/library/ey2t2ys5(v=vs.100)) or the generic version, [IComparable](https://msdn.microsoft.com/en-us/library/4d7sx9hd(v=vs.100)). **IComparable** declares the [CompareTo](https://msdn.microsoft.com/en-us/library/4ah99705(v=vs.100)) method, which returns an integer that specifies a less than, equal to, or greater than relationship between two objects of the same type. **IComparable** can be used as the basis of a sort algorithm. Although using a delegate comparison method as the basis of a sort algorithm would be valid, it is not ideal. Because the ability to compare belongs to the class and the comparison algorithm does not change at run time, a single-method interface is ideal.
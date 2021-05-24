# Tuple Class

- Namespace:

  [System](https://docs.microsoft.com/en-us/dotnet/api/system?view=netcore-3.1)

- Assembly:

  System.Runtime.dll

Provides static methods for creating tuple objects.

C#Copy

```csharp
public static class Tuple
```

- Inheritance

  [Object](https://docs.microsoft.com/en-us/dotnet/api/system.object?view=netcore-3.1)Tuple

## Examples

The following example creates an 8-tuple (octuple) that contains prime numbers that are less than 20.

C#CopyRun

```csharp
var primes = Tuple.Create(2, 3, 5, 7, 11, 13, 17, 19);
Console.WriteLine("Prime numbers less than 20: " + 
                  "{0}, {1}, {2}, {3}, {4}, {5}, {6}, and {7}",
                  primes.Item1, primes.Item2, primes.Item3, 
                  primes.Item4, primes.Item5, primes.Item6,
                  primes.Item7, primes.Rest.Item1);
// The example displays the following output:
//    Prime numbers less than 20: 2, 3, 5, 7, 11, 13, 17, and 19
```

## Remarks

A tuple is a data structure that has a specific number and sequence of elements. An example of a tuple is a data structure with three elements (known as a 3-tuple or triple) that is used to store an identifier such as a person's name in the first element, a year in the second element, and the person's income for that year in the third element. The .NET Framework directly supports tuples with one to seven elements. In addition, you can create tuples of eight or more elements by nesting tuple objects in the [Rest](https://docs.microsoft.com/en-us/dotnet/api/system.tuple-8.rest?view=netcore-3.1) property of a [Tuple](https://docs.microsoft.com/en-us/dotnet/api/system.tuple-8?view=netcore-3.1) object.

Tuples are commonly used in four ways:

- To represent a single set of data. For example, a tuple can represent a database record, and its components can represent individual fields of the record.
- To provide easy access to, and manipulation of, a data set.
- To return multiple values from a method without using `out` parameters (in C#) or `ByRef` parameters (in Visual Basic).
- To pass multiple values to a method through a single parameter. For example, the [Thread.Start(Object)](https://docs.microsoft.com/en-us/dotnet/api/system.threading.thread.start?view=netcore-3.1#System_Threading_Thread_Start_System_Object_) method has a single parameter that lets you supply one value to the method that the thread executes at startup time. If you supply a [Tuple](https://docs.microsoft.com/en-us/dotnet/api/system.tuple-3?view=netcore-3.1) object as the method argument, you can supply the thread's startup routine with three items of data.

The [Tuple](https://docs.microsoft.com/en-us/dotnet/api/system.tuple?view=netcore-3.1) class does not itself represent a tuple. Instead, it is a class that provides static methods for creating instances of the tuple types that are supported by the .NET Framework. It provides helper methods that you can call to instantiate tuple objects without having to explicitly specify the type of each tuple component.

Although you can create an instance of a tuple class by calling its class constructor, the code to do so can be cumbersome. The following example uses a class constructor to create a 7-tuple or septuple that contains population data for New York City for each census from 1950 through 2000.

C#CopyRun

```csharp
// Create a 7-tuple.
var population = new Tuple<string, int, int, int, int, int, int>(
                           "New York", 7891957, 7781984, 
                           7894862, 7071639, 7322564, 8008278);
// Display the first and last elements.
Console.WriteLine("Population of {0} in 2000: {1:N0}",
                  population.Item1, population.Item7);
// The example displays the following output:
//       Population of New York in 2000: 8,008,278
```

Creating the same tuple object by using a helper method is more straightforward, as the following example shows.

C#CopyRun

```csharp
// Create a 7-tuple.
var population = Tuple.Create("New York", 7891957, 7781984, 7894862, 7071639, 7322564, 8008278);
// Display the first and last elements.
Console.WriteLine("Population of {0} in 2000: {1:N0}",
                  population.Item1, population.Item7);
// The example displays the following output:
//       Population of New York in 2000: 8,008,278
```

The [Create](https://docs.microsoft.com/en-us/dotnet/api/system.tuple.create?view=netcore-3.1) helper methods directly support the creation of tuple objects that have from one to eight components (that is, singletons through octuples). Although there is no practical limit to the number of components a tuple may have, helper methods are not available to create a tuple with nine or more components. To create such a tuple, you must call the [Tuple.Tuple](https://docs.microsoft.com/en-us/dotnet/api/system.tuple-8.-ctor?view=netcore-3.1) constructor.
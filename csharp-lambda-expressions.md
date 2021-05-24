# Lambda expressions (C# reference)

A *lambda expression* is an expression of any of the following two forms:

- [Expression lambda](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-expressions#expression-lambdas) that has an expression as its body:

  C#Copy

  ```csharp
  (input-parameters) => expression
  ```

- [Statement lambda](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-expressions#statement-lambdas) that has a statement block as its body:

  C#Copy

  ```csharp
  (input-parameters) => { <sequence-of-statements> }
  ```

Use the [lambda declaration operator `=>`](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-operator) to separate the lambda's parameter list from its body. To create a lambda expression, you specify input parameters (if any) on the left side of the lambda operator and an expression or a statement block on the other side.

Any lambda expression can be converted to a [delegate](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/reference-types#the-delegate-type) type. The delegate type to which a lambda expression can be converted is defined by the types of its parameters and return value. If a lambda expression doesn't return a value, it can be converted to one of the `Action` delegate types; otherwise, it can be converted to one of the `Func` delegate types. For example, a lambda expression that has two parameters and returns no value can be converted to an [Action](https://docs.microsoft.com/en-us/dotnet/api/system.action-2) delegate. A lambda expression that has one parameter and returns a value can be converted to a [Func](https://docs.microsoft.com/en-us/dotnet/api/system.func-2) delegate. In the following example, the lambda expression `x => x * x`, which specifies a parameter that's named `x` and returns the value of `x` squared, is assigned to a variable of a delegate type:

C#CopyRun

```csharp
Func<int, int> square = x => x * x;
Console.WriteLine(square(5));
// Output:
// 25
```

Expression lambdas can also be converted to the [expression tree](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/expression-trees/) types, as the following example shows:

C#CopyRun

```csharp
System.Linq.Expressions.Expression<Func<int, int>> e = x => x * x;
Console.WriteLine(e);
// Output:
// x => (x * x)
```

You can use lambda expressions in any code that requires instances of delegate types or expression trees, for example as an argument to the [Task.Run(Action)](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.run#System_Threading_Tasks_Task_Run_System_Action_) method to pass the code that should be executed in the background. You can also use lambda expressions when you write [LINQ in C#](https://docs.microsoft.com/en-us/dotnet/csharp/linq/), as the following example shows:

C#CopyRun

```csharp
int[] numbers = { 2, 3, 4, 5 };
var squaredNumbers = numbers.Select(x => x * x);
Console.WriteLine(string.Join(" ", squaredNumbers));
// Output:
// 4 9 16 25
```

When you use method-based syntax to call the [Enumerable.Select](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.select) method in the [System.Linq.Enumerable](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable) class, for example in LINQ to Objects and LINQ to XML, the parameter is a delegate type [System.Func](https://docs.microsoft.com/en-us/dotnet/api/system.func-2). When you call the [Queryable.Select](https://docs.microsoft.com/en-us/dotnet/api/system.linq.queryable.select) method in the [System.Linq.Queryable](https://docs.microsoft.com/en-us/dotnet/api/system.linq.queryable) class, for example in LINQ to SQL, the parameter type is an expression tree type [`Expression>`](https://docs.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression-1). In both cases you can use the same lambda expression to specify the parameter value. That makes the two `Select` calls to look similar although in fact the type of objects created from the lambdas is different.

## Expression lambdas

A lambda expression with an expression on the right side of the `=>` operator is called an *expression lambda*. An expression lambda returns the result of the expression and takes the following basic form:

C#Copy

```csharp
(input-parameters) => expression
```

The body of an expression lambda can consist of a method call. However, if you are creating [expression trees](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/expression-trees/) that are evaluated outside the context of the .NET common language runtime, such as in SQL Server, you should not use method calls in lambda expressions. The methods will have no meaning outside the context of the .NET common language runtime.

## Statement lambdas

A statement lambda resembles an expression lambda except that its statements are enclosed in braces:

C#Copy

```csharp
(input-parameters) => { <sequence-of-statements> }
```

The body of a statement lambda can consist of any number of statements; however, in practice there are typically no more than two or three.

C#CopyRun

```csharp
Action<string> greet = name =>
{
    string greeting = $"Hello {name}!";
    Console.WriteLine(greeting);
};
greet("World");
// Output:
// Hello World!
```

You cannot use statement lambdas to create expression trees.

## Input parameters of a lambda expression

You enclose input parameters of a lambda expression in parentheses. Specify zero input parameters with empty parentheses:

C#Copy

```csharp
Action line = () => Console.WriteLine();
```

If a lambda expression has only one input parameter, parentheses are optional:

C#Copy

```csharp
Func<double, double> cube = x => x * x * x;
```

Two or more input parameters are separated by commas:

C#Copy

```csharp
Func<int, int, bool> testForEquality = (x, y) => x == y;
```

Sometimes the compiler can't infer the types of input parameters. You can specify the types explicitly as shown in the following example:

C#Copy

```csharp
Func<int, string, bool> isTooLong = (int x, string s) => s.Length > x;
```

Input parameter types must be all explicit or all implicit; otherwise, a [CS0748](https://docs.microsoft.com/en-us/dotnet/csharp/misc/cs0748) compiler error occurs.

Beginning with C# 9.0, you can use [discards](https://docs.microsoft.com/en-us/dotnet/csharp/discards) to specify two or more input parameters of a lambda expression that aren't used in the expression:

C#Copy

```csharp
Func<int, int, int> constant = (_, _) => 42;
```

Lambda discard parameters may be useful when you use a lambda expression to [provide an event handler](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/events/how-to-subscribe-to-and-unsubscribe-from-events).

 Note

For backwards compatibility, if only a single input parameter is named `_`, then, within a lambda expression, `_` is treated as the name of that parameter.

## Async lambdas

You can easily create lambda expressions and statements that incorporate asynchronous processing by using the [async](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/async) and [await](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/await) keywords. For example, the following Windows Forms example contains an event handler that calls and awaits an async method, `ExampleMethodAsync`.

C#Copy

```csharp
public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
        button1.Click += button1_Click;
    }

    private async void button1_Click(object sender, EventArgs e)
    {
        await ExampleMethodAsync();
        textBox1.Text += "\r\nControl returned to Click event handler.\n";
    }

    private async Task ExampleMethodAsync()
    {
        // The following line simulates a task-returning asynchronous process.
        await Task.Delay(1000);
    }
}
```

You can add the same event handler by using an async lambda. To add this handler, add an `async` modifier before the lambda parameter list, as the following example shows:

C#Copy

```csharp
public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
        button1.Click += async (sender, e) =>
        {
            await ExampleMethodAsync();
            textBox1.Text += "\r\nControl returned to Click event handler.\n";
        };
    }

    private async Task ExampleMethodAsync()
    {
        // The following line simulates a task-returning asynchronous process.
        await Task.Delay(1000);
    }
}
```

For more information about how to create and use async methods, see [Asynchronous Programming with async and await](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/async/).

## Lambda expressions and tuples

Starting with C# 7.0, the C# language provides built-in support for [tuples](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/value-tuples). You can provide a tuple as an argument to a lambda expression, and your lambda expression can also return a tuple. In some cases, the C# compiler uses type inference to determine the types of tuple components.

You define a tuple by enclosing a comma-delimited list of its components in parentheses. The following example uses tuple with three components to pass a sequence of numbers to a lambda expression, which doubles each value and returns a tuple with three components that contains the result of the multiplications.

C#CopyRun

```csharp
Func<(int, int, int), (int, int, int)> doubleThem = ns => (2 * ns.Item1, 2 * ns.Item2, 2 * ns.Item3);
var numbers = (2, 3, 4);
var doubledNumbers = doubleThem(numbers);
Console.WriteLine($"The set {numbers} doubled: {doubledNumbers}");
// Output:
// The set (2, 3, 4) doubled: (4, 6, 8)
```

Ordinarily, the fields of a tuple are named `Item1`, `Item2`, etc. You can, however, define a tuple with named components, as the following example does.

C#CopyRun

```csharp
Func<(int n1, int n2, int n3), (int, int, int)> doubleThem = ns => (2 * ns.n1, 2 * ns.n2, 2 * ns.n3);
var numbers = (2, 3, 4);
var doubledNumbers = doubleThem(numbers);
Console.WriteLine($"The set {numbers} doubled: {doubledNumbers}");
```

For more information about C# tuples, see [Tuple types](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/value-tuples).

## Lambdas with the standard query operators

LINQ to Objects, among other implementations, have an input parameter whose type is one of the [Func](https://docs.microsoft.com/en-us/dotnet/api/system.func-1) family of generic delegates. These delegates use type parameters to define the number and type of input parameters, and the return type of the delegate. `Func` delegates are very useful for encapsulating user-defined expressions that are applied to each element in a set of source data. For example, consider the [Func](https://docs.microsoft.com/en-us/dotnet/api/system.func-2) delegate type:

C#Copy

```csharp
public delegate TResult Func<in T, out TResult>(T arg)
```

The delegate can be instantiated as a `Func<int, bool>` instance where `int` is an input parameter and `bool` is the return value. The return value is always specified in the last type parameter. For example, `Func<int, string, bool>` defines a delegate with two input parameters, `int` and `string`, and a return type of `bool`. The following `Func` delegate, when it's invoked, returns Boolean value that indicates whether the input parameter is equal to five:

C#CopyRun

```csharp
Func<int, bool> equalsFive = x => x == 5;
bool result = equalsFive(4);
Console.WriteLine(result);   // False
```

You can also supply a lambda expression when the argument type is an [Expression](https://docs.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression-1), for example in the standard query operators that are defined in the [Queryable](https://docs.microsoft.com/en-us/dotnet/api/system.linq.queryable) type. When you specify an [Expression](https://docs.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression-1) argument, the lambda is compiled to an expression tree.

The following example uses the [Count](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.count) standard query operator:

C#CopyRun

```csharp
int[] numbers = { 5, 4, 1, 3, 9, 8, 6, 7, 2, 0 };
int oddNumbers = numbers.Count(n => n % 2 == 1);
Console.WriteLine($"There are {oddNumbers} odd numbers in {string.Join(" ", numbers)}");
```

The compiler can infer the type of the input parameter, or you can also specify it explicitly. This particular lambda expression counts those integers (`n`) which when divided by two have a remainder of 1.

The following example produces a sequence that contains all elements in the `numbers` array that precede the 9, because that's the first number in the sequence that doesn't meet the condition:

C#CopyRun

```csharp
int[] numbers = { 5, 4, 1, 3, 9, 8, 6, 7, 2, 0 };
var firstNumbersLessThanSix = numbers.TakeWhile(n => n < 6);
Console.WriteLine(string.Join(" ", firstNumbersLessThanSix));
// Output:
// 5 4 1 3
```

The following example specifies multiple input parameters by enclosing them in parentheses. The method returns all the elements in the `numbers` array until it encounters a number whose value is less than its ordinal position in the array:

C#CopyRun

```csharp
int[] numbers = { 5, 4, 1, 3, 9, 8, 6, 7, 2, 0 };
var firstSmallNumbers = numbers.TakeWhile((n, index) => n >= index);
Console.WriteLine(string.Join(" ", firstSmallNumbers));
// Output:
// 5 4
```

## Type inference in lambda expressions

When writing lambdas, you often don't have to specify a type for the input parameters because the compiler can infer the type based on the lambda body, the parameter types, and other factors as described in the C# language specification. For most of the standard query operators, the first input is the type of the elements in the source sequence. If you are querying an `IEnumerable<Customer>`, then the input variable is inferred to be a `Customer` object, which means you have access to its methods and properties:

C#Copy

```csharp
customers.Where(c => c.City == "London");
```

The general rules for type inference for lambdas are as follows:

- The lambda must contain the same number of parameters as the delegate type.
- Each input parameter in the lambda must be implicitly convertible to its corresponding delegate parameter.
- The return value of the lambda (if any) must be implicitly convertible to the delegate's return type.

Note that lambda expressions in themselves don't have a type because the common type system has no intrinsic concept of "lambda expression." However, it's sometimes convenient to speak informally of the "type" of a lambda expression. In these cases the type refers to the delegate type or [Expression](https://docs.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression) type to which the lambda expression is converted.

## Capture of outer variables and variable scope in lambda expressions

Lambdas can refer to *outer variables*. These are the variables that are in scope in the method that defines the lambda expression, or in scope in the type that contains the lambda expression. Variables that are captured in this manner are stored for use in the lambda expression even if the variables would otherwise go out of scope and be garbage collected. An outer variable must be definitely assigned before it can be consumed in a lambda expression. The following example demonstrates these rules:

C#Copy

```csharp
public static class VariableScopeWithLambdas
{
    public class VariableCaptureGame
    {
        internal Action<int> updateCapturedLocalVariable;
        internal Func<int, bool> isEqualToCapturedLocalVariable;

        public void Run(int input)
        {
            int j = 0;

            updateCapturedLocalVariable = x =>
            {
                j = x;
                bool result = j > input;
                Console.WriteLine($"{j} is greater than {input}: {result}");
            };

            isEqualToCapturedLocalVariable = x => x == j;

            Console.WriteLine($"Local variable before lambda invocation: {j}");
            updateCapturedLocalVariable(10);
            Console.WriteLine($"Local variable after lambda invocation: {j}");
        }
    }

    public static void Main()
    {
        var game = new VariableCaptureGame();

        int gameInput = 5;
        game.Run(gameInput);

        int jTry = 10;
        bool result = game.isEqualToCapturedLocalVariable(jTry);
        Console.WriteLine($"Captured local variable is equal to {jTry}: {result}");

        int anotherJ = 3;
        game.updateCapturedLocalVariable(anotherJ);

        bool equalToAnother = game.isEqualToCapturedLocalVariable(anotherJ);
        Console.WriteLine($"Another lambda observes a new value of captured variable: {equalToAnother}");
    }
    // Output:
    // Local variable before lambda invocation: 0
    // 10 is greater than 5: True
    // Local variable after lambda invocation: 10
    // Captured local variable is equal to 10: True
    // 3 is greater than 5: False
    // Another lambda observes a new value of captured variable: True
}
```

The following rules apply to variable scope in lambda expressions:

- A variable that is captured will not be garbage-collected until the delegate that references it becomes eligible for garbage collection.
- Variables introduced within a lambda expression are not visible in the enclosing method.
- A lambda expression cannot directly capture an [in](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/in-parameter-modifier), [ref](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/ref), or [out](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/out-parameter-modifier) parameter from the enclosing method.
- A [return](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/return) statement in a lambda expression doesn't cause the enclosing method to return.
- A lambda expression cannot contain a [goto](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/goto), [break](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/break), or [continue](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/continue) statement if the target of that jump statement is outside the lambda expression block. It's also an error to have a jump statement outside the lambda expression block if the target is inside the block.

Beginning with C# 9.0, you can apply the `static` modifier to a lambda expression to prevent unintentional capture of local variables or instance state by the lambda:

C#Copy

```csharp
Func<double, double> square = static x => x * x;
```

A static lambda can't capture local variables or instance state from enclosing scopes, but may reference static members and constant definitions.
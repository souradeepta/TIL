# Task Class

## Definition

- Namespace:

  [System.Threading.Tasks](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks?view=netcore-3.1)

- Assembly:

  System.Runtime.dll

Represents an asynchronous operation.

C#Copy

```csharp
public class Task : IAsyncResult, IDisposable
```

- Inheritance

  [Object](https://docs.microsoft.com/en-us/dotnet/api/system.object?view=netcore-3.1)Task

- Derived

  [System.Threading.Tasks.Task](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task-1?view=netcore-3.1)

- Implements

  [IAsyncResult](https://docs.microsoft.com/en-us/dotnet/api/system.iasyncresult?view=netcore-3.1) [IDisposable](https://docs.microsoft.com/en-us/dotnet/api/system.idisposable?view=netcore-3.1)

## Remarks

The [Task](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task?view=netcore-3.1) class represents a single operation that does not return a value and that usually executes asynchronously. [Task](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task?view=netcore-3.1) objects are one of the central components of the [task-based asynchronous pattern](https://docs.microsoft.com/en-us/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap) first introduced in the .NET Framework 4. Because the work performed by a [Task](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task?view=netcore-3.1) object typically executes asynchronously on a thread pool thread rather than synchronously on the main application thread, you can use the [Status](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.status?view=netcore-3.1) property, as well as the [IsCanceled](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.iscanceled?view=netcore-3.1), [IsCompleted](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.iscompleted?view=netcore-3.1), and [IsFaulted](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.isfaulted?view=netcore-3.1) properties, to determine the state of a task. Most commonly, a lambda expression is used to specify the work that the task is to perform.

For operations that return values, you use the [Task](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task-1?view=netcore-3.1) class.

In this section:

[Task instantiation examples](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task?view=netcore-3.1#Instant)
[Creating and executing a task](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task?view=netcore-3.1#Creating)
[Separating task creation and execution](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task?view=netcore-3.1#Separating)
[Waiting for one or more tasks to complete](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task?view=netcore-3.1#WaitingForOne)
[Tasks and culture](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task?view=netcore-3.1#Culture)
[For debugger developers](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task?view=netcore-3.1#Debugger)



### Task instantiation

The following example creates and executes four tasks. Three tasks execute an [Action](https://docs.microsoft.com/en-us/dotnet/api/system.action-1?view=netcore-3.1) delegate named `action`, which accepts an argument of type [Object](https://docs.microsoft.com/en-us/dotnet/api/system.object?view=netcore-3.1). A fourth task executes a lambda expression (an [Action](https://docs.microsoft.com/en-us/dotnet/api/system.action?view=netcore-3.1) delegate) that is defined inline in the call to the task creation method. Each task is instantiated and run in a different way:

- Task `t1` is instantiated by calling a Task class constructor, but is started by calling its [Start()](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.start?view=netcore-3.1#System_Threading_Tasks_Task_Start) method only after task `t2` has started.
- Task `t2` is instantiated and started in a single method call by calling the [TaskFactory.StartNew(Action, Object)](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.taskfactory.startnew?view=netcore-3.1#System_Threading_Tasks_TaskFactory_StartNew_System_Action_System_Object__System_Object_) method.
- Task `t3` is instantiated and started in a single method call by calling the [Run(Action)](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.run?view=netcore-3.1#System_Threading_Tasks_Task_Run_System_Action_) method.
- Task `t4` is executed synchronously on the main thread by calling the [RunSynchronously()](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.runsynchronously?view=netcore-3.1#System_Threading_Tasks_Task_RunSynchronously) method.

Because task `t4` executes synchronously, it executes on the main application thread. The remaining tasks execute asynchronously typically on one or more thread pool threads.

C#Copy

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

class Example
{
    static void Main()
    {
        Action<object> action = (object obj) =>
                                {
                                   Console.WriteLine("Task={0}, obj={1}, Thread={2}",
                                   Task.CurrentId, obj,
                                   Thread.CurrentThread.ManagedThreadId);
                                };

        // Create a task but do not start it.
        Task t1 = new Task(action, "alpha");

        // Construct a started task
        Task t2 = Task.Factory.StartNew(action, "beta");
        // Block the main thread to demonstrate that t2 is executing
        t2.Wait();

        // Launch t1 
        t1.Start();
        Console.WriteLine("t1 has been launched. (Main Thread={0})",
                          Thread.CurrentThread.ManagedThreadId);
        // Wait for the task to finish.
        t1.Wait();

        // Construct a started task using Task.Run.
        String taskData = "delta";
        Task t3 = Task.Run( () => {Console.WriteLine("Task={0}, obj={1}, Thread={2}",
                                                     Task.CurrentId, taskData,
                                                      Thread.CurrentThread.ManagedThreadId);
                                   });
        // Wait for the task to finish.
        t3.Wait();

        // Construct an unstarted task
        Task t4 = new Task(action, "gamma");
        // Run it synchronously
        t4.RunSynchronously();
        // Although the task was run synchronously, it is a good practice
        // to wait for it in the event exceptions were thrown by the task.
        t4.Wait();
    }
}
// The example displays output like the following:
//       Task=1, obj=beta, Thread=3
//       t1 has been launched. (Main Thread=1)
//       Task=2, obj=alpha, Thread=4
//       Task=3, obj=delta, Thread=3
//       Task=4, obj=gamma, Thread=1
```



### Creating and executing a task

[Task](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task?view=netcore-3.1) instances may be created in a variety of ways. The most common approach, which is available starting with the .NET Framework 4.5, is to call the static [Run](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.run?view=netcore-3.1) method. The [Run](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.run?view=netcore-3.1) method provides a simple way to start a task using default values and without requiring additional parameters. The following example uses the [Run(Action)](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.run?view=netcore-3.1#System_Threading_Tasks_Task_Run_System_Action_) method to start a task that loops and then displays the number of loop iterations:

C#Copy

```csharp
using System;
using System.Threading.Tasks;

public class Example
{
   public static async Task Main()
   {
      await Task.Run( () => {
                                  // Just loop.
                                  int ctr = 0;
                                  for (ctr = 0; ctr <= 1000000; ctr++)
                                  {}
                                  Console.WriteLine("Finished {0} loop iterations",
                                                    ctr);
                               } );
   }
}
// The example displays the following output:
//        Finished 1000001 loop iterations
```

An alternative, and the most common method to start a task in .NET Framework 4, is the static [TaskFactory.StartNew](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.taskfactory.startnew?view=netcore-3.1) method. The [Task.Factory](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.factory?view=netcore-3.1) property returns a [TaskFactory](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.taskfactory?view=netcore-3.1) object. Overloads of the [TaskFactory.StartNew](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.taskfactory.startnew?view=netcore-3.1) method let you specify parameters to pass to the task creation options and a task scheduler. The following example uses the [TaskFactory.StartNew](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.taskfactory.startnew?view=netcore-3.1) method to start a task. It is functionally equivalent to the code in the previous example.

C#Copy

```csharp
using System;
using System.Threading.Tasks;

public class Example
{
   public static void Main()
   {
      Task t = Task.Factory.StartNew( () => {
                                  // Just loop.
                                  int ctr = 0;
                                  for (ctr = 0; ctr <= 1000000; ctr++)
                                  {}
                                  Console.WriteLine("Finished {0} loop iterations",
                                                    ctr);
                               } );
      t.Wait();
   }
}
// The example displays the following output:
//        Finished 1000001 loop iterations
```

For more complete examples, see [Task-based Asynchronous Programming](https://docs.microsoft.com/en-us/dotnet/standard/parallel-programming/task-based-asynchronous-programming).



### Separating task creation and execution

The [Task](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task?view=netcore-3.1) class also provides constructors that initialize the task but that do not schedule it for execution. For performance reasons, the [Task.Run](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.run?view=netcore-3.1) or [TaskFactory.StartNew](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.taskfactory.startnew?view=netcore-3.1) method is the preferred mechanism for creating and scheduling computational tasks, but for scenarios where creation and scheduling must be separated, you can use the constructors and then call the [Task.Start](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.start?view=netcore-3.1) method to schedule the task for execution at a later time.



### Waiting for one or more tasks to complete

Because tasks typically run asynchronously on a thread pool thread, the thread that creates and starts the task continues execution as soon as the task has been instantiated. In some cases, when the calling thread is the main application thread, the app may terminate before any the task actually begins execution. In others, your application's logic may require that the calling thread continue execution only when one or more tasks has completed execution. You can synchronize the execution of the calling thread and the asynchronous tasks it launches by calling a `Wait` method to wait for one or more tasks to complete.

To wait for a single task to complete, you can call its [Task.Wait](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.wait?view=netcore-3.1) method. A call to the [Wait](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.wait?view=netcore-3.1) method blocks the calling thread until the single class instance has completed execution.

The following example calls the parameterless [Wait()](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.wait?view=netcore-3.1#System_Threading_Tasks_Task_Wait) method to wait unconditionally until a task completes. The task simulates work by calling the [Thread.Sleep](https://docs.microsoft.com/en-us/dotnet/api/system.threading.thread.sleep?view=netcore-3.1) method to sleep for two seconds.

C#Copy

```csharp
using System;   
using System.Threading;
using System.Threading.Tasks;

class Program
{
    static Random rand = new Random();

    static void Main()
    {
        // Wait on a single task with no timeout specified.
        Task taskA = Task.Run( () => Thread.Sleep(2000));
        Console.WriteLine("taskA Status: {0}", taskA.Status);
        try {
          taskA.Wait();
          Console.WriteLine("taskA Status: {0}", taskA.Status);
       } 
       catch (AggregateException) {
          Console.WriteLine("Exception in taskA.");
       }   
    }    
}
// The example displays output like the following:
//     taskA Status: WaitingToRun
//     taskA Status: RanToCompletion
```

You can also conditionally wait for a task to complete. The [Wait(Int32)](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.wait?view=netcore-3.1#System_Threading_Tasks_Task_Wait_System_Int32_) and [Wait(TimeSpan)](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.wait?view=netcore-3.1#System_Threading_Tasks_Task_Wait_System_TimeSpan_) methods block the calling thread until the task finishes or a timeout interval elapses, whichever comes first. Since the following example launches a task that sleeps for two seconds but defines a one-second timeout value, the calling thread blocks until the timeout expires and before the task has completed execution.

C#Copy

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

public class Example
{
   public static void Main()
   {
      // Wait on a single task with a timeout specified.
      Task taskA = Task.Run( () => Thread.Sleep(2000));
      try {
        taskA.Wait(1000);       // Wait for 1 second.
        bool completed = taskA.IsCompleted;
        Console.WriteLine("Task A completed: {0}, Status: {1}",
                         completed, taskA.Status);
        if (! completed)
           Console.WriteLine("Timed out before task A completed.");                 
       }
       catch (AggregateException) {
          Console.WriteLine("Exception in taskA.");
       }   
   }
}
// The example displays output like the following:
//     Task A completed: False, Status: Running
//     Timed out before task A completed.
```

You can also supply a cancellation token by calling the [Wait(CancellationToken)](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.wait?view=netcore-3.1#System_Threading_Tasks_Task_Wait_System_Threading_CancellationToken_) and [Wait(Int32, CancellationToken)](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.wait?view=netcore-3.1#System_Threading_Tasks_Task_Wait_System_Int32_System_Threading_CancellationToken_) methods. If the token's [IsCancellationRequested](https://docs.microsoft.com/en-us/dotnet/api/system.threading.cancellationtoken.iscancellationrequested?view=netcore-3.1) property is `true` or becomes `true` while the [Wait](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.wait?view=netcore-3.1) method is executing, the method throws an [OperationCanceledException](https://docs.microsoft.com/en-us/dotnet/api/system.operationcanceledexception?view=netcore-3.1).

In some cases, you may want to wait for the first of a series of executing tasks to complete, but don't care which task it is. For this purpose, you can call one of the overloads of the [Task.WaitAny](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.waitany?view=netcore-3.1) method. The following example creates three tasks, each of which sleeps for an interval determine by a random number generator. The [WaitAny(Task[\])](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.waitany?view=netcore-3.1#System_Threading_Tasks_Task_WaitAny_System_Threading_Tasks_Task___) method waits for the first task to complete. The example then displays information about the status of all three tasks.

C#Copy

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

public class Example
{
   public static void Main()
   {
      var tasks = new Task[3];
      var rnd = new Random();
      for (int ctr = 0; ctr <= 2; ctr++)
         tasks[ctr] = Task.Run( () => Thread.Sleep(rnd.Next(500, 3000)));

      try {
         int index = Task.WaitAny(tasks);
         Console.WriteLine("Task #{0} completed first.\n", tasks[index].Id);
         Console.WriteLine("Status of all tasks:");
         foreach (var t in tasks)
            Console.WriteLine("   Task #{0}: {1}", t.Id, t.Status);
      }
      catch (AggregateException) {
         Console.WriteLine("An exception occurred.");
      }
   }
}
// The example displays output like the following:
//     Task #1 completed first.
//     
//     Status of all tasks:
//        Task #3: Running
//        Task #1: RanToCompletion
//        Task #4: Running
```

You can also wait for all of a series of tasks to complete by calling the [WaitAll](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.waitall?view=netcore-3.1) method. The following example creates ten tasks, waits for all ten to complete, and then displays their status.

C#Copy

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

public class Example
{
   public static void Main()
   {
      // Wait for all tasks to complete.
      Task[] tasks = new Task[10];
      for (int i = 0; i < 10; i++)
      {
          tasks[i] = Task.Run(() => Thread.Sleep(2000));
      }
      try {
         Task.WaitAll(tasks);
      }
      catch (AggregateException ae) {
         Console.WriteLine("One or more exceptions occurred: ");
         foreach (var ex in ae.Flatten().InnerExceptions)
            Console.WriteLine("   {0}", ex.Message);
      }   

      Console.WriteLine("Status of completed tasks:");
      foreach (var t in tasks)
         Console.WriteLine("   Task #{0}: {1}", t.Id, t.Status);
   }
}
// The example displays the following output:
//     Status of completed tasks:
//        Task #2: RanToCompletion
//        Task #1: RanToCompletion
//        Task #3: RanToCompletion
//        Task #4: RanToCompletion
//        Task #6: RanToCompletion
//        Task #5: RanToCompletion
//        Task #7: RanToCompletion
//        Task #8: RanToCompletion
//        Task #9: RanToCompletion
//        Task #10: RanToCompletion
```

Note that when you wait for one or more tasks to complete, any exceptions thrown in the running tasks are propagated on the thread that calls the `Wait` method, as the following example shows. It launches 12 tasks, three of which complete normally and three of which throw an exception. Of the remaining six tasks, three are cancelled before they start, and three are cancelled while they are executing. Exceptions are thrown in the [WaitAll](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.waitall?view=netcore-3.1) method call and are handled by a `try`/`catch` block.

C#Copy

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

public class Example
{
   public static void Main()
   {
      // Create a cancellation token and cancel it.
      var source1 = new CancellationTokenSource();
      var token1 = source1.Token;
      source1.Cancel();
      // Create a cancellation token for later cancellation.
      var source2 = new CancellationTokenSource();
      var token2 = source2.Token;
       
      // Create a series of tasks that will complete, be cancelled, 
      // timeout, or throw an exception.
      Task[] tasks = new Task[12];
      for (int i = 0; i < 12; i++)
      {
          switch (i % 4) 
          {
             // Task should run to completion.
             case 0:
                tasks[i] = Task.Run(() => Thread.Sleep(2000));
                break;
             // Task should be set to canceled state.
             case 1:   
                tasks[i] = Task.Run( () => Thread.Sleep(2000),
                         token1);
                break;         
             case 2:
                // Task should throw an exception.
                tasks[i] = Task.Run( () => { throw new NotSupportedException(); } );
                break;
             case 3:
                // Task should examine cancellation token.
                tasks[i] = Task.Run( () => { Thread.Sleep(2000); 
                                             if (token2.IsCancellationRequested)
                                                token2.ThrowIfCancellationRequested();
                                             Thread.Sleep(500); }, token2);   
                break;
          }
      }
      Thread.Sleep(250);
      source2.Cancel();
       
      try {
         Task.WaitAll(tasks);
      }
      catch (AggregateException ae) {
          Console.WriteLine("One or more exceptions occurred:");
          foreach (var ex in ae.InnerExceptions)
             Console.WriteLine("   {0}: {1}", ex.GetType().Name, ex.Message);
       }   

      Console.WriteLine("\nStatus of tasks:");
      foreach (var t in tasks) {
         Console.WriteLine("   Task #{0}: {1}", t.Id, t.Status);
         if (t.Exception != null) {
            foreach (var ex in t.Exception.InnerExceptions)
               Console.WriteLine("      {0}: {1}", ex.GetType().Name,
                                 ex.Message);
         }
      }
   }
}
// The example displays output like the following:
//   One or more exceptions occurred:
//      TaskCanceledException: A task was canceled.
//      NotSupportedException: Specified method is not supported.
//      TaskCanceledException: A task was canceled.
//      TaskCanceledException: A task was canceled.
//      NotSupportedException: Specified method is not supported.
//      TaskCanceledException: A task was canceled.
//      TaskCanceledException: A task was canceled.
//      NotSupportedException: Specified method is not supported.
//      TaskCanceledException: A task was canceled.
//   
//   Status of tasks:
//      Task #13: RanToCompletion
//      Task #1: Canceled
//      Task #3: Faulted
//         NotSupportedException: Specified method is not supported.
//      Task #8: Canceled
//      Task #14: RanToCompletion
//      Task #4: Canceled
//      Task #6: Faulted
//         NotSupportedException: Specified method is not supported.
//      Task #7: Canceled
//      Task #15: RanToCompletion
//      Task #9: Canceled
//      Task #11: Faulted
//         NotSupportedException: Specified method is not supported.
//      Task #12: Canceled
```

For more information on exception handling in task-based asynchronous operations, see [Exception Handling](https://docs.microsoft.com/en-us/dotnet/standard/parallel-programming/exception-handling-task-parallel-library).



### Tasks and culture

Starting with desktop apps that target the .NET Framework 4.6, the culture of the thread that creates and invokes a task becomes part of the thread's context. That is, regardless of the current culture of the thread on which the task executes, the current culture of the task is the culture of the calling thread.
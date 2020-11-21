# Test Method

**Where do we put our test code?**

***\*Encode each test as a single \*Test Method\* on some class.\****
![img](http://xunitpatterns.com/Test%20Method.gif)
Sketch Test Method embedded from Test Method.gif



[Fully Automated Tests](http://xunitpatterns.com/Goals of Test Automation.html#Fully Automated Test) *(see Goals of Test Automation on page X)* consist of test logic. That logic has to live somewhere before we can compile and execute it.

## How It Works

We define each [test](http://xunitpatterns.com/test.html) as a method, procedure or function that implements the [four phases](http://xunitpatterns.com/Four Phase Test.html) *(see Four-Phase Test on page X)* necessary to realize a [Fully Automated Test](http://xunitpatterns.com/Goals of Test Automation.html#Fully Automated Test). Most notably, the *Test Method* must include [assertions](http://xunitpatterns.com/assertion.html) if it is to be a [Self-Checking Test](http://xunitpatterns.com/Goals of Test Automation.html#Self-Checking Test) *(see Goals of Test Automation)*.

We organize the test logic following one of the standard *Test Method* templates to make the type of test easily recognizable by test readers. In a [Simple Success Test](http://xunitpatterns.com/Test Method.html#Simple Success Test) we have a purely linear flow of control from fixture setup through exercising the [system under test (SUT)](http://xunitpatterns.com/SUT.html) to result verification. In an [Expected Exception Test](http://xunitpatterns.com/Test Method.html#Expected Exception Test) we use language-based structures to direct us to error handling code; if we get there, we pass the test and if we don't , we fail it. In a [Constructor Test](http://xunitpatterns.com/Test Method.html#Constructor Test) we simply instantiate an object and do assertions against its attributes.

## Why We Do This

We have to encode the test logic somewhere. In the procedural world, we would have to encode each test as a test case procedure located in a file or module. In object oriented programming languages the preferred option is to encode them as methods on a suitable [Testcase Class](http://xunitpatterns.com/Testcase Class.html) *(page X)* and turn these *Test Methods* into [Testcase Objects](http://xunitpatterns.com/Testcase Object.html) *(page X)* at runtime using either [Test Discovery](http://xunitpatterns.com/Test Discovery.html) *(page X)* or [Test Enumeration](http://xunitpatterns.com/Test Enumeration.html) *(page X)*.

We follow the standard test templates to keep our *Test Methods* as simple as possible. This greatly increases their utility as [System Documentation](http://xunitpatterns.com/Goals of Test Automation.html#Tests as Documentation) *(see Goals of Test Automation)* by making it easier to find the description of the basic behavior of the [SUT](http://xunitpatterns.com/SUT.html). It is a lot easier to recognize which tests describe this basic behavior if only [Expected Exception Tests](http://xunitpatterns.com/Test Method.html#Expected Exception Test) contain error handling language constructs such as try/catch.

## Implementation Notes

We still need a way to run all the *Test Methods* tests on the [Testcase Class](http://xunitpatterns.com/Testcase Class.html). One solution is to define the run method to call each of the test methods. Of course, we would also have to deal with counting the tests and how many passed and failed. Since all this functionality is needed anyway for a [test suite](http://xunitpatterns.com/test suite.html), a simple solution is to instantiate a [Test Suite Object](http://xunitpatterns.com/Test Suite Object.html) *(page X)* to hold the each *Test Method*. (See the sidebar [There's Always an Exception](http://xunitpatterns.com/There is Always an Exception.html) *(page X)* to see when this isn't the case.) This requires that we create an instance of the [Testcase Class](http://xunitpatterns.com/Testcase Class.html) for each *Test Method* using either [Test Discovery](http://xunitpatterns.com/Test Discovery.html) or [Test Enumeration](http://xunitpatterns.com/Test Enumeration.html).

In statically typed languages such as Java and C#, we may have to include a throws clause as part of the *Test Method* declaration so that the compiler won't complain about the fact that we are not handling the checked exceptions that the [SUT](http://xunitpatterns.com/SUT.html) has declared it may throw. We are, in effect, telling the compiler that "The [Test Runner](http://xunitpatterns.com/Test Runner.html) *(page X)* will deal with the exceptions."

Of course, different kinds of functionality need different types of *Test Methods* but almost all tests can be boiled down to one of the following three basic types:



#### Variation: Simple Success Test

Most software has an obvious "success scenario" (or "happy path"). A *Simple Success Test* verifies the success scenario in a simple and easily recognized way. We create an instance of the [SUT](http://xunitpatterns.com/SUT.html) and call the method(s) that we want to test. We then assert that the expected outcome has occurred. In other words, the normal steps of a [Four-Phase Test](http://xunitpatterns.com/Four Phase Test.html). What we don't do is catch any exceptions that **could** happen. Instead, we let the [Test Automation Framework](http://xunitpatterns.com/Test Automation Framework.html) *(page X)* catch and report them. Doing otherwise would result in [*Obscure Tests*](http://xunitpatterns.com/Obscure Test.html) *(page X)* and would mislead the test reader by making it appear that exceptions were expected. See [Tests as Documentation](http://xunitpatterns.com/Goals of Test Automation.html#Tests as Documentation) for the rationale behind this.

Another benefit of avoiding try/catch-style code is that when errors do occur it is a lot easier to track them down because the [Test Automation Framework](http://xunitpatterns.com/Test Automation Framework.html) will report the location where the actual error occurred deep in the [SUT](http://xunitpatterns.com/SUT.html) rather than the place in our test where we called an [Assertion Method](http://xunitpatterns.com/Assertion Method.html) *(page X)* such as fail or assertTrue. These kinds of errors turn out to be much easier to troubleshoot than assertion failures.



#### Variation: Expected Exception Test

Writing software that passes the [Simple Success Test](http://xunitpatterns.com/Test Method.html#Simple Success Test) is pretty straightforward. Most of the defects in software are in the various alternate paths, especially the ones that relate to error scenarios. This occurs because these error scenarios are often [*Untested Requirements*](http://xunitpatterns.com/Production Bugs.html#Untested Requirement) *(see Production Bugs on page X)* and/or [*Untested Code*](http://xunitpatterns.com/Production Bugs.html#Untested Code) *(see Production Bugs)*. An *Expected Exception Test* helps us verify that the error scenarios have been coded correctly.We set up the test fixture and exercise the [SUT](http://xunitpatterns.com/SUT.html) in each way that should result in an error. We ensure that the expected error has occurred by using whatever language construct we have available to catch the error. If the error is raised, flow will pass to the error-handling block. This may be enough to let the test pass, but if the type or message contents of the exception or error is important (such as when the error message will be shown to a user), we can use an [Equality Assertion](http://xunitpatterns.com/Assertion Method.html#Equality Assertion) *(see Assertion Method)* to verify it. If the error is not raised, we call fail to report that the [SUT](http://xunitpatterns.com/SUT.html) failed to raise an error as expected.

We should write an *Expected Exception Test* for each kind of exception that the [SUT](http://xunitpatterns.com/SUT.html) is **expected** to raise. It may raise the error because of something invalid that the client (namely our test) has asked it to do, or it may translate or pass through an error raised by some other component it uses. We should not write an *Expected Exception Test* for exceptions that the [SUT](http://xunitpatterns.com/SUT.html) might raise but which we cannot force. These kinds of errors should show up as test failures in the [Simple Success Tests](http://xunitpatterns.com/Test Method.html#Simple Success Test). If we want to verify that these kinds of errors **are** handled properly, we should find a way to force them. The most common way to do this is to use a [Test Stub](http://xunitpatterns.com/Test Stub.html) *(page X)* to control the [indirect input](http://xunitpatterns.com/indirect input.html) of the [SUT](http://xunitpatterns.com/SUT.html) and raise the appropriate errors in the [Test Stub](http://xunitpatterns.com/Test Stub.html).

Exception tests are very interesting to write about because of the different ways the [xUnit](http://xunitpatterns.com/xUnit.html) frameworks express them. [JUnit](http://xunitpatterns.com/JUnit.html) 3.x provides a special ExpectedException class to inherit from but since this forces us to create a [Testcase Class](http://xunitpatterns.com/Testcase Class.html) for each [Test Method](http://xunitpatterns.com/Test Method.html) *(page X)* it really doesn't save any effort over coding a try/catch block. Later versions of [JUnit](http://xunitpatterns.com/JUnit.html) and [NUnit](http://xunitpatterns.com/NUnit.html) (for .NET) provide a special ExpectedException [method attribute](http://xunitpatterns.com/method attribute.html) (called an [annotation](http://xunitpatterns.com/annotation.html) in Java) to tell the [Test Automation Framework](http://xunitpatterns.com/Test Automation Framework.html) to fail the test if that exception **isn't** raised. It allows us to include the expected message text if we want to specify exactly what text to expect in addition to the type of the exception.

Languages with [blocks](http://xunitpatterns.com/block.html) like Smalltalk and Ruby can provide special assertions to which we pass the block of code to be executed as well as the expected exception/error object. The [Assertion Method](http://xunitpatterns.com/Assertion Method.html) implements the error handling logic required to determine whether or not the error has in fact occured. This makes our *Test Methods* much simpler though we may need to look at the names of the assertions more closely to see which type of test we have.



#### Variation: Constructor Test

We would have a lot of [*Test Code Duplication*](http://xunitpatterns.com/Test Code Duplication.html) *(page X)* if every test we wrote had to verify that the objects it creates in its [fixture setup](http://xunitpatterns.com/fixture setup.html) phase were correctly instantiated. We avoid this by testing the constructor(s) separately from other *Test Methods* whenever the constructor contains anything more complex than simple field assignment from the constructor parameters. These *Constructor Tests* provide better [Defect Localization](http://xunitpatterns.com/Goals of Test Automation.html#Defect Localization) *(see Goals of Test Automation)* than including constructor logic verification in other tests. We may need to write one or more tests for each constructor signature. Most *Constructor Tests* will follow a [Simple Success Test](http://xunitpatterns.com/Test Method.html#Simple Success Test) template, however, we can use an [Expected Exception Test](http://xunitpatterns.com/Test Method.html#Expected Exception Test) to verify that the constructor correctly reports invalid arguments by raising an exception.

We should verify each attribute of the object or data structure regardless of whether we expect it to be initialized or not. For attributes that should be initialized, we can use an [Equality Assertion](http://xunitpatterns.com/Assertion Method.html#Equality Assertion) to specify the correct value. For attributes that should not be initialized, we can use a [Stated Outcome Assertion](http://xunitpatterns.com/Assertion Method.html#Stated Outcome Assertion) *(see Assertion Method)* appropriate to the type of the attribute (e.g., assertNull(anObjectReference) for object variables or pointers.) Note that if we are organizing our tests with one [Testcase Class per Fixture](http://xunitpatterns.com/Testcase Class per Fixture.html) *(page X)*, we can put each assertion into a separate *Test Method* to give optimal [Defect Localization](http://xunitpatterns.com/Goals of Test Automation.html#Defect Localization).



#### Variation: Dependency Initialization Test

When we have an object with a [substitutable dependency](http://xunitpatterns.com/substitutable dependency.html) we also need to make sure that the attribute that holds the reference to the [depended-on component (DOC)](http://xunitpatterns.com/DOC.html) is initialized to the real [DOC](http://xunitpatterns.com/DOC.html) when the software is run in production. A *Dependency Initialization Test* is a [Constructor Test](http://xunitpatterns.com/Test Method.html#Constructor Test) that asserts that this attribute is initialized correctly. It is often done in a different *Test Method* from the normal [Constructor Tests](http://xunitpatterns.com/Test Method.html#Constructor Test) to improve its visibility.



## Example: Simple Success Test

The following illustrates a test where the novice [test automater](http://xunitpatterns.com/test automater.html) has included code to catch exceptions that he knows might occur (or that he might have encountered while debugging his code.)

```csharp
   public void testFlightMileage_asKm() throws Exception {
      // setup fixture
      Flight newFlight = new Flight(validFlightNumber);
      try {
         // exercise SUT
         newFlight.setMileage(1122);
         // verify results
         int actualKilometres = newFlight.getMileageAsKm();   
         int expectedKilometres = 1810;
         // verify results
         assertEquals( expectedKilometres, actualKilometres);
      } catch (InvalidArgumentException e) {
         fail(e.getMessage());
      } catch (ArrayStoreException e) {
         fail(e.getMessage());
      }
   }
Example SimpleSuccessTestOvercomplicated embedded from java/com/xunitpatterns/testtemplates/BadExamples.java
```

The majority of the code is unnecessary and just obscures the intent of the test. Luckily for us all this exception handling can be avoided. [xUnit](http://xunitpatterns.com/xUnit.html) has built in support for catching **unexpected** exceptions. We can rip out all the exception handling code and let the [Test Automation Framework](http://xunitpatterns.com/Test Automation Framework.html) catch any **unexpected** exception that might be thrown. Unexpected exceptions are counted as [test errors](http://xunitpatterns.com/test error.html) since the test is terminating in a way we didn't anticipate. This is useful information and is not considered to be any more severe than a [test failure](http://xunitpatterns.com/test failure.html).

```java
   public void testFlightMileage_asKm() throws Exception {
      // setup fixture
      Flight newFlight = new Flight(validFlightNumber);
      newFlight.setMileage(1122);
      // exercise mileage translater
      int actualKilometres = newFlight.getMileageAsKm();   
      // verify results
      int expectedKilometres = 1810;
      assertEquals( expectedKilometres, actualKilometres);
   }
Example SimpleSuccessTest embedded from java/com/xunitpatterns/testtemplates/GoodExamples.java
```

Since this example is in Java, a statically-typed language, we've had to declare the exception that the [SUT](http://xunitpatterns.com/SUT.html) says it may throw as part of the [Test Method](http://xunitpatterns.com/Test Method.html) signature. The [Test Runner](http://xunitpatterns.com/Test Runner.html) expects this so don't fret about it.



## Example: Expected Exception Test using Try/Catch

The following is a partially complete test to verify an exception case. The novice [test automater](http://xunitpatterns.com/test automater.html) has setup up the right test condition to cause the [SUT](http://xunitpatterns.com/SUT.html) to raise an error.

```java
   public void testSetMileage_invalidInput() throws Exception {
      // setup fixture
      Flight newFlight = new Flight(validFlightNumber);
      // exercise SUT
      newFlight.setMileage(-1122);  // invalid
      // how do we verify an exception was thrown?
   }
Example beforeException embedded from java/com/xunitpatterns/testtemplates/BadExamples.java
```



Because the [Test Automation Framework](http://xunitpatterns.com/Test Automation Framework.html) will catch the exception and fail the test, the [Test Runner](http://xunitpatterns.com/Test Runner.html) will not exhibit the [green bar](http://xunitpatterns.com/green bar.html) even though the [SUT's](http://xunitpatterns.com/SUT.html) behavior is correct. We can introduce an error-handling block around the exercise phase of the test and use it to invert the pass/fail criteria (pass when the exception **is** thrown and fail when it is not.) Here's how to verify that the [SUT](http://xunitpatterns.com/SUT.html) fails as expected in JUnit:

```java
   public void testSetMileage_invalidInput() throws Exception {
      // setup fixture
      Flight newFlight = new Flight(validFlightNumber);
      try {
         // exercise SUT
         newFlight.setMileage(-1122);
         fail("Should have thrown InvalidInputException");
      } catch( InvalidArgumentException e) {
         // verify results
         assertEquals( "Flight mileage must be positive", e.getMessage());
      }
   }
Example ExpectedException embedded from java/com/xunitpatterns/testtemplates/GoodExamples.java
```

This style of try/catch can only be used in languages that let us specify exactly what exception to catch. It won't work if we want to catch a generic exception or the same exception that fail throws because that will send us into the catch clause. In these cases we need to use the same style of [Expected Exception Test](http://xunitpatterns.com/Test Method.html#Expected Exception Test) as used in tests of [Custom Assertions](http://xunitpatterns.com/Custom Assertion.html) *(page X)*.

```java
   public void testSetMileage_invalidInput2() throws Exception {
      // setup fixture
      Flight newFlight = new Flight(validFlightNumber);
      try {
         // exercise SUT
         newFlight.setMileage(-1122);
         // Cannot fail() here if SUT throws same kind of ex
      } catch( AssertionFailedError e) {
         // verify results
         assertEquals( "Flight mileage must be positive", e.getMessage());
         return;
      }
      fail("Should have thrown InvalidInputException");
   }
Example ExpectedExceptionCustomAssertionStyle embedded from java/com/xunitpatterns/testtemplates/GoodExamples.java
```



## Example: Expected Exception Test using Method Attribute

[NUnit](http://xunitpatterns.com/NUnit.html) provides an [method attribute](http://xunitpatterns.com/method attribute.html) that does the same thing without forcing us to code a try/catch block explicitly:

```csharp
   [Test]
   [ExpectedException(typeof( InvalidArgumentException), "Flight mileage must be > zero")]
   public void testSetMileage_invalidInput_AttributeWithMessage()
   {
      // setup fixture
      Flight newFlight = new Flight(validFlightNumber);
      // exercise SUT
      newFlight.setMileage(-1122);
   }
Example ExpectedExceptionNUnit embedded from CSharp/NUnitExamples/GoodExamples.cs
```

This does make the test much more compact but note that it doesn't provide a way to specify anything but the type of the exception or the message it contains. If we want to do any assertions on other contents of the exception (to avoid [*Sensitive Equality*](http://xunitpatterns.com/Fragile Test.html#Sensitive Equality) *(see Fragile Test on page X)*), we'll need to use try/catch.



## Example: Expected Exception Test using Block Closure

Smalltalk's [SUnit](http://xunitpatterns.com/SUnit.html) provides another mechanism to achieve the same thing:

```java
   testSetMileageWithInvalidInput
      self
         should: [Flight new mileage: -1122]
         raise: RuntimeError new 'Should have raised error'
Example ExpectedExceptionSUnit embedded from Smalltalk/Test Templates.st
```

Because Smalltalk supports block closures, we pass the block of code to be executed to the method should:raise: along with the expected Exception object. Ruby's [Test::Unit](http://xunitpatterns.com/TestUnit.html) uses the same approach:

```ruby
def testSetMileage_invalidInput
   flight = Flight.new();
   assert_raises( RuntimeError, "Should have raised error") do
      flight.setMileage(-1122)
   end
end
Example ExpectedExceptionRubyUnit embedded from Ruby/TestTemplates.rb
```

The code between the do/end pair is a closure that gets executed by the assert_raises method. If it doesn't raise an instance of the first argument (the class RuntimeError) the test is failed with the error message supplied.



## Example: Constructor Test

In this example, we need to build a flight to be able to test the conversion of the flight distance from miles to kilometers. First, we'll just make sure the Flight is constructed properly.

```java
   public void testFlightMileage_asKm2() throws Exception {
      // setup fixture
      // exercise contructor
      Flight newFlight = new Flight(validFlightNumber);
      // verify constructed object
      assertEquals(validFlightNumber, newFlight.number);
      assertEquals("", newFlight.airlineCode);
      assertNull(newFlight.airline);
      // setup mileage
      newFlight.setMileage(1122);
      // exercise mileage translater
      int actualKilometres = newFlight.getMileageAsKm();    
      // verify results
      int expectedKilometres = 1810;
      assertEquals( expectedKilometres, actualKilometres);
      // now try it with a canceled flight:
      newFlight.cancel();
      try {
         newFlight.getMileageAsKm();
         fail("Expected exception");
      } catch (InvalidRequestException e) {
         assertEquals( "Cannot get cancelled flight mileage", e.getMessage());
      }
   }
Example EagerTest embedded from java/com/xunitpatterns/testtemplates/BadExamples.java
```

This test is not a [Single Condition Test](http://xunitpatterns.com/Principles of Test Automation.html#Single Condition Test) *(see Principles of Test Automation on page X)* because it is testing both object construction as well as distance conversion behavior. If object construction fails, we won't know that was the problem until we start debugging.

It would be better to separate this [*Eager Test*](http://xunitpatterns.com/Assertion Roulette.html#Eager Test) *(see Assertion Roulette on page X)* into two tests each of which is a [Single Condition Test](http://xunitpatterns.com/Principles of Test Automation.html#Single Condition Test). This is most easily done by cloning the *Test Method*, renaming each copy to reflect what it would do if it were a [Single Condition Test](http://xunitpatterns.com/Principles of Test Automation.html#Single Condition Test) and then removing any code that doesn't satisfy that goal.

Here's an example of a simple *Test Method*:

```java
   public void testFlightConstructor_OK() throws Exception {
      // setup fixture
      // exercise SUT
      Flight newFlight = new Flight(validFlightNumber);
      // verify results
      assertEquals( validFlightNumber, newFlight.number );
      assertEquals( "", newFlight.airlineCode );
      assertNull( newFlight.airline );
   }
Example ConstructorTest embedded from java/com/xunitpatterns/testtemplates/GoodExamples.java
```

While we are at it, we might as well specify what should occur if an invalid argument is passed to the constructor by using the [Expected Exception Test](http://xunitpatterns.com/Test Method.html#Expected Exception Test) template for our *Test Method*:

```java
   public void testFlightConstructor_badInput() {
      // setup fixture
      BigDecimal invalidFlightNumber = new BigDecimal(-1023);
      // exercise SUT
      try {
         Flight newFlight = new Flight(invalidFlightNumber);
         fail("Didn't catch negative flight number!");
      } catch (InvalidArgumentException e) {
         // verify results
         assertEquals( "Flight numbers must be positive", e.getMessage());
      }
   }
Example ConstructorExceptionTest embedded from java/com/xunitpatterns/testtemplates/GoodExamples.java
```

Now that we know that our constructor logic is well tested, we can go ahead and write our [Simple Success Test](http://xunitpatterns.com/Test Method.html#Simple Success Test). Note how much simpler it has become because we can focus on the business logic:

```java
   public void testFlightMileage_asKm() throws Exception {
      // setup fixture
      Flight newFlight = new Flight(validFlightNumber);
      newFlight.setMileage(1122);
      // exercise mileage translater
      int actualKilometres = newFlight.getMileageAsKm();   
      // verify results
      int expectedKilometres = 1810;
      assertEquals( expectedKilometres, actualKilometres);
   }
Example SimpleSuccessTest embedded from java/com/xunitpatterns/testtemplates/GoodExamples.java
```

So what happens if the constructor logic **is** defective? This test will likely fail because its output depends on the value passed to the constructor. The contstructor test will also fail. That will be our key to look at the constructor logic first. Once that is fixed, this test will likely pass and if it doesn't, then we can focus on fixing the getMileageAsKm method logic. This is a good example of [Defect Localization](http://xunitpatterns.com/Goals of Test Automation.html#Defect Localization).
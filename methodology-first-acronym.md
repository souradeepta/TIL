# FIRST Principles as Solid Rules for Tests

### An in-depth explanation about the principles behind the FIRST acronym and how its application will help elevate and increase the quality of your tests.

In my [previous article](https://dzone.com/articles/rest-apis-test-pyramid) about `Test Pyramid` I mentioned a concept known as `FIRST` principles. I have always believed that this concept should be more popular because of how helpful it is. That is why I want to give you a more detailed explanation of these principles and why it may be beneficial for you and your team to stick to them. I hope that it will provide you with some guidelines to follow and help you make your tests better.

## **The Idea Behind FIRST**

As you probably know from experience, writing reliable tests is quite a task. `FIRST` principles were proposed by Robert C. Martin in his book "Clean Code: A Handbook of Agile Software Craftsmanship" to help a developer make writing reliable tests easier by presenting a set of rules which they can follow. Its content is originally aimed at increasing the quality of unit tests but, in my opinion, can be considered useful for any type of tests.

### A Letter by Letter Explanation

#### F: Fast

Your tests should be as fast as possible. The faster the tests, the more willing we are to run them and, by extension, the more profitable they are. Execution time is especially important for unit tests; they should be executed by every member of your team at least one time per each task they do (before committing changes to check that nothing was unintentionally broken down). As an additional guarantee, you may add them to your `CI` process and run them after each commit in your `Git` repository is made. The execution of unit tests should not take more than a few seconds, even for more complex systems with a large number of unit tests. A longer execution time may indicate some issues with your codebase. Of course, an execution time of 10 or 15 minutes is something absolutely wrong and you can be sure that there are some serious problems with your production and/or test code. 

Additionally, more complex algorithms, like sorting, (especially on large data sets) are a completely understandable exception from this rule. On the other hand, for around hundreds of thousands or millions of unit tests, execution time should not be a primary concern. Moreover, other types of tests may require a more complex setup and, by extension, take more time to execute. Keep in mind that it may be profitable for you and your team to stop for a moment and think about how you can make your tests faster without losing the assurance they give.

#### I: Isolated

There is nothing complicated about this rule so I will keep it simple: tests should not depend on one another. It is a very good and useful rule for all types of tests. Its breaking may result in a large number of false-positive or false-negative tests with various results depending on the order of running tests. It is an easy way to make your tests lose their worth and do more harm than good. Each test should do any setup required to be executed correctly and clear any context with which it is used. It is quite easy to achieve for unit tests but is harder for integration or end-to-end tests. For security or performance tests this rule should not be taken into consideration (and sometimes it even ought to be broken). Some more complex performance issues or security vulnerabilities may occur only in very specific scenarios.

#### R: Repeatable

Your tests should run in every environment and, most importantly, their results should be the same on each of them. Of course, performance or security tests on a developer's local machine will give totally different results than one done on a `pre-prod` environment. Tests whose results differ from environment to environment cannot bring any good to your team and can lead to any number of curious or even funny situations. No one wants to see that during `pre-prod` or prod deployment all tests included in your `CI` process failed, while a few stages earlier during deploy to `dev` every one of them passed and were all green. Especially, when it is Friday and you want to go home after pressing the deploy button.

#### S: Self-Validating

Each of your tests should be able to auto-detect if it passed or not. It is not advisable to make any kind of manual comparison between test output reports to check if a test failed or not. If you are forced to do it, the number of failed or passed tests may depend on various reasons and thus be completely random. In the case of performance or security tests, you can automate them to be able to easily verify if your application meets requirements or not.

#### T: Timely/Thorough

People who believe in TDD, Robert. C. Martin included, explain this letter as `Timely` and state that tests should be written in the correct time—for TDD it means while writing a production code. This should force you to write a cleaner and more testable code. Additionally, they claim that writing tests after your production code can lead to a more dirty and less testable code. It may bring some benefits for the unit tests but may not be so good for other test types. 

That is one of the reasons why I, personally, prefer to explain this letter as `Thorough`. It means that when we test a method, we should cover not only the execution of happy paths but also possible errors and negative paths. It helps make our tests more meaningful, more complete, and gives us a better understanding of more complex parts of our code. Naturally, there is no big deal in following these two explanations together and combining their advantages.

## **Summary**

Every programming paradigm has its own unique set of rules which was created to help us—developers build more fault-tolerant and easily maintainable systems. `FIRST` is their counterpart which you can follow while writing your tests. It is independent of the programming style which you use and you may apply these principles for tests written in any language and for any type of code. I hope that reading this article will give you some useful advice in writing your tests.

Topics:

 

UNIT TESTS, PRINCIPLES, FIRST PRINCIPLES, CLEAN CODE, BEST PRACTICE
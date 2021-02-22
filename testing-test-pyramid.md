# REST APIs Test Pyramid

### If you want to know how you should structure your tests to make them more reliable and profitable here you will find my proposition on how to do it.

As developers, we all love tests. Even when we did not manage to write any for our production code, we still want to include them in our codebase. Unfortunately, or fortunately, today, I will not be talking about finding time for writing tests in your projects or how to convince your manager to give you additional time for that. Today's topic will be my proposal of what the Test Pyramid for API-s should look like. No matter if you are a junior, a senior, or an intern, you must have heard something about Test Pyramid. Here I will present my view from a developer’s perspective on what it should look like and how I would like my test to be structured. We will start our journey by diving deeper into the theory behind the Pyramid.

## **What the Test Pyramid Is and Why it Matters**

It is a concept that is used to describe the number of different types of tests visually. Test types are sorted so that the base is represented by the test type of the highest quantity. Moving higher in the pyramid, each level is represented by the type with the lower number of tests.

In my opinion, the best representation of this pyramid is presented by Robert C. Martin in his book, *The Clean Coder: A Code of Conduct for Professional Programmers*.![XUnit pyramid](https://dzone.com/storage/temp/14247465-uncle-bob-pyramid.png)

This representation is excellent at presenting the ideal test coverage and how our tests should be structured. Following such a structure gives us more reassurance that we will not be surprised by our system in any negative way.

Unfortunately, in most cases, our tests' structure looks more or less like the one presented below.

![manual testing pyramid](https://dzone.com/storage/temp/14247481-1608394620523.png)

The structure of tests presented above gives us almost no guarantee about our system's inner workings and in longer perspective will cost us much more in terms of money and, even more importantly, our client’s trust. Fortunately for us, in the case of APIs, we do not need any manual tests. Everything can be done with the correct usage of appropriate tools.

We are done with the introduction. Now it is time for the main course of the day.

## **The Test Pyramid** **for** **APIs**

Starting from the bottom of the pyramid

## **Unit Tests**

My pyramid proposition base will be the same as the one above, so unit tests, unit tests, and unit tests once more. Everybody knows the benefits of unit tests and why we should have as many of them we reasonably can. The higher unit tests coverage, the better for you and your product. On the other hand, we have to write our unit tests wisely. In most cases, it is pointless to test getters, setters, or constructors. Private methods will probably be included in some public methods tests, so you do not have to worry about them too much. Also, methods responsible for or heavily rely on 3rd part integrations may not be best suited for unit testing.

It means that in many cases, your coverage will actually not reach 100% but values up to 70 or 80% are totally understandable and acceptable. Additionally, 100% code coverage of unit tests can be viewed as bad practice because it may require creating lots of mocks. This could make your tests less reliable and more time-consuming. Keep in mind that your unit tests should follow the F.I.R.S.T principles. There are counterparts of the S.O.L.I.D principles for tests. It should make these tests more trustworthy, easier to use, and, in the end, more beneficial for your team.

### Tools:

Almost every language has its own unit test library or framework. Below you can see some of the most popular:

Java – Junit, TestNG

Python – PyTest, PyUnit

Scala – ScalaTest

Javascript — Jest

## **Integration Tests**

We should cover any integration with 3rd party systems that our API uses in this stage of our tests.

Here the code coverage does not matter so much as testing of particular functionalities. For example, if the correct execution of our method depends on an external system, we should test if our method is really able to connect with this system and execute the desired method on its side. For example: placing an order in the delivery system, checking our user permission in some identity provider, and the most common CRUD operations on the database.

It would be best to keep in mind that you use some demo environment for this test, so we only test integration logic and not integration itself. There is still a chance that it may fail in a production environment for various reasons. It starts from a simple typo in config properties file, network visibility issues, 3rd party system production environment issues, and ending at differences in provider environments' configuration. It is technically the 3rd party system provider's fault that your API is not working as expected but try to say it to your users.

### Tools:

You will probably need some mocking utilities; in the case of Java, it can be Mockito. Remember not to overuse mocking because you will start to test it instead of your code. In-memory databases, like H2, is pretty useful for integration tests. Docker could also help you here because it allows you to build and clear the test environment quickly.

## **End to End Tests**

Here we should test the full features of our API. The coverage of the E2E test should be based on checking the main business features offered by API. Their main point should be answering the question if, after sending a request to our API, the response we get meets the required conditions, and all external systems were updated as needed. The only difference between E2E tests and integration tests is that here we test our API's full path from REST request to checking the state of integrated systems. We do not trigger our method from the code, but we send an actual request to a running server.

Tools, in this case, are pretty similar to those from integration tests. Remember to use far fewer mocks; in fact, do not use mocks at all if possible. Moreover, tools like Rest Assured could be handy here.

## **Performance/Throughput Tests**

These layers should contain tests that measure our API estimated throughput and performance. Their task is rather simple, that is to answer the question: How many requests per second and for how long our system can handle. It seems quite easy; however, be aware that if we want to do it properly, it becomes a tough and complex topic. There are several articles about this - I will put some links at the end of the article. Finally, we must remember that such simulations of production load are only what they are, namely simulations. Our users can surprise us in their fondness for using our API. Quoting one of the speakers about performance testing traps: “One can simulate production load only in production.”

Tools:

Here two most popular tools are probably JMeter and Gatling; both have some pros and cons and different trade-offs. Both are language and frameworks agnostic, so you can use them to test everything that is “REST-based.” Read about them and choose one that suits you better.

## **Security Tests**

Nowadays, when the leakage of users’ personal data might cause us a great deal of trouble. Security tests should be one of our biggest concerns. The coverage of security tests should not be measured in any way, shape, or form. We should check if our API does not allow things like permission escalation, unauthorized access to user data, or even SQL injection. Additionally, we need to check if CQRS protection is enabled and if all of our endpoints are secured. Fortunately for APIs, we do not need to think about Front-end related vulnerabilities (e.g., XSS attacks), so we do not need to do a great amount of mostly irritating checks.

### Tools:

There are tools available to help you do your API security tests, such as ZAP, HackerOne, Vooki, or Acunetix, but I recommend doing some research on your own to choose the one.

## **Monitoring**

Yes, yes, I know that this article was supposed to cover only tests. However, I cannot hold myself from mentioning such an important topic tightly coupled with tests. Monitoring is an integral part of assuring our API quality. Together with performance tests, we can give us lots of information about our environment's behavior and how many resources, such as CPU and RAM, our API will utilize in production. Such information can help us prepare our production environment infrastructure, saving us a great deal of trouble. In my opinion, monitoring is a must-have, especially when you work with such a popular topic as microservices. It is less critical for monolithic systems, but it does not mean that you should not have it.

Finally, our pyramid should look more or less like a tower from Lego bricks than a pyramid. I think that comparison to Lego bricks is pretty accurate here. Each type of test is a part (brick) in assuring the quality of our system. Additionally, I have added a big circle in the background, representing monitoring, to show that it should be an integral part of our API quality assurance process. I visualized this whole structure below.

![monitoring bubble](https://dzone.com/storage/temp/14247486-1608395676030.png)

## **Summing Up**

I presented my view on how API tests should be structured and which tools can help you build the described structure. I hope that it will give you some more general knowledge about the tests and tools you may use. Please be aware that it is only a suggestion, and you can modify it as it suits you.

Also, remember that tests should provide as much guarantee as possible, and even the best-written tests do not give you 100% assurance that everything will work well in production. There is always a possibility that something goes wrong or we encounter a case we did not foresee. Tests only help us to reduce the chance for such a case to occur.

## **Links**

### **Performance** **Test****s** **Related Articles**

http://www.mercury-consulting-ltd.co/wp/Performance_Testing_Traps.html

https://www.guru99.com/performance-testing.html

https://www.softwaretestingclass.com/performance-testing-metrics-baseline-and-benchmark-testing

https://thenewstack.io/five-principles-monitoring-microservices

**Robert C. Martin's book mentioned in the article:**

https://www.pearson.com/us/higher-education/program/Martin-Clean-Coder-The-A-Code-of-Conduct-for-Professional-Programmers/PGM8366.html

### **Tools**

https://gatling.io/

https://jmeter.apache.org/

https://rest-assured.io/
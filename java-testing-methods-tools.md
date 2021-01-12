# Testing Issues in Java Technology

INTRODUCTION:

Software testing is the method of examining software, to confirm that it satisfy its necessities and to identify errors. Software testing is an experimental examination which is conducted to afford stakeholders with information about the quality of the product or service under test, with respect to the context in which it is intended to operate. Testing can never entirely establish the exactness of computer software. As an alternative, it provides an analysis or a relationship that compares with the state or behavior of the product against a specification. Over its survival, computer software has sustained to grow in convolution and dimension. Testing in the world of Java technology proposes many distinctive and dissimilar challenges. Today’s Java engineers are not only faced with multiple platforms, but also the scalability issues like limited memory, nonstandard input, network traffic considerations which are encountered with an increasing array of Java technology-based computational devices—from Java Rings, to telephones, to set-top boxes. Because of dealing with many different platforms, and many different types of devices the tests are written in the Java programming language. An automatic software testing is a software function which is used to authenticate that a particular unit of source codes is running as expected. Software testing can be written as:

- Unit Tests;
- Integration Tests; or
- Acceptance Tests.

The report also gives an overview of the role of these technologies in Java testing. (ece.cmu.edu)

Integration Testing:

Integration tests is used to test integration of several classes as against to testing classes in isolation. Mainly in J2EE environments like Web or EJB container gives a more important functionality, hence integration testing has to be conducted in a container. This would test interaction across different application tiers like access to database, EJBs and also other resources. Integration test is occasionally done by Programmers but not as frequently as unit test.

Acceptance Tests:

Acceptance Test is explained as the group of tests which guarantee the contract between the application API and end user. These tests are done for the completed and deployed application. It is used to check each use-case in which the application is supported. It provides less test coverage and it is main in testing integration of application tiers such as containers and web servers. These tests are done by Quality Assurance testers and not by developers as test operates on external Application Programming Interfaces.

Unit Tests

Checking of the Developed Project, Programmers write unit tests to verify their own code. Unit testing vary from integration testing, which works well together, and acceptance testing, works according to the customers requirements. Unit testing is the testing mechanism which is used to test a single unit of code. In the case of Java, a unit testing that frequently connects to a distinct class. A unit test is fully automatic, non interactive, and dual ie, it moreover succeeds or be unsuccessful. By executing the code and verifying the output is not a testing. Neither instead of writing a small “test driver” that neither drives the code nor allows checking logs to see if it’s working correctly. Unit testing is the code which is written, as a good thing since it leads to higher-quality code, higher productivity, and lower maintenance with good evolution costs. The following report will analyze various Unit Testing technologies like JUnit, TestNG and JTiger. (devx.com)

JUnit:

JUnit is a unit testing method which is simple and open source frame work for regression testing released by IBM under common license version 5.0 and to write and run repeatable tests on the Java programming language it was hosted on the source forge.Junit is a instance of Xunit architecture which was used for unit testing framework , XUnit was developed by Erich Gamma and Kent Beck. Knowledge and Experience gained with JUnit is very use full in developing test driven application development, and due to this knowledge of JUnit is in use in test driven development. JUnit is also ported to other programming languages like PHP, C#,python, Fortran, Perl,and C++. It is used as PHPUnit in PHP, Nunit in C#, PyUnit in Python , fUnit in Fortran , Test::Class and Test::Unitin Perl and CPPUnit in C++ . All this unit testing frameworks family is collectively reffered as XUnit. Now recently due to the development of new client frameworks like AJAX, Junit has also been developed for use in java script as JSUnit.Junit improves the quality of code and increases the speed of programming. (en.Wikipedia.org)

Junit Features include:

- API for Easily creating Java test Cases

- Assertions for testing expected results –verify expected versus actual result

- Test fixtures to share test data commonly

- Test runners to run tests

- For forums and mailing lists.

- Aggregating tests (suites)

Junit Mechanics:

- Define a subclass of TestCase.

- Override the setUp() & tearDown()methods.

- Define one or more public testXXX()methods

- Exercise the object(s) under test.

- Asserts the expected results.

- Define a static suite() factory method

- Create a TestSuite containing all the tests.

- Optionally define main() to run the TestCase in batch mode.

JUnit Extensions:

- JUnitReport

-Apache Ant extension task

-Uses XML and XSLT to generate HTML

- Cactus

-Simple unit testing framework for server side Java coding

- JWebUnit

-Framework for creating acceptance testing for web based applications

- XMLUnit

– Provides an XMLTestCase class which enables assertions to be made about the

Content and structure of XML

- MockObject

– “double agent” used to test the behavior of other objects

– Dummy object which mimics the external behavior of a true implementation

– observes how other objects interact with its methods and compares actual behavior with preset expectations

- StrutsTestCase

– testing code based on the Struts framework (javapassion.com)

TestNG:

TestNG is an annotation-driven Java unit testing framework inspired by JUnit and NUnit which aims to overcome many limitations of JUnit. TestNG isn’t just really powerful, innovative, extensible, and flexible; it also illustrates an interesting application of Java Annotations, a great new feature in JDK 5.0. TestNG is designed in such away that it covers all categories in testing like: unit, functional, end-to-end, integration, etc…(testng.org)

TestNG Features include:

- Annotations; no mandatory naming patterns for test methods or extending classes

- Also Supports Java doc annotations for Java 1.4

- Powerful test-fixture set-up and tear-down options

- JDK 5 Annotations

- JDK 1.4 also supports JavaDoc annotations.

- Test configuration is flexible.

- Data driven testing support using @Data Provider.

- Parameters support.

- Distribution of tests in slave systems is allowed.

- Execution model is much Powerful no need of Test Suite.

- It is supported many plug-ins and variety of tools like Eclipse, Maven, IDEA, etc.

- By Embedding with Bean Shell improves flexibility.

- JDK functions are available by default for logging and runtime.

- Methods for testing of application server.

TestNG Mechanics:

- Write the business logic of the test and insert TestNG annotations in the code.

- Adds information’s about the test in testng.xml file or in a build.xml (eg. class name etc).

- Execute TestNG.

JTiger:

JTiger is a framework for unit testing and has tools for Java 2 Platform. It gives useful abstraction for writing unit test cases and fixtures. Functionalities provided by JTiger is most desired in unit testing of software’s. Generally Test-Driven Development is encouraged by JTiger development, though it is not mandatory, and any unit testing technique in software development is sufficient. Many features of Java Programming Language 1.5 is heavily used by JTiger like Generics, variable argument lists, annotations, etc,. JTiger also encourages users to do documentation of unit test cases and fixtures to give a robust and easy maintain regression harness and unit test. Published and documented API is provided by JTiger frame work if there is a need for extending the functionality. The ability to run the test cases written by using JUint framework is a good example of extending JTiger framework. As a part of JTiger framework JUnit plugin implementation classes are included. (en.Wikipedia.org)

JTiger Features include:

JTiger makes every effort to ensure a robust unit test harness by providing a rich set of tools, and a reliable, usable framework on which to develop unit test cases. JTiger is an implementation that is based on improvements in software development methodologies, such as eXtreme Programming (XP), which have matured since their inception. JTiger has been implemented using the same techniques that it encourages, specifically, Test Driven Development. JTiger includes its own unit test and regression harness with 100% method coverage. This aids in new or modified requirements for JTiger that arise in the unforeseen future. The JTiger source code base has been designed to ensure the absolute maximum possible amount of decoupling of components and encapsulation such that future enhancements will not have a negative impact on future versions on JTiger. (userdoc.com)

JTiger System Requirements:

JTiger requires that tests are executed using a Java Virtual Machine version 1.5 or higher. This does not mean that the code under test must be written using Java 1.5 language features. The software under test may target any JVM version such as 1.2, 1.3 or 1.4. It is merely the test run that must execute under a JVM version 1.5. JTiger is capable of executing test cases that have been written using the JUnit test framework.

JTiger Extensions:

JTiger future development is being investigated. Some of the features and topics that are under review include the development of a swing GUI interface from which to execute unit test cases, the development of IDE (Integrated Development Environment) plugins for Eclipse, and Intellij IDEA, and the possibility of a mock objects package that mocks J2SE and J2EE core classes. (userdoc.com)

Testing Of Java Language Recommended By Other Companies:

Microsoft Java Virtual Machine Support:

To be familiar with the need is to afford a smooth evolution for present users of the Microsoft® Java Virtual Machine (MSJVM), Sun Microsystems and Microsoft have agreed to widen Microsoft’s license to use Sun’s Java source code along with their compatibility test cases and the test suites. (microsoft.com)

Open Quality Program to Improve the Quality of Java Software:

Agitar: – This Software has declared a new multi-vendor scheme committed to take very careful attention on the testing side to improve the efficiency and the quality of java software packages. Based on the Open Quality Program, Agitar software has published a wide range of unit level testing metrics in its own products, It has even including a formal weekly build results on its future unannounced new products and the new trends over time. The Agitar software has also published these software testing metrics for many popular open source software Java projects, which including JUnit, Hibernate, Cruise Control, Struts, spring, and even lot of other commercial products which is based on the open source such as the Oracle’s BerkeleyDB Java edition and JasperSoft’s JasperServer. (qthreads.com)

Recent Testing News on upcoming Java products:

- Java Graphical User Interface Testing Tool Known as Squish is Supporting the New Eclipse Ganymede version 3.4

*Five weeks Two days ago(*2008-07-08*)* Germany BasedHamburg( froglogic GmbH )has announced today , to give support for automated the testing of new Java Rich Client Platform the RCP applications which is based on the new Eclipse version 3.4 release code that is named as Ganymede. The Squish GUI for Java is a leading functional Graphical User Interface and also regression testing tool which is enabling the execution, creation and Modification of automated Graphical User Interface tests for Java programming on AWT/Swing and SWT/RCP applications. The Squish GUI, and all the other all tests created with this, are completely made as cross-platform software and work on almost all the top leading operating systems like the Windows Operating Systems, Linux Operating Systems, Unix Operating Systems, and Mac Operating Systems X and even also on the embedded Linux. The support for testing Eclipse Version 3.4 the latest one, RCP Software applications has been completed and it is also available with the just newly released GUI Squish version 3.4. The availability of the third party tools, like the Graphical User Interface testing tools, is very important for the open source Eclipse community to increase it demand. The resent glad news is to see froglogic has very quickly adopting the new Eclipse version releases with its Testing GUI Squish tool”, said Mr. Mike Milinkovich, Who is the Executive Director of the Company Eclipse Foundation. The new Ganymede version release is said to be another very important and great release of the Company Eclipse community which has make it even more easy, attractive and flexible for the application developers who uses it. The Quickly integrated support for testing tools on Eclipse software version 3.4 applications with the new GUI Squish tool has been a new and great logical step for the company, said Mr. Harri Porten, platform chief of froglogic’s company. Squish software offers a multipurpose testing based framework for Graphical User Interface (GUI) applications with a base choice of the popular test scripting languages like Perl , Python, Tcl, TSL and JavaScript, VBScript extended by the new test specific functions scripts, lot of new open interfaces, many add-ons, with integrations to the test management tools, a new powerful Integrated Development Environment (IDE) aiding the selecting, creation and finally debugging of the tests and a collection of command line testing tools and the test management integrations facilitating the fully automated test runs on the applications. Contact email: squish@froglogic.com or visit the URL: www.froglogic.com/squish to get an evaluation pack or to purchase the GUI Squish for the java Application or to know more about the product. Squish GUI tool also supports automated testing of java applications based on GUI technologies like as Trolltech’s Qt, Swing-AWT, and Qtopia, also supports Web technologies DOM, HTML, AJAX, DHTML Mac OS X Carbon/Cocoa , JavaScript and other technologies also. (embedded-computing.com)

JAVA‘S ENCOURAGEMENT IN TESTING:

Today’s software manufacturers face a variety of challenges when creating products to meet the increasing demand for software that takes advantage of Java technology. The growing complexity and diversity of software’s—with their varying operating systems, processors, and memory configurations—increases the need for thorough testing to ensure customers will be satisfied. At the same time, service providers and manufacturers face the challenge of managing—and, if possible, lowering—internal costs caused by excessive engineering overhead, disorganized development of test cases, or the impact of new data services on support operations. The Java software Test Suite simplifies quality assurance and reduces time-to-market for Java implementations by providing comprehensive tests and a robust test manager. These enable suite users to evaluate, validate, and verify the quality of implementations on particular software. The Java Test Suite that helps the software manufacturers ensures their reputation for quality, while building customer satisfaction and loyalty. It helps lower engineering costs by standardizing and simplifying testing, and by minimizing the need to write quality assurance tests manually. Java is free and open. So java Testing is an important part of software development because of Open Source Testing Tools which are available in Java for an Effective testing key factor which reduces the total cost of maintenance of any application over its lifetime. Because of the reduced cost and time of development; this can increase savings on quality assurance and of course on sustaining. Without knowing when to invest in better design, in post development quality assurance, in manual tests or in automatic testing forms a basic difference between successful and unsuccessful software projects in these tough and competitive days. (newsgroups.derkeiler.com)

Sun’s Recommendation for testing Java:

- Testing Java in an Object-Oriented Way

- To apply eXVantage (a tool suite for code coverage testing, debugging, performance profiling, etc.) to a large, complex Java application at the implementation and unit testing phases in Avaya.

- Two open source-based tools that can help to generate workloads for the enterprise application: SLAMD Distributed Load Generation Engine and Sun Java Studio Enterprise software.

- Container-Free Testing With Mockrunner

- Fitnesse Testing for Fast-Paced Agile Web Development

- JUnit Reloaded

- Using In-Process Testing Metrics to Estimate Software Reliability

ECLIPSE IN JAVA SOFTWARE TESTING:

Eclipse is a very widely used Open Source Integrated Development Environment (IDE), particularly for Java- and Web-based applications. It’s very widely extensible with the use of scores of plugins available to help in developing these applications, so here I would like to discuss some of the plugins available for software testing. Eclipse TPTP (an evolution of Eclipse’s prior “Hyades” project) is an Open Source collaborative project that seeks to provide a common platform upon which specialized, differentiated, and interoperable offerings for software test and performance tools are created. TPTP, which stands for Test and Performance Tools Platform, supplies extensible frameworks and services for test and performance tools that are used throughout the application lifecycle, from development through production. It also delivers extensible exemplary tools that verify the utility of, illustrate the appropriate use of, and support the development and maintenance of the platform itself. (freshmeat.net)

- TPTP

— This provides a reference implementation of the recent UML2 Testing Profile based upon the Eclipse Modeling Framework (EMF). For logging and tracing, TPTP employs the Common-Base-Event (CBE) Format, which defines the structure of events in a unifying format. Data pools are a concept which appears to be innovative for testing in Eclipse. A datapool contains data usable during a test run. Data pools are stored in a comma-separated file and can be displayed as an Excel-alike data sheet. Test cases can load datapools in the setUp() method and use the dedicated iterations to retrieve cell values for evaluation purposes. Separating test data from the tests themselves appears to be new functionality. Test Deployment requires the allocation of test artifacts to test locations. These locations can also be workbench locations. Through the concept of service abstraction, the Automatable services framework introduces a Service-Oriented Architecture (SOA) concept to TPTP. The framework introduces a layered architecture in which a client at a lower layer employs a model adapter to employ a service provided at a higher layer. Automation Client Adapters allow normal Java programs to consume Eclipse services. These adapters can launch a headless Eclipse from a specified Eclipse home, and are available through the tptp-automation-client jar. Various properties are made available through this API (e.g., the project property specifying a specific project in the workspace of the Eclipse instance). (freshmeat.net)

- Solex – Web Application Testing with Eclipse

–Solex is a free open source Web application testing tool built as a plug-in for the Eclipse IDE. It provides functions to record a client session adjust it according to various parameters and replay it later typically in order to ensure non regression of the application’s behaviour (with stress testing capabilities being added at a later stage). By recording, we mean that Solex acts as an HTTP proxy and records all HTTP requests and responses going through the wire between a Web client (eg. a Web browser) and a Web server. The task of replaying a scenario consists in sending the previously recorded and eventually customized HTTP requests to the server and asserting each response. Solex 0.5.0 works with Eclipse Release 2.1.2, Build id: 200311030802 and Solex 0.5.3 works with Eclipse Release 3.1.1, Build id: M20050929-0840. (Solex.com)

- Unit testing

–Checking of the Developed Project, Programmers write unit tests to verify their own code. Unit testing vary from integration testing, which works well together, and acceptance testing, works according to the customers requirements. Unit testing is the testing mechanism which is used to test a single unit of code. In the case of Java, a unit testing that frequently connects to a distinct class. A unit test is fully automatic, non interactive, and dual ie, it moreover succeeds or be unsuccessful. By executing the code and verifying the output is not a testing. Neither instead of writing a small “test driver” that neither drives the code nor allows checking logs to see if it’s working correctly. Unit testing is the code which is written, as a good thing since it leads to higher-quality code, higher productivity, and lower maintenance with good evolution costs. (devx.com)

- Unit Testing in Eclipse Using JUnit

–Unit Testing in Eclipse 3.1 move towards through the JUnit mechanism which was constructed in the Workplace. Eclipse permits a quick creation of test case class and also to provide a testing suite class to write the testing codes in. By means of Eclipse, we use a Test Driven Development (TDD) which suit for a very simple mechanism to systematize and implement. The class which we wanted to test is created at 1st and so that the Eclipse find’s a class that is underneath test and construct the test case class. The test cases are constructed with the desired trade in and expansion for JUnit to Execute. The test case classes are constructed with the real test case and then it is coded in through the programmer. The formation of test suite in Eclipse is still simpler. By designing a test suite, Eclipse will specify a name for specifying all of the test cases in the scope of the project. The code is to Execute test suites and to add test cases in the created software is added to the test suites. (open.ncsu.edu)

JUnit Naming Convention:

- Testing Cases and Classes: Named with [classnames]First.java, hear classname is being tested for the classnames.

- Testing Cases with Test Methods: Name test [methodnames], where methodnames are the method name to test.

- Applying Test Suites: Eclipse which has a default name as First.java

It is a well known thought-out for a good performance in testing, and to split the test case code from the application code. It is also a good idea which is to separate the JUnit and FIT tests as well. (open.ncsu.edu)

EMPIRICAL i.e. STATISTICS AND NON-EMPIRIC TESTING:

Empirical testing:

Similar to theoretical tests, there is one more test called empirical test; which is being done by computers. As per the computer point of view ‘empirical’ means ‘experiments’ with certain PRNG. The generator itself is treated as black box; only the sequence of PRNs is generated and is taken for evaluating the test statistic. In order to find the empirical test, one has to implement the PRNG and the test statistic. The limitations of empirical testing are commonly proposed by the amount of time and memory needed due to the complexity of the computations. (random.mat.sbg.ac.at)

Non-empirical testing:

Non-empirical testing deals with non-parametric statistical models and non-parametric inference, including non-parametric statistical tests. Nonparametric methods which are often referred as distribution free methods, since they do not rely on assumptions in which the data are drawn from a given probability distribution. Non-parametric statistic can be explained as a statistic (a function on a sample) whose interpretation does not depend on the population fitting any parameterized distributions. The example for one such statistic is order statistics; which plays a vital role in many non-parametric approaches. (en.wikipedia.org)

Applications and purpose:

For studying populations that has been taken on a ranked order (such as movie reviews receiving one to four stars) is called Non-parametric methods. The uses of non-parametric methods may be necessary, when the data has a ranking but no clear numerical interpretation, such as when assessing the preferences. Usually non-parametric methods make fewer assumptions; other than the parametric methods, depends upon their applicability. In particular, they may be applied in situations where less is known about the application in question. Non-parametric methods are more robust due to the reliance on fewer assumptions and simplicity. Non-parametric methods may be easier to use in certain cases, when the use of parametric methods is justified. As per some statisticians, non-parametric methods are not useful because of their simplicity and greater robustness leading to leave less room for improper use and misunderstanding. (en.wikipedia.org)

Recommendations:

Example of a JAVA Based Application:

The Table below represents an example security standard for a Java application; and mentions the type of software testing used in each controls. These Kind of security standard in applications will define how exactly the application security functionalities will behave.

| TYPE                         | Questions                                                    | Unit | Integration | Acceptance |
| ---------------------------- | ------------------------------------------------------------ | ---- | ----------- | ---------- |
| InputValidation              | Whether all the user inputs are proper in length and data type? | X    | X           | X          |
| Authorization of Application | Whether the application properly controls the accessing privileges? |      | X           | X          |
| Storage                      | Whether authentication privileges are stored securely?       |      | X           |            |
| Manipulation                 | Whether application enforces its access controls?            |      | X           | X          |
| SpecialCharacters            | Whether special characters are handled securely?             | X    | X           | X          |
| ErrorMessages                | Whether error message generic?                               |      | X           | X          |
| Legacy data                  | Whether data are removed?                                    |      |             | X          |

It is clear that the Above Types can be tested more on using functional than, integration testing techniques. Lesser security functionality is tested by Unit tests, as lot of security functionality are provided in other modules like, web server and web container.

The following sections will provide more details on performing security based tests in unit test, integration test and acceptance test:

Testing in Unit Tests:

Testing of classes and methods individually provides a best approach to functionality testing of codes. A unit test has to be performed on classes and methods individually without a any dependency on the other methods and classes. This issue gives limitation to the types of security tests which has to be performed, Hence this is going to test is going to be executed in very early stage of the development process.

Testing Independently:

- Unit tests should only depends on the single independent class and it should not depend on Composition class or any base classes

Testing Vulnerability:

- The number of security Types that are been verified by us
# Step by Step Guide to Building a Test Automation Strategy

Automation is about using tools to perform specific testing tasks more efficiently. It means that automated testing isn’t adopted to replace manual one but is always used to enhance it.

When an idea to implement automation crosses your mind, instead of *how* part, focus on *why* at first. Automation for the sake of automation often comes along with the following arguments:

- it sounds good, so it must be good;
- the guys from that other company already use it;
- we can cover 100% of functionality by automated tests;

This kind of reasoning is commonly mistaken for green light for the adoption of automated testing. Meanwhile, learning about test automation strategy is a good idea for those who want to make software delivery cycles shorter and leave less mundane tasks for QA engineers.

## What Is Test Automation Strategy?

It is the way a QA team applies automation during the quality assurance process. If you start decomposing test automation strategy, you’ll get a list of elements that will look approximately like this:

- test plans, both manual and automated;
- test environments, with definitions and descriptions;
- test cases;
- automation scripts;
- test data;
- test results;
- execution logs.

A cost-effective test automation strategy implies a result-oriented approach that helps companies keep up with the market trends and outperform, or at least reach their ambitious business goals.

A test automation plan and strategy are specific for every project. Even when you test similar products, some aspects require adjustments. You can use the same template to create a test automation strategy, but the final version will be at least slightly different.

## How to Build a Test Automation Strategy

So you already know that efficient automation starts with setting your goals and objectives. Let me specify what those are. Goals define what you aim to achieve in a long-term perspective, for example:

- better product quality and, as a result, higher user satisfaction;
- minimizing human effort and testing time;
- increasing efficiency of testing while reducing costs for it.

An objective is more like a ‘one step at a time’ thing. These are smaller tasks you have to complete to achieve the goals. The objectives of test automation include:

- broader test coverage;
- creation of tests that are easy to run and maintain;
- enhanced product quality after each iteration;
- improved stability and reliability;
- increased efficiency of QA engineers and better motivation

If you would like to get a more detailed explanation of each item on the list, you can find it in the post about [automated testing goals and objectives](https://www.qamadness.com/automated-testing-goals-objectives/). Now, let’s move on to the step-by-step instructions.
[![Build a test automation strategy](https://res.cloudinary.com/practicaldev/image/fetch/s--9ZjVi3bN--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/a173u53wfo59xohw7g8x.jpeg)](https://res.cloudinary.com/practicaldev/image/fetch/s--9ZjVi3bN--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/a173u53wfo59xohw7g8x.jpeg)

### Step 1. Decide what to automate

Setting up test automation is a time-consuming process, so consider what you plan to achieve in both short-time and long-time perspectives. Set reasonable expectations. Consider how frequently a test will be repeated and what scope of work it requires. In the perfect case, you will pick tests that match at least several of the following criteria:

- time-consuming if done manually;
- involve large data sets;
- cover stable software components;
- require the checkup in multiple systems (browsers, OS, hardware, etc.);
- mundane and don’t require creativity;
- with clear pass/fail results;
- highly-repetitive.

On the other hand, some tests are not suitable for automation. For instance:

- tests that will be executed once;
- tests based on visual perception;
- tests without evident pass/fail results;
- anti-automation features, like CAPTCHA;
- raw and unstable functionality.

I wouldn’t recommend automating these things. The process will take a lot of time, and the results you’ll get after running tests won’t be accurate. For example, UX testing requires clear human input and estimation. Exploratory testing relies on QA engineer’s knowledge, experience, even intuition and serendipity. Obviously, manual inspection is much more reasonable here.

### Step 2. Choose a test approach

A test automation approach defines how you will carry out the tests. A proactive approach initiates the test design process at the early stages of SDLC. A QA team finds defects before a build is created. In a reactive approach, testing starts after the coding is completed. A QA team examines an initial version of a product that is supposedly ready for the release.

[![Types of testing to automate](https://res.cloudinary.com/practicaldev/image/fetch/s--PKp-742q--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/28dqhtqe2nxl8pk72qw5.jpeg)](https://res.cloudinary.com/practicaldev/image/fetch/s--PKp-742q--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/28dqhtqe2nxl8pk72qw5.jpeg)

A test approach also covers types of testing a team is going to perform. The most common candidates for the automation are:
Unit Testing – a checkup of the smallest functioning elements of code by a development team that is familiar with the backend.

- Component testing – a checkup of every object or part of the software separately by a testing team on the frontend.
- Integration testing – an inspection of the behavior of unit combinations.
- API testing – a segment of integration testing that focuses on API functionalities.
- Compatibility testing – an analysis of software behavior in different browsers, OS, network environments, hardware, etc.
- Smoke testing – a review of a new build aimed to prove the basic functionality (installation, launching, registration, etc.) works so that the team can move on to testing the rest of the features.
- GUI testing – a check aimed to prove that the graphic user interface meets the specifications stated in software requirements.
- End-to-end testing – an examination of the complete workflow from the beginning to the end by replicating real user scenarios.
- Regression testing – an analysis expected to verify that code changes haven’t affected untouched parts of software features after the latest iteration.

**The Test Pyramid Principle**

The Test Pyramid is a principle that helps to choose cases for automation smartly. Unit tests that cover the lowest level of software code and have the highest ROI since they help avoid more serious mistakes. Thus, it is reasonable to automate this level to the maximum.

Component, integration, and API tests are the next in terms of priority. Regression is one more candidate for automation, as it covers the familiar functionality that requires revision after every iteration.

And only then goes GUI testing. Though very common, GUI automation is highly impractical. These tests are difficult to maintain, expensive, and with a high number of false positives and negatives.

[![Software testing pyramid](https://res.cloudinary.com/practicaldev/image/fetch/s--F9jsdfxF--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/7rk74uabqd48knf0vtmk.jpg)](https://res.cloudinary.com/practicaldev/image/fetch/s--F9jsdfxF--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/7rk74uabqd48knf0vtmk.jpg)

A traditional approach suggests the opposite: more automation for GUI and less for unit tests, and that is not always efficient.

**Factors to Suggest when Choosing a Test Approach**

- Technology the app is based on. App specifications affect the types of testing required and later – the choice of a platform and tools.

- Team roles. Make sure that every team member knows their responsibilities and is qualified to cover a particular aspect of QA on the project.

- Risks involved. To eliminate the most probable and damaging risks, you need to cover the vulnerable areas. And for this, you should know the risks. Create a document where you record the following parameters:

- - risk level – the difficulty to handle the risk if it becomes reality;
  - probability – the likeness of risk turning into reality;
  - mitigation – actions to take to resolve the risks;
  - cost estimation – the cost of mitigating the risk.

### Step 3. Select an automation framework

A test automation framework is a set of detailed guidelines, coding standards, project hierarchies, reporting mechanisms, etc. that create a scaffolding for the automated test scripts.

Simply put, it is a practice that makes test automation efficient. A QA team needs to choose a suitable framework to create scripts with higher re-usage, easier portability, and reduced maintenance cost.

Some of the most popular practices are:

- linear scripting;
- library architecture;
- modular framework;
- data-driven framework;
- keyword-driven framework;
- hybrid framework.

[![Test Automation Frameworks](https://res.cloudinary.com/practicaldev/image/fetch/s--xjf1u2WG--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/oa9ulwrmlx5nnog1g8xx.jpg)](https://res.cloudinary.com/practicaldev/image/fetch/s--xjf1u2WG--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/oa9ulwrmlx5nnog1g8xx.jpg)

**Record & playback**
Also known as linear scripting or record & replay, it is the simplest of all the frameworks. It allows QA engineers to record each step of the testing process and the validation parameters manually so they can play it back anytime later.

Linear scripting is the fastest way to generate a script, and it doesn’t even require automation experience. The downsides are little reuse and difficulties in maintenance.

**Structured scripting**
This framework also has alternative names also – library architecture or functional decomposition. It takes scripts created using record & playback, groups similar ones into functions, saves them into the library, and calls whenever needed.

The library architecture framework ensures a higher level of reuse that comes along with less costly development and less messy maintenance. On the other hand, it also requires expertise and more planning.

**Modular framework**
A test automation engineer divides an AUT (application under test) into smaller components – units, functions, sections – and creates a script for each to test them in isolation. Combining these scripts hierarchically, you can create an overall script for a broader test run.

You cannot use multiple datasets since the data are hardcoded, but if an application undergoes changes, you only need to fix an individual test script.

**Data-driven framework**
This framework separates test data from script logic and stores it externally. Test scripts use this file to read input or output parameters. The data-driven framework is perfect for situations when you need to test the same feature multiple times with different data sets. Thus, we can test scenarios with varying data quickly with a minimum number of scripts.

Setting up the data-driven framework is time-consuming. It is a task for a skilled test automation engineer able to format data properly and write scripts that can utilize all the advantages of this framework.

**Keyword-driven framework**
Used mainly for GUI automation, this framework also implies storing test data separately from scripts. The instructions are words that describe actions in consecutive order (login, click, etc.).

With the keyword-driven framework, you get reusable code to apply across multiple test scripts that can be built independently of the AUT. It also takes much time to set up and a lot of effort to keep in order. The high cost of the setup is justified if the application is large. Then, you can use the scripts for several years.

**Hybrid framework**
Every framework comes with its pros and cons. When test automation engineers start to maximize the positive features and minimize the downsides, they may come up with a combination of different frameworks that works the best for a particular project. Such a blend makes a hybrid framework, the most common option for an agile and DevOps test automation strategy.

### Step 4. Find qualified people to join your team

Don’t forget that software testing automation is a task for a person hired specifically for it. Some try to make automation a part-time job of manual QA engineers. However, these specialists already have other tasks to cover and, most likely, lack skills to set up automated testing. It would be more efficient to find a person with experience in scripting or at least relevant knowledge to be responsible for this scope of tasks.

### Step 5. Select a testing tool (or several tools)

The first thing you pay attention to is the popularity of a tool, and it’s not a mistake. Popular tools usually prove to be reliable and widely discussed online. An automation engineer can find many useful tips and interesting practices on the web.

Also, it is easier to find QA engineers well-versed in Selenium, Appium, or Cucumber than a tool that has just recently appeared and seems good. Besides popularity, there are other criteria to consider.

[![Testing tools](https://res.cloudinary.com/practicaldev/image/fetch/s--vzUJqXvO--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/cf804je397ufux4lkecl.jpg)](https://res.cloudinary.com/practicaldev/image/fetch/s--vzUJqXvO--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/cf804je397ufux4lkecl.jpg)

**Tech stack:**

- The technology software is built on.
- Requirements of the system under test.
- Programming languages you are familiar with.

**Tool features:**

- Supported OS, platforms, environments (e. g. automated browser testing, native/hybrid app testing, etc.).
- Cross-platform testing support.
- Complexity vs ease of use.

**Budget:**

- Are you looking for an open-source or a paid option?
- Are you looking for a development or runtime license in a paid tool?
- How frequently are you going to use this tool?
- Does the price justify the functionality it provides?

The experience shows that building a custom framework upon an open-source tool is the best option. Try several solutions. The majority comes with a free trial period so you can weigh all the pros and cons of a commercial tool before paying for the license.

### Step 6. Create and execute automated tests

So you know what to automate, what types of testing to apply, what framework and tools to use. It’s time to move from the arrangements to actual work and start writing scripts. Here are some tips that will help you with this:

- Outline day-to-day tasks and procedures.
- Create test case templates to use in multiple projects.
- Before writing a new test case, remember to check if similar cases already exist.
- Write tests that are concise and easy to understand, for in the future, other people will work with them, too.
- Make test scripts as minimal as possible, so they’ll be simple to understand and maintain.
- Break bulky tests into sequences and check each individually. The smaller they are, the less is the probability of a mistake.
- Make sure tests aren’t dependent on the UI. Using UI elements and pathways that may change is a sure way to create useless scripts.
- Have a clear plan of how to resolve failed test cases (perform feature analysis, just report to a dev team, etc.).
- Learn to prioritize test cases.

### Step 7. Maintain your scripts

People that have experience in delivering automated software testing services admit that things tend to get messy on the maintenance stage. So first and foremost, categorize the tests according to their purpose or come up with an alternative classification that will allow finding relevant cases quickly and easily in the future.

Don’t forget to categorize new cases. Review and clear up existing cases. Many of them aren’t meant to last forever, and that’s okay. Address product requirements to decide whether old cases are efficient and should be used or or they’re not relevant anymore. Never try to reanimate what is useless.

## Don’t Forget About Documentation

Documentation has nothing to do with bureaucracy and delays if you manage it right. On the contrary, documentation is more reliable than memory and helps you keep a clear head. So what to put on paper?

1. Outline the scope of work, then set milestones and timelines for each task. It will keep every team member on the same page.
2. Document the approach to automation – with the process, roles, and technology. It will become a set of guidelines you can address when you need to clarify something or resolve a dilemma.
3. Outline the automation environment. Create a pipeline for software to pass before it is ready for the release. Conduct risk analysis to be prepared for a ‘sth goes wrong’ scenario. Risks always exist, and ignoring the possibility of risk is just careless.
4. Create an execution plan to ensure bug-free testing. Write down day-to-day tasks and procedures. Run separate tests before launching automated regression. Make sure there are no fake results.

## Writing a Test Automation Strategy for Agile Projects

Test automation strategy in agile usually implies a proactive approach and a preference for automated unit testing. These are the basic things that make fast releases possible.
In addition to this, consider using a keyword-driven automation framework. If you choose words that correlate with business requirements, you will be able to use the scripts for as long as these words correspond to known commands. Keywords can also be a part of a hybrid framework, which enables flexibility so essential in agile SDLC.

The automation strategy for agile projects never overlooks unit and component testing. Testing one small feature at a time helps to extract true value from the automation. On the one hand, you can create a script for a feature right after a developer writes a piece of code. On the other hand, compiling smaller pieces into a test script that covers a large part of the functionality helps to minimize mistakes made on the way.

[![Test Automation Strategy for In-House and Outsource](https://res.cloudinary.com/practicaldev/image/fetch/s--B5P1Dffw--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/y8b013kcwxqjnxnag29k.jpg)](https://res.cloudinary.com/practicaldev/image/fetch/s--B5P1Dffw--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/y8b013kcwxqjnxnag29k.jpg)

## Test Automation Strategy for In-House and Outsource

There is no fundamental difference between a QA automation strategy for in-house and outsource teams. In both cases, you need to decide what test approach and framework to use, select automation tools, and find qualified people to handle the tasks.

The distinction lies in the scope of managerial work. Of course, a project owner (or whoever is in charge) has to manage the process in any case. And if you are reading about how to build a test automation strategy, you probably need to figure out some things about it.

In both cases, you need to start with the hiring process. Outsource testing is a time-saver here. Assembling an in-house team is not a quick process. Hopefully, you will find qualified people who are familiar with the best practices of test automation.

If you go for QA outsource, you get a fully functional team with relevant experience in software test automation services, well-tried automation strategies, favorite tools, and test script templates.

In other words, an in-house team will require more involvement and management, at least at the beginning. With an outsourced QA, you request reports, specify the details that interest you, but the rest is a task for them to handle.

Companies that have QA departments often decide to cover manual software testing with internal QA resources and outsource automation since it is more cost-effective. Some choose to start with an external team and hire automation specialists later if the flow of tasks becomes steady.

## Bottom Line

Implementing test automation takes time, effort, and specific skills. Automated testing is not always essential. It doesn’t replace manual testing and doesn’t benefit a project unless you define clear goals and objectives. So how to draw those advantages everyone is talking about?

Know why exactly you need QA automation. Keep reasonable expectations. Start small and focus on a long-term perspective. Don’t give up on automation if the results aren’t immediate. But first and foremost, work carefully on your automation strategy. Hopefully, I helped you with the latter, at least a little.
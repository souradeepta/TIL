## [Test Automation Anti Patterns](https://testvagrant.tumblr.com/post/95444422751/test-automation-anti-patterns)

­Test automation needs no introduction. For some it is a career boost, for some it is improved quality software, for some it’s faster time to market and many more. In a nutshell, it is perceived the problem solver to a lot of Quality questions in the IT services industry.

So if i had to put it briefly in industry terms this is what we expect.

![image](https://64.media.tumblr.com/864c87e03331163009f022b740a47bdd/tumblr_inline_ph2c5jMHCI1sma618_500.png)



Now having said that, though most of us start with one of the above mentioned objective – are we really achieving the same ? The answer is NO, more often than not. And the background behind the post. I have been doing test consulting and have met many teams who started doing test automation hoping it will solve their problems but are more distressed and confused than ever before. They have diverted effort and investment towards test automation but the return on investment is hardly encouraging. 

See if you could relate to one of the following symptoms:

- You have a good test bed of automated tests but your team still struggles(read working late evenings, running post to pillars) every release.
- You have probably never seen all tests passing all at once.
- Your release cycles haven’t really reduced.
- You spend a lot of time to get a traceability of the automated tests to features/stories.
- A lot of the tests are intermittent. 

If any of the above is happening to you, this post is for you. Otherwise, you are doing awesome my friend. No need to read further.

We have seen a lot of these symptoms common across the teams we have consulted in the past and based on the same i have compiled the following list. We call it Test Automation Anti Patterns, something you should strictly not be doing:

![image](https://64.media.tumblr.com/faae2f61e99cc3c40207c4ad8dae3362/tumblr_inline_ph2c5jp6wI1sma618_500.png)



**Using a different language for test code compared to the application code.**

This is a common anti pattern that a lot of teams do. They choose a language/tool to suite the QA’s for writing functional tests without being bothered about what the developers in the team are using to writing application code. This is a big NO NO. The disadvantages of doing this could be:

- Losing on leveraging the developers knowledge while creating test framework and writing functional tests.
- Even if developers agree to help, it is a lot of context switch for them to write application code in a different language and then look at the test code in another language.
- You would be then forced to create a separate test project and hence manage it separately.
- More repos and branches to maintain

**No team ownership of the functional tests.**

More often than not, it is assumed that functional tests belongs to QA’s and they should be owning it. While this may be happening traditionally, it is a big NO for Agile teams. Agile teams are all about faster and quality delivery where QA’s do not play a gate keeper’s role but play more of a navigator. The way you should be dealing with functional tests in terms of ownership is:

- A QA playing a proxy customer where he drives what needs to be tested.
- Developers and QA’s pair and write most of the functional tests.
- Developers should own smoke tests that runs as part of every check-in.
- Business should be involved in designing functional tests and validating that the right workflows are getting tested.

**Creating a solution where a completely non-technical person can handle automated testing.**

Test code is no secondary citizen. It has equal importance as your application code, if not more. It has a key role to play in the successful delivery of the project. Having said that, this is a purely technical area in IT. You need people with experience and expertise in achieving the objective of good and comprehensive automated test suite. The time you start thinking of creating something where manual testers with no coding knowledge should be able to write/manage functional tests, you start over-engineering, and eventually complicating things.

The most common mistake is trying to build an intelligent keyword driven framework. A way to write scenarios using pre-decided keywords and then write an intelligent program that parses the keywords to run a test. Sounds simple but this is worst of the lot. You end up compromising on:

- Readability of the tests
- Anything that a well design test frameworks (Junit/TestNG/Cucumber) offers. e.g. Clear Assertions, reports, test grouping and many more.
- Easy integration to your CI process.
- Plus i have never seen a successful implementation in my experience. It is always a nightmare for maintaining such an over-intelligent framework.

**Creating new automated test scenario for every new story.**

Another common mistake that teams do. A successful test automation solution is all about how easily can you maintain your test suite. I have seen several teams blindly adding new test cases for every new story played in the sprint/iteration. More often than not, people do it for some sort of stupid traceability that a manager wants. And in the process you end up duplicating tests and eventually your suite is so bloated that it is big pain to run/maintain all the tests.

Stories are more of a way to manage work efficiently in your sprints. The focus of a story is very specific, to get a piece of functionality done. And a story becomes irrelevant once the sprint is over, so why keep the traceability. 

A tester is a proxy customer, he always sees the system the way a user sees it which is more functional. And our automated tests should also be a document that detail the functionality and hence we should think of test in terms of functional coverage and not story coverage. What that means is trace your functional coverage and not story coverage. That way every time a new story comes up for test automation you first see if the same fits into an existing functional tests and if so append the same for said acceptance criteria. Otherwise, you add a new functional test.

**Estimating test automation work separately**

Do you always have a test automation backlog in the team despite your best efforts? If so, i am sure you would relate to this.

A lot of teams i have worked with tend to estimate the story and the work needed to automate functional tests around the same separately. More often than not these are teams where developers do not own/write/edit functional tests at all and they only care for development complete in their estimate. And then the testers separately estimate the test automation effort for the same story. Now this is an anti-pattern i have already talked about. 

But if your team is different, i.e. The whole team (developers+testers+business) owns functional tests, you are lucky. Now assuming this teams is following such an anti-pattern, then this is what i have to say

- Development complete is an illusion, delivery completeness is the reality. A story is not complete until it can be delivered to business i.e. Ready to be deployed and that needs functional tests.
- The team should estimate automation effort as part of the story, that forces everyone in the team to understand and care about functional tests.
- Once you start doing this actively, your backlog is same as a story backlog and would be dealt with the same priority as any other story in your sprint.

**Now if you have automated tests in place, a few common mistakes i have seen.**

- **Unconditional waits.** I would start with saying i hate Thread.sleep in functional tests. I might be harsh to some of you but if you are using it in your functional tests, this is the worst of all. You should never expect your solution to be consistent and i am sure you are already seeing a lot of false “intermittent” failures so to say.Though this is the most easy fix when you find the common element not found in your tests, you should religiously stop doing it. Whatever test frameworks and the browser driver you are using in your solution, does provide you a way to put conditional waits in the test. Selenium does it and so does all the other tools, they are there for a reason. Please use 
- **Tests not integrated to CI.** So you have functional tests but you don’t bother to run them on every check-in. If so, you are not using your tests effectively. The whole idea to have functional tests is to enable fast feedback. Ensure that your tests, at least a subset (smoke tests) run on every check-in. 
- **No parallel test-runs.** I recently met a team who have 4000 plus tests and it takes them almost 2 days to completely run the whole suite. If i were part of the team, i would just leave the hope. To me even 2 hours sounds a lot to get the feedback. There are so many solutions as part of all the modern test frameworks to parallelize test runs, it should be a given for any team using an automated test bed.
- **Re-running failed tests.** Another common mistake i see people doing is to re-run the failed test. I agree that sometimes there are valild reasons and almost all of them are around test environments. If that is the case, i would probably not question that. But i have seen a lot of team re-running failed tests where they think that the script passes intermittently without the environment being in question. 90% of these cases are to do with your shoddy thread.sleep. Just stop doing it and fix your tests. Re-running can hide issues in your test automation solution but not for long. It might sound funny but i have actually seen some teams rerunning the failed tests a second and third time. This is plain and simple wrong.



[
  ](https://t.umblr.com/redirect?z=http%3A%2F%2Fwww.testvagrant.com%2F&t=YTg2YWE0ZjcwMWMxYzYxOTAzNjBkY2Q5MTdjNzFlMjJlN2Y3OTQyMSxoWEs1bUxVbA%3D%3D&b=t%3Adjh42wJ-H9ACAXiZwogd8g&p=https%3A%2F%2Ftestvagrant.tumblr.com%2Fpost%2F95444422751%2Ftest-automation-anti-patterns&m=1&ts=1605919913)
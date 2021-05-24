# Bug Triage

[
  ](https://developer.mozilla.org/en-US/docs/Mozilla)

Bug triage is the process of going through a list of bugs to find bugs that need assistance, escalation, or follow-up. There are many different types of bugs and each type requires a slightly different process. The following tries to explain the different types of bugs and how to triage them.

#### 1 Regression Window Wanted Bugs

Regressions are bugs caused by some other change landed in the Firefox code repository. When these bugs are reported it is fairly common for developers to ask QA to request a regression window. We track these bugs with a *regressionwindow-wanted* keyword added to the bug report. When triaging regression window wanted bugs we look at the bugs from a couple of different angles:

1. **Is the bug still valid?** In some cases these bugs were reported some time ago and may no longer be an issue, in which case the regressionwindow-wanted keyword should be removed and the bug report should be closed. Be sure to confirm with the person who requested the regression window that the request is still valid before closing the bug. In other cases it's possible that the information in the bug has gone stale. In this case QA would ask the bug reporter to confirm the issue still reproduces in a recent Firefox build and to provide more detailed information on how to reproduce it. Examples of useful information may be steps to reproduce, a minimized testcase, or particulars about the system the reporter is using. Once QA has accurate information to enable us to reproduce the bug we move on to #2.
2. **Is the bug reproducible?** Assuming the bug is reproducible we will try to find a regression window; quite often a short list of changes that may have introduced the bug. A bug becomes actionable by developers once the regression window has been provided. If the regression window can be narrowed down to a single change, mark the bug as blocking the offending bug and use the *needinfo?* flag to bring it to the attention of the developer responsible for that change. Be sure to remove the regressionwindow-wanted keyword once the request has been satisfied.

For more information on regression windows and how to find them, please [read this howto](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/Mozmill/How_to_do_regression_testing).

#### 2 QA Wanted Bugs

QA Wanted bugs are bugs which have been flagged as needing QA assistance. Typically these bugs are missing some crucial information to make them actionable by developers. Sometimes these will be regression window wanted bugs (see above). However sometimes these will be bugs that need steps to reproduce, minimized testcases, testing to verify a fix, or exploratory testing to discover other variables at play (eg. hardware, software, configuration, etc). These bugs are signified by having a *qawanted* keyword in the bug report.

As with regression window wanted bugs, we need to keep in mind when triaging that some bugs may be old requests that are no longer valid. In these cases we need to flag the person who made the request originally to see if help is still wanted. Assuming the request is still valid, triagers should read the entire bug to make sure they completely understand the nature of the bug and the nature of the request. If more information is needed, the reporter or qawanted requestor should be flagged to provide the information required.

The keyword should be removed from the bug once the request has been satisfied.

#### 3 Unconfirmed Bugs

Unconfirmed bugs are bugs which have been reported but have not been proven to be legitimate. In some cases developers will have already triaged these bugs -- usually these bugs will be set in a state of NEW or ASSIGNED. However this does not happen all the time. Our primary goal with triaging unconfirmed bugs is to make sure there are zero valid bugs which are unactionable. We also need to be on the lookout for severe bugs and regressions.

The first step to triaging unconfirmed bugs is to read the bug to see if it has already been triaged. If the bug has been triaged but has not been updated in a while, comment in the bug to see if there's something needed to move the bug along. If the bug was triaged recently you can ignore it.

The second step to triaging unconfirmed bugs is to see if the bug is reproducible. If the information provided does not enable you to reproduce the bug then prompt the bug reporter for more information. Once the bug is confirmed we should set the bug status to NEW and check to make sure it's in the appropriate bugzilla component. For example, if the bug seems to be talking about a DOM event the bug should be moved to the *DOM: Events* component, assuming it isn't already.

Assuming you reproduced the bug, your next step would be to check if it's a [regression](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/Mozmill/How_to_do_regression_testing). If it is be sure it is flagged with a *regression* keyword. The development team will then have a look at it and follow up with you if more information is required.

#### 4 Tracking Bugs

Tracking bugs are those which have been deemed to have significant user impact and worthy of being on Release Management's radar. These bugs are signified by having one of the tracking flags set. For example, a bug with *tracking-firefox36:+* is being tracked for the Firefox 36 release, whereas a bug with *tracking-firefox36:-* is not tracked. It is QA's job to make sure these bugs are actionable for developers and verify the fixes when these bugs are resolved. We also act as an extra set of eyes for Release Management to make sure these bugs are staying on track as we get closer to release. If you come across a bug that you think needs to be tracked, use the ? flag value -- you will be prompted in the bug comment to provide a justification for why it should be tracked.

You'll want to tackle these bugs in triage from a few of different angles:

1. If the bug is tracked but has not been updated in a long time, try to re-escalate the bug to either Release Management or the developer. It's possible the bug does not need to be tracked anymore but that should ultimately be Release Management's decision. Our job in this case is to just make sure the bug is given attention.
2. If the bug is tracked but needs more information our job is to try to provide that information. In most cases these bugs will already be flagged with the qawanted keyword and should show up in the QA Wanted bugs as described above. If not, treat them as if they were.
3. If the bug is tracked and resolved we need to decide if the bug needs verification. If it does the *qe-verify+* flag should be set and the bug should be assigned to a QA person to testing. If it does not, the *qe-verify-* flag can be set. If you are unsure you can set the *qe-verify?* flag and someone in QA will make the decision (more on this later).

You can skip over the bug if it is tracked and appears to be in progress. However, you may want to raise concern in the bug as the release draws close.

#### 5 In-Testsuite Bugs

Many times when a bug is fixed it will be landed with automated tests. When this happens the code committer will usually set the *in-testsuite+* flag. However, in some cases this might be overlooked. In other cases you may see the in-testsuite- flag which signifies that the bug is not covered by in-testsuite tests. As such, you'll have to triage these bugs from a couple of different angles:

1. If the bug has *in-testsuite+* you can probably skip over it. For full due diligence you may want to ask the developer if further test coverage is needed or if there are areas of risk that should be checked manually.
2. If the bug has *in-testsuite-* you will want to ask for clarification. In some cases this may be set because automated coverage is not possible, in which case you may want to flag this for QA verification (qe-verify?) or flag it for consideration for one of QA's automation test suites (in-qa-automation?). In either case you should double check with the developer to see what assistance you can provide, if any.
3. If the bug has *in-testsuite?* it has been nominated to have test coverage added. If this is the case you may want to ask the developer if there's something you can do to help.

As mentioned above, sometimes the bug will be fixed and no in-testsuite flag will be set. In these circumstances you'll want to check if the landed patches included tests. If so it is probably safe to set the *in-testsuite+* flag. However you may want to double check with the code author. You can set the *in-testsuite?* flag if you are uncertain.

#### 6 Intermittent-Failure Bugs

Every time a new Firefox build is generated there is a whole suite of tests which run against it. Sometimes these tests pass, sometimes they fail, and sometimes they fail intermittently. When tests fail intermittently they get a bug filed automatically which is marked with the *intermittent-failure* keyword. For the most part QA does not need to triage these bugs. However it's probably a good idea to keep an eye on these once in a while. Sometimes these intermittent failures can be indicative of a regression which may need QA's help in investigating. That said, these cases will likely be called to QA's attention by other means (eg. the regressionwindow-wanted keyword).

Here are some things to keep in mind when investigating an intermittent failure bug:

- How reproducible is the failure? Can you reproduce the failure in 20 test runs locally? Can you reproduce on the [try server](https://wiki.mozilla.org/ReleaseEngineering/TryServer)? If you have a loaner machine, does the bug reproduce?
- Can you set up a remotely accessible Linux machine? Can you capture an [rr trace](http://rr-project.org/) of the failure? This might help determine when the issue started.
- When did the failure first occur? Has this test failed since it initially landed? Has this test only recently begun to fail after an extended period of success?
- Use [Treeherder](https://treeherder.mozilla.org/) to retrigger testruns to help narrow down a failure occurrence.
- Locally reproducing the failure will make it easier to investigate and bisect

If all else fails, don't be afraid to ask for help on the bug or in the [DOM chat room](https://chat.mozilla.org/#/room/#dom:mozilla.org) on [Matrix](https://wiki.mozilla.org/Matrix). Developers are usually pretty keen on helping especially if it means resolving a bug.

#### 7 Crashes

Crashes are taken very seriously and must be dealt with as quickly as possible. The more frequent and destructive the crash, the more likely it is to result in a user having a bad experience forcing them to stop using Firefox. As such, we take these very seriously and act as quickly as possible. A crash bug is signified by having the *crash* keyword in the bug report. Here are a few things to know about triaging crashes:

1. If a crash bug has not been updated in a while it may no longer be an issue. You'll see one or more crash signatures indicated in the bug report. Clicking these signatures will allow you to see how many reports have been generated recently. If there have been very few reports recently then the bug can probably be resolved as WORKSFORME.
2. If a crash bug is not currently being tracked for a current Firefox version, check the signature to see how many reports exist. If there is a considerable volume of crashes it may be worth tracking. If a crash is really high volume (ie. in the [top 10](https://crash-stats.mozilla.com/home/products/Firefox)) it should definitely be tracked. If the crash is a *topcrash*, make sure it has the topcrash keyword in the bug report.
3. Check to see if the bug needs information to become actionable. If it does, make an attempt to reproduce it. If you can't reproduce it see if you can get more information from the crash reports. Each report will usually indicate some information (platform, addons, URLs, graphics card, driver versions, changed preferences, comments, etc). Use this information to see if you can reproduce it. If you can't reproduce it then ask for help from a developer -- sometimes they can suggest things to test based on the crash stack.

In some cases, these crashes might be regressions in which case you should take the same approach as the regression window wanted bugs mentioned earlier.

#### 8 Top Crashes

These are crashes which occur most frequently across the user population. As such, topcrashes are typically given more attention than regular crashes. Unlike crashes where we can't possibly report a bug for each one, we try to report as many of the top crashes as possible. Typically we try to have a bug report on file and tracked for the top 10 to 20 crashes (eg. [DOM topcrashes](http://bit.ly/1EeqRhC)). We have a couple of different objectives in triaging topcrashes:

1. Make sure the top crashes have an associated bug report. We typically use a 1% threshold for DOM (ie. any crash that has a volume share of 1% or more should be reported).
2. For any topcrash that has an associated bug report, check the report to make sure it's still active. If the bug has not been updated for a while then see if there's something you can do to help. If the bug is active then it's probably okay to skip it.
3. For any topcrash that has an associated bug report that is resolved, check the recent statistics for the signature(s) in the bug report. If there are recent reports with recent Firefox versions then it may be worth reopening or filing a follow-up bug. Be sure you flag the developer in the bug to draw attention to the crash reports and ask for advice on next steps.
4. When a topcrash reduces in volume below the threshold but is still impacting users we drop the *topcrash* keyword but the *crash* keyword should always remain on the bug.

In some cases, these crashes might be regressions in which case you should take the same approach as the regression window wanted bugs mentioned earlier.

#### 9 QE-Verify Bugs

When someone wants to call attention to a bug that might need further QA testing they will typically set the *qe-verify?* flag. These bugs should be triaged periodically to make sure they are getting attention. We tend to take a couple of different approaches when triaging these bugs.

1. If we agree that the bug needs testing and the risks are well documented, we will set the *qe-verify+* flag and the bug gets assigned to someone in QA for testing. However, if the risks are not well documented the triager should follow up with the person requesting QA verification to get more details.
2. If we do not think the bug needs testing we may ask for clarification from the person who requested it. However, in most cases we will just set the *qe-verify-* flag. If you do set this flag be sure to provide a detailed justification for that decision as the developer or Release Management may rebut that decision.

Once a bug has been assigned to a QA person it will be prioritized for testing. Typically this will be done by someone on the Release QA team though not always. Any bugs that are found during testing should be reported separately and linked to the original bug. The Release Management team will then decide whether those issues warrant backing out the changes, in which case the originating bug will be reopened. If no bugs are found during testing the bug can be marked verified fixed.

Note: if the bug is tracked for multiple Firefox versions it needs to be tested and verified on all versions indicated.

#### 10 In-QA-Testsuite Bugs

Similar to in-testsuite, in-qa-testsuite bugs are bugs which need test automation. For the most part, QA will only triage the bugs flagged as *in-qa-testsuite?* as *in-qa-testsuite+* usually inidicates that automation is sufficient and a *in-qa-testsuite-* typically indicates automation is not possible. When triaging these bugs you should be looking for something that you can assign to yourself to write automated tests. Think of these as a QA automation backlog.
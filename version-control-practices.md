# Version control Best Practices - Gitlab

1. Determine a branching strategy
2. Make frequent small changes
3. Write descriptive commit messages
4. Develop using branches
5. Conduct regular code reviews

Version control and collaboration goes beyond simply tracking changes and includes practices such as:

- Enabling development teams to work in distributed and asynchronous environments
- Managing changes and versions of code and artifacts
- Enabling code reviews and other assets
- Tracking approvals of proposed changes
- Resolving merge conflicts and related integration issues

## Determine a branching strategy

Determining a branching workflow depends on several factors, including team size, level of experience, scaling requirements, and industry limitations

Common workflows include:

### Centralized workflow: 

A centralized workflow includes a single repository and a master branch. Teams don’t use any other branches for development, so there is a high risk of overwriting changes. Although there is no collaboration cadence in this type of workflow, it can work well on small teams (fewer than 5 developers) that use good communication to ensure two developers never try to work on the same code simultaneously. 

### Feature branching 

Feature branching means creating a new branch for each feature that needs to be added. Everyone who needs to work on the feature commits to the `feature` branch. Feature branching and GitFlow connects various teams, because it requires a bigger set of code reviews, push rules, code approvers, and a wider set of tests.

### GitFlow

GitFlow is an extreme version of feature branching. Developing with GitFlow incorporates a `master`branch and a separate `develop` branch, as well as supporting branches for features, releases, and hotfixes. Development occurs on the `develop` branch, moves to a `release`branch, and merges into the `master` branch.

### Task-branch development 

GitLab Flow is an example of this type of development and combines feature-driven development and `feature` branches with issue tracking. GitLab Flow provisions multiple environments like staging, pre-production, and production by using separate dedicated branches so that commits flow downstream to ensure that everything has been tested on all environments.

Task-branch development sets a pretty fast pace, forcing team members to decompose requirements into small chunks of value that will be delivered via task branches. This type of workflow embeds collaboration practices such as code snippets, code reviews, and unit testing. Furthermore, if a test breaks, team members can work together to see what’s wrong.

## Make frequent, small changes

The most effective way to develop software with the intention of delivering business value and meeting customer demand is to commit each time you have a working set of tests and code.

Teams should frequently push to the feature branch, even when it’s not yet ready for review, because sharing work in a feature branch or a merge request can prevent teammates from duplicating work. Sharing work before it’s complete also facilitates discussion and feedback, which can help improve the code before it gets to review. Breaking work into individual commits offers context for developers and other teams (e.g. Quality and Security) that will review code at a later time. Smaller commits clearly identify how a feature was developed, making it easy to roll back to a specific point in time or to revert one code change without reverting several unrelated changes. Look for ways to simplify projects into small steps and then make frequent commits to complete a larger goal. 

## Write descriptive commit messages

A commit message should reflect intention, not just the contents of the commit. It’s easy to see the changes in a commit, so the commit message  should  explain  why  those  changes  were made. 

An example of a good commit message is: 

 *“Combine templates to reduce duplicate code in the user views.”* The words “change,” “improve,” “fix,” and “refactor” don’t add much information to a commit message.

 For example, *“Improve XML generation”* could be better written as *“Properly escape special characters in XML generation.”* 

Descriptive   commit   messages   cultivate   transparency and offer insight into progress so that team members, customers, and future contributors can understand the development process. When conducting code reviews, commit messages help team members follow iterations and determine what changes have been made since a release, discussion, or change in requirements. Detailed commit messages also assist Quality and Security teams when examining code to identify areas of concern and revert a specific change. Furthermore, when developers write detailed commit messages, it can prevent teammates from duplicating work, limiting delays and helping a project progress more steadily. It’s important to establish a commit message convention to ensure consistency across teams and decrease confusion and miscommunication.

## Develop using branches

Teams can be creative without the fear of making the master branch unstable. Furthermore, team members can collaborate to ensure a solution works well before merging it to the master branch. Operations, quality, and security teams can review code before it gets deployed, ensuring that everyone has visibility and the opportunity to discuss ideas and propose any potential problems before shipping.

## Conduct regular code reviews

When conducting a code review, team members should:

- Explain which changes are necessary (e.g. fixes a bug, improves the user experience, refactors existing code).
- Prefix comments with “Not blocking:” if there are small, non-mandatory improvements, helping the author understand that the suggestion is optional and can be resolved immediately or in another iteration. 
- Offer alternative implementations, but assume the author already considered them (e.g. “What do you think about using a custom validator here?”).
- Try to be thorough to reduce the number of iterations.
- Identify ways to simplify the code while still solving the problem.

By collaborating in code reviews, teammates learn different coding practices, workflow techniques, and new ways of approaching problems, which increases innovation and efficiency and decreases knowledge silos.






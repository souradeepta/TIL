## Guide: Writing Testable Code

To keep our code at Google in the best possible shape we provided our software engineers with these constant reminders. Now, we are happy to share them with the world.

Also thanks to Blaine R Southam who has turned it into a [pdf book](http://misko.hevery.com/attachments/Guide-Writing Testable Code.pdf).

[Flaw #1: Constructor does Real Work](http://misko.hevery.com/code-reviewers-guide/flaw-constructor-does-real-work/)

**Warning Signs**

- new keyword in a constructor or at field declaration
- Static method calls in a constructor or at field declaration
- Anything more than field assignment in constructors
- Object not fully initialized after the constructor finishes (watch out for initialize methods)
- Control flow (conditional or looping logic) in a constructor
- Code does complex object graph construction inside a constructor rather than using a factory or builder
- Adding or using an initialization block

[Flaw #2: Digging into Collaborators](http://misko.hevery.com/code-reviewers-guide/flaw-digging-into-collaborators/)

**Warning Signs**

- Objects are passed in but never used directly (only used to get access to other objects)
- Law of Demeter violation: method call chain walks an object graph with more than one dot (.)
- Suspicious names: context, environment, principal, container, or manager

[Flaw #3: Brittle Global State & Singletons](http://misko.hevery.com/code-reviewers-guide/flaw-brittle-global-state-singletons/)

**Warning Signs**

- Adding or using singletons
- Adding or using static fields or static methods
- Adding or using static initialization blocks
- Adding or using registries
- Adding or using service locators

[Flaw #4: Class Does Too Much](http://misko.hevery.com/code-reviewers-guide/flaw-class-does-too-much/)

**Warning Signs**

- Summing up what the class does includes the word “and”
- Class would be challenging for new team members to read and quickly “get it”
- Class has fields that are only used in some methods
- Class has static methods that only operate on parameters
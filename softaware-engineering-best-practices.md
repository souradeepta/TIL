### How are we doing in the software field?

- Good news - world economy is becoming more and more software dependent - more jobs for software professionals. Some of the best paying jobs are CS jobs. And the job prospects for CS majors are promising.
- Yes there are many successes but there are too many failures that you never hear of.

### What are the symptoms of software development problems?

- Poor software quality
- Unacceptable software performance
- Software that is hard to maintain or extend
- Inaccurate understanding of end user needs
- Inability to deal with changing requirements
- Late discovery of serious project flaws

### What has been the traditional approach to software development?

The traditional approach is sometimes called the "waterfall development".

- Requirements Analysis
- Design
- Code
- Subsystem Testing
- System Testing

The entire development process goes through these phases linearly and in lock step. First all the requirements are defined, then the design is completed, and finally the code is written and tested. The key assumptions are that when design begins, requirements no longer change. When coding starts, the design ceases to change. These assumptions do not take into account a changing universe.

### What are the root causes of poor software development?

- Ambiguous communication
- Overwhelming complexity
- Insufficient testing
- Insufficient requirements management
- Undetected inconsistencies among requirements, designs and implementations
- Brittle architecture

### What are the best practices in software engineering?

- Develop iteratively
- Manage requirements
- Use component architecture
- Model software visually
- Verify quality
- Control change

Best practices are a set of empirically proven approaches to software development. When used in combination they strike at the root causes of software development problems. They are called "best practices" not because we can precisely quantify their value but rather they are observed to be commonly used in industry by successful organizations.

### Best Practices address the root causes of poor software development

#### 1. Develop Iteratively

- Critical risks are resolved before making large investments
- Initial iterations enable early user feedback
- Testing and integration are continuous
- Objective milestones provide short term focus

#### 2. Manage Requirements

- Requirements are dynamic - expect them to change during software development
- User's own understanding of the requirements evolves over time
- Gain agreement with user on what the system should do and not how
- Maintain forward and backward traceability of requirements

#### 3. Use Component Based Architecture

- Using components permits reuse
- Choice of thousands of commercially available components
- Improved maintainability and extensibility
- Promotes clean division of work among teams of developers

#### 4. Visually Model Software

- Visual modeling improves our ability to manage software complexity
- Capture the structure and behavior of components
- Hide or expose details as appropriate for the task
- Promote unambiguous communication

#### 5. Verify Software Quality

- What is quality? - The characteristic of producing a product which meets or exceeds agreed upon requirements by some agreed upon objective measures.
- Software problems are 100 to 1000 times more costly to find and repair after deployment
- Develop test suites for each iteration and test for -
  - Functionality
  - Reliability
  - Performance

#### 6. Control Changes to Software

- Without explicit control parallel development degrades to chaos
- Decompose the architecture into subsystems and assign responsibility of each subsystem to a team. Establish secure workspaces for each team i.e. each team is isolated from changes made in other workspaces.
- Establish an enforceable change control mechanism where
  - Change requests are prioritized
  - Impact of the change request is assessed
  - Plan put in place to introduce change in a particular iteration

#### Best Practices Reinforce Each Other

Each of the best practices reinforces and enables the others. Although it is possible to use one best practice without the others, it is not recommended. The whole is much greater than the sum of the parts.

#### What is Software Architecture?

- Selection of structural elements and their interfaces
- Looks at the "fit" of the system in the the operational and development context
- Organization of components based on their interfaces and behavior as specified in collaborations with other structural elements
- Software Architecture is concerned with
  - Usage
  - Functionality
  - Performance
  - Resilience
  - Reuse
  - Maintainability
  - Aesthetics

#### Why do we model?

- A model is a simplification of reality. We model because we cannot comprehend the complexity of a system in its entirety.
- We model to visualize, specify, construct, and document the structure and behavior of a system's architecture.
- A model is a complete description of a system from a particular perspective.

#### Principles of Modeling

- The choice of what models to create has a profound influence on how a problem is attacked and how a solution is shaped.
- Every model maybe expressed at different levels of precision.
- The best models are connected to reality.
- No single model is sufficient. Every non-trivial system is best approached through a small set of nearly independent models.
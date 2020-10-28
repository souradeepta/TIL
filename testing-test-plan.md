# Test Plan Template: Sample Document with Web Application Example

## What is test plan template?

**TEST PLAN TEMPLATE** is a detailed document that describes the test strategy, objectives, schedule, estimation and deliverables, and resources required for testing. Test Plan helps us determine the effort needed to validate the quality of the application under test. The test plan serves as a blueprint to conduct software testing activities as a defined process which is minutely monitored and controlled by the test manager.

Creating a[ Test Plan ](https://www.guru99.com/what-everybody-ought-to-know-about-test-planing.html)is mandatory to ensure success of your Software testing project.If you are new to Test Planning refer this tutorial on [How to Create a Test Plan](https://www.guru99.com/what-everybody-ought-to-know-about-test-planing.html)

Below find important constituents of a test plan-

- [1 Introduction](https://www.guru99.com/test-plan-for-project.html#1)
- [1.1 Scope](https://www.guru99.com/test-plan-for-project.html#11)
- [1.1.1 In Scope](https://www.guru99.com/test-plan-for-project.html#111)
- [1.1.2 Out of Scope](https://www.guru99.com/test-plan-for-project.html#112)
- [1.2 Quality Objective](https://www.guru99.com/test-plan-for-project.html#12)
- [1.3 Roles and Responsibilities](https://www.guru99.com/test-plan-for-project.html#13)
- [2 Test Methodology](https://www.guru99.com/test-plan-for-project.html#2)
- [2.1 Overview](https://www.guru99.com/test-plan-for-project.html#21)
- [2.2 Test Levels](https://www.guru99.com/test-plan-for-project.html#22)
- [2.3 Bug Triage](https://www.guru99.com/test-plan-for-project.html#23)
- [2.4 Suspension Criteria and Resumption Requirements](https://www.guru99.com/test-plan-for-project.html#24)
- [2.5 Test Completeness](https://www.guru99.com/test-plan-for-project.html#25)
- [3 Test Deliverables](https://www.guru99.com/test-plan-for-project.html#3)
- [4 Resource & Environment Needs](https://www.guru99.com/test-plan-for-project.html#4)
- [4.1 Testing Tools](https://www.guru99.com/test-plan-for-project.html#41)
- [4.2 Test Environment](https://www.guru99.com/test-plan-for-project.html#42)

## 1) Introduction

Brief introduction of the test strategies, process, workflow and methodologies used for the project

## 1.1) Scope

------

### 1.1.1) In Scope

Scope defines the features, functional or non-functional requirements of the software that **will be** tested

### 1.1.2) Out of Scope

Out Of Scope defines the features, functional or non-functional requirements of the software that **will NOT be** tested

## 1.2) Quality Objective

------

Here make a mention of the overall objective that you plan to achive with your manual testing and automation testing.

Some objectives of your testing project could be

- Ensure the Application Under Test conforms to functional and non-functional requirements
- Ensure the AUT meets the quality specifications defined by the client
- Bugs/issues are identified and fixed before go live

## 1.3) Roles and Responsibilities

------

Detail description of the Roles and responsibilities of different team members like

- QA Analyst
- Test Manager
- Configuration Manager
- Developers
- Installation Team

Amongst others

## 2) Test Methodology

### 2.1) Overview

------

Mention the reason of adopting a particular test methodology for the project. The test methodology selected for the project could be

- WaterFall
- Iterative
- Agile
- Extreme Programming

The methodology selected depends on multiple factors. You can read about Test Methodology [here](https://www.guru99.com/testing-methodology.html)

### 2.2) Test Levels

------

**Test Levels define the Types of Testing to be executed on the Application Under Test (AUT**). The Testing Levels primarily depends on the scope of the project, time and budget constraints.

### 2.3) Bug Triage

------

The goal of the triage is to



- To define the type of resolution for each bug
- To prioritize bugs and determine a schedule for all "To Be Fixed Bugs'.

### 2.4) Suspension Criteria and Resumption Requirements

------

Suspension criteria define the criteria to be used to suspend all or part of the testing procedure while Resumption criteria determine when testing can resume after it has been suspended

### 2.5) Test Completeness

------

Here you define the criterias that will deem your testing complete.

For instance, a few criteria to check Test Completeness would be

- 100% test coverage
- All Manual & Automated Test cases executed
- All open bugs are fixed or will be fixed in next release

## 3) Test Deliverables

Here mention all the Test Artifacts that will be delivered during different phases of the testing lifecycle.

Here are the simple deliverables

Test PlanTest CasesRequirement Traceability MatrixBug ReportsTest StrategyTest MetricsCustomer Sign Off

## 4) Resource & Environment Needs

### 4.1) Testing Tools

------

Make a list of Tools like

- Requirements Tracking Tool
- Bug Tracking Tool
- Automation Tools

Required to test the project











### 4.2) Test Environment

------

It mentions the minimum **hardware** requirements that will be used to test the Application.

Following **software's** are required in addition to client-specific software.

- Windows 8 and above
- Office 2013 and above
- MS Exchange, etc.

## 5) Terms/Acronyms

Make a mention of any terms or acronyms used in the project

| **TERM/ACRONYM** | **DEFINITION**                |
| ---------------- | ----------------------------- |
| API              | Application Program Interface |
| AUT              | Application Under Test        |

[Download the above Test Plan Template Format](https://drive.google.com/uc?export=download&id=0ByI5-ZLwpo25LThJNUZzUzdkQXM)

## Sample Test Plan Document Banking Web Application Example

### 1 Introduction

The Test Plan is designed to prescribe the scope, approach, resources, and schedule of all testing activities of the project Guru99 Bank.

The plan identify the items to be tested, the features to be tested, the types of testing to be performed, the personnel responsible for testing, the resources and schedule required to complete testing, and the risks associated with the plan.

#### 1.1 Scope

#### 1.1.1 In Scope

All the feature of websiteGuru99 Bank which were defined in software requirement [specs](https://docs.google.com/document/d/1rPW5DV82VJT6vtA1VDSrfxaCBuAduxW0zb1yfTh_VMk/edit?pli=1) are need to bested

| Module Name          | Applicable Roles | Description                                                  |
| -------------------- | ---------------- | ------------------------------------------------------------ |
| Balance Enquiry      | Manager Customer | **Customer**: A customer can have multiple bank accounts. He can view balance of his accounts only **Manager**: A manager can view balance of all the customers who come under his supervision |
| Fund Transfer        | Manager Customer | **Customer:** A customer can have transfer funds from his “own” account to any destination account. **Manager**: A manager can transfer funds from any source bank account to destination account |
| Mini Statement       | Manager Customer | A Mini statement will show last 5 transactions of an account **Customer:** A customer can see mini-statement of only his “own” accounts **Manager:** A manager can see mini-statement of any account |
| Customized Statement | Manager Customer | A customized statement allows you to filter and display transactions in an account based on date, transaction value **Customer:** A customer can see Customized- statement of only his “own” accounts **Manager**: A manager can see Customized -statement of any account |
| Change Password      | Manager Customer | **Customer:** A customer can change password of only his account. **Manager**: A manager can change password of only his account. He cannot change passwords of his customers |
| New Customer         | Manager          | **Manager**: A manager can add a new customer.               |
|                      | Manager          | **Manager:** A manager can edit details like address, email, telephone of a customer. |
| New Account          | Manager          | Currently system provides 2 types of accounts • Saving • Current A customer can have multiple saving accounts (one in his name, other in a joint name etc). He can have multiple current accounts for different companies he owns. Or he can have a multiple current and saving accounts. **Manager:** A manager can add a new account for an existing customer. |
| Edit Account         | Manager          | Manager: A manager can add a edit account details for an existing account |
| Delete Account       | Manager          | Manager: A manager can add a delete an account for a customer. |
| Delete Customer      | Manager          | A customer can be deleted only if he/she has no active current or saving accounts **Manager:** A manager can delete a customer. |
| Deposit              | Manager          | **Manager:** A manager can deposit money into any account. Usually done when cash is deposited at a bank branch. |
| Withdrawal           | Manager          | **Manager:** A manager can withdraw money from any account. Usually done when cash is withdrawn at a bank branch. |

#### 1.1.2 Out of Scope

These feature are not be tested because they are not included in the software requirement specs

- User Interfaces
- Hardware Interfaces
- Software Interfaces
- Database logical
- Communications Interfaces
- Website Security and Performance

### 1.2 Quality Objective

The test objectives are to **verify** the Functionality of website Guru99 Bank, the project should focus on testing the **banking operation** such as Account Management, Withdrawal, and Balance…etc. to **guarantee** all these operation can work **normally** in real business environment.

### 1.3 Roles and Responsibilities

The project should use **outsource** members as the tester to save the project cost.

| **No.** | **Member**         | **Tasks**                                                    |
| ------- | ------------------ | ------------------------------------------------------------ |
| **1.**  | Test Manager       | Manage the whole project Define project directions Acquire appropriate resources |
| **2.**  | Test               | Identifying and describing appropriate test techniques/tools/automation architecture Verify and assess the Test Approach Execute the tests, Log results, Report the defects. Outsourced members |
| **3.**  | Developer in Test  | Implement the test cases, test program, test suite etc.      |
| **4.**  | Test Administrator | Builds up and ensures test environment and assets are managed and maintained Support Tester to use the test environment for test execution |
| **5.**  | SQA members        | Take in charge of quality assurance Check to confirm whether the testing process is meeting specified requirements |

### 2 Test Methodology

### 2.1 Overview

### 2.2 Test Levels

In the project Guru99 Bank, there're 3 types of testing should be conducted.

- **Integration** Testing (Individual software modules are combined and tested as a group)
- **System** Testing: Conducted on a **complete**, **integrated** system to evaluate the system's compliance with its specified requirements
- **API testing:** Test all the APIs create for the software under tested

### 2.3 Bug Triage

### 2.4 Suspension Criteria and Resumption Requirements

If the team members report that there are **40%** of test cases **failed**, suspend testing until the development team fixes all the failed cases.

### 2.5 Test Completeness

- Specifies the criteria that denote a **successful** completion of a test phase
- **Run** rate is mandatory to be **100%** unless a clear reason is given.
- **Pass** rate is **80%,** achieving the pass rate is **mandatory**

### 2.6 Project task and estimation and schedule

| **Task**                          | **Members**                | **Estimate effort** |
| --------------------------------- | -------------------------- | ------------------- |
| **Create the test specification** | Test Designer              | 170 man-hour        |
| **Perform Test Execution**        | Tester, Test Administrator | 80 man-hour         |
| **Test Report**                   | Tester                     | 10 man-hour         |
| **Test Delivery**                 |                            | 20 man-hour         |
| **Total**                         |                            | **280 man-hour**    |

**Schedule to complete these tasks**

### 3 Test Deliverables

Test deliverables are provided as below

**Before testing phase**

- Test plans document.
- Test cases documents
- Test Design specifications.

**During the testing**

\- Test Tool Simulators.

\- Test Data

\- Test Trace-ability Matrix - Error logs and execution logs.

**After the testing cycles is over**

- Test Results/reports
- Defect Report
- Installation/ Test procedures guidelines
- Release notes

### 4 Resource & Environment Needs

### 4.1 Testing Tools

| **No.** | **Resources** | **Descriptions**                                             |
| ------- | ------------- | ------------------------------------------------------------ |
| **1.**  | Server        | Need a Database server which install MySQL server Web server which install Apache Server |
| **2.**  | Test tool     | Develop a Test tool which can auto generate the test result to the predefined form and automated test execution |
| **3.**  | Network       | Setup a LAN Gigabit and 1 internet line with the speed at least 5 Mb/s |
| **4.**  | Computer      | At least 4 computer run Windows 7, Ram 2GB, CPU 3.4GHZ       |

### 4.2 Test Environment

Test Environment to be setup as per figure below
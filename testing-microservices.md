## [Testing in Microservice Architecture](https://testvagrant.tumblr.com/post/116287001816/testing-in-microservice-architecture)

***Why are we talking about \**Microservices\**?***

A significant amount of traction has been built in past couple of years around “**microservice architecture**”. Its been talked about in almost all major developer conferences and seminars. It has acquired quite a lot of interest in the active development community and slowly people have started writing about it.

Recently I found an interesting technology curve on Twitter by InfoQ. If you notice the first segment which is Innovators, you can spot Microservices there along with Reactive programming and Machine Learning. It is evident from this curve that the microservices are going to play an important role in near future and a majority of the community still needs to explore and adopt.

![image](https://64.media.tumblr.com/6f122cb13d9c2ff5939ed77bf6d4b40d/tumblr_inline_nmi572o6z31sma618_1280.png)

*[Image Courtesy of InfoQ]*

Martin Fowler (ThoughtWorks) has written quite a lot about it [here](https://t.umblr.com/redirect?z=http%3A%2F%2Fmartinfowler.com%2Ftags%2Fmicroservices.html&t=ZjliM2M3OWU5N2Y3OTAwYzBmMmZjMDY1NDg3N2RmN2MyOTkyZjY0NSxETGY5U25wSQ%3D%3D&b=t%3Adjh42wJ-H9ACAXiZwogd8g&p=https%3A%2F%2Ftestvagrant.tumblr.com%2Fpost%2F116287001816%2Ftesting-in-microservice-architecture&m=1&ts=1605912724) . And you can also find couple of articles on [infoQ](https://t.umblr.com/redirect?z=http%3A%2F%2Fwww.infoq.com%2Fpresentations%2FMicro-Services&t=MmQ5ZTBlYTIzMTI3NTUyMTEwZGNiYjJhMGUyOTdmZDBkOGYwNGFjYyxETGY5U25wSQ%3D%3D&b=t%3Adjh42wJ-H9ACAXiZwogd8g&p=https%3A%2F%2Ftestvagrant.tumblr.com%2Fpost%2F116287001816%2Ftesting-in-microservice-architecture&m=1&ts=1605912724) .

Some of the early adopters who have moved from monolithic architecture to microservices architecture are Netflix, Amazon, Paypal, Twitter. As microservice is becoming more popular and is being adopted fast, it becomes essential for test engineers and organisations to align their testing strategy with the architecture of the application. This requires change in perspective from traditional way of testing applications.

So while the developers have slowly started implementing it and pushing guidelines on the way it has to be implemented, there is also some interest picking up in how to go about testing it.

***Some questions you may have ?***

***a) Why is it important for test engineer ?***

Essentially a microservice is a small, independent, deployable piece of software which encapsulates a definite business logic. As it contains business logic, it makes perfect sense to test the logic right here versus testing it as part of the bigger application.

***b) What do developers test ? and What test engineer should focus on?*** 

While developer’s focus is to write tests for each of the layers in microservice architecture and various integration points, test engineer’s focus is to write tests for the business logic which it encapsulates, and its behaviour in various unexpected conditions

**c) Can I exercise a business use-cases (System test) ?**

The answer is a resounding YES. While each microservice encapsulates certain business logic, collaboration between microservices achieves a use case or system test. In some cases an orchestration service would be created which internally interact with one or more services to achieve certain use case.

**d) Wouldn’t the traditional approach of testing work ? if not why ?**

No.

In most of the traditional testing strategies, entire application is considered as a blackbox. Testing and test-automation is done at UI layer predominantly. Also, in most cases test engineer is not bothered about the internals and mechanics of the application under test.

Whereas to test microservice architecture effectively and comprehensively, test engineer needs to understand the purpose of each service, boundaries, interactions with other services and more.



## [Change in perspective](https://testvagrant.tumblr.com/post/116708350781/restful-services-testing-change-in-perspective)

## Introduction

In the [previous blog](https://t.umblr.com/redirect?z=https%3A%2F%2Flnkd.in%2Fe66sd_g&t=MDhiYmM4YjZiMDljZTM0ZWNhMDNjMzQ5OTYxYTAwMjIwYzIxOTQyZSx4NGpaWDhmbA%3D%3D&b=t%3Adjh42wJ-H9ACAXiZwogd8g&p=https%3A%2F%2Ftestvagrant.tumblr.com%2Fpost%2F116708350781%2Frestful-services-testing-change-in-perspective&m=1&ts=1605912766) we kick started a discussion “Testing in Microservice Architecture” with its role and importance in near future and how test engineers and organisations need a change in Perspective. 

So the blog progresses explaining a typical Web Application Architecture, traditional testing approach and finally proposed new approach and change in perspective .

## What happens in a typical web application ?

Lets take a look what happens in a typical web application. Lets assume the services are RESTful. Below image shows typical interactions.

![image](https://64.media.tumblr.com/c4e79bd2924b7c899db5eb00db24934f/tumblr_inline_nmyoctOU6m1sma618_1280.png)

User opens up the application via a browser. Performs actions on the application which sends requests to web application over HTTP. Web application then forms a REST request and invokes corresponding services.

Further services would interact with Resources and so on… for this article’s purpose we need not go beyond Services layer.

REST response would be returned by the service. This response is sent back on to the web application and finally back to the browser for rendering.

## What is traditional testing approach ?

Manual Testing - Typically Test engineer considers entire application as a black-box (grey box in the image :-) ) and performs a set of actions on the application expecting a desired result. Also Test Engineer, puts himself into consumer’s shoes and tests for various use cases.

Automation Testing - Test Engineer automates test scenarios using UI automation tools like selenium. Executes these test scripts periodically.

![image](https://64.media.tumblr.com/64a03fdf92ef72a5f76451678a184632/tumblr_inline_nmyok6ljWP1sma618_1280.png)

## Services testing approach

As shown in below image the test engineer directly interacts with the services layer in order to execute the use cases. Although he is skipping the web application layer, he still continues to put himself in consumer’s shoes and test the services for various use cases.

Rather than web application constructing the REST requests, these would be programmatically constructed or hand-coded. Then these requests would be sent to service APIs. The responses received are verified manually or programmatically for correctness.

![image](https://64.media.tumblr.com/d0d03c56f3c78ed5ce563bfd2d6298f4/tumblr_inline_nmyoqmBHnd1sma618_1280.png)

## Change in perspective

As you would have observed, the black box has shrunk. Now test engineer is leaving web application behind for a moment and testing the services. He needs to take a closer look at every service for it’s purpose, boundaries and test for its behaviour

We are certainly not saying that the web application should be totally omitted from testing, however could be minimised to test the rendering aspects.

Lets see below image for another interesting use case

![image](https://64.media.tumblr.com/775a16b1c2c5789477db84a8434df9e9/tumblr_inline_nmypzjcF4F1sma618_540.png)

As we can see there could be more than one variety consumer for the services (not just the web application). Also consumers could be either internal or 3rd party. Each consumer would have their own business goals and are consuming the services exposed to achieve their business goals. This makes certification of services all the more important. Services needs to be treated as product and certify them for what they are built for.  

Test engineer needs to explore how to test and certify these services and arrive at strategies for Manual, Automation, Load Testing etc.. 



## [Testing in Microservice architecture - Test approach with a Sample Application](https://testvagrant.tumblr.com/post/117589223431/testing-in-microservice-architecture-test)

In this blog we are introducing a sample application with microservices. We would talk about test strategy (keeping in mind the architecture of the application) :

1. Testing one microservice at a time
2. Testing orchestration services
3. End to End testing

In later blogs we would use this sample application to illustrate concepts and code snippets.

## Sample Application

Let’s assume we are building a retail application. As part of it we are working on “Product Review” feature. Typical functionality would be:

- Customers should be able to read and write reviews for a product.
- Customers should be able to add comments to reviews.

Please find below reference image from Amazon, where in we could see a review and a comment underneath for iPhone 6. This is just for illustration purpose.

![image](https://64.media.tumblr.com/88e9d214d48baa98499fdaff6a3b3913/tumblr_inline_nngwbkaKFv1sma618_1280.png)

Further, lets assume we are developing below two microservices to build this functionality

- **Review** - Exposes APIs to perform CRUD (Create, Retrieve, Update and Delete) operations for reviews.
- **Comments** - Exposes APIs to perform CRUD operations for comments.

## Testing one service at a time

Lets take a pause and reiterate characteristics of microservices. They are small, atomic and should serve one purpose only. I liked the way it is described [here](https://t.umblr.com/redirect?z=http%3A%2F%2Fmartinfowler.com%2Farticles%2Fmicroservice-testing%2F%23definition&t=MzBlMjc2YTc0YzUwYmFlMDU4ZDk4NDI4MTYxZjZmZWI3YWRhNjg5YyxRajBMRWZaUw%3D%3D&b=t%3Adjh42wJ-H9ACAXiZwogd8g&p=https%3A%2F%2Ftestvagrant.tumblr.com%2Fpost%2F117589223431%2Ftesting-in-microservice-architecture-test&m=1&ts=1605912955) *- Microservices are nothing but single responsibility principle applied at architecture level.*

Applying the same principle to review microservice it should - Allow user to read reviews. Create new review. Update and delete review. Nothing more and nothing less.

Below image illustrates scope of testing/certification of:

1. Review microservice APIs - CreateReview, GetReview, GetAllReviews, UpdateReview and DeleteReview
2. Review Microservice persists review details into repository correctly

![image](https://64.media.tumblr.com/bb66766fa945ba178cf4d44a6f639d7d/tumblr_inline_nngwcvNXhr1sma618_640.png)

Now that we know review service APIs and their purpose, lets wear a QA hat and certify this microservice.

1. Positive tests - Verify the behaviour of service APIs for valid set of inputs
2. Negative tests - Verify the behaviour of service APIs for invalid set of inputs
3. Boundary Tests - Verify the behaviour of APIs at various boundary condition inputs
4. Contract Tests - Tests to verify the contract is intact

In same way the comments microservice also should be tested

## Testing Orchestration Services

While each microservice encapsulates certain business logic, it’s not enough to achieve business requirement.

Lets say we have a business requirement to build product details page with details of product, availability, price and reviews. This means I would need a service “ProductDetails” which fetches all product details. Assuming we have services to fetch inventory, price and reviews already, it would look like below image.

![image](https://64.media.tumblr.com/28eb483d361abe649eb816f5b682d6fa/tumblr_inline_nngwdyq9vL1sma618_1280.png)

ProductDetailsService acts as an orchestration service or sometimes referred as choreographic service - It interacts with one or more microservices - Pricing, Inventory, and Review in this case, collates the responses received from individual services, then returns it to the consumer.

Now that we know the purpose of ProductDetails service, we need to certify this orchestration service. Below are few scenarios:

1. Positive tests - Verify the behaviour of service for valid set of inputs. For example: Add Review to product -> Add Comment to the review -> Call Product Details service to make sure it returns review along with comment added
2. Negative tests - Verify the behaviour of service when inventory/pricing/review service is unavailable
3. Boundary Tests - Verify the behaviour of service when there are no reviews associated to product and when there are too many reviews to display.

## End-To-End Testing

![image](https://64.media.tumblr.com/f5fcd67f66c1aa8c10c5ce48536d23fe/tumblr_inline_nngwegMYup1sma618_1280.png)

Lets take a look at above image. The layer at the top represents user interactions with the application. User action triggers a service call to a service in orchestration layer and finally orchestration service would interact with one or more microservices at the bottom most layer.

An end to end test would start by browsing a product, adding it to cart and all the way till order gets placed. In order to test this use case, we need to make calls to various service APIs in sequence.

For instance following APIs needs to be called in sequence to place an order:

1. BrowseProduct - Gets details of product including inventory, pricing, reviews etc..
2. AddProductToCart - Add a product to cart
3. AddShippingAddress - Address where product needs to be shipped to
4. AddPayments - Mechanism to pay via various payment methods
5. PlaceOrder - Place the order

At the end of each API call, the response needs to be asserted for correctness.

## Quick Recap

We got introduced to bunch of microservices like Review, Inventory etc.. Talked about testing an individual microservice for its behaviour. Orchestration service for achieving a business requirement and what needs to be tested as part of it. Finally arrived at an end-to-end use case and testing the same by calling sequence of APIs.

## Advantages

Assuming we have tested and automated test scenarios as described above:

1. *Faster feedback:* As most of our test automation would be at service layers, the test execution time would be much faster compared to UI test automation way, leading to faster feedback cycles. Also these tests could be integrated to Continuous Integration system for automatic execution
2. *Defects could be identified faster:* As we have tests at each layer, a failing test would indicate a possible defect right at that particular layer. This would save time in defect detection and failure analysis.
3. *Quality of test suite:* Having right tests at right layer would help build right test mix and arrive at healthy [test pyramid](https://t.umblr.com/redirect?z=http%3A%2F%2Fmartinfowler.com%2Fbliki%2FTestPyramid.html&t=YWY3NzIzMjNlNzk0NTkwNDZkYzZhYjcwMDM0NDM5N2ZjMzJmZjhkNSxRajBMRWZaUw%3D%3D&b=t%3Adjh42wJ-H9ACAXiZwogd8g&p=https%3A%2F%2Ftestvagrant.tumblr.com%2Fpost%2F117589223431%2Ftesting-in-microservice-architecture-test&m=1&ts=1605912955). This would improve efficiency of the test suite.



## [Closer Look at Sample Application](https://testvagrant.tumblr.com/post/121583900746/closer-look-at-sample-application)

As we are assuming that the services we are testing are RESTful, it is essential to get familiarised with concepts and terminologies of RESTful services. Here are some helpful links to get us a good start.

[http://www.ibm.com/developerworks/library/ws-restful/](https://t.umblr.com/redirect?z=http%3A%2F%2Fwww.ibm.com%2Fdeveloperworks%2Flibrary%2Fws-restful%2F&t=ZjhhMzAyNWE4NTMwZjcyOGUwMzgyMzhiYTgzNTNjYzU5ZDQ2NTQwOSxGMXRYQ1RxNw%3D%3D&b=t%3Adjh42wJ-H9ACAXiZwogd8g&p=https%3A%2F%2Ftestvagrant.tumblr.com%2Fpost%2F121583900746%2Fcloser-look-at-sample-application&m=1&ts=1605913451)

[http://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm](https://t.umblr.com/redirect?z=http%3A%2F%2Fwww.ics.uci.edu%2F~fielding%2Fpubs%2Fdissertation%2Frest_arch_style.htm&t=ZWEwNjVlMDUwZGZhMTU1ZGQ1ZWZjMTZkN2E3ZjdiYjQ2YWExMzY1MSxGMXRYQ1RxNw%3D%3D&b=t%3Adjh42wJ-H9ACAXiZwogd8g&p=https%3A%2F%2Ftestvagrant.tumblr.com%2Fpost%2F121583900746%2Fcloser-look-at-sample-application&m=1&ts=1605913451)

[http://stackoverflow.com/tags/rest/info](https://t.umblr.com/redirect?z=http%3A%2F%2Fstackoverflow.com%2Ftags%2Frest%2Finfo&t=MzE2Zjc0NDA3MzY2OWQ1MDc5MmQ4ZmVkMjg1OTUyMmFhYTVkMmUxYSxGMXRYQ1RxNw%3D%3D&b=t%3Adjh42wJ-H9ACAXiZwogd8g&p=https%3A%2F%2Ftestvagrant.tumblr.com%2Fpost%2F121583900746%2Fcloser-look-at-sample-application&m=1&ts=1605913451)

## Brief of terminologies

Here is brief of some important terminologies extracted from above sources.

**Resource**

We got introduced to Review and Comments. A review or a comment is piece of information. Any such information is a resource. A resource can have relationship with other resources.  

**Resource Identifiers**

Identifier of a particular resource.

**HTTP - methods**

A resource’s state can only be altered by HTTP methods - GET, POST, PUT, DELETE. Essentially these standard methods allows us to perform CRUD operations on a resource.

**Representation**

Representation refers resource’s current state or intended state. XML/JSON are few commonly used formats of representation

## Contract

Contract describes the behaviour a service. In the previous blog we said Review service offers an API called CreateReview. The contract would describe the behaviour of CreateReview API when it gets invoked with set of input.

Lets see how a contract looks like in case of Reviews and Comments services.

## Contract of Reviews service

### Create Review

| Functionality      | Allows user to create review                                 |
| ------------------ | ------------------------------------------------------------ |
| URL                | /reviews                                                     |
| Method             | POST                                                         |
| Format             | json/xml                                                     |
| Example Request    | {  "title" : “Palm Tree”,  "body" : “Palm trees are a botanical family of perennial lianas, shrubs, and trees. They are in the family Arecaceae. They grow in hot climates”,  "author" : “Tom”,  "email" : “tom@tv.com” } |
| Example Response   | HTTP: 201 {  "id" : “1” }                                    |
| Request Parameters |                                                              |
| title              | string, required, minlength=5, maxlength=40                  |
| body               | string, required, minlength=20, maxlength=500                |
| author             | string, optional, maxlength=20                               |
| mail               | string, required, maxlength=40                               |
|                    |                                                              |

| Error Scenario                                             | Error Code              | Error Message              |
| ---------------------------------------------------------- | ----------------------- | -------------------------- |
| if the input parameter title is less than 5 characters     | ERROR_MIN_LENGTH_TITLE  | Title is too short         |
| if the input parameter title is greater than 40 characters | ERROR_MAX_LENGTH_TITLE  | Title is too long          |
| if the input parameter body is either missing or empty     | ERROR_REQUIRED_BODY     | Body can’t be blank        |
| if the input parameter body is less than 20 characters     | ERROR_MIN_LENGTH_BODY   | Body is too short          |
| if the input parameter body is more than 500 characters    | ERROR_MAX_LENGTH_BODY   | Body is too long           |
| if the input parameter author is more than 20 characters   | ERROR_MAX_LENGTH_AUTHOR | Author is too long         |
| if the input parameter email is missing or empty           | ERROR_REQUIRED_EMAIL    | Email can’t be blank       |
| if the input parameter email is not of valid pattern       | ERROR_INVALID_EMAIL     | Email pattern is incorrect |
| if the input parameter email is more than 40 characters    | ERROR_MAX_LENGTH_EMAIL  | Email is too long          |

### Get All Reviews

| Functionality    | Returns all reviews                                          |
| ---------------- | ------------------------------------------------------------ |
| URL              | /reviews                                                     |
| Method           | GET                                                          |
| Format           | json/xml                                                     |
| Example Request  |                                                              |
| Example Response | HTTP: 200 { “reviews” : [  {   "id" : “1”,   "title" : “Palm Tree”,   "body" : “Palm trees are a botanical family of perennial lianas, shrubs, and trees. They are in the family Arecaceae. They grow in hot climates”,   "author" : “Tom”,   "email" : “tom@tv.com”,   "comments": [    {     "id": “101”,     "comment": “Palm trees are a botanical family of perennial lianas, shrubs, and trees. They are in the family Arecaceae. They grow in hot climates”,     "commenter": “Jerry”,     "email": “jerry@tv.com”    },    {     "id": “102”,     "comment": “Palm tree seeds are the highest level fruit tree patch seeds”,     "commenter": “Tuffy”,     "email": “tuffy@tv.com”    }   ]  },  {   "id" : “2”   "title" : “Neem Tree”,   "body" : “Neem (Azadirachta indica) is a tree in the mahogany family Meliaceae.”,   "author" : “Tom”,   "email" : “tom@tv.com”,   "comments": [    {     "id": “201”,     "comment": “The neem plant is a fast growing and long living tree”,     "commenter": “Jerry”,     "email": “jerry@tv.com”    }   ]  },  {   "id" : “3”   "title" : “Banyan Tree”,   "body" : “Banyan often refers specifically to the Indian banyan (Ficus benghalensis), which is the national tree of the Republic of India”,   "author" : “Mickey”,   "email" : “mickey@tv.com”   "comments": [    {     "id": “301”,     "comment": “The seeds of banyans are dispersed by fruit-eating birds”,     "commenter": “Donald”,     "email": “donald@tv.com”    }   ]  } ]} |

### Get Review By Id

| Functionality    | Returns Review by id                                         |
| ---------------- | ------------------------------------------------------------ |
| URL              | /reviews/{id}                                                |
| Method           | GET                                                          |
| Format           | json/xml                                                     |
| Example Request  |                                                              |
| Example Response | HTTP: 200 {  "id" : “2”  "title" : “Neem Tree”,  "body" : “Neem (Azadirachta indica) is a tree in the mahogany family Meliaceae.”,  "author" : “Tom”,  "email" : “tom@tv.com”  "comments": [   {    "id": “201”,    "comment": “The neem plant is a fast growing and long living tree”,    "commenter": “Jerry”,    "email": “jerry@tv.com”   }  ] } |

### Edit Review

| Functionality      | Allows user to edit review                                   |
| ------------------ | ------------------------------------------------------------ |
| URL                | /reviews/{id}                                                |
| Method             | PUT                                                          |
| Format             | json/xml                                                     |
| Example Request    | {  "title" : “Palm tree”,  "body" : “Palm trees are a botanical family of perennial lianas, shrubs, and trees. They are in the family Arecaceae. They grow in hot climates”,  "author" : “Tom”,  "email" : “tom@tv.com” } |
| Example Response   | HTTP: 200 {  "id" : “1” }                                    |
| Request Parameters |                                                              |
| title              | string, required, minlength=5, maxlength=40                  |
| body               | string, required, minlength=20, maxlength=500                |
| author             | string, optional, maxlength=20                               |
| mail               | string, required, maxlength=40                               |
|                    |                                                              |

| Error Scenario                                             | Error Code              | Error Message              |
| ---------------------------------------------------------- | ----------------------- | -------------------------- |
| if the input parameter title is less than 5 characters     | ERROR_MIN_LENGTH_TITLE  | Title is too short         |
| if the input parameter title is greater than 40 characters | ERROR_MAX_LENGTH_TITLE  | Title is too long          |
| if the input parameter body is either missing or empty     | ERROR_REQUIRED_BODY     | Body can’t be blank        |
| if the input parameter body is less than 20 characters     | ERROR_MIN_LENGTH_BODY   | Body is too short          |
| if the input parameter body is more than 500 characters    | ERROR_MAX_LENGTH_BODY   | Body is too long           |
| if the input parameter author is more than 20 characters   | ERROR_MAX_LENGTH_AUTHOR | Author is too long         |
| if the input parameter email is missing or empty           | ERROR_REQUIRED_EMAIL    | Email can’t be blank       |
| if the input parameter email is not of valid pattern       | ERROR_INVALID_EMAIL     | Email pattern is incorrect |
| if the input parameter email is more than 40 characters    | ERROR_MAX_LENGTH_EMAIL  | Email is too long          |

### Delete Review

| Functionality    | Allows user to delete review |
| ---------------- | ---------------------------- |
| URL              | /reviews/{id}                |
| Method           | DELETE                       |
| Format           | json/xml                     |
| Example Request  |                              |
| Example Response | HTTP: 200                    |

## Contract of Comments service



### Add Comment

| Functionality      | Allows user to add comment for a review                      |
| ------------------ | ------------------------------------------------------------ |
| URL                | /comments                                                    |
| Method             | POST                                                         |
| Format             | json/xml                                                     |
| Example Request    | {  "reviewId" : “1”,  "comment" : “Palm trees are a botanical family of perennial lianas, shrubs, and trees. They are in the family Arecaceae. They grow in hot climates”,  "commenter" : “Jerry”,  "email" : “jerry@tv.com” } |
| Example Response   | HTTP: 201 {  "id" : “101” }                                  |
| Request Parameters |                                                              |
| comment            | string, required, minlength=20, maxlength=500                |
| commenter          | string, optional, minlength=5, maxlength=40                  |
| email              | string, required, maxlength=40                               |

| Error Scenario                                               | Error Code                 | Error Message              |
| ------------------------------------------------------------ | -------------------------- | -------------------------- |
| if the input parameter comment is either missing or empty    | ERROR_REQUIRED_COMMENT     | Comment can’t be blank     |
| if the input parameter comment is less than 20 characters    | ERROR_MIN_LENGTH_COMMENT   | Comment is too short       |
| if the input parameter comment is more than 500 characters   | ERROR_MAX_LENGTH_COMMENT   | Comment is too long        |
| if the input parameter commenter is missing or empty         | ERROR_REQUIRED_COMMENTER   | Commenter can’t be blank   |
| if the input parameter commenter is less than 5 characters   | ERROR_MIN_LENGTH_COMMENTER | Commenter is too short     |
| if the input parameter commenter is greater than 40 characters | ERROR_MAX_LENGTH_COMMENTER | Commenter is too long      |
| if the input parameter email is missing or empty             | ERROR_REQUIRED_EMAIL       | Email can’t be blank       |
| if the input parameter email is not of valid pattern         | ERROR_INVALID_EMAIL        | Email pattern is incorrect |
| if the input parameter email is more than 40 characters      | ERROR_MAX_LENGTH_EMAIL     | Email is too long          |

### Get All Comments

| Functionality    | Returns all comments for a review                            |
| ---------------- | ------------------------------------------------------------ |
| URL              | /comments/{review_id}                                        |
| Method           | GET                                                          |
| Format           | json/xml                                                     |
| Example Request  |                                                              |
| Example Response | HTTP: 200 {  "comments":[   {    "id": “101”,    "comment": “Palm trees are a botanical family of perennial lianas, shrubs, and trees. They are in the family Arecaceae. They grow in hot climates”,    "commenter": “Jerry”,    "email": “jerry@tv.com”   },   {    "id": “102”,    "comment": “Palm tree seeds are the highest level fruit tree patch seeds”,    "commenter": “Tuffy”,    "email": “tuffy@tv.com”   }  ] } |

### Delete Comment

| Functionality    | Allows user to delete a comment |
| ---------------- | ------------------------------- |
| URL              | /comments/{id}                  |
| Method           | DELETE                          |
| Format           | json/xml                        |
| Example Request  |                                 |
| Example Response | HTTP: 200                       |



## [Getting started with first test](https://testvagrant.tumblr.com/post/126314918981/getting-started-with-first-test)

## Scenario

Lets start with use case “An API consumer should be able to create a review successfully”

## Postman

Lets observe below image of [Postman](https://t.umblr.com/redirect?z=http%3A%2F%2Fwww.techrepublic.com%2Farticle%2Fchrome-add-on-postman-streamlines-testing-apis%2F&t=MjQzMzkzYThmNTM4NTQzMzNlYjEyZDc4ZDgyZDgxZjk4NmY1MmRiYyxHT3V3UVB4Mg%3D%3D&b=t%3Adjh42wJ-H9ACAXiZwogd8g&p=https%3A%2F%2Ftestvagrant.tumblr.com%2Fpost%2F126314918981%2Fgetting-started-with-first-test&m=1&ts=1605915871) rest client, which demonstrates the scenario.

![image](https://64.media.tumblr.com/72e2196c0ce444518bff791c8210f497/tumblr_inline_nsmb9qC9yu1sma618_1280.png)

A) URL
B) HTTP Method. POST in this case indicates that a new resource would be created
C) Request Body. Representation of the resource to be created
D) Status code of response. 201 indicating resource was created successfully
E) Response body

In the postman image the URL, HTTP Method and Request body forms a REST request. When this request was fired we got a REST response with HTTP status code and response body. The response body having “id”:“1” indicates identifier for the resource created.

## Rest Assured

REST Assured is a Java DSL for simplifying testing of REST services. Lets see below image where the scenario described above has been automated.



```java
    @Test
    public void shouldCreateAReview() {
        given()
                .request().with()
                    .queryParam("format", "json")
                    .body("\"title\":\"Palm Tree\"," +
                          "\"body\":\"Palm trees are a botanical family of perennial lianas, shrubs, and trees. They are in the family Arecaceae. They grow in hot climates\"," +
                          "\"author\":\"Tom\"," +
                          "\"email\":\"tom@tv.com\"")
        .when()
                .post("http://localhost:8080/reviews")
        .then()
                .assertThat()
                .statusCode(201)
                .body("id", notNullValue());
    }
```

Those who are familiar with [BDD](https://t.umblr.com/redirect?z=http%3A%2F%2Fmartinfowler.com%2Fbliki%2FGivenWhenThen.html&t=MDY1YWRhNDgzNzYwNDAwYjExM2VhMjU3ODgwOTg1M2M4YmNjYWYzNixHT3V3UVB4Mg%3D%3D&b=t%3Adjh42wJ-H9ACAXiZwogd8g&p=https%3A%2F%2Ftestvagrant.tumblr.com%2Fpost%2F126314918981%2Fgetting-started-with-first-test&m=1&ts=1605915871) syntax can easily read the test and understand the intent. This is one of the biggest advantage of REST Assured. The “given” block describes the REST request. “when” block calls the reviews API with HTTP post method. “then” block has the assertions. 

Lets compare the postman image with the test. Line number 11 has A) URL and B) HTTP Method, Line 5 to 9 represents C) Request body. Line 14 and 15 asserts 4) Status code and 5) Response body respectively.

Complete example code is available [here](https://t.umblr.com/redirect?z=https%3A%2F%2Fgithub.com%2Ftestvagrant%2FMockReviews%2Fblob%2Fmaster%2Fsrc%2Ftest%2Fjava%2FChapter1.java&t=ZDU3YmU3YTkwNTEzMmJjYWIyNDI1NzUwYzg1ZDdkYmIxMDhlMmViOSxHT3V3UVB4Mg%3D%3D&b=t%3Adjh42wJ-H9ACAXiZwogd8g&p=https%3A%2F%2Ftestvagrant.tumblr.com%2Fpost%2F126314918981%2Fgetting-started-with-first-test&m=1&ts=1605915871) as a maven project.

## [Retrieve a review](https://testvagrant.tumblr.com/post/126902572591/retrieve-a-review)

## Scenario

“An API consumer should be able to retrieve a review and assert it’s title”

## Postman

Lets observe below image of Postman rest client, which is showing retrieving of review.

![image](https://64.media.tumblr.com/6b50053bc663d9d0e38337b7e42f3e7c/tumblr_inline_nt2eaztxzJ1sma618_1280.png)

Observe that HTTP Method is GET signifying we are retrieving a resource. There is no request body. The URL specifies the identifier of the resource to be retrieved. HTTP Status 200 signifies that the request was successfully processed.

## Rest Assured

```java
    @Test
    public void shouldAssertTitleOfReview() {
        given()
                .request().with()
                    .queryParam("format", "json")
        .when()
                .get("http://localhost:8080/reviews/1")
        .then()
                .assertThat().body("title", equalTo("Palm Tree"));
    }
```



The “given” block creates REST request. “when” block calls the reviews API with HTTP GET method. “then” block asserts title in the response body.

## Create And Retrieve Review as Single Test

Lets combine create and retrieve review into a single scenario. Typical steps would be:

1. Create Review
2. Extract the review id
3. Retrieve the review by review id
4. Perform assertions

```java
    @Test
    public void shouldCreateAndGetReview() {
        String title = "Palm Tree";
        String body = "Palm trees are a botanical family of perennial lianas, shrubs, and trees. " +
                      "They are in the family Arecaceae. They grow in hot climates";
        String author = "Tom";
        String email = "tom@tv.com";
        String requestBody = String.format("\"title\":%s," +
                "\"body\":%s," +
                "\"author\":%s," +
                "\"email\":%s", title, body, author, email);


        String reviewId =
                given()
                    .request().with()
                        .queryParam("format", "json")
                        .body(requestBody)
                .when()
                    .post("http://localhost:8080/reviews")
                .then()
                    .extract().jsonPath().get("id").toString();


        given()
                .request().with()
                    .queryParam("format", "json")
        .when()
                .get(String.format("http://localhost:8080/reviews/%s", reviewId))
        .then()
                .assertThat().body("title",     equalTo(title))
                .assertThat().body("body",      equalTo(body))
                .assertThat().body("author",    equalTo(author))
                .assertThat().body("email",     equalTo(email));
    }
}
```

Line number 3 to 11, we are storing details of review to be created in variables and forming request body using them. Line number 13 to 21, create review and extract the “id” from the json response body using JsonPath. We would cover JsonPath in coming chapters.

Line number 23 to 32, we are retrieving the review based on id and asserting every attribute.



## [Object Mapping](https://testvagrant.tumblr.com/post/127464089421/object-mapping)

## Review class

Till now we have not used the benefits that java provides being an object oriented programming language. So lets leverage the object oriented programming concepts and move away from primitive types to java objects. That would make our life much easier as we can compose information together that can be reused as needed. e.g. Instead of dealing with title, author, body, email separately, we can deal with a single Review object that is composed of all these as shown in below snippet



```java
public class Review {
    private String title;
    private String body;
    private String author;
    private String email;
    private String id;
}
```

## Java Object instead of JSON Representation

In the previous blogs we saw that the request body was in the JSON string format or JSON representation. Hand coding JSON strings is not only time consuming but also error prone especially in case of contract changes. As a result we would end up with tests with significantly high maintenance effort.

REST Assured accepts java object (in the request) instead of JSON representation. It will serialize the java object to JSON representation, then fires the request. All that you need is to have

1. Either [Jackson](https://t.umblr.com/redirect?z=http%3A%2F%2Fwiki.fasterxml.com%2FJacksonHome&t=NjBjYTU3Njk0Y2FmNGVjMjM2Y2E3Yzg5MjZmM2Q0YjViYzNhMmNkYyxCWEc0U3dCZw%3D%3D&b=t%3Adjh42wJ-H9ACAXiZwogd8g&p=https%3A%2F%2Ftestvagrant.tumblr.com%2Fpost%2F127464089421%2Fobject-mapping&m=1&ts=1605912748) or [Gson](https://t.umblr.com/redirect?z=https%3A%2F%2Fsites.google.com%2Fsite%2Fgson%2Fgson-user-guide&t=NDE5YWNmNzExOThlMjgxYTE5YjYwMWQwM2M3N2MzMDA1ZTY4ZWQ3OCxCWEc0U3dCZw%3D%3D&b=t%3Adjh42wJ-H9ACAXiZwogd8g&p=https%3A%2F%2Ftestvagrant.tumblr.com%2Fpost%2F127464089421%2Fobject-mapping&m=1&ts=1605912748) object mapper in the classpath and
2. Content-Type is be set as “application/json”

```java
    @Test
    public void shouldCreateAndGetReview() {
        Review review = new Review(
                "Palm Tree",
                "Palm trees are a botanical family of perennial lianas, shrubs, and trees. " +
                        "They are in the family Arecaceae. They grow in hot climates",
                "Tom",
                "tom@tv.com");


        String reviewId =
                given()
                    .request().with()
                        .queryParam("format", "json")
                        .contentType("application/json")
                        .body(review)
               .when()
                    .post("http://localhost:8080/reviews")
               .then()
                    .extract().response().as(Review.class).getId();


        Review actualReview =
                given()
                    .request().with()
                        .queryParam("format", "json")
                        .contentType("application/json")
                .when()
                    .get(String.format("http://localhost:8080/reviews/%s", reviewId))
                .then()
                    .extract().response().as(Review.class);


        assertEquals(actualReview.getTitle(), review.getTitle());
        assertEquals(actualReview.getBody(), review.getBody());
        assertEquals(actualReview.getAuthor(), review.getAuthor());
        assertEquals(actualReview.getEmail(), review.getEmail());
    }
```

Lets observe the refactored code above.

- We have a review object at line 3. This represents the review we would like to create. Previously we used primitive types.
- Line number 14 we are mentioning Content-Type as JSON and in line number 15 we are passing review object to the body(). Previously we passed JSON string.
- Line number 19 we are extracting the REST response and mapping it to object of type “Review”, then returning the id of the review got created.
- Line number 29 we are extracting the REST response and mapping it to object of type “Review”.
- Line 31 to 34, the assertions are now on review object
# REST APIs

#### In this guide, you’ll learn how REST APIs, an architectural style for managing networked applications, can ease software development processes.

## Table of Contents

- [What is a REST API?](https://www.ibm.com/cloud/learn/rest-apis?utm_medium=OSocial&utm_source=Youtube&utm_content=000023UA&utm_term=10010608&utm_id=YTDescription-101-API-vs-SDK-LH-REST-API-Guide&cm_mmc=OSocial_Youtube-_-Cloud+and+Data+Platform_SFT+Cloud+Platform+Digital-_-WW_WW-_-YTDescription-101-API-vs-SDK-LH-REST-API-Guide&cm_mmca1=000023UA&cm_mmca2=10010608#toc-what-is-a--MEwTVhOV)
- [How they work](https://www.ibm.com/cloud/learn/rest-apis?utm_medium=OSocial&utm_source=Youtube&utm_content=000023UA&utm_term=10010608&utm_id=YTDescription-101-API-vs-SDK-LH-REST-API-Guide&cm_mmc=OSocial_Youtube-_-Cloud+and+Data+Platform_SFT+Cloud+Platform+Digital-_-WW_WW-_-YTDescription-101-API-vs-SDK-LH-REST-API-Guide&cm_mmca1=000023UA&cm_mmca2=10010608#toc-how-they-w-wze0SSQu)
- [Different types](https://www.ibm.com/cloud/learn/rest-apis?utm_medium=OSocial&utm_source=Youtube&utm_content=000023UA&utm_term=10010608&utm_id=YTDescription-101-API-vs-SDK-LH-REST-API-Guide&cm_mmc=OSocial_Youtube-_-Cloud+and+Data+Platform_SFT+Cloud+Platform+Digital-_-WW_WW-_-YTDescription-101-API-vs-SDK-LH-REST-API-Guide&cm_mmca1=000023UA&cm_mmca2=10010608#toc-different--BMH4PPTT)
- [Securing REST APIs](https://www.ibm.com/cloud/learn/rest-apis?utm_medium=OSocial&utm_source=Youtube&utm_content=000023UA&utm_term=10010608&utm_id=YTDescription-101-API-vs-SDK-LH-REST-API-Guide&cm_mmc=OSocial_Youtube-_-Cloud+and+Data+Platform_SFT+Cloud+Platform+Digital-_-WW_WW-_-YTDescription-101-API-vs-SDK-LH-REST-API-Guide&cm_mmca1=000023UA&cm_mmca2=10010608#toc-securing-r-ggXsQ9u-)
- [REST APIs and IBM Cloud](https://www.ibm.com/cloud/learn/rest-apis?utm_medium=OSocial&utm_source=Youtube&utm_content=000023UA&utm_term=10010608&utm_id=YTDescription-101-API-vs-SDK-LH-REST-API-Guide&cm_mmc=OSocial_Youtube-_-Cloud+and+Data+Platform_SFT+Cloud+Platform+Digital-_-WW_WW-_-YTDescription-101-API-vs-SDK-LH-REST-API-Guide&cm_mmca1=000023UA&cm_mmca2=10010608#toc-rest-apis--9v3PZhJ5)

## What is a REST API?

A REST API is a web service that uses the REST (Representational State Transfer) architecture to handle a request on a frontend web service. If you go to an online bookstore and look for the latest novel from your favorite author, the website would use a REST API to query the company database. The website would then parse the data and present it to you on the site.

API stands for Application Programming Interface and is simply the underlying code structure that lets different applications speak to each other.

See “[What is an API?](https://developer.ibm.com/apiconnect/docs/what-is-an-api/)” from IBM Developer for a closer look at APIs.

The IBM Knowledge Center also provides a wealth of additional reading on [REST APIs](https://www.ibm.com/support/knowledgecenter/SSLKT6_7.6.1/com.ibm.mif.doc/gp_intfrmwk/rest_api/c_rest_overview.html).

## How they work

REST APIs use HTTP (i.e., HyperText Transfer Protocol, the language of the web) to process data requests. The process is as follows:

- The GET request retrieves data from a resource. For example, you’d use a GET request to retrieve a record from a database.
- The POST request creates a new resource, such as a new record in a database.
- The PUT request updates an existing resource.
- The DELETE request deletes a resource.

REST APIs can be programmed with a number of languages, including Python, Node.js, and Java.

## **REST API framework**

The REST API is part of the integration framework and handles requests from external consumers.

The following diagram provides an overview of how the REST API handles requests.

![The image is described in text](https://www.ibm.com/support/knowledgecenter/SSLKT6_7.6.1/com.ibm.mif.doc/images/TPAe75_apiOverview.gif)

When an external consumer initiates a request, a REST API controller directs the resource request to the appropriate resource handler. The resource handler interacts with resources to execute the request. When the request is complete, the resulting resource or resource collection is processed by the resource serializer. The resource serializer returns the representation as the resource response. Serializers are provided for XML and JSON.

Because the REST API is within the integration framework, the REST API can use process automation engine authentication, authorization, and system properties.

The REST API controller is a servlet. Performance and tuning for load and scalability is done at the application server level as it is for other web components.

### How to use REST APIs

Besides specifying your data request, working with REST APIs also requires specifying the languages it supports, how responses should be formatted, how to use callback information, if there are API keys involved, and any request limitations.

Once you work through those details, here’s what you’ll need to do:

1. **Find your endpoint:** HTTP requests depend on a particular URL, and root endpoints may be formatted differently depending on the API you’re using. You also need to determine the path or the resource you’re requesting, which may involve looking through the API documentation.
2. **Choose your method:** You can use REST APIs to send any of the requests mentioned earlier—GET, POST, PUT, or DELETE—as well as a few others. In all cases, the methods allow you to perform functions like Creating, Reading, Updating, and Deleting, (otherwise known as CRUD).
3. **Identify your header:** If a REST API request is like sending a letter, think of headers as the envelope containing valuable information for the person receiving it. In this case, the header could have details that help explain what will be in the “body” content of the request or, perhaps, important information about authentication and security. Headers can be grouped by their context or how various proxies handle them.
4. **Determine if you need message data:** If your requirements include sending specific information to the server—in POST, PUT, or DELETE requests, for example—this becomes what is sometimes referred to as the “body” of your requests.

Much of what happens at this stage will be influenced in part by the platform you use to work with REST APIs, the most common choices being Python and Java.

## Different types

### Python REST API

The Python REST API uses the Python programming language to program the backend of a web service. If you’re already familiar with programming in Python, you might choose the Python REST API platform based on the cleanliness of its syntax, its standard libraries, and the broad developer community it enjoys. Some also opt for this platform given the number of third-party open source tools available and the fact it is increasingly common within large enterprise environments. There is no specific server required for you to use the Python REST API. Instead, Python works best with Flask, a microframework designed for Python, when creating REST APIs.

#### Python REST API Example

Imagine you have to create an application that will oversee or control all the devices used in a particular department in a large company. You could use Python to build a REST API to create, read, or delete those resources (the devices) that the API stores as objects. These resources would be stored as a collection, and some of the requests you might add here would include GET (to view the collection of devices), POST (to add a new device), and similar requests to manage individual devices within a collection.

### Java REST API

You can also develop REST APIs using Java. Java has attracted millions of developers and includes tools—like the Java Virtual Machine—that have been around for more than 20 years. Integrating Java with Spring or Eclipse allows you to build an API with a standard, extensible library.

#### Java REST API example

Whether you’re in a startup or a large enterprise, a big part of many businesses involves managing orders for products and services. Think about how you could use Java and REST APIs to create an order processing system. Requests like GET would help fetch a list of all orders currently in process, or PUT requests could be used to add to an existing order. Of course, customers sometimes cancel their orders, which is where DELETE would be the appropriate request to use.

#### How to use Java REST APIs

REST APIs are easy enough to work with that you can keep the process entirely manual—reading responses into a string and parsing it manually, for example—but you can also look at some of the libraries that automate parts of the process, including [JAX-RS](https://jax-rs-spec.java.net/), an API for RESTful web services used in Java Enterprise Edition (J2EE).

#### Java REST API framework

[Jersey](https://jersey.github.io/) is a Java REST API framework option to consider if you prefer to use standard specifications. Another framework, [Spring REST](https://spring.io/guides/gs/rest-service/), takes advantage of the Spring modular framework. [RESTEasy](https://resteasy.github.io/) is a good option if you're working with specific application servers like JBOSS, since RESTEasy was developed by JBOSS to be integrated into the server as the primary Java framework.

## Securing REST APIs

Some of the fundamentals of securing REST APIs including the following:

- Do not expose unnecessary information, such as user names, within the URLs you’re using.
- Be sure to hash any passwords.
- Use the secure socket layer (SSL) capabilities in HTTPS.
- Add a timestamp to requests.
- Use basic authentication in the header. The IBM Knowledge Center provides [more information about header authentication](https://www.ibm.com/support/knowledgecenter/en/SSPREK_7.0.0/com.ibm.isam.doc_70/ameb_webseal_guide/concept/con_http_hdr_authe_overview.html).
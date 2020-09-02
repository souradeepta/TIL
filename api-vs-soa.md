# The SOA, the API and “REST” of the API



When I jumped on the API bandwagon a couple years ago, I thought I just caught the last train of the API phenomenon. Today API is still hot and getting hotter each day with no signs of abating.

With things where they are now, I thought that it would be a good time to look back at what has happened and where things are going with the API phenomenon as well as do a comparison of Service Oriented Architecture (SOA) and API.

Many of the views in this post are my personal observations and may or may not coincide with other views on the same subject.

## **The Brief SOA and API History**

People often say today’s web API is just SOA with a new name. I used to subscribe to that notion and there is actually a certain truth to it. However, I have been converted now. I do see how API stands out on its own and occupies a unique spot in the world of web services and integration. To understand where it came from and how it has taken the world by storm, it’s best we take a quick look at the API’s humble beginning.

When I was doing C programming on UNIX some 20 years ago, we used to call a C-library. Either you do a “man” page or you can consult a printed manual to figure out how to do it. Later on, there was the famous Windows 32 API (a book that’s 2 bricks thick), then MFC API, and then Java API. I consider those the true origin of today’s modern web API.

The modern web API share a tangled history with the development of web services and SOA.

Sometime in the 90’s, computer networking became ubiquitous. Instead of simply using C/C++ API in an application on a local PC, applications started to call processes across the network leading to the creation of RPC (Remote Procedure Call). At about the same period, two other things were developing simultaneously: the internet and OO (object oriented programming). OO programming placed an emphasis on encapsulation and object interface. The Internet put network computing on steroid. With the marriage of both, we got CORBA/COM/RMI. All of these technologies are OO based and they are all binary protocols. Developers prefer human readable protocols. That was when SOAP entered the scene. SOAP is a XML based protocol. SOAP based web services quickly spread all over the place and that eventually lead to the SOA.



In certain sense, the traditional SOA web services are the natural extension of OO to the internet. Instead of doing OO with C++/Java on local servers, SOA does OO with XML/SOAP over the internet. SOA also occupies a unique space in the enterprise integration world. It emphasizes disciplined integration of enterprises services. SOA relies heavily on XSD, which is powerful and still the golden standard if you want a well-defined data structure. SOA reached its full maturity with the wide adoption of COE (Center of Excellence) a few years later.

Going back to the turn of the century, internet companies start to emerge. Besides big companies embracing SOA as the choice for enterprise integration, new companies like Salesforce, eBay, Amazon, Google and other social media companies start to put their application capabilities online. Unlike the old guards in big traditional enterprises, the younger developers in these companies do not love the heavy weight SOAP services as much. They prefer JSON, HTTP, and lighter stacks. When they publish their services online, they intend for everyone to use them, just like the author of the C-library, or Java API. These web services tend to come with self-explanatory document (reminiscent of the old “man” page). Enter the terminology “Web API”, or simply API.

Here is schematic view of the rough timeline of SOA and API:

[![The SOA, the API and “REST” of the API](https://i1.wp.com/blogs.perficient.com/files/2017/06/soa-api-timeline-1.jpg?resize=604%2C161&ssl=1)](https://i1.wp.com/blogs.perficient.com/files/2017/06/soa-api-timeline-1.jpg?ssl=1)



## **SOA vs. API**

As we have seen, SOA and API are related. Some differences are superficial, such as SOA is heavily based on SOAP, whereas, API tends to go with JSON and REST. Some differences are more subjective, for example, SOA is often associated with enterprise services integration, whereas, API intends to expose the functions of a system and let users create their applications using the API (we all heard the buzzword “composable” services).

Here is a more detailed comparison table:

|                                | Web service                 | SOA                                                          | API                                                          |
| ------------------------------ | --------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| What is it?                    | Generic online service      | An architectural style; commonly based on SOAP protocol, more intended for integration purpose | Web services expose the functionalities of a system that intends to *enable* client to build new applications |
| Typical audience               | Any                         | Enterprise customers                                         | Enterprise or at large audiences                             |
| Typical use                    | Any                         | integration                                                  | Build application, integration                               |
| Foundation                     | Grows out RPC, and internet | Grow out of RPC , OO discipline and internet                 | Grow out C++/java library and internet                       |
| Security                       | varies                      | strictly-defined, enterprise oriented, not flexible enough for internet applications | internet friendly (e.g. oauth), policy controlled            |
| Analytics                      | Not emphasized              | Not emphasized                                               | Yes                                                          |
| Management UI                  | varies                      | Not emphasized nor standardized                              | Commonly come with web management console with standard features |
| Stability, resilient to change | varies                      | Intends to be stable, not as resilient to change             | Interface intends to be stable, resilient to internal changes especially if REST principle is followed |
| Other Characteristics          |                             | More strictly controlled, tends to be for inter/intra enterprises integration | nimble, strongly promoting general access                    |

 

## **The “Rest” of the API**

It would be amiss not to mention REST while discussing the API. In year 2000, Roy Fielding, in his PhD dissertation proposed a new REST (Representational State Transfer) software architectural style. The intension was to design “good” software that can last for decades vs. last for years.

Combining REST and API, we have the REST API. This is where the API world is heading towards now. Details of REST API is beyond the scope of this post. It deserves an entire separate discussion.

I would like to conclude this post with a stacked view of the things we discussed. Please note both SOA and REST are architectural styles and design disciplines. SOAP is a protocol and has a strong typed WSDL definition that contains header, body, and type etc. REST is *not* a protocol. Although it promotes the use of HTTP verb, REST in itself does not mandate any specific format (protocol). Most REST implementations based on RAML, OpenAPI do conform to HTTP verbs, such as GET, PUT, POST, PATCH etc.

[![The SOA, the API and “REST” of the API](https://i1.wp.com/blogs.perficient.com/files/2017/06/soa-rest-api-stack.jpg?resize=569%2C182&ssl=1)](https://i1.wp.com/blogs.perficient.com/files/2017/06/soa-rest-api-stack.jpg?ssl=1)
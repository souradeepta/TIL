# Rest Architectural Elements

## What are Rest Architectural Elements?

***Rest Architectural*** style defines parts of Restful service as key conceptual elements. For e.g. while we make a Rest request to a Server, the information returned by the Server is called a ***Resource***. Let us take a quick look at the key conceptual elements in a ***RESTful Web Service***.

 

| ***Element***           | ***Example***                                                |
| ----------------------- | ------------------------------------------------------------ |
| Resource                | Information stored on a Server, which can be requested by a client. It could be Weather info or may be employee details |
| Resource Identifier     | Now that we have a resource defined, we need to uniquely identify the resource. That is actually the complete URL |
| Representation          | A resource is the actual data. Now this data can be represented as an XML, HTML or may be simple text. That is what is called a Representation |
| Representation Metadata | In order for the Clients to specify and process a resources given in a particular Representation (XML or HTML etc) some extra data (Metadata) needs to be passed in the request. |

 

 

### ***Resource***

A resource in the Restful architecture is an asset that is available on a Server. For e.g. the weather API that we discussed earlier (***[http://restapi.demoqa.com/customer-api/v1/ping/weather?city=hyderabad](https://restapi.demoqa.com/customer-api/v1/ping/weather?city=hyderabad)\***) returns the weather information about the city specified. Hyderabad in this case. The returned weather data is a resource on the Server. A Resource can be

- ***Temporal\***
- ***Static\***

Temporal resource is one that keeps changing with time. For e.g. the Weather. A Static resource is one that stays constant over longer time durations. For e.g. a webpage containing some static text. Like the one you are reading now can just be a Static resource on the server.

 

### ***Resource Identifier\***

Every Resource on the Server needs to be uniquely identifiable. To do that every resource has a unique URL that can be used to get to that Resource. Taking an example of it, ToolsQA has hosted a web service that can return you the location from which the Client is making a call.

URL: ***[http://restapi.demoqa.com/utilities/locationutil/mylocation](https://restapi.demoqa.com/utilities/locationutil/mylocation)***

You can see that to access your location you have to hit the above URL. Here the ***Resource\*** is your location while the unique way to access it is the ***Resource Identifier (URL).\***

An important point to note here is that a resource is not necessarily a static piece of information. For example, this URL when hit by someone from the United States will give their location while when hit from India will give some location from India. The location is calculated at run time and sent back to the Client. Even dynamic information is also called a ***Resource.\***

A similar example would be the weather API expose by ToolsQA. Here is the ***Resource Identifier\*** for the weather API: ***[http://restapi.demoqa.com/utilities/weather/city/hyderabad](https://restapi.demoqa.com/utilities/weather/city/hyderabad)***

Here also we can see that every city can result in a unique ***Resource Identifier\*** for e.g. ***[http://restapi.demoqa.com/utilities/weather/city/redmond](https://restapi.demoqa.com/utilities/weather/city/redmond)*** will give us the weather details of Redmond.

 

### ***Representation and Representation Metadata***

Now that we have our head cleared about a ***Resource\*** and a ***Resource Identifier,\*** it is time to understand what a ***Resource Representation\*** means. A Representation of resource is Data plus Meta Data explaining about the data. Meta Data in the strict sense of Request and Response will be some header explain the format type of the Response body. For e.g. a Resource meta data may say that the content in the Response body is of Json format. This will inform Clients about how to interpret the data. Let us take an example of a Sample API. Let us try to see what the Request and Response headers look like for the Weather API exposed by ToolsQA.

***Resource Identifier: [http://restapi.demoqa.com/utilities/weatherfull/city/hyderabad](https://restapi.demoqa.com/utilities/weatherfull/city/hyderabad)\***

This API gives the Weather details of the city plus the Request and Response data that was transferred during the transaction. Look out for the ***Content-Type\*** Header in the Response. The value of the ***Content-Type\*** Header will be ***application/json; charset=utf-8.\*** You would have already guessed that the data in the response body will be of format **Json.** Which can be verified by looking at the Response body at the end of the page. Here is an image pointing out two important parts of the Response

 

Similarly, we can see that if we hit the ***Resource Identifier\*** of equivalent XML based Weather web service we will get the same data ***represented\*** as XML. What it means is the we have changed the ***Resource Representation\*** from Json to XML. On the same lines, we can even change the representation of data from XML to HTML. Hence the name of the architecture **RE**presentation **S**tate **T**ransfer.
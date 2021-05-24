# HTTP Response

## What is HTTP Response?

***HTTP Response*** is the packet of information sent by *Server* to the *Client* in response to an earlier *Request* made by *Client*. *HTTP Response* contains the information requested by the Client. For example, the request to Weather Web Service made in the [***HTTP Request***](https://toolsqa.com/client-server/http-request/) tutorial will contain the weather details of the location.

Just like HTTP Request, HTTP Response also has the same structure:

- ***Status Line\***
- ***Headers, 0 or more Headers in the request\***
- ***An optional Body of the Request\***

 

### ***Response Status Line\***

A ***Status\*** ***Line\*** consists of three parts:

1. *HTTP Protocol Version*
2. *Status Code*
3. *Reason Phrase*

In the weather Rest Web Service example, lets scroll down the page to see the Response section. In the response section the first line is called the ***Status Line\***. As shown in the below image:

![HTTP Response](https://cdn-anlbg.nitrocdn.com/dKKErbUyoNysjatCgltCzbTJJilTMwLi/assets/static/optimized/rev-4f664c4/wp-content/gallery/restapi/Response-Status-Line.png)

It is clearly visible that the ***Status Line\*** has ***;following information:\***

- *HTTP Protocol Version as* ***(HTTP/1.1)\***
- *Status Code as **200***
- Status Message as **OK**

 

### ***Response Header***

Just after the *Status Line*, Headers are displayed. Just like a *Request Header*, *Response Header* also contains zero or more *Header lines*. However, it is very uncommon to have zero Headers in the response. Lines just after the ***Status Line\*** and before the ***Response Body\*** are all ***Response Headers*** lines. Headers are used to pass additional information to the Client. See the image below:
![HTTP Response Header](https://cdn-anlbg.nitrocdn.com/dKKErbUyoNysjatCgltCzbTJJilTMwLi/assets/static/optimized/rev-4f664c4/wp-content/gallery/restapi/Response-Header.png)

In the response Header, there is a header named ***Content-Type.\*** The value of the header is ***application/json; charset=utf-8.\*** It means that server is informing the client that the body of the response will contain a ***JSON*** formatted data. If client needs to make any sense out of the response body it should be interpret it as a *JSON*.

 

### ***Response Body\***

***Response Body*** contains the resource data that was requested by the client. In our example, *City Hyderabad* was requested for the weather data. . Look at the ***Response Body,\*** it contains the weather information of the City. It also has the information about the ***Temperature, Humidity, Weather description\*** *and a few more weather properties of the city****.\***

![HTTP Response Body](https://cdn-anlbg.nitrocdn.com/dKKErbUyoNysjatCgltCzbTJJilTMwLi/assets/static/optimized/rev-4f664c4/wp-content/gallery/restapi/Response-Body.png)

***Note:** Response Body contains text in JSON format, as one of the Response headers suggested.
*

 

In terms of Restful Web Services, the information requested by a client is referred to as a ***Resource.\*** In this example, the Weather data corresponding to a city is a ***Resource\***.
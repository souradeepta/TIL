# HTTP Request

## What is HTTP Request?

***HTTP Request*** is a packet of information that one computer sends to another computer to communicate something. To its core, *HTTP Request* is a packet of binary data sent by the Client to server. An *HTTP Request* contains the following parts

1. ***Request Line\***
2. ***Headers, 0 or more Headers in the request\***
3. ***An optional Body of the Request\***

 

To understand different parts of an HTTP Request let’s use this ***Test Weather REST Web Service*** that ToolsQA created for demo purpose.

- ***Resource URL**: http://restapi.demoqa.com/utilities/weatherfull/*
- ***Parameter:** city/cityName=city/<NameFoTheCity>*

City Name is provided in the URL via a parameter query at the end of the URL. Let’s say that we want to see the weather details of Hyderabad. In that case the URL will become *[http://restapi.demoqa.com/utilities/weatherfull/city/hyderabad](https://restapi.demoqa.com/utilities/weatherfull/city/hyderabad)*

Copy this URL and paste it in any browser’s address bar and scroll down on the page till you see a section called request. This request section shows you the request that is sent from your browser (Client) to the Server. The request section breaks down your request into different parts. Let us go by them one by one.

 

### ***Request Line\***

A ***Request Line*** specifies the Method Token (***GET, PUT*** … ) followed by the Request URI and then the *HTTP Protocol* that is being used. You can see this in the first line of the request section named: ***Request Line:\***
![HTTP Request](https://cdn-anlbg.nitrocdn.com/dKKErbUyoNysjatCgltCzbTJJilTMwLi/assets/static/optimized/rev-4f664c4/wp-content/gallery/restapi/Request-Line.png)

It is clearly visible that the request has the ***Method Type\*** as ***Get*** followed by the ***URI\*** which then is followed by the ***HTTP Protocol\***.

 

### ***Request Header\***

A *HTTP Request* contains zero or more ***Request Headers***. In the request section, whatever follows ***Request Line\*** till before ***Request Body\*** everything is a *Header*. Headers are used to pass additional information about the request to the server.

***For example,** In the Request section that is open in the browser look at the **Accept-Language.** It tells the server about the language preferences of the Client. This can be used to reply to the client in the preferred language. Marked as **1** in the image below.*

![HTTP Request Header](https://cdn-anlbg.nitrocdn.com/dKKErbUyoNysjatCgltCzbTJJilTMwLi/assets/static/optimized/rev-4f664c4/wp-content/gallery/restapi/Request-Header.png)

### ***Request Body\***

***Request Body*** is the part of the *HTTP Request* where additional content can be sent to the server.

***For example**, a file type of JSON or XML.*

Request body tries to send additional information required by the server to process current request properly. In the current example we haven’t sent a body in the request to the server. This can be seen by an empty ***Request Body*** section.
![Request Body](https://cdn-anlbg.nitrocdn.com/dKKErbUyoNysjatCgltCzbTJJilTMwLi/assets/static/optimized/rev-4f664c4/wp-content/gallery/restapi/Request-Body.png)
# Application Programming Interface (API)

It specifies how a software component should interact. It's like a contract between a client and a server: if a client request a specific format, the server will always respond in a documented format or initiate a defined action.

We deal with web-based APIs.

They are extensively used to incorporate a third-party functionality to your service. APIs can be free or paid. All APIs should have some form of documentation.

You connect to an API through an HTTP request. First things, first.

"HTTP" is protocol. More specifically, it's the **HyperText Transfer Protocol**. It basically specifies **how** requests and responses (between client and server) are to be formatted and transmitted. This is how all the surface Internet works. Everything you see on the surface web is a response from a server, which is a computer machine responsible for storing files. Your computer is the client.

When your computer makes a request to that server. The server gives you a response: it sends all the files to your computer... which, in turn, downloads it temporarily and builds the webpage with the help of the browser. The browser, actually, connects all the files together in order to give the purposed visual representation of them. The web page.

The two most popular HTTP requests are **GET** and **POST**.

GET | POST
--- | ----
Obtain data from a server | Alter state or send confidential information
Can be bookmarked |Parameters are added in a separate body
Parameters are added directly to the URL | -
_Not used for sensitive information_ | -


If everything is fine with your request, you should get a 200 status code. If things go wrong, you get the 4044 status code error of page not found.

# JSON

The name comes from "JavaScript Object Notation" but nowadays it is language-independent. It's got nothing to do with Javascript anymore.

JSON is the standard format for data exchange in the web.

When the client sends an API request to the server, it responds by sending the data in the JSON format. It also workds with POSTs requests - the response is in the JSON format.
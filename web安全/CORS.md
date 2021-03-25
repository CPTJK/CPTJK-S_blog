# cross origin resource sharing(CORS)

## brief

The browser's same origin policy blocks reading a resource from a different origin . This mechanism stops a malicious site from reading another site's data , but it also prevents legitimate uses . What if you want to get weather data from another country ?

In a modern web application, an application often wants to get resources from a different  origin. For example,you want to retrieve JSON data from a different domain or load images from another site into a `<canvas>` element.

In other words, there are public resources that should be available for anyone to read , but the same-origin policy blocks that . Developers have used work-around such as JSONP, but **cross origin resource sharing** fixes this in a standard way .

Enabling CORS lets the server tell the browser it is permitted to use an additional origin .

## How does a resource request work on the web ?

A browser and a server can exchange data over the network using the HTTP. HTTP defines the communication rules between the requester and the responder , including what information is needed to get a resource .

The HTTP header is used to negotiate the type of message exchange between the client and the server and is used to determine access . Both the browser's request and the server's response message are divided into two parts: **header** and **body**:

### How does CORS work?

Remember , the same origin policy tells the browser to block cross origin request . When you want to get a public resource form a different origin , the resource providing server needs to tell the browser "This origin where the request is coming from can access my access my resource". The browser remember that and allows cross origin remember sharing .

### step 1: client (browser ) request

When the browser is making a cross origin request , the browser adds an `Origin` header with the current origin (scheme , host , port)

### step 2: server response

On the server side , when a server sees this header , and wants to allow access , it needs to add an `Access-Control-Allow-Origin` header to the response specifying the requesting origin (or "*" to allow any origin )

### step 3:browser receives response 

When the browser sees this response with an appropriate `Access-Control-Allow-Origin`

# cross origin resource sharing(CORS)

## brief

The browser's same origin policy blocks reading a resource from a different origin . This mechanism stops a malicious site from reading another site's data , but it also prevents legitimate uses . What if you want to get weather data from another country ?

In a modern web application, an application often wants to get resources from a different  origin. For example,you want to retrieve JSON data from a different domain or load images from another site into a `<canvas>` element.

In other words, there are public resources that should be available for anyone to read , but the same-origin policy blocks that . Developers have used work-around such as JSONP, but **cross origin resource sharing** fixes this in a standard way .

Enabling CORS lets the server tell the browser it is permitted to use an additional origin .

## How does a resource request work on the web ?

A browser and a server can exchange data over the network using the HTTP. HTTP defines the communication rules between the requester and the responder , including what information is needed to get a resource .

The HTTP header is used to negotiate the type of message exchange between the client and the server and is used to determine access . Both the browser's request and the server's response message are divided into two parts: **header** and **body**:

## How does CORS work?

Remember , the same origin policy tells the browser to block cross origin request . When you want to get a public resource form a different origin , the resource providing server needs to tell the browser "This origin where the request is coming from can access my resource". The browser remember that and allows cross origin remember sharing .

### step 1: client (browser ) request

When the browser is making a cross origin request , the browser adds an `Origin` header with the current origin (scheme , host , port)

### step 2: server response

On the server side , when a server sees this header , and wants to allow access , it needs to add an `Access-Control-Allow-Origin` header to the response specifying the requesting origin (or "*" to allow any origin )

### step 3:browser receives response

When the browser sees this response with an appropriate `Access-Control-Allow-Origin` header , the browser allows the response data to be shared with the client site .

## share credentials with CORS

For privacy reason , CORS is normally used for "anonymous requests" --ones where the request doesn't identify the requestor. If you want to send cookies when using CORS(which could identify the sender ),you need to additional headers to the request and response .

### request

Add `credentials: 'include'` to the fetch options like below .This will include the cookie with the request.

```JavaScript
fetch('https://example.com', {
  mode: 'cors',
  credentials: 'include'
})
```

### response

`Access-Control-Allow-Origin` must be set to a specific origin (no wildcard using *) and must set `Access-Control-Allow-Credentials` to `true`.

```yaml
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://example.com
Access-Control-Allow-Credentials: true
```

## Preflight requests for complex HTTP  calls

If a web app needs a complex HTTP request , the browser adds a preflight request to the front of the request chain .

The CORS specification defines a **complex request** as

* A request the use methods other than GET , POST or HEAD
* A request that includes headers other than `Accept`, `Accept-Language` or `Content-Language`
* A request that has a `Content-Type` header other than `application/x-www-form-urlencoded`, `multipart-form-data` or `test/plain`

Browser create a preflight request if it is needed . It's an `OPTIONS` request like below and is sent before actual request message .

```yaml
OPTIONS /data HTTP/1.1
Origin: https://example.com
Access-Control-Request-Method: DELETE
```

On the server side, an application needs to response to the preflight request with information about the methods the application accepts from this origin .

```yaml
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://example.com
Access-Control-Allow-Methods: GET, DELETE, HEAD, OPTIONS
```

The server response can also include an `Access-Control-Max-Age` header to specify the duration (in seconds ) to cache preflight results so the client doesn't need to make a preflight request every time it sends complex request .

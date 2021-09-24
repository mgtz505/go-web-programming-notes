# go-web-programming-notes
Notes for Go Web Programming by Sau Sheong Chang

## 1.1. Using Go for web applications

Go is a relatively new programming language, with a thriving and growing community. It is well suited for writing server-side programs that are fast. It’s simple and familiar to most programmers who are used to procedural programming, but it also provides features of functional programming. It supports concurrency by default, has a modern packaging system, does garbage collection, and has an extensive and powerful set of built-in standard libraries.

Go provides a viable alternative to existing languages and platforms for developing large-scale web applications. Large-scale web applications typically need to be
- Scalable
Go scales well vertically with its excellent support for concurrent programming. A single Go web application with a single OS thread can be scheduled to run hundreds of thousands of goroutines with efficiency and performance. Go can scale well horizontally as well as by layering a proxy above a number of instances of a Go web app. Go web applications are compiled as static binaries, without any dynamic dependencies, and can be distributed to systems that don’t have Go built in. This allows you to deploy Go web applications easily and consistently.

- Modular

Although Go is statically typed, it has an interface mechanism that describes behavior and allows dynamic typing. Functions can take interfaces, which means you can introduce new code into the system and still be able to use existing functions by implementing methods required by that interface. Go implements a number of features usually associated with functional programming, including function types, functions as values, and closures. These features allow you to build more modular code by providing the capability of building functions out of other functions.

Go is also often used to create microservices. In microservice architecture large-scale applications can be created by composing smaller independent services. These services are interchangeable and organized around capabilities 


- Maintainable

Go was designed to encourage good software engineering practices. It has a clean and simple syntax that’s very readable. Go’s package system is flexible and unambiguous, and there’s a good set of tools to enhance the development experience and help programmers to write more readable code. 

Testing is built into Go. gotest discovers test cases built into the same package and runs functional and performance testing. Go also provides web application testing tools by emulating a web client and recording responses generated by the server.

- High-performance

High performance means being able to process a large volume of requests within a short period of time. It also means being able to respond to the client quickly and making operations faster for end users.

One of Go’s design goals is to approach the performance of C, and although it hasn’t reached this goal, the current results are quite competitive. Go compiles to native code, which generally means it’s faster than other interpreted languages and frameworks.

## 1.2 How Web Applications Work

In a purist and narrow sense, a web application is a computer program that responds to an HTTP request by a client and sends HTML back to the client in an HTTP response. But isn’t this what a web server is? From this definition, there is no difference between a web server and a web application. The web server is the web application.

An application is a software program that interacts with a user and helps the user to perform an activity. This includes accounting systems, human resource systems, desktop publication software, and so on. A web application is then an application that’s deployed and used through the web.

In other words, a program needs to fulfill only two criteria to be considered a web app:
- The program must return HTML to a calling client that renders HTML and displays to a user.
- The data must be transported to the client through HTTP.
- As an extension of this definition, if a program doesn’t render HTML to a user but instead returns data in any other format to another program, it is a web service (that is, it provides a service to other programs).

## 1.3 A Quick Intro to HTTP

HTTP is the application-level communications protocol that powers the World Wide Web. Everything that you see on a web page is transported through this seemingly simple text-based protocol.
**HTTP** is a stateless, text-based, request-response protocol that uses the client-server computing model.

Request-response is a basic way two computers talk to each other. The first computer sends a request to the second computer and the second computer responds to that request. A client-server computing model is one where the requester (the client) always initiates the conversation with the responder (the server). As the name suggests, the server provides a service to the client. 
- In HTTP, the client is also known as the user-agent and is often a web browser. The server is often called the web server.
- HTTP is a stateless protocol. Each request from the client to the server returns a response from the server to the client, and that’s all the protocol remembers. Subsequent requests to the same server have absolutely no idea what happened before. 
- HTTP sends and receives protocol-related data in plain text (as opposed to sending and receiving in binary), like many other internet-related protocols. The rationale behind this is to allow you to see what goes on with the communications without a specialized protocol analyzer, making troubleshooting a lot easier.

## 1.5 HTTP Request
HTTP is a request-response protocol, so everything starts with a request. The HTTP request, like any HTTP message, consists of a few lines of text in the following order:
1) Request-line 
2) Zero or more request headers
3) An Empty Line
4) Message Body (optional)

Example:
```
GET /Protocols/rfc2616/rfc2616.html HTTP/1.1 
Host: www.w3.org
User-Agent: Mozilla/5.0
(empty line)
```

Anatomy of the above example:
```
GET /Protocols/rfc2616/rfc2616.html HTTP/1.1
```
The first word in the request-line is the request method, followed by the Uniform Resource Identifier (URI) and the version of HTTP to be used. 
The next two lines are the request headers. Notice the last line is an empty line, which must exist even though there’s no message body. Whether the message body exists depends on the request method.

### 1.5.1 Request Methods
GET —Tells the server to return the specified resource.
HEAD —The same as GET except that the server must not return a message body. This method is often used to get the response headers without carrying the weight of the rest of the message body over the network.
POST —Tells the server that the data in the message body should be passed to the resource identified by the URI. What the server does with the message body is up to the server.
PUT —Tells the server that the data in the message body should be the resource at the given URI. If data already exists at the resource identified by the URI, that data is replaced. Otherwise, a new resource is created at the place where the URI is.
DELETE —Tells the server to remove the resource identified by the URI.
TRACE —Tells the server to return the request. This way, the client can see what the intermediate servers did to the request.
OPTIONS —Tells the server to return a list of HTTP methods that the server supports.
CONNECT —Tells the server to set up a network connection with the client. This method is used mostly for setting up SSL tunneling (to enable HTTPS).
PATCH —Tells the server that the data in the message body modifies the resource identified by the URI.

### 1.5.2 Safe Request Methods
A method is considered safe if it doesn’t change the state of the server—that is, the server provides only information and nothing else
- GET, HEAD, OPTIONS, and TRACE are safe methods because they aren’t supposed to change anything on the server.
- POST, PUT, and DELETE methods do change the state of the server; for example, after a POST request is sent, data at the server is supposed to be changed.

### 1.5.3 Idempotent Request Methods
A method is considered idempotent if the state of the server doesn’t change the second time the method is called with the same data. Safe methods by definition are considered idempotent as well 

PUT and DELETE are idempotent but not safe. This is because PUT and DELETE don’t change the state of the server the second time they’re called. PUT with the same resource will result in the same actions being taken by the server, because after the first request the resource at the URI is either already updated or created. DELETE with the same resource might result in an error by the server, but the state doesn’t change.

### 1.5.5. Request headers
Although the HTTP request method defines the action requested by the calling client, other information on the request or the client is often placed in HTTP request headers.
- Request headers are colon-separated name-value pairs in plain text, terminated by a carriage return (CR) and line feed (LF).
- HTTP request headers are mostly optional. The only mandatory header in HTTP 1.1 is the Host header field. 
- But if the message has a message body (which is optional, depending on the method), you’ll need to have either the Content-Length or the Transfer-Encoding header fields.

## 1.6. HTTP response
An HTTP response message is sent every time there’s a request. Like the HTTP request, the HTTP response consists of a few lines of plain text:

1) A status line
2) Zero or more response headers
3) An empty line
4) The message body (optional)

Example:
```
200 OK
Date: Sat, 22 Nov 2014 12:58:58 GMT
Server: Apache/2
  Last-Modified: Thu, 28 Aug 2014 21:01:33 GMT
Content-Length: 33115
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/
     TR/xhtml1/DTD/xhtml1-strict.dtd"> <html xmlns='http://www.w3.org/1999/
     xhtml'> <head><title>Hypertext Transfer Protocol -- HTTP/1.1</title></
     head><body>...</body></html>
```

 There are five classes of HTTP response status codes, depending on the first digit of the code
 
 - 1xx: 	Informational. This tells the client that the server has already received the request and is processing it.
 - 2xx:   Success. This is what clients want; the server has received the request and has processed it successfully. The standard response in this class is 200 OK.
 - 3xx:   Redirection. This tells the client that the request is received and processed but the client needs to do more to complete the action. Most of the status codes in this class are for URL redirection.
 - 4xx:   Client Error. This tells the client that there’s something wrong with the request. The most widely known status in this class is 404 Not Found, where the server tells the client that the resource it’s trying to get isn’t found at that URL.
 - 5xx:   Server Error. This tells the client that there’s something wrong with the request but it’s the server’s fault. The generic status code in this class is 500 Internal Server Error.

## 1.7 URI
The URI is an umbrella term that includes both the URN and the URI, and they have similar syntax and format. This book uses only URLs, so for all purposes, both the URI and URL can be used interchangeably.

## 1.9 Parts of a Web App
From the previous sections you’ve seen that a web application is a piece of program that does the following:

1)  Takes input through HTTP from the client in the form of an HTTP request message
2)  Processes the HTTP request message and performs necessary work
3)  Generates HTML and returns it in an HTTP response message

As a result, there are two distinct parts of a web app: the handlers and the template engine.
A **handler** receives and processes the HTTP request sent from the client. It also calls the template engine to generate the HTML and finally bundles data into the HTTP response to be sent back to the client.
- In the MVC pattern the handler is the controller, but also the model. In an ideal MVC pattern implementation, the controller would be thin, with only routing and HTTP message unpacking and packing logic. The models are fat, containing the application logic and data.

A **template** is code that can be converted into HTML that’s sent back to the client in an HTTP response message. Templates can be partly in HTML or not at all. A template engine generates the final HTML using templates and data. 

# HTTP

# What is HTTP

**HTTP** stands for **H**yper**t**ext **T**ransfer **P**rotocol. It is a protocol which specifies how your browser and web server should communicate. 

At its core, HTTP operates on a beautifully simple principle: one device asks for something (the client), and another device responds with that thing (the server). This request-response model is what makes the web work. When you type a URL into your browser or your app fetches data from an API, that's an HTTP request flying across the network. The server processes that request and sends back an HTTP response containing the data you asked for-or an error message if something went wrong.

```
Your Browser  →  "GET /users/123 HTTP/1.1 headers..."  →  Server
Your Browser  ←  "HTTP/1.1 200 OK headers... {data}"   ←  Server
```
## Understanding Statelessness
HTTP is a *stateless* protocol, which means that each leg of the request-and-response cycle is treated as a completely independent action. This means that the target server has no memory of the any other request 

# How HTTP Request work

when you send a request to a server it holds several parts:
1. **Request line**: POST /api/users HTTP/1.1
2. **Headers**: Host, Content-Type, Content-Length (soemtimes even more data)
3. **Blank line**: Required! 
4. **Body**: JSON data

example:
```

POST /api/users HTTP/1.1
Host: api.example.com
Content-Type: application/json
Content-Length: 27

{"name":"Alice","age":30}
```

when sending an HTTP request with python we send it usally with the python requests package from the python standard library (`import requests`) this will format your python code to what the target server is looking for

## breaking down the URL

while looking the example request you can create a basic url. (you can also create `GET` requests manully this way, see below)

the **Host** is the main address (or server name)
and the **endpoint** in the request line is the exact address or page where you are sending the request

the full URL will look something like this

```
http://api.example.com/api/users

<protocol>://<HOST>/<endpoint>
```

# HTTP Methods

There are may types of HTTP reaquest types. These types do a few things

- describe the way the data is passed from your code to the target server
- Shows what is the purpose of the request is

This can be broken down into the basic **CRUD** methods of API design

- **C**reate -> `POST` requests
- **R**ead -> `GET` requests
- **U**pdate -> `PUT` or `PATCH` request
- **D**elete/**D**estroy -> `DELETE` request

## The `GET` Method

The `GET` Method is used to **reteve data** from the target server.

when working with websites the information sent as a `GET` request can be viewed in the main URL (on the URL bar)

for example:
```
https://www.google.com/search?q=http+guide

<protocol>://<Host>/<endpoint>?<key>=<value>&<key2>=<value2>
```
The `?` is the seperator in the url to say we are passing extra information to find what we are looking for in the API. These are also called **Query perameters**

if you have more than one **Query perameters** you seperate each `key:value` pair with a `&`
### How webbrowsers work

Generally web browsers work by sending and HTTP `GET` request to the target webserver

and the response is the HTML page with the attached CSS and JS 

then the web browser translates the HTML/CSS/JS into the webpage that you are looking at

```
GET Request -> server(website) -> response (raw HTML/CSS/JS) -> renders the code
```

## The `POST` Method

`POST` methods are used to send data to the webserver can be used to update or create (more modern usage is only for creation of new data or records) 

## The `PATCH` Method

The `PATCH` Method is uesed to **Update** a resource.

## The `PUT` Method

The `PUT` Method is for **replacing** an resource

## The `DELETE` Method

The `DELETE` method is used for removing a resouce


# HTTP Request Headers
Headers are key-value pairs (like `dict`) that provide metadata.
example
```json
header_name:value 
```
all of this extra data tells the target server what and how to process the request.
## Common Headers
The following are the most common request headers that you will see.

There are many more depending on what the requesting server is expecting (most third-party APIs have clear documentation for what headers and content they expect for each request)

### `HOST` (Required)
Which server to route to
as a URL
```json
Host: api.example.com 
```
or an I.P. Address
```json
Host: 172.27.253.209
```

### Authorization

Authentication credentials 
```json
Authorization: Bearer eyJhbGc...
```
### Content-Type

Format of request body

```json
Content-Type: application/json

```
### Accept
What response formats you'll accept

```json
Accept: application/json
```

# HTTP Response 

when the target server respondes to a request it responds in the following format (similar to the requst)

1. Start/Status line
2. response headers
3. blank line (required to seperate the body from the headers)
4. Body 

```
HTTP/1.1 400 Bad Request 
Date: Sun, 18 Oct 2012 10:36:20 GMT 
Server: Apache/2.2.14 (Win32) 
Content-Length: 230 
Content-Type: text/html; 
charset=iso-8859-1 
Connection: Closed

<html>
    ...
</html>
```
The Response body can be many things depending on what the request was

for `GET` requests they often return data as a JSON (webpages return as HTML). sometimes you can also get many different data types (determend by the `Content-Type` header on the response object)

for all of the other request methods that you send they often return ether a JSON or a string defining the results of your request

sometimes the target server responds with a blank body and relies on the status code to inform the user
## HTTP response status codes

After you send and HTTP request the target surver (or website) sends a responce package that inclueds a code that is a short-hand for how the server responded to the request

each status code is a three didget number (hundred) where the first number denotes what is the main purpose of the message

- **100** -> Information 
- **200** -> success
- **300** -> redirect
- **400** -> Client side errors
- **500** -> server side errors

### common response codes
The following are common codes that you will see often

|Code|Meaning| long explination
|---|---|---|
|200| OK | standard success|
|201| Created| Resource created|
|204| No content| Success without body|
|301| Moved Permanently| Permanent redirect|
|304| Not Modified| Use cached resource
|400| Bad Request| Invalid input |
|401| Unauthorized | Auth required|
|403| Forbidden | No permission |
|404| Not Found | Resource missing |
|429| Too Many Requests| Rate limited|
|500| Internal Server Error| Server issue|
|502| Bad Gateway | Upstream issue|
|503| Service Unavailable| Temporary downtime|

# Summery 

- HTTP Methods are used to send data to another server
- HTTP Requests methods define how and what you are trying to do 
- HTTP Requests my need defined headers
- HTTP Responses return with a status code
- HTTP Responses return with a header that sends extra information
- The response body can be returned in many data types
- all HTTP responses have a code that informs the client about what happened to their request 


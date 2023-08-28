# An overview of HTTP

HTTP is a protocol (protocol) that allows the retrieval of resources such as HTML documents. It is the basis of any data exchange on the Web and is a client-server protocol, which means that requests are initiated by the recipient, usually a Web browser. A complete document is reconstructed from the different sub-documents obtained, as per example text, layout description, images, videos, scripts and much more.

Clients and servers communicate by exchanging individual messages (as opposed to a stream of data). Messages sent by the client, usually a web browser, are called requests, and messages sent by the server in response are called responses.

![image](https://github.com/DarlanNoetzold/computer_science/assets/41628589/6ebd5fe4-5477-4c68-9110-89721dd10980)

Designed in the early 1990s, the HTTP protocol is extensible and has evolved over time. It operates at the application layer and is sent over the TCP protocol, or over a TLS-encrypted TCP connection, although any reliable transport protocol could theoretically be used. Due to its extensibility, it is used not only to fetch hypertext documents, but also images and videos or to publish content on servers, such as in HTML form results. HTTP can also be used to fetch parts of documents to update web pages on demand.

## HTTP-based system components
HTTP is a client-server protocol: requests are sent by one entity, the user-agent (or a proxy on its behalf). Most of the time, the user-agent is a web browser, but it could be anything, such as a robot that crawls the web to populate and maintain a search engine index and gather information.

Each individual request is sent to a server, which will handle it and provide a result, called a response. Between the request and the response there are several entities, collectively referred to as proxies, which perform different operations and act as gateways (intermediaries) or caches, for example.

![image](https://github.com/DarlanNoetzold/computer_science/assets/41628589/e404f166-c949-4b90-9b10-d363ab3e9776)

In reality, there are many other computers between the browser and the server handling the request: there are routers, modems, and more. Thanks to the layered model of the Web, these functionalities are hidden in the network and transport layers, respectively. HTTP is at the top of the application layer. While diagnosing connectivity issues is important, the underlying layers are irrelevant to describing HTTP.

### Client: the user-agent
The user-agent is any tool that acts on behalf of the user. This function is predominantly performed by the web browser; a few exceptions are programs used by engineers and web developers to debug their applications.

The browser is always the entity initiating requests, never the server side (although some mechanisms have been added over the years to simulate server-initiated messages).

To display a Web page, the browser sends a request to fetch the page's HTML document. It then performs a parsing of this file, looking for additional requests corresponding to execution scripts, layout information (CSS) for presentation and sub-resources contained in the page (usually images and videos). Then the browser interprets these resources to show the user the complete page. There are scripts executed by the browser that fetch more resources in subsequent phases and as the page is used and the browser refreshes the page accordingly.

A Web page is a hypertext (HTML) document. This means that some parts of the displayed text are links (links to other pages or resources on the Web), which can be activated (usually by a mouse click) to fetch a new page, allowing the user to redirect their user-agent and navigate through the Internet. The browser translates these addresses into HTTP requests and then interprets the HTTP responses to show the user a transparent response.

### The web page server
On the other side of the communication channel is the server that serves the document requested by the user. A server presents itself virtually as just one machine: this is because the server can be a collection of servers sharing the load (through a technique called load balancing) or also as a complex program that accesses other servers (such as a cache, a server database, e-commerce servers (virtual stores), etc.), generating all or part of the requested document.

A server is not necessarily just one machine, but multiple servers can be hosted on the same machine. With HTTP/1.1 and the Host header, they can even share the same IP address.

### Proxies (or representatives)
Between the web browser and the server, various computers and machines transmit the HTTP messages. Due to the layered structure of the Web stack, most of these machines operate at one of the layers: transport, network or physical, being transparent in the HTTP application layer, and potentially having a big impact on performance. These machines operating at the application layer are commonly known as **proxies** (or proxies, etc.). They can be transparent or not (request changes do not pass through them), and they can perform several functions:

caching (the cache can be public or private, like the cache of browsers)
filtering (such as an antivirus scanner, access control, etc.)
load balancing (to allow multiple servers to respond to different requests)
authentication (to control who has access to resources)
authorization (to control who has access to certain information)
information logging (allows the storage of historical information)

## Basic aspects of HTTP
### HTTP is simple
Even with the more complexity introduced in HTTP/2.0 by encapsulating HTTP messages in frames, HTTP was designed to be simple and human-readable. HTTP messages can be read and understood by anyone, providing easier development and testing and reducing complexity for students.

### HTTP is extensible
Introduced in HTTP/1.0, HTTP headers make this protocol easy to extend and use for experiments. New functionality can even be introduced by simple agreement between a client and a server on the new semantics of a header.

### HTTP is stateless but has sessions
HTTP is stateless: there is no relationship between two requests being made over the same connection. This presents an immediate problem for users who interact with some pages coherently, for example using an e-commerce shopping cart*. But because the basic foundation of HTTP is statelessness, HTTP cookies allow sessions to be stateful. Using header extensibility, cookies are added to the HTTP stream, allowing the session creation in each HTTP request to share the same context, or the same state.

Note: * The e-commerce shopping cart problem and the HTTP protocol: as the HTTP protocol does not store the state of requests and responses, it is impossible to make a website store the information of a shopping cart only through HTTP . For example, imagine that you are going to buy a new computer and a set of tea cups. So that this data can be kept while you browse the e-commerce site looking at more products (each page visited generates a new request/response pair), two strategies can be used, since HTTP by itself would not allow this :

You have an e-commerce registration and a program written on the server is responsible for storing your cart information; or
A program written in client language (such as JavaScript) manages this information through cookies and databases that the browsers themselves make available to the applications, for the temporary storage of this cart information.
HTTP and connections
A connection is controlled at the transport layer, and therefore fundamentally outside the control of HTTP. However, HTTP does not require that the transport protocol used be connection-based, it only requires that it be reliable or not lose messages (without at least showing errors). Of the two most common transport protocols on the Internet, TCP is reliable and UDP is not. Therefore, HTTP uses the TCP standard, which is connection-based, even though a connection is not always required.

In the HTTP/1.0 protocol, a TCP connection was opened for each request/response pair exchanged, introducing two major flaws: opening a connection requires several round trips of messages, and is therefore slow, but it becomes more efficient when messages are sent. sent in greater numbers or more frequently: “warm connections” are more efficient than “cold connections” (which send few messages or with low frequency).

To work around these flaws, the HTTP/1.1 protocol introduced the concept of production lines (or pipelining) — which proved difficult to implement — and persistent connections: TCP connections made underneath can be partially controlled using the HTTP Connection header. HTTP/2.0 went one step further, multiplexing multiple messages across a single connection, helping to keep the connection warmer, and more efficient.

Experiments are underway to design a transport protocol better suited to HTTP. For example, Google is testing QUIC which is built on top of UDP to provide a more reliable and efficient transport protocol.

## What can be controlled by HTTP?
The extensible nature of HTTP has allowed more control and functionality for the internet over time. Caching and authentication have been supported features since the early history of HTTP. The ability to relax source constraints, in contrast, was added in the 2010s.

Here is a list of common features controllable with HTTP:

Caching The way documents are cached can be controlled by HTTP. The server can instruct proxies and clients what to cache and for how long. The client can instruct intermediate caching proxies to ignore the stored document.
Relaxation of origin restrictions To prevent snoopers and other privacy invaders, browsers strictly enforce the separation of Web sites. Only pages from the same origin can access all information on a Web page. Although this restriction is a great burden on servers, HTTP headers can relax this strict separation on the server side, allowing a document to be composed of multiple sources of information in other domains (and may even have specific security reasons for doing so), like a patchwork fabric.
Authentication Some pages can be secured so that only specific users can access it. Basic authentication can be provided over HTTP, either using the WWW-Authenticate header and the like, or by configuring a specific session using HTTP cookies.
Proxy and Tunneling (en-US) Servers and/or clients are often located on intranets and hide their true IP address from others. HTTP requests turn to proxies to bypass this network barrier. But not all proxies are HTTP proxies. The SOCKS protocol, for example, operates at a lower level. Other protocols such as ftp can be handled by these proxies.
Sessions Using HTTP cookies, allows you to link requests with server state. This creates the sessions, even though the basic HTTP protocol is stateless. This is useful not only for e-commerce shopping carts, but also for any site that allows user-level customization of responses.

## HTTP stream
When the client wants to communicate with a server, this being an end server or a proxy, it performs the following steps:

Opens a TCP connection: The TCP connection will be used to send a request, or several, and receive a response. The client can open a new connection, reuse an existing connection, or open multiple connections to servers.
Send an HTTP message: HTTP messages (before HTTP/2.0) are human-readable. With HTTP/2.0, these simple messages are encapsulated within frames, making them impossible to read directly, but the principle remains the same.GET / HTTP/1.1 Host: developer.mozilla.org Accept-Language: fr Copy to Clipboard
Server response reads:HTTP/1.1 200 OK Date: Sat, 09 Oct 2010 14:28:02 GMT Server: Apache Last-Modified: Tue, 01 Dec 2009 20:18:22 GMT ETag: "51142bc1-7449-479b075b2891b " Accept-Ranges: bytes Content-Length: 29769 Content-Type: text/html <!DOCTYPE html... (here comes the 29769 bytes of the requested web page) Copy to Clipboard
Closes or reuses the connection for future requests.
If the assembly line (pipelining) is enabled, several requests can be sent without the first response being fully received. The HTTP assembly line has proven difficult to implement in existing networks where older pieces of software coexist with modern versions. The HTTP assembly line has been replaced in HTTP/2.0 with more robust multiplexing of requests within a frame.

## HTTP messages
HTTP/1.1 and older HTTP messages are human-readable. In HTTP/2.0, these messages are embedded in a new binary structure, a frame, allowing for optimizations such as header compression and multiplexing. Even if only part of the original HTTP message is sent in this version of HTTP, the semantics of each message remain unchanged and the client (virtually) reconstitutes the original HTTP/1.1 request. It is therefore useful to understand HTTP/2.0 messages in the HTTP/1.1 version format.

There are two types of messages, requests and responses, each with its own format.

### Requests
Example of an HTTP request:
![image](https://github.com/DarlanNoetzold/computer_science/assets/41628589/acb8f3e2-fbe6-44cf-9f93-82853ae6689b)

Requests consist of the following elements:

An HTTP method is usually a verb like GET, POST, DELETE, PUT, etc., or a noun like OPTIONS or HEAD that defines what operation the client wants to do. Typically, a client wants to get a resource (using GET) or post data from an HTML form (using POST), although more operations may be needed in other cases.
The resource path to fetch; the URL of the resource without the context elements, for example without the protocol protocol (http://), the domain domain (here as developer.mozilla.org), or the TCP port (here indicated by 80 which is hidden because it is the default port number)
The HTTP protocol version.
Optional headers that contain additional information for servers.
Or a data body, for some methods like POST, similar to response bodies, which contains the requested resource.
Answers
### Example HTTP response:
![image](https://github.com/DarlanNoetzold/computer_science/assets/41628589/fcc16328-a2f0-44ec-a3c9-615d106e0c5e)

Responses consist of the following elements:

The version of the HTTP protocol they follow.
A status code, indicating whether or not the request was successful and why.
A status message, a short informal description about the status code.
HTTP headers, like those for requests.
Optionally, a body with data from the requested resource.

## HTTP based APIs
The most widely used API built on top of HTTP is XMLHttpRequest, which can be used to exchange data between a user agent (en-US) and a server.

Another API, server-sent events, is a one-way service that allows a server to send events to the client, using HTTP as a transport mechanism. Using the EventSource interface, the client opens a connection and establishes event handlers. The client browser automatically converts incoming messages over the HTTP stream into appropriate Event objects, handing them to event handlers that have been registered for the type event types if known, or to the onmessage (en-US) event handler if no type-specific event handlers have been defined.

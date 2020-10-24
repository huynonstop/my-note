https://www.cloudflare.com/learning/

https://developer.mozilla.org/en-US/docs/Glossary

https://academind.com/learn/web-dev/

# Network Layer

[https://www.electronicdesign.com/unused/article/21800810/whats-the-difference-between-the-osi-sevenlayer-network-model-and-tcpip#:~:text=The%20Internet%20Protocol%20layer%20is,%2C%206%2C%20and%207%20combined.](https://www.electronicdesign.com/unused/article/21800810/whats-the-difference-between-the-osi-sevenlayer-network-model-and-tcpip#:~:text=The Internet Protocol layer is,%2C 6%2C and 7 combined.)

https://www.techopedia.com/definition/6006/application-layer

https://www.techopedia.com/definition/8955/presentation-layer

https://www.techopedia.com/definition/9322/session-layer

https://www.techopedia.com/definition/9760/transport-layer

https://www.techopedia.com/definition/24204/network-layer

https://www.techopedia.com/definition/18698/data-link-layer

https://www.techopedia.com/definition/8866/physical-layer

https://www.totolink.vn/article/136-mo-hinh-osi-la-gi-chuc-nang-cua-cac-tang-giao-thuc-trong-mo-hinh-osi.html

| OSI Layer                         | Description                                                  |
| --------------------------------- | ------------------------------------------------------------ |
| Appl­ication Layer                | Approaches protocols for network applic­ations (web browsers or email programs) interaction with the network. Examples: HTTP, HTTPS, SMTP, FTP |
| Presentation Layer (Syntax Layer) | With a focus on end-user services, the application layer helps to facilitate process-to-process connections over Internet protocol. The presentation layer is responsible for the following:<br/>Data encryption/decryption<br/>Character/string conversion<br/>Data compression<br/>Graphic handling<br />The presentation layer is responsible for integrating all formats into a standard format for efficient and effective communication. |
| Session Layer                     | Controls the connections between multiple computers. The session layer tracks the dialogs between computers, which are also called sessions. This layer establishes, controls and ends the sessions between local and remote applications. |
| Tran­sport Layer                  | Be responsible for end-to-end communication over a network,deliver and receive data without errors. (TCP/UDP)<br />The send side breaks application messages into segments (packets) and passes them on to the network layer<br />The receiving side then reassembles segments into messages and passes them to the application layer |
| Network Layer                     | Addresses and packages data for transm­ission. Routes the packets across the network. (Internet Protocol) |
| Data Link Layer                   | The data link layer is used for the encoding, decoding and logical organization of data bits. Data packets are framed and addressed by this layer, which has two sublayers.(media access control - MAC, logical link control, ethernet, WIFI) |
| Physical Layer                    | This layer plays with most of the network’s physical connections—wireless transmission, cabling, cabling standards and types, connectors and types, network interface cards |

![Kết quả hình ảnh cho TCP/IP model](https://download.huawei.com/mdl/imgDownload?uuid=d6bd7de144f045e09d2e3a95e9667a14.png)

# Network Protocol

https://en.wikipedia.org/wiki/Lists_of_network_protocols

A **network protocol** is a set of rules/­con­ven­tions that dictate how a network operates.

| Protocol              | A family of protocols that dictate how devices on the same network segment format and transmit data. | Port   |
| --------------------- | ------------------------------------------------------------ | ------ |
| **Wi-Fi** or **WLAN** | A family of protocols that deal with wireless transm­ission. |        |
| **TCP**               | **T**r­ans­mission **C**o­ntrol **P**r­otocol: splits (and later reasse­mbles) data into packets. Also involves error checking, as expects an acknow­led­gement transm­ission within a set time frame. |        |
| **UDP**               | **U**ser **D**a­tagram **P**r­otocol: splits (and no reasse­mbles) data into packets. No error checking |        |
| **IP**                | **I**n­ternet **P**r­otocol: each device has an IP address. Packets are 'addre­ssed' to ensure they reach the correct user. |        |
| **HTTP**              | **H**y­pertext **T**r­ansfer **P**r­otocol: used to access a web-page from a web server. | 80     |
| **HTTPS**             | **H**y­pertext **T**r­ansfer **P**r­otocol **S**e­cure: uses encryption to protect data. | 443    |
| **FTP**               | **F**ile **T**r­ansfer **P**r­otocol: handles file uploads and downloads, transfers data and programs. | 20, 21 |
| **SMTP**              | **S**imple **M**ail **T**r­ansfer **P**r­otocol: handles outbound email. SMTP servers have databases of user's email addresses. | 25     |
| **IMAP**              | **I**n­ternet **M**e­ssage **A**ccess **P**r­otocol: handles inbound emails. |        |
| **SSH**               | **Secured Shell (SSH)** protocol uses encryption to secure the connection between a client and a server. All user authentication, commands, output, and file transfers are encrypted to protect against attacks in the network. | 22     |
| DNS                   | Domain Name System                                           | 53     |
| DHCP                  | Dynamic Host Configuration Protocol                          | 67, 68 |

# Internet

The internet is the wider network that allows computer networks around the world run by companies, governments, universities and other organizations to talk to one another.

The Internet works through a packet routing network in accordance with the *Internet Protocol (IP)*, the *Transport Control Protocol* (TCP) and other protocols.

# Resource 

A **web resource** is anything that can be obtained from the World Wide Web (web pages, e-mails, data, ...)

# What is DNS?

The Domain Name System (DNS) is the phonebook of the Internet.

key/value pair of domain and IP

## Domain name (host name)

A domain name is your website name.

It’s what comes after “@” in an email address, or after “www.” in a web address

google.com
wikipedia.org
youtube.com

Anyone can purchase a domain name by going to a domain host or registrar

## Domain registrar

A domain registrar is a company that sells domain names that aren't currently owned and are available for you to register. A simple search on a domain registrar’s page should tell you if the domain name you want is available and how much it will cost. Most of these companies also offer domain hosting.

## DNS host

A DNS host is a company that manages your domain's configuration (also known as DNS resource records) that makes sure your domain name points to your website and email. Most domain hosts also offer domain name registration.

# URL

https://developer.mozilla.org/en-US/docs/Glossary/URL

**Uniform Resource Locator** (**URL**) is a text string that specifies where a resource (such as a web page, image, or video) can be found on the Internet.

While the domain is the name of the website, a URL will lead to any one of the pages within the website. Every URL contains a protocol, a domain name, as well as other components needed to locate the specific page or piece of content.

`http://www.google.com`

`https://en.wikipedia.org/wiki/umami`

`https://www.youtube.com/feed/trending`

`https://devqa.io/index.html`

# Origin

An origin is defined as a combination of URI scheme, hostname, and port number

# Proxy Server

https://www.varonis.com/blog/what-is-a-proxy-server/

A proxy server acts as a gateway between you and the internet. It’s an intermediary server separating end users from the websites they browse. Proxy servers provide varying levels of functionality, security, and privacy depending on your use case, needs, or company policy.

# Client/server model in web development

[https://en.wikipedia.org/wiki/Client%E2%80%93server_model](https://en.wikipedia.org/wiki/Client–server_model)

https://www.geeksforgeeks.org/client-server-model/

https://developer.mozilla.org/en-US/docs/Learn/Server-side/First_steps/Client-Server_overview

https://medium.com/@subhangdxt/beginners-guide-to-client-server-communication-8099cf0ac3af

The Client-server model is a distributed application structure that partitions task or workload between the providers of a resource or service, called servers, and service requesters called clients.

its basically the **Client** requesting something and the **Server** serving it as long as its present in the database.

![img](https://media.geeksforgeeks.org/wp-content/uploads/20191016114416/801.png)

## How it's work

- User enters the **URL**(Uniform Resource Locator) of the website or file. The Browser then requests the **DNS**(DOMAIN NAME SYSTEM) Server.
- **DNS Server** lookup for the address of the **WEB Server**.
- **DNS Server** responds with the **IP address** of the **WEB Server**.
- Browser sends over an **HTTP/HTTPS** request to **WEB Server’s IP** (provided by **DNS server**).
- Server sends over the necessary files of the website.
- Browser then renders the files and the website is displayed. This rendering is done with the help of **DOM** (Document Object Model) interpreter, **CSS** interpreter and **JS Engine** collectively known as the **JIT** or (Just in Time) Compilers.

![img](https://media.geeksforgeeks.org/wp-content/uploads/20191016120927/8110.png)

**Advantages of Client-Server model:**

- Centralized system with all data in a single place.
- Cost efficient requires less maintenance cost and Data recovery is possible.
- The capacity of the Client and Servers can be changed separately.

**Disadvantages of Client-Server model:**

- Clients are prone to viruses, Trojans and worms if present in the Server or uploaded into the Server.
- Server are prone to Denial of Service (DOS) attacks.
- Data packets may be spoofed or modified during transmission.
- Phishing or capturing login credentials or other useful information of the user are common and MITM(Man in the Middle) attacks are common.

# Website

A website is a collection of content, often on multiple pages, that is grouped together under the same domain. You can think of it like a store, where the domain is the store name, the URL is the store address, and the website is the actual store, with shelves full of products and a cash register.

A website, need a few things to be accessed:

- A domain name (such as yoursite.com)
- A domain registrar and host (such as [Google Domains](https://domains.google/))
- A do-it-yourself website
- Digital content — the text, images, videos, and other media that visitors will see when they come to your site.

## Web hosting

Web hosting is the service of providing storage space for a website or application on a server on the internet. Once your website is made available on the internet, it can be accessed by other computers connected to the internet.

![img](https://qph.fs.quoracdn.net/main-qimg-3350f84445b9e9e6fe8381f2c7c3e895)

**Shared Hosting **(shared with the orther) < **VPS hosting** (shard the server but not the resources ) < **Dedicated Hosting** (full-control)

**Cloud Hosting**

## How the web work

https://www.youtube.com/watch?v=hJHvdBlSxug

![image-20200521225002397](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200521225002397.png)

# HTTP/HTTPS

## **HTTP** 

https://developer.mozilla.org/en-US/docs/Web/HTTP

https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP

https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview

Stands for **H**yper **T**ext **T**ransfer **P**rotocol  (port 80)

It is the foundation of any data exchange on the Web and it is a client-server protocol, which means requests are initiated by the recipient, usually the Web browser. A complete document is reconstructed from the different sub-documents fetched, for instance text, layout description, images, videos, scripts, and more.

It is a communications protocol and is used to send and receive web pages and files over the internet by sending **HTTP Requests** and receiving **HTTP Responses**

1. A client (a browser) sends an **HTTP request** to the server
2. An web server receives the request
3. The server runs an application to process the request
4. The server returns an **HTTP response** (output) to the browser
5. The client (the browser) receives the response

## Method

- GET: read resource
- HEAD: read resource header
- POST: create resource
- PUT: update (replace) resource with full payload (full data of the resource ).
- PATCH: update (modify) resource with part payload (data which you want to update)
- DELETE: delete resource.
- CONNECT
- OPTIONS
- TRACE

## Header

https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers

HTTP headers let the client and the server pass additional information with an HTTP request or response such as the type of Browser, type of content in the message, and what type of response is accepted in return.

Headers can be grouped according to their contexts:

- [General headers](https://developer.mozilla.org/en-US/docs/Glossary/General_header) apply to both requests and responses, but with no relation to the data transmitted in the body.
- [Request headers](https://developer.mozilla.org/en-US/docs/Glossary/Request_header) contain more information about the resource to be fetched, or about the client requesting the resource.
- [Response headers](https://developer.mozilla.org/en-US/docs/Glossary/Response_header) hold additional information about the response, like its location or about the server providing it.
- [Entity headers](https://developer.mozilla.org/en-US/docs/Glossary/Entity_header) contain information about the body of the resource, like its [content length](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Length) or [MIME type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types). 

## Messages

Clients and servers communicate by exchanging individual messages (as opposed to a stream of data). The messages sent by the client, usually a Web browser, are called *requests* and the messages sent by the server as an answer are called *responses*.

HTTP requests, and responses, share similar structure and are composed of:

1. A *start-line* describing the requests to be implemented, or its status of whether successful or a failure. This start-line is always a single line.

2. An optional set of *HTTP headers* specifying the request, or describing the body included in the message.

3. A blank line indicating all meta-information for the request has been sent.

4. An optional *body* containing data associated with the request (like content of an HTML form), or the document associated with a response. The presence of the body and its size is specified by the start-line and HTTP headers.

   <img src="https://mdn.mozillademos.org/files/13827/HTTPMsgStructure2.png" alt="Requests and responses share a common structure in HTTP" style="zoom:80%;background-color:white" />

### Request

messages sent by the client to initiate an action on the server

<img src="https://mdn.mozillademos.org/files/13821/HTTP_Request_Headers2.png" alt="Example of headers in an HTTP request" style="zoom:100%; background-color: white" />

#### start-line 

- HTTP method
-  *request target*, usually a URL
- The *HTTP version*

GET /background.png HTTP/1.0
HEAD /test.html?query=alibaba HTTP/1.1

#### Headers

- General headers
- *Request headers*, like `User-Agent`, `Accept-Type`, 
- *Entity headers*, like `Content-Type`

#### Body

Not all requests have one: `GET`, `HEAD`, `DELETE`, or `OPTIONS`, usually don't need one.

Bodies can be broadly divided into two categories:

- Single-resource bodies, consisting of one single file, defined by the two headers: `Content-Type` and `Content-Length`
- Multiple-resource bodies, consisting of a multipart body, each containing a different bit of information.

### Response

https://developer.mozilla.org/en-US/docs/Web/HTTP/Status

*responses*, the answer from the server

<img src="https://mdn.mozillademos.org/files/13823/HTTP_Response_Headers2.png" alt="Example of headers in an HTTP response" style="zoom:100%; background-color: white" />

#### Status line

The start line of an HTTP response

1. The *protocol version*, usually `HTTP/1.1`.
2. A *status code*
3. A *status text*. A brief, purely informational, textual description of the status code to help a human understand the HTTP message.

`HTTP/1.1 404 Not Found`

#### Headers

- *General headers*
- *Response headers*
- *Entity headers*

#### Body

Not all responses have one: responses with a status code that sufficiently answers the request without the need for corresponding payload (like 201 **`Created`** or 204 **`No Content`**) usually don't.

## HTTP are Insecured

HTTP’s not encrypted

When information is sent over regular HTTP, the information is broken into packets of data that can be easily “sniffed” using free software. This makes communication over the an unsecure medium, such as public Wi-Fi, highly vulnerable to interception. In fact, all communications that occur over HTTP occur in plain text, making them highly accessible to anyone with the correct tools, and vulnerable to man-in the-middle attacks.

It is possible for Internet service providers (ISPs) or other intermediaries to inject content into webpages without the approval of the website owner.

> This commonly takes the form of advertising, where an ISP looking to increase revenue injects paid advertising into the webpages of their customers. Unsurprisingly, when this occurs, the profits for the advertisements and the quality control of those advertisements are in no way shared with the website owner.

## HTTPS 

https://kipalog.com/posts/https-hoat-dong-nhu-the-nao

HTTP +  Secure Sockets Layer (port 443)

HTTPS uses HTTP and an encryption protocol (TLS/SSL) to encrypt communications. 

<img src="https://s3-ap-southeast-1.amazonaws.com/kipalog.com/WRNrD.png_q1sk0vahes" alt="img" style="zoom:200%;" />

![img](https://cache.digistar.vn/wp-content/uploads/2017/10/img_ssl_how_it_works_1-1.jpg?x90642)

Và tất nhiên, các session key sẽ được tạo ra ngẫu nhiên và khác nhau trong mỗi phiên làm việc với server. Ngoài encryption thì cơ chế hashing sẽ được sử dụng để đảm bảo tính Integrity cho các thông điệp được trao đổi.

### TLS (transport layer security)

### SSL (secure socket layer)

https://www.cloudflare.com/learning/ssl/what-is-ssl/

https://viblo.asia/p/https-va-ssl-OeVKBg4AZkW

This protocol secures communications by using what’s known as an asymmetric public key infrastructure. This type of security system uses two different keys to encrypt communications between two parties:

1. The private key - this key is controlled by the owner of a website and it’s kept, as the reader may have speculated, private. This key lives on a web server and is used to decrypt information encrypted by the public key.
2. The public key - this key is available to everyone who wants to interact with the server in a way that’s secure. Information that’s encrypted by the public key can only be decrypted by the private key.

### SSL certificate

https://www.cloudflare.com/learning/ssl/what-is-an-ssl-certificate/

### Certificate Authority

## Why is HTTPS important

HTTPS prevents websites from having their information broadcast in a way that’s easily viewed by anyone snooping on the network

With HTTPS, traffic is encrypted such that even if the packets are sniffed or otherwise intercepted, they will come across as nonsensical characters.

Before encryption

```http
This is a string of text that is completely readable
```

After encryption

```http
ITM0IRyiEhVpa6VnKyExMiEgNveroyWBPlgGyfkflYjDaaFf/Kn3bo3OfghBPDWo6AfSHlNtL8N7ITEwIXc1gU5X73xMsJormzzXlwOyrCs+
```

HTTPS eliminates the ability of unmoderated third parties to inject advertising into web content.

## CORS

Cross-Origin Resource Sharing ([CORS](https://developer.mozilla.org/en-US/docs/Glossary/CORS)) is a mechanism that uses additional [HTTP](https://developer.mozilla.org/en-US/docs/Glossary/HTTP) headers to tell browsers to give a web application running at one [origin,](https://developer.mozilla.org/en-US/docs/Glossary/Origin) access to selected resources from a different origin. 

A web application executes a **cross-origin HTTP request** when it requests a resource that has a different origin (domain, protocol, or port) from its own.

![image-20200611213702513](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611213702513.png)

=> You have to tell the browser Cross-Origin Resource Sharing is OK, set a header in the **backend** the to signal the **browser** that this access is fine, that we allow this access - a middleware to handle cors header

![image-20200611214236251](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611214236251.png)

[More](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

#  Same-origin policy

The **same-origin policy** is a critical security mechanism that restricts how a document or script loaded from one origin  can interact with a resource from another origin. It helps isolate potentially malicious documents, reducing possible attack vectors.

# Content Security Policy (CSP)

CSP allow you to define where browser can download resource (script,image,font,css,...) from and check hash of them

# JSON

JSON stands for JavaScript Object Notation and is a text representation that is also valid JavaScript code.

# API

 [API.md](API.md) 

# Web service

Provide raw data for applications (JSON,...) , these data will be used to display for end user by front-end apps

Applications which are accessed via HTTP APIs are often called Web Services. In other words, a web service is a function that can be accessed by other programs over the web (HTTP).

# Web hooks

https://rapidapi.com/blog/api-glossary/webhooks/

# Client-side Rendering And Server-side Rendering

![img](https://images.viblo.asia/df2bcc4e-5dec-4469-8e0b-a71e31d7152f.png)

# Redis

Redis is an in-memory key-value NoSQL database (with disk-persistence) primarily used as a cache system

With redis, the data is (ideally) all in memory, making the lookups much faster. You don't get the same functionality (relationships, complex queries, foreign keys, etc.) but you get speed.

strings, hashes, lists, sets, sorted

# Framework vs library

![image-20200611174849202](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611174849202.png)

# Framework Front-end

VueJS, Angular, React

https://academind.com/learn/angular/angular-vs-react-vs-vue-my-thoughts/

![image-20200611175234911](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611175234911.png)

- Helping us interact with a big a DOM Trees easily (may have thousands of nodes)
- Helping us searching and update the real DOM efficiently with the **virtual DOM**

## virtual DOM

A way of representing the actual DOM with Javascript Objects

![image-20200626221721144](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200626221721144.png)

virtual DOM is the "blueprint"

actual DOM is the "building"

![image-20200626222105080](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200626222105080.png)

Change the DOM (repaint element to the browser, repaint CSS, ...) is the heaviest and slowest work and you may want to do that only when needed and not update the DOM nodes or everything in the real DOM

![image-20200626221905921](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200626221905921.png)

# Flux

https://facebook.github.io/flux/

Action => dispatcher => store => View => Action

![img](https://qph.fs.quoracdn.net/main-qimg-ef494b3986df9d9f624cfd6230fc632f)

# MVC

https://towardsdatascience.com/10-common-software-architectural-patterns-in-a-nutshell-a0b47a1e9013

https://www.geeksforgeeks.org/model-view-controllermvc-architecture-for-node-applications/?ref=rp

https://developer.mozilla.org/en-US/docs/Glossary/MVC

In MVC pattern, application and its development are divided into three interconnected parts

- Model is data part.
- View is User Interface part.
- Controller is request-response handler.

<img src="https://mdn.mozillademos.org/files/16042/model-view-controller-light-blue.png" alt="Diagram to show the different parts of the mvc architecture." style="zoom:100%;background-color: white;" />

# MVVM

https://www.geeksforgeeks.org/introduction-to-model-view-view-model-mvvm/?ref=rp

https://www.wintellect.com/model-view-viewmodel-mvvm-explained/

https://stackoverflow.com/questions/667781/what-is-the-difference-between-mvc-and-mvvm

https://docs.microsoft.com/en-us/xamarin/xamarin-forms/enterprise-application-patterns/mvvm

MODEL: ( Reusable Code – DATA ) Business Objects that encapsulate data and behavior of application domain, Simply holds the data.
VIEW**:** ( Platform Specific Code – USER INTERFACE ) What the user sees, The Formatted data.
VIEWMODEL: ( Reusable Code – LOGIC ) Link bewteen Model and View OR It Retrieves data from Model and exposes it to the View (using Observer design pattern to binding, 1 way or 2 ways ). This is the model specifically designed for the View.

![img](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20190530163655/Capture5.png)

![img](https://images.viblo.asia/52a8cf0b-b5fb-457b-967f-ce406975b2d0.png)

# Single thread

One command at a time

# Multi thread

Many command in parallel

# Synchronous

Run one at a time and in order

# Asynchronous

Run not  in order

# Polyfill

Code add a newer feature of Javascript which the older engine version don't support

# Syntax sugar

![image-20200612212716624](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612212716624.png)

# Service worker

![image-20200623144206679](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200623144206679.png)

# Tracking pixel url

https://en.ryte.com/wiki/Tracking_Pixel

https://medium.com/analytics-and-data/cookies-tracking-and-pixels-where-does-your-web-data-comes-from-ff5d9b8bc8f7

https://www.quora.com/Retargeting-What-is-the-difference-between-a-tracking-Pixel-and-a-cookie-are-they-in-essence-the-same-thing
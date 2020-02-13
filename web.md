# Layer

| **Appl­ication Layer** | Where the network applic­ations, such as web browsers or email programs, operate. Examples: HTTP, HTTPS |
| ---------------------- | ------------------------------------------------------------ |
| **Tran­sport Layer**   | Sets up the commun­ication between the two hosts and they agree settings such as ‘language’ and size of packets. |
| **Network Layer**      | Addresses and packages data for transm­ission. Routes the packets across the network. |
| **Data Link Layer**    | This is where the network hardware such as the NIC (network interface card) is located. OS device drivers also sit here. |

![Kết quả hình ảnh cho TCP/IP model](https://download.huawei.com/mdl/imgDownload?uuid=d6bd7de144f045e09d2e3a95e9667a14.png)

# Network Protocol (giao thức)

A **network protocol** is a set of rules/­con­ven­tions that dictate how a network operates.

| **Ethernet**          | A family of protocols that dictate how devices on the same network segment format and transmit data. |
| --------------------- | ------------------------------------------------------------ |
| **Wi-Fi** or **WLAN** | A family of protocols that deal with wireless transm­ission. |
| **TCP**               | **T**r­ans­mission **C**o­ntrol **P**r­otocol: splits (and later reasse­mbles) data into packets. Also involves error checking, as expects an acknow­led­gement transm­ission within a set time frame. |
| **UDP**               | **U**ser **D**a­tagram **P**r­otocol: splits (and no reasse­mbles) data into packets. No error checking |
| **IP**                | **I**n­ternet **P**r­otocol: each device has an IP address. Packets are 'addre­ssed' to ensure they reach the correct user. |
| **HTTP**              | **H**y­pertext **T**r­ansfer **P**r­otocol: used to access a web-page from a web server. |
| **HTTPS**             | **H**y­pertext **T**r­ansfer **P**r­otocol **S**e­cure: uses encryption to protect data. |
| **FTP**               | **F**ile **T**r­ansfer **P**r­otocol: handles file uploads and downloads, transfers data and programs. |
| **SMTP**              | **S**imple **M**ail **T**r­ansfer **P**r­otocol: handles outbound email. SMTP servers have databases of user's email addresses. |
| **IMAP**              | **I**n­ternet **M**e­ssage **A**ccess **P**r­otocol: handles inbound emails. |

# Internet

The internet is the wider network that allows computer networks around the world run by companies, governments, universities and other organizations to talk to one another.

The Internet works through a packet routing network in accordance with the *Internet Protocol (IP)*, the *Transport Control Protocol* (TCP) and other protocols.

# What is DNS?

The Domain Name System (DNS) is the phonebook of the Internet.

Domain =>(DNS) IP

# Domain name

A domain name is your website name. A domain name is the address where Internet users can access your website

# Origin

An origin is defined as a combination of URI scheme, hostname, and port number

# Web hosting

Web hosting (in layman’s terms) is the service of providing storage space for a website or application on a server on the internet. Once your website is made available on the internet, it can be accessed by other computers connected to the internet.![img](https://qph.fs.quoracdn.net/main-qimg-3350f84445b9e9e6fe8381f2c7c3e895)

**Shared Hosting **(shared with the orther) < **VPS hosting** (shard the server but not the resources ) < **Dedicated Hosting** (full-control)

**WordPress Hosting**

**Cloud Hosting**

# SSH

SSH, hoặc được gọi là Secure Shell, là một giao thức điều khiển từ xa cho phép người dùng kiểm soát và chỉnh sửa server từ xa qua Internet

# HTTP/HTTPS

## **HTTP**  stands for **H**yper **T**ext **T**ransfer **P**rotocol 

Communication between client computers and web servers is done by sending **HTTP Requests** and receiving **HTTP Responses**

1. A client (a browser) sends an **HTTP request** to the web

2. An web server receives the request

3. The server runs an application to process the request

4. The server returns an **HTTP response** (output) to the browser

5. The client (the browser) receives the response

   **Traffic monitoring** : 'nghe lén' thông tin

## HTTPS = HTTP + encrypted (TLS-tranport layer security/SSL-secure socket layer)

1. Client gửi yêu cầu (request) cho một trang bảo mật (secure page) (có URL bắt đầu với https://)

2. Server gửi lại cho client certificate của nó.

3. Client (trình duyệt web) tiến hành xác thực certificate này bằng cách kiểm tra (verify) tính hợp lệ của chữ ký số của CA được kèm theo certificate.

Giả sử certificate đã được xác thực và còn hạn sử dụng hoặc client vẫn cố tình truy cập mặc dù Web browser đã cảnh báo rằng không thể tin cậy được certificate này (do là dạng self-signed SSL certificate hoặc certificate hết hiệu lực, thông tin trong certificate không đúng) thì mới xảy ra bước 4 sau.

4. Client tự tạo ra ngẫu nhiên một symmetric encryption key (hay session key), rồi sử dụng public key (lấy trong certificate) để mã hóa session key này và gửi về cho server.

5. Server sử dụng private key (tương ứng với public key trong certificate ở trên) để giải mã ra session key ở trên.

6. Sau đó, cả server và client đều sử dụng session key đó để mã hóa/giải mã các thông điệp trong suốt phiên truyền thông.

Và tất nhiên, các session key sẽ được tạo ra ngẫu nhiên và khác nhau trong mỗi phiên làm việc với server. Ngoài encryption thì cơ chế hashing sẽ được sử dụng để đảm bảo tính Integrity cho các thông điệp được trao đổi.

# Browser

# ![img](https://miro.medium.com/max/499/1*RL0pnuf_hmLJ76oY6DViZw.png)

**Javascript Interpreter** = **JavaScript Engine**

## Rendering Engine

- Chrome and Safari uses WebKit
- Firefox uses Gecko
- Internet Explorer uses Trident
- Edge uses EdgeHTML

![img](https://miro.medium.com/max/600/1*cfQpu6Xvb7e9IiH4CCuiCg.png)

![img](https://viblo.asia/uploads/d6710955-9dc3-4a28-944d-069c0dac03c0.png)

## Storage/Data Persistenace

localStorage, sessionStorage, and cookies are all **client storage** solutions. Session data is held on the server where it remains under your direct control. (It's unique per `protocol://host:port`(**Origin**) combination)

 They are only able to store values as strings.

+ Cookies

  An HTTP cookie (web cookie, browser cookie) is a small piece of data that a server sends to the user's web browser. The browser may store it and send it back with the next request to the same server.

  - Stores data that has to be sent back to the server with subsequent requests. Its expiration varies based on the type and the **expiration duration** can be set from either server-side or client-side (normally from server-side).
  - Cookies can be read on server-side and client-side.
  - Size must be less than 4KB.
  - Cookies can be made secure by setting the httpOnly flag as true for that cookie. This prevents client-side access to that cookie

  Cookies are mainly used for three purposes:

  - Session management: Logins, shopping carts, game scores, or anything else the server should remember

  - Personalization: User preferences, themes, and other settings

  - Tracking: Recording and analyzing user behavior

    **Cross-Site Request Forgery** (XSRF).

    **Third-party cookies**

+ SessionStorage (1 Tab)

  + The sessionStorage object stores data only for a session, meaning that the data is stored until the browser (or tab) is closed.
  + Data is never transferred to the server.
  + Storage limit is larger than a cookie (at least 5MB).
  + SessionStorage can only be read on client-side

+ LocalStorage (Browser)

  - Stores data with no expiration date, and gets cleared only through JavaScript, or clearing the Browser cache / Locally Stored Data
  - Storage limit is the maximum amongst the three
  - Data is never transferred to the server.
  - LocalStoragecan only be read on client-side

# Client-side Rendering And Server-side Rendering

![img](https://images.viblo.asia/df2bcc4e-5dec-4469-8e0b-a71e31d7152f.png)

 SSR means there is no need for loaders or spinners for the initial load.

# CORS

Cross-Origin Resource Sharing ([CORS](https://developer.mozilla.org/en-US/docs/Glossary/CORS)) is a mechanism that uses additional [HTTP](https://developer.mozilla.org/en-US/docs/Glossary/HTTP) headers to tell browsers to give a web application running at one [origin,](https://developer.mozilla.org/en-US/docs/Glossary/Origin) access to selected resources from a different origin. A web application executes a **cross-origin HTTP request** when it requests a resource that has a different origin (domain, protocol, or port) from its own.

#  **Same-origin policy**

The **same-origin policy** is a critical security mechanism that restricts how a document or script loaded from one origin  can interact with a resource from another origin. It helps isolate potentially malicious documents, reducing possible attack vectors.

# Content Security Policy (CSP)

CSP cho bạn ngôn ngữ để chỉ ra những nơi mà trình duyệt được phép tải tài nguyên về. Bạn có thể định nghĩa một danh sách các nguồn scripts, ảnh, font chữ, css cho từng cái một, cũng như kiểm tra hash của các tài nguyên đã được tải đó với chữ kí có sẵn.

# REST

REST (**RE**presentational **S**tate **T**ransfer) is basically an architectural style of development having some principles...

- Uniform interface
- Client–server
- Stateless
- Cacheable
- Layered system
- Code on demand (optional)

**REST based services** follow some of the above principles and not all

**RESTFUL services** means it follows all the above principles.

# DOM

*DOM is “Document Object Model”. It’s the browsers’ programming interface for HTML (and XML) documents that treats them as tree structures. The DOM API can be used to change a document structure, style, and content.*![img](https://miro.medium.com/max/753/1*ZrzXoRljG5Co5KvEsWJNjA.png)
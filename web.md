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

# Client-side Rendering And Server-side Rendering

![img](https://images.viblo.asia/df2bcc4e-5dec-4469-8e0b-a71e31d7152f.png)

 SSR means there is no need for loaders or spinners for the initial load.

# CORS

Cross-Origin Resource Sharing ([CORS](https://developer.mozilla.org/en-US/docs/Glossary/CORS)) is a mechanism that uses additional [HTTP](https://developer.mozilla.org/en-US/docs/Glossary/HTTP) headers to tell browsers to give a web application running at one [origin,](https://developer.mozilla.org/en-US/docs/Glossary/Origin) access to selected resources from a different origin. A web application executes a **cross-origin HTTP request** when it requests a resource that has a different origin (domain, protocol, or port) from its own.

#  **Same-origin policy**

The **same-origin policy** is a critical security mechanism that restricts how a document or script loaded from one origin  can interact with a resource from another origin. It helps isolate potentially malicious documents, reducing possible attack vectors.

# Content Security Policy (CSP)

CSP cho bạn ngôn ngữ để chỉ ra những nơi mà trình duyệt được phép tải tài nguyên về. Bạn có thể định nghĩa một danh sách các nguồn scripts, ảnh, font chữ, css cho từng cái một, cũng như kiểm tra hash của các tài nguyên đã được tải đó với chữ kí có sẵn.

# HTTP request method

Có tất cả 9 loại request, GET và POST là 2 loại thông dụng được sử dụng nhiều hiện này:

- **GET**: được sử dụng để lấy thông tin từ server theo URI đã cung cấp.
- **HEAD**: giống với GET nhưng response trả về không có body, chỉ có header.
- **POST**: gửi thông tin tới server thông qua các biểu mẫu HTTP.
- **PUT**: ghi đè tất cả thông tin của đối tượng với những gì được gửi lên.
- **PATCH**: ghi đè các thông tin được thay đổi của đối tượng.
- **DELETE**: xóa tài nguyên trên server.
- **CONNECT**: thiết lập một kết nối tới server theo URI.
- **OPTIONS**: mô tả các tùy chọn giao tiếp cho resource.
- **TRACE**: thực hiện một bài test loop – back theo đường dẫn đến resource.

# Web service

Trong khi đó **Web Service** là một dịch vụ web, nó cung cấp các thông tin thô, và khó hiểu với đa số người dùng và vì vậy nó được sử dụng bởi các ứng dụng (Web front end, app di động). Các ứng dụng này sẽ chế biến các dữ liệu thô trước khi trả về cho bạn (*người dùng cuối*).

# Resource 

Và quản lý một resource của một website bao gồm 4 tác vụ chính:

- Thêm một resource. (Ta dùng POST)
- Lấy thông tin một resource. (Ta dùng GET)
- Cập nhật một resource. (Ta dùng PUT hay PATCH)
- Xoá một resource. (Ta dùng DELETE)

# How does webpage work ?

![image-20200521225002397](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200521225002397.png)

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

# Redis

Redis is an in-memory key-value NoSQL database (with disk-persistance) primarily used as a cache system

With redis, the data is (ideally) all in memory, making the lookups much faster. You don't get the same functionality (relationships, complex queries, foreign keys, etc.) but you get speed.

strings, hashes, lists, sets, sorted

Redis có những đặc điểm nổi bật như:

- Cập nhật thêm mới và xóa các dữ liệu trên redis rất nhanh chóng vì nó hỗ trợ nhiều command tiện ích.
- Redis lấy và save dữ liệu trên RAM nhưng tại một thời điểm thì dữ liệu được lưu trữ vào file lưu trên disk.
- Redis lưu dữ liệu dưới dạng `Key-Value` kiểu dữ liệu của `Key` nhưng kiểu dữ liệu của `Value` thì đa dạng hơn có thể là: `List, Set, Sorted Set, Hash...`
- Có hỗ trợ multiple database với nhiều commands có thể tự động remove `key-value` từ một database tới một database khác.
- Redis hỗ trợ mở rộng `master-slave` giúp chúng ta lưu trữ an toàn hoặc linh hoạt mở rộng lưu trữ data.

# Client/server

# MVC

# MVVM




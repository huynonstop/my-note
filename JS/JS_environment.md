# JavaScript engines

JavaScript is an **interpreted programming language**. It means that source code isn’t compiled into binary code prior to execution.

The **JavaScript engine** translates your script into runnable machine code instructions so it can be executed by the CPU of the host machine. The engine translates scripts during runtime. Your code won’t be compiled unless you run it. **(JIT (Just-In-Time) compiler)**

![JavaScript engine resposibility](http://dolszewski.com/wp-content/uploads/2018/04/javascript-engine.png)

- **Chrome V8** – As you probably guessed the engine shipped in *Google Chrome*. It’s an open source project written in C++. V8 is also used in *Opera*, *NodeJS*, and *Couchbase*.
- **SpiderMonkey** – The open source engine implemented in C++. It’s maintained by Mozilla Foundation. You can find it in *Firefox*.
- **Nitro** – The engine developed by Apple. It’s used in *Safari*.
- **Chakra** – Developed by Microsoft as the JavaScript engine for *Edge* browser.

# JavaScript runtime environment (NodeJS, Browser)

**The JavaScript engine works inside an environment**

These can be utility libraries or APIs which allow **communicating with the world surrounding the engine**

The **JavaScript runtime environment** provides your scripts with utility libraries which can be used during execution. It’s your script that references these libraries. The engine itself doesn’t depend on them.

The cool thing is **the JavaScript engine implementation is totally independent of the runtime environment.** Engines aren’t developed with any particular environment in mind.

![Kết quả hình ảnh cho JavaScript runtime environment](https://codecute.com/wp-content/uploads/2019/04/js-runtime-enviroment-1.png)

## Event loop

![image-20200609164035886](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609164035886.png)

## Node.JS

Node.js® is a JavaScript runtime built on [Chrome’s V8 JavaScript engine](https://developers.google.com/v8/). cho phép chúng ta chạy Javascript trên servers với cơ chế non-blocking I/O

Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient.

Node.js’ package ecosystem, [npm](https://www.npmjs.com/), is the largest ecosystem of open source libraries in the world.

![img](https://cdn-media-1.freecodecamp.org/images/1*B_UCsFOPfRDKO8ovHpxphg.png)

- Realtime: Đúng như định nghĩa, xử lí giao tiếp theo thời gian thực.
- Very Fast: V8 Javascript Engine nên việc thực thi chương trình rất nhanh
- Single thread: Không giống như những ngôn ngữ đa luồng khác, nodejs chỉ có một luồng chạy duy nhất - vòng lặp sự kiện (Event loop). Tất cả các request đi đều chạy vào luồng này để xử lí, giúp tối ưu hóa bộ nhớ và tốc độ.
- None-blocking: Tất cả các API của NodeJS đều không đồng bộ, nó chủ yếu dựa trên nền của NodeJS Server và chờ đợi Server trả dữ liệu về. Việc di chuyển máy chủ đến các API tiếp theo sau khi gọi và cơ chế thông báo các sự kiện của Node.js giúp máy chủ để có được một phản ứng từ các cuộc gọi API trước (Realtime).
- No cache: Gần như ko có đệm thêm gì ở đầu ra, chủ yếu là dữ liệu.

## Browser

![img](https://miro.medium.com/max/499/1*RL0pnuf_hmLJ76oY6DViZw.png)

### Rendering Engine

- Chrome and Safari uses WebKit
- Firefox uses Gecko
- Internet Explorer uses Trident
- Edge uses EdgeHTML

![img](https://miro.medium.com/max/600/1*cfQpu6Xvb7e9IiH4CCuiCg.png)

![img](https://viblo.asia/uploads/d6710955-9dc3-4a28-944d-069c0dac03c0.png)

### Storage/Data Persistence

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
  - LocalStorage can only be read on client-side

![JavaScript web browser environment](http://dolszewski.com/wp-content/uploads/2018/04/javascript-web-env.png)

# Garbage collector

![image-20200523225819599](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200523225819599.png)

# DOM

The DOM is an interface to an HTML document. It is used by browsers as a first step towards determining what to render in the viewport, and can used by programs to modify the content, structure, or styling of the page.

Although similar to other forms of the source HTML document, the DOM is different in a number of ways:

- It is always valid HTML
- It is a living model that can be modified by JavaScript
- It doesn't include pseudo-elements (e.g. `::after`)
- It does include hidden elements (e.g. with `display: none`)

![img](https://miro.medium.com/max/753/1*ZrzXoRljG5Co5KvEsWJNjA.png)

# Is JavaScript single-threaded?

It’s absolutely correct that **your JavaScript code is executed in a single thread**. But, it doesn’t mean that the whole JavaScript runtime environment works in a single thread. The **thread pool exists in JavaScript runtime**. Fortunately, you don’t have to worry about thread management because the environment does it for you.

# JavaScript outside of browser (MEAN/MERN STACK)

- **server-side** development – Thanks to [NodeJS](https://nodejs.org/en/) which is the most powerful JavaScript runtime environment. NodeJS gives you access to the file system, network, and other things which aren’t allowed in web browsers.
- **network** – JSON format, which is currently the most popular human-readable format for data transportation, originates from JavaScript.
- creating **mobile applications** – Because JavaScript isn’t tight into any particular mobile vendor, it’s a perfect choice for a medium language for different mobile platforms. Frameworks like [PhoneGap](https://phonegap.com/) or [Appcelerator Titanium](https://www.appcelerator.com/Titanium/) try to provide a unified development environment for different devices and OSs.
- **databases** – You can easily find countless ready for use databases implemented with JavaScript. But there are other uses. For instance, the query language for [MongoDB](https://www.mongodb.com/) is based on JavaScript syntax.

# AJAX (**A**synchronous **J**avaScript **A**nd **X**ML)

AJAX allows web pages to be updated asynchronously by exchanging data with a web server behind the scenes. This means that it is possible to update parts of a web page, without reloading the whole page. Allows applications to transport data to/from a server asynchronously without refreshing the page

- AJAX is based on the following open standards −

  - Browser-based presentation using HTML and Cascading Style Sheets (CSS).
  
  - Data is stored in XML format and fetched from the server. (Actually JSON)
  
- Behind-the-scenes data fetches using XMLHttpRequest objects in the browser. (performs asynchronous interaction with the server.)
  
  - JavaScript to make everything happen.
  
    ## XHR - XML Http Request
  
    - Update a web page without reloading the page
    - Request data from a server - after the page has loaded
    - Receive data from a server - after the page has loaded
    - Send data to a server - in the background
  

![AJAX](https://www.w3schools.com/whatis/img_ajax.gif)

### What are the advantages and disadvantages of using Ajax?

**Advantages**

- Better interactivity. New content from the server can be changed dynamically without the need to reload the entire page.
- Reduce connections to the server since scripts and stylesheets only have to be requested once.
- State can be maintained on a page. JavaScript variables and DOM state will persist because the main container page was not reloaded.
- Basically most of the advantages of an SPA.

**Disadvantages**

- Dynamic webpages are harder to bookmark.
- Does not work if JavaScript has been disabled in the browser.
- Some webcrawlers do not execute JavaScript and would not see content that has been loaded by JavaScript.
- Webpages using Ajax to fetch data will likely have to combine the fetched remote data with client-side templates to update the DOM. For this to happen, JavaScript will have to be parsed and executed on the browser, and low-end mobile devices might struggle with this.
- Basically most of the disadvantages of an SPA.

# Does React require Node.js?

**In Production:**

Server Side: Not needed unless you are using Node.js to create a web server and server your ReactJS files.

Client Side: Only browser is enough.

**In development:**

Server Side: Needed to download react libraries and other dependencies using NPM. Needed by webpack to create production bundles.

Client Side: Only browser is enough.

# Middleware

![image-20200227181407311](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200227181407311.png)

# Term 

1. **AJAX**: viết tắt cho cụm “Asynchronous JavaScript and XML” (JavaScript và HTML không đồng bộ): công nghệ giúp giúp tạo ra những trang Web động mà không phải reload lại trang, giúp tác vụ chạy mượt và đẹp hơn.
2. [**API**](https://topdev.vn/blog/api-la-gi/)**:** viết tắt của cụm từ “Application Programming Interface” (Giao diện lập trình ứng dụng): phương thức, giao thức kết nối với các thư viện và ứng dụng khác. Ngoài ra, API cung cấp khả năng truy xuất đến một tập các hàm hay dùng, từ đó có thể trao đổi dữ liệu giữa các ứng dụng.
3. **Native API**: native API là một tính năng tích hợp có sẵn trong môi trường lập trình. Ví dụ: document.querySelector() đuợc gọi là native API để chọn các phần tử HTML (HTML elements)
4. **Browser console**: bạn có thể truy cập vào developer toolbox với một số trình duyệt web. Với Firefox và Chrome trên Mac, nhấm tổ hợp phím Command + Option + I, trên Linux (hoặc Windows nếu mình nhớ không nhầm) là phím F12. Sau đó màn hình interactive console có thể gõ và thực hiện lệnh JavaScript. Console này cũng sẽ chỉ ra các lỗi và cảnh báo khác từ chương trình JavaScript.
5. **Debugger**: là công cụ được xây dựng giúp dev tìm ra lỗi và chỗ nào chương trình ngưng hoạt động. JavaScript cũng có hướng dẫn debugger dừng đúng ở câu lệnh có lỗi.
6. **Browser API hay Web API:** giống như native APIs, Web API là tính năng cụ thể có sẵn trên trình duyệt web, và dev có thể sử dụng ngay lập tức sau vài buớc cài đặt đơn giản. Ví dụ như setTimeout, setInterval, console.log. Xem thêm full list Web APIs.
7. **ECMAScript**: là tên chính thức của JavaScript. JavaScript sau đó vào năm 1996 được tiêu chuẩn hóa bởi ECMA, một tổ chức chuyên về tiêu chuẩn hóa các ngành công nghệ và hệ thống.
8. **ES5**: đồng nghĩa với ECMAScript 2009, là phiên bản thứ 5 của JavaScript. Nhằm tránh nhầm lẫn, người ta hay sử dụng cú pháp “ECMAScript + năm” để xác định phiên bản JavaScript mình muốn đề cập.
9. **ES6**: viết tắt của ECMAScript 2015, phiên bản thứ 6 của JavaScript. Từ năm 2015, JavaScript committee quyết định sẽ cho ra mắt các tính năng mới hằng năm. Từ đó, ECMAScript 2016, ECMAScript 2017, ECMAScript 2018 lần lượt ra đời.
10. [**JavaScript engine**: ](https://topdev.vn/blog/hieu-hon-ve-cach-hoat-dong-cua-javascript-engine/)là một phần của browser và có khả năng biên dịch (compile) và phiên dịch (interpret) JavaScript code. JavaScript engine đọc các đoạn code JavaScript rồi chuyển nó sang mã máy để máy tính (hoặc phần mềm máy tính như trình duyệt web, server node.js…) có thể hiểu và chạy được.
11. **JavaScript specification**: là bản mô tả chức năng cho ECMAScript. Trong mỗi ấn bản này, người ta định nghĩa các tính năng của ngôn ngữ lập trình ECMAScript theo một cách viết rất hàn lâm, với hàng đống những thuật ngữ khoa học. Loại tài liệu academic này chắc chắn không hợp khẩu vị của đa số JavaScript developer, nhưng lại rất quan trọng đối với các nhóm phát triển web browser và JavaScript engine. Họ sẽ tham khảo đặc tả và lần lượt tích hợp chức năng vào sản phẩm của họ.
12.  **Node.js**: là môi trường chạy JavaScript bên ngoài browser, bao gồm JavaScript engine và V8 để compile và execute đoạn code. Nodejs không chạy trên một trình duyệt mà chạy trên Server.
13. [**Node package manager**](https://topdev.vn/blog/npm-la-gi/): viết tắt là npm, là một công cụ tạo và quản lý các thư viện lập trình Javascript cho [Node.js](https://nodejs.org/). Trong cộng đồng Javascript, các lập trình viên chia sẻ hàng trăm nghìn các thư viện với các đoạn code đã thực hiện sẵn một chức năng nào đó. Nó giúp cho các dự án mới tránh phải viết lại các thành phần cơ bản, các thư viện lập trình hay thậm chí cả các [framework](https://topdev.vn/blog/framework-la-gi/).
14. **HTTP request**: hay còn gọi là thông báo yêu cầu được gửi từ client đến server, để yêu cầu server làm việc gì đó. Ví dụ như khi bạn đang ở trang web từ trình duyệt. Trang web này ngược lại có thể yêu cầu HTTP request để lấy dữ liệu, hầu hết là về REST APIs. (xem thêm bên dưới).
15. **HTTP error**: một số lỗi thường gặp với web services và server trả về các error cùng với các mã số quen thuộc như: 500 (server error), 404 (not found), 403 (forbidden), ..
16. [**JSON**](https://topdev.vn/blog/json-la-gi/)**:** viết tắt cho cụm từ JavaScript Object Notation, là một kiểu định dạng dữ liệu tuân theo một quy luật nhất định mà hầu hết các ngôn ngữ lập trình hiện nay đều có thể đọc được. JSON là một tiêu chuẩn mở để trao đổi dữ liệu trên web.
17. **REST API**: REST là một dạng chuyển đổi cấu trúc dữ liệu, API là giao diện lập trình ứng dụng giúp tạo ra các phương thức kết nối với các thư viện và ứng dụng khác nhau. REST API là một ứng dụng chuyển đổi cấu trúc dữ liệu có các phương thức để kết nối với các thư viện và ứng dụng khác. REST API không được xem là một công nghệ, nó là một giải pháp để tạo ra các ứng dụng web services thay thế cho các kiểu khác như SOAP, WSDL (Web Service Definition Language),…
18. **Transpiler**: những trình duyệt cũ không hỗ trợ cú pháp JavaScript mới từ phiên bản ECMAScript 2015 trở về sau. Lúc này, transpiler sẽ có nhiệm vụ biên dịch cú pháp JavaScript mới thành các phiên bản tương thích (như ECMAScript 2009).
19. **Proposal**: sự phát triển JavaScript được thông qua bởi hội đồng TC39. Thành viên từ nhóm này có thể đề xuất proposal để cải thiện hay thêm một số tính năng cho ngôn ngữ này. Proposal là một văn bản đặc tả viết bằng ngôn ngữ học thuật, mô tả những tính năng mới và cách áp dụng trong JavaScript.
20. **Stage N**: một bản JavaScript proposal sẽ bắt đầu với Stage 0. Càng nhận được nhiều đồng thuận từ hội đồng TC39, bản proposal càng có nhiều lợi thế ở những giai đoạn sau: 1, 2, 3 và 4. “Stage 1” hay “stage 2” chỉ giai đoạn của bản proposal đó. Nếu bản proposal đang ở “stage 2” thì nó đang tiến triển khá tốt và có khả năng được duyệt vào giai đoạn tiếp theo. Giai đoạn kết thúc là stage 4, đồng nghĩa với việc tính năng mới sẽ được áp dụng vào ngôn ngữ này.
21. **Vanilla JavaScript**: là cách gọi của những ứng dụng JavaScript “nguyên thủy”, ví dụ như những ứng dụng không cần đến sự trợ giúp của frontend library như React, Vue hay Angular.
22. **XMLHttpRequest**: XMLHttpRequest được thiết kế để đọc nguồn dữ liệu từ URL một cách đồng bộ (synchronous) hoặc không đồng bộ (asynchronous). Đọc dữ liệu một cách không đồng bộ giúp người dùng vẫn có thể thao tác với trình duyệt trong quá trình XMLHttpRequest đang đọc nguồn dữ liệu từ xa. XMLHttpRequest là một phần của “gia phả” AJAX, được sử dụng để thực hiện việc toàn bộ quy trình trao đổi thông tin giữa trình duyệt (máy khách) và máy chủ mà không yêu cầu phải tải lại trang.
23. **FetchAPI**: là một API đơn giản cho việc gửi và nhận request bằng js. Với fetch thì việc thực hiện các yêu cầu web và xử lý phản hồi dễ dàng hơn so với XMLHttpRequest cũ, nó khá tương đồng với XMLHttpRequest nhưng cải tiến hơn và được xây dựng dựa trên ECMAScript 2015 Promises.
24. [**CORS**](https://topdev.vn/blog/cors-la-gi/): viết tắt cho Cross-Origin Resource Sharing, CORS là một cơ chế cho phép nhiều tài nguyên khác nhau (fonts, Javascript, v.v…) của một trang web có thể được truy vấn từ domain khác với domain của trang đó. Xem thêm cách sử dụng CORS tại đây.
25. [**WebSocket**:](https://topdev.vn/blog/socket-la-gi-websocket-la-gi/) là một giao thức giúp truyền dữ liệu hai chiều giữa server-client qua một kết nối TCP duy nhất. Không giống với giao thức HTTP là cần client chủ động gửi yêu cầu cho server, client sẽ chờ đợi để nhận được dữ liệu từ máy chủ. Hay nói cách khác với giao thức Websocket thì server có thể chủ động gửi thông tin đến client mà không cần phải có yêu cầu từ client.
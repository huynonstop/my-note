# JavaScript engines

JavaScript is an **interpreted programming language**. It means that source code isn’t compiled into binary code prior to execution.

The **JavaScript engine** translates your script into runnable machine code instructions so it can be executed by the CPU of the host machine. The engine translates scripts during runtime. Your code won’t be compiled unless you run it. **(JIT (Just-In-Time) compiler)**

![JavaScript engine resposibility](assets/JS_environment/javascript-engine.png)

- **Chrome V8** – As you probably guessed the engine shipped in *Google Chrome*. It’s an open source project written in C++. V8 is also used in *Opera*, *NodeJS*, and *Couchbase*.
- **SpiderMonkey** – The open source engine implemented in C++. It’s maintained by Mozilla Foundation. You can find it in *Firefox*.
- **Nitro** – The engine developed by Apple. It’s used in *Safari*.
- **Chakra** – Developed by Microsoft as the JavaScript engine for *Edge* browser.

# JavaScript runtime environment (NodeJS, Browser)

**The JavaScript engine works inside an environment**

These can be utility libraries or APIs which allow **communicating with the world surrounding the engine**

The **JavaScript runtime environment** provides your scripts with utility libraries which can be used during execution. It’s your script that references these libraries. The engine itself doesn’t depend on them.

The cool thing is **the JavaScript engine implementation is totally independent of the runtime environment.** Engines aren’t developed with any particular environment in mind.

![Kết quả hình ảnh cho JavaScript runtime environment](assets/JS_environment/js-runtime-enviroment-1.png)

## Event loop

![image-20200609164035886](assets/JS_environment/image-20200609164035886.png)

## Node.JS

Node.js® is a JavaScript runtime built on [Chrome’s V8 JavaScript engine](https://developers.google.com/v8/). cho phép chúng ta chạy Javascript trên servers với cơ chế non-blocking I/O

Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient.

Node.js’ package ecosystem, [npm](https://www.npmjs.com/), is the largest ecosystem of open source libraries in the world.

![image-20201208090447225](assets/JS_environment/image-20201208090447225.png)

- Realtime: Đúng như định nghĩa, xử lí giao tiếp theo thời gian thực.
- Very Fast: V8 Javascript Engine nên việc thực thi chương trình rất nhanh
- Single thread: Không giống như những ngôn ngữ đa luồng khác, nodejs chỉ có một luồng chạy duy nhất - vòng lặp sự kiện (Event loop). Tất cả các request đi đều chạy vào luồng này để xử lí, giúp tối ưu hóa bộ nhớ và tốc độ.
- None-blocking: Tất cả các API của NodeJS đều không đồng bộ, nó chủ yếu dựa trên nền của NodeJS Server và chờ đợi Server trả dữ liệu về. Việc di chuyển máy chủ đến các API tiếp theo sau khi gọi và cơ chế thông báo các sự kiện của Node.js giúp máy chủ để có được một phản ứng từ các cuộc gọi API trước (Realtime).
- No cache: Gần như ko có đệm thêm gì ở đầu ra, chủ yếu là dữ liệu.

## Browser

![image-20201208090507467](assets/JS_environment/image-20201208090507467.png)

### Rendering Engine

- Chrome and Safari uses WebKit
- Firefox uses Gecko
- Internet Explorer uses Trident
- Edge uses EdgeHTML

![image-20201208090648812](assets/JS_environment/image-20201208090648812.png)

<img src="assets/JS_environment/d6710955-9dc3-4a28-944d-069c0dac03c0.png" alt="img" style="zoom:150%;background: white" />

### Storage/Data Persistence

`localStorage`, `sessionStorage`, and cookies are all **client storage** solutions. Session data is held on the server where it remains under your direct control. (It's unique per `protocol://host:port`(**Origin**) combination)

 They are only able to store values as strings.

+ Cookies

  An HTTP cookie (web cookie, browser cookie) is a small piece of data that a server sends to the user's web browser. The browser may store it and send it back with the next request to the same server.

  - Stores data that has to be sent back to the server with subsequent requests. Its expiration varies based on the type and the **expiration duration** can be set from either server-side or client-side (normally from server-side).
  - Cookies can be read on server-side and client-side.
  - Size must be less than 4KB.
  - Cookies can be made secure by setting the `httpOnly` flag as true for that cookie. This prevents client-side access to that cookie

  Cookies are mainly used for three purposes:

  - Session management: Logins, shopping carts, game scores, or anything else the server should remember

  - Personalization: User preferences, themes, and other settings

  - Tracking: Recording and analyzing user behavior

    **Cross-Site Request Forgery** (XSRF).

    **Third-party cookies**

+ `sessionStorage` (1 Tab)

  + The `sessionStorage` object stores data only for a session, meaning that the data is stored until the browser (or tab) is closed.
  + Data is never transferred to the server.
  + Storage limit is larger than a cookie (at least 5MB).
  + `sessionStorage` can only be read on client-side

+ `localStorage`(Browser)

  - Stores data with no expiration date, and gets cleared only through JavaScript, or clearing the Browser cache / Locally Stored Data
  - Storage limit is the maximum amongst the three
  - Data is never transferred to the server.
  - `localStorage`can only be read on client-side

![JavaScript web browser environment](assets/JS_environment/javascript-web-env.png)

# Garbage collector

![image-20200523225819599](assets/JS_environment/image-20200523225819599.png)

# DOM

The DOM is an interface to an HTML document. It is used by browsers as a first step towards determining what to render in the viewport, and can used by programs to modify the content, structure, or styling of the page.

Although similar to other forms of the source HTML document, the DOM is different in a number of ways:

- It is always valid HTML
- It is a living model that can be modified by JavaScript
- It doesn't include pseudo-elements (e.g. `::after`)
- It does include hidden elements (e.g. with `display: none`)

![image-20201208090809656](assets/JS_environment/image-20201208090809656.png)

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
  

![AJAX](assets/JS_environment/img_ajax.gif)

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

![image-20200227181407311](assets/JS_environment/image-20200227181407311.png)

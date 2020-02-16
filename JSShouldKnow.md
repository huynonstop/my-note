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

# Is JavaScript single-threaded?

It’s absolutely correct that **your JavaScript code is executed in a single thread**. But, it doesn’t mean that the whole JavaScript runtime environment works in a single thread. The **thread pool exists in JavaScript runtime**. Fortunately, you don’t have to worry about thread management because the environment does it for you.

# What is JavaScript Event loop?

The **Event loop**, which is a part of the JavaScript runtime environment, **is a mechanism responsible for handling callbacks**.![Event loop step-by-step processing](http://dolszewski.com/wp-content/uploads/2018/04/event-loop-processing.png)

# Browser JavaScript runtime environment![JavaScript web browser environment](http://dolszewski.com/wp-content/uploads/2018/04/javascript-web-env.png)

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

# Node.JS

Node.js® is a JavaScript runtime built on [Chrome’s V8 JavaScript engine](https://developers.google.com/v8/).

Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient.

Node.js’ package ecosystem, [npm](https://www.npmjs.com/), is the largest ecosystem of open source libraries in the world.![img](https://cdn-media-1.freecodecamp.org/images/1*B_UCsFOPfRDKO8ovHpxphg.png)

# Does React require Node.js?

**In Production:**

Server Side: Not needed unless you are using Node.js to create a web server and server your ReactJS files.

Client Side: Only browser is enough.

**In development:**

Server Side: Needed to download react libraries and other dependencies using NPM. Needed by webpack to create production bundles.

Client Side: Only browser is enough.


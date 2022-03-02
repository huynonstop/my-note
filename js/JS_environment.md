# JavaScript engines

JavaScript is an **interpreted programming language**. It means that source code isn’t compiled into binary code prior to execution.

The **JavaScript engine** translates your script into runnable machine code instructions so it can be executed by the CPU of the host machine. The engine translates scripts during runtime. Your code won’t be compiled unless you run it. **(JIT (Just-In-Time) compiler)**

![JavaScript engine resposibility](assets/JS_environment/javascript-engine.png)

- **Chrome V8** – As you probably guessed the engine shipped in *Google Chrome*. It’s an open source project written in C++. V8 is also used in *Opera*, *NodeJS*, and *Couchbase*.
- **SpiderMonkey** – The open source engine implemented in C++. It’s maintained by Mozilla Foundation. You can find it in *Firefox*.
- **Nitro** – The engine developed by Apple. It’s used in *Safari*.
- **Chakra** – Developed by Microsoft as the JavaScript engine for *Edge* browser.

## JavaScript runtime environment (NodeJS, Browser)

The JavaScript engine works inside an environment

These can be utility libraries or APIs which allow **communicating with the world surrounding the engine**

The **JavaScript runtime environment** provides your scripts with utility libraries which can be used during execution. It’s your script that references these libraries. The engine itself doesn’t depend on them.

The cool thing is **the JavaScript engine implementation is totally independent of the runtime environment.** Engines aren’t developed with any particular environment in mind.

![Kết quả hình ảnh cho JavaScript runtime environment](assets/JS_environment/js-runtime-enviroment-1.png)

## Event loop

<https://www.youtube.com/watch?v=6MXRNXXgP_0>

![image-20200609164035886](assets/JS_environment/image-20200609164035886.png)

## Browser

![image-20201208090507467](assets/JS_environment/image-20201208090507467.png)

### Rendering Engine

- Chrome and Safari uses WebKit
- Firefox uses Gecko
- Internet Explorer uses Trident
- Edge uses EdgeHTML

![image-20201208090648812](assets/JS_environment/image-20201208090648812.png)

<img src="assets/JS_environment/d6710955-9dc3-4a28-944d-069c0dac03c0.png" alt="img" style="zoom:150%;background: white" />

## Garbage collector

![image-20200523225819599](assets/JS_environment/image-20200523225819599.png)

## DOM

The DOM is an interface to an HTML document. It is used by browsers as a first step towards determining what to render in the viewport, and can used by programs to modify the content, structure, or styling of the page.

Although similar to other forms of the source HTML document, the DOM is different in a number of ways:

- It is always valid HTML
- It is a living model that can be modified by JavaScript
- It doesn't include pseudo-elements (e.g. `::after`)
- It does include hidden elements (e.g. with `display: none`)

![image-20201208090809656](assets/JS_environment/image-20201208090809656.png)

## Is JavaScript single-threaded?

It’s absolutely correct that **your JavaScript code is executed in a single thread**. But, it doesn’t mean that the whole JavaScript runtime environment works in a single thread. The **thread pool exists in JavaScript runtime**. Fortunately, you don’t have to worry about thread management because the environment does it for you.

# What is NodeJS

https://nodejs.org/en/docs/

A javascript runtime - Allow javascript to run outside of browser

NodeJS use V8 engine and adds more features to it (File system, ...)

![image-20200706164537151](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200706164537151.png)

![image-20200707115214004](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200707115214004.png)

# V8 javascript engine

Taking JS code => compiling to machine code

# Usage of NodeJS

A javascript runtime => Build server, Utility script, helper tool, app, ... Make javascript not be limited to the browser and can do everything that other programming langue can do

## web dev

Run server: create sever and listening request

Business logic: Handle request, validate inputs, connect to database

Return response: return html, JSON

# REPL

https://nodejs.org/en/knowledge/child-processes/how-to-spawn-a-child-process/

https://nodejs.org/en/knowledge/REPL/how-to-create-a-custom-repl/

https://nodejs.org/en/knowledge/command-line/how-to-prompt-for-command-line-input/

https://nodejs.org/en/knowledge/REPL/how-to-use-nodejs-repl/

Read-Eval-Print Loop

# module

```js
const products = [];
router.post('/product', (req, res, next) => {
  const {title} = req.body;
  product.push({title})
  res.redirect('/');
});
module.exports = {
    products,
    router
}
```

Share to all users

# NodeJS web work

https://nodejs.org/en/knowledge/HTTP/clients/how-to-access-query-string-parameters/

https://nodejs.org/en/knowledge/errors/what-is-the-error-object/

Client => request => Server => response => Client

![image-20200706201622744](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200706201622744.png)

# Node Server

https://nodejs.org/en/knowledge/HTTP/servers/how-to-create-a-HTTP-server/

https://nodejs.org/en/knowledge/HTTP/servers/how-to-create-a-HTTPS-server/

https://nodejs.org/en/knowledge/HTTP/servers/how-to-read-POST-data/

https://nodejs.org/en/knowledge/HTTP/servers/how-to-serve-static-files/

https://nodejs.org/en/knowledge/file-system/how-to-store-local-config-data/

https://nodejs.org/en/knowledge/HTTP/servers/how-to-handle-multipart-form-data/

```js
const http = require("http");

const rqListener = (req,res) => {
    console.log(req);
}

const server = http.createServer(rqListener);
server.listen(3000)
```

# Streams and Buffers

a file size can be larger than your free memory space, making it impossible to read the whole file into the memory in order to process it

https://nodesource.com/blog/understanding-streams-in-nodejs

https://stackoverflow.com/questions/1216380/what-is-a-stream

https://nodejs.org/en/knowledge/advanced/streams/what-are-streams/

https://medium.freecodecamp.org/node-js-streams-everything-you-need-to-know-c9141306be93

![image-20200706235605988](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200706235605988.png)

What makes streams unique, is that instead of a program reading a file into memory **all at once** like in the traditional way, streams read chunks of data piece by piece, processing its content without keeping it all in memory.

Using streams to process smaller chunks of data, makes it possible to read larger files.

## chunk

chunk are a part of **file data**

## buffer

https://nodejs.org/en/knowledge/advanced/buffers/how-to-use-buffers/

![image-20200815223001292](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200815223001292.png)

basically give us access to chunk

## Why streams

Streams basically provide two major advantages compared to other data handling methods:

1. **Memory efficiency:** you don’t need to load large amounts of data in memory before you are able to process it
2. **Time efficiency:** it takes significantly less time to start processing data as soon as you have it, rather than having to wait with processing until the entire payload has been transmitted

## pipe

https://nodejs.org/en/knowledge/advanced/streams/how-to-use-stream-pipe/

Streams make for quite a handy abstraction, and there's a lot you can do with them - as an example, let's take a look at `stream.pipe()`, the method used to take a readable stream and connect it to a writeable steam.

![image-20200815224109613](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200815224109613.png)

## file system

https://nodejs.org/en/knowledge/advanced/streams/how-to-use-fs-create-read-stream/

https://nodejs.org/en/knowledge/advanced/streams/how-to-use-fs-create-write-stream/

https://nodejs.org/en/knowledge/file-system/how-to-read-files-in-nodejs/

https://nodejs.org/en/knowledge/file-system/how-to-search-files-and-directories-in-nodejs/

https://nodejs.org/en/knowledge/file-system/how-to-write-files-in-nodejs/

### delete file

![image-20200815233158554](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200815233158554.png)

# Request

https://nodejs.org/en/knowledge/HTTP/clients/how-to-create-a-HTTP-request/

```js
const rqListener = (req,res) => {
    const {url,method,headers} = req;
    if(url === "/") {/*do blah blah blah*/}
    else if(url === "/something") {/*do blah blah blah*/}
}
```

# Response

```js
res.statusCode
res.setHeader()
/*
	Content-type (response format)
	Location (redirect)
*/
res.write()
res.end()
```

# Callback

Callbacks are basically just functions that you pass to other functions.

This is possible in JavaScript because functions are first class objects.

A function can call the callback both synchronously and asynchronously.

# Async await

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function

sugar syntax

![image-20200816150649356](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816150649356.png)

## Top level await (node 14>)

# NodeJS behind the scenes

https://nodejs.org/en/knowledge/getting-started/globals-in-node-js/

https://nodejs.org/en/knowledge/file-system/how-to-use-the-path-module/

https://nodejs.org/en/knowledge/getting-started/how-to-use-util-inspect/

https://nodejs.org/en/knowledge/getting-started/how-to-debug-nodejs-applications/

https://nodejs.org/en/knowledge/getting-started/the-console-module/

https://nodejs.org/en/knowledge/getting-started/the-process-module/

https://nodejs.org/en/knowledge/getting-started/what-is-node-core-verus-userland/

![image-20200707104435586](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200707104435586.png)

## Node life cycle 

![image-20200706202546754](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200706202546754.png)

## Event loop

https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/

https://dev.to/cpuram1/nodejs-event-loop-architecture-53jn

![image-20200707104401166](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200707104401166.png)

## Event driven

https://nodejs.org/en/knowledge/getting-started/control-flow/what-are-event-emitters/

https://dev.to/cpuram1/events-and-event-driven-architecture-in-nodejs-792

In NodeJS, there are certain Objects called events emitters that emit named events as soon as something important happens in the app like request hitting a server or a timer is expiring, or a file finished to read. These events are picked up by and event listeners that we developers setup which will fire up callback functions that are attached to each listener. i.e, on one hand, we have event emitters, on the other hand, we have event listeners that will react by calling callbacks.

## Blocking and non-blocking code

https://nodejs.org/en/knowledge/getting-started/control-flow/how-to-write-asynchronous-code/

https://nodejs.org/en/knowledge/getting-started/control-flow/what-are-callbacks/

```js
const fs = require("fs")
fs.writeFileSync(filename, message); // wait file created then move to next code

fs.writeFile(filename, message, cb) // offload write file process to the OS (which is multi-threading) and move to next code and dispatching the callback when the OS complete writing file
```

=> NodeJS high performance because it never block code execution
[WebSockets in 100 Seconds & Beyond with Socket.io - YouTube](https://www.youtube.com/watch?v=1BfCnjr_Vjg)

![image-20200816155150812](assets/websockets/image-20200816155150812.png)

# WebSockets

https://www.npmjs.com/package/express-ws

![image-20200816155407038](assets/websockets/image-20200816155407038.png)

## socket.io

https://socket.io/

https://socket.io/docs/rooms/

![image-20200907222159837](assets/websockets/image-20200907222159837.png)

### server

```js
const io = require('socket.io')();
// or
const Server = require('socket.io');
const io = new Server();
```

![image-20200816163948479](assets/websockets/image-20200816163948479.png)

![image-20200816163912881](assets/websockets/image-20200816163912881.png)

> the callback function will run when a client emit event

![image-20200816164200864](assets/websockets/image-20200816164200864.png)

> ## server.of(nsp)
>
> - `nsp` *(String|RegExp|Function)*
> - **Returns** `Namespace`

### namespaces 

= endpoint (workspace)

Represents a pool of sockets connected under a given scope identified by a pathname (eg: `/chat`).

A client always connects to `/` (the main namespace), then potentially connect to other namespaces (while using the same underlying connection).

> ## namespace.emit(eventName[, …args])
>
> - `eventName` *(String)*
> - `args`
>
> **Emits an event to all connected clients**.
>
> ```js
> const io = require('socket.io')();
> io.emit('an event sent to all connected clients'); // main namespace
> 
> const chat = io.of('/chat');
> chat.emit('an event sent to all connected clients in chat namespace');
> ```
>
> **Note:** acknowledgements are not supported when emitting from namespace.
>
> ## namespace.to(room)
>
> - `room` *(String)*
> - **Returns** `Namespace` for chaining
>
> Sets a modifier for a subsequent event emission that the event will only be ***broadcasted*** to clients that have joined the given `room`.
>
> To emit to multiple rooms, you can call `to` several times.
>
> ```js
> const io = require('socket.io')();
> const adminNamespace = io.of('/admin');
> 
> adminNamespace.to('level1').emit('an event', { some: 'data' });
> ```
>
> ## namespace.use(fn)
>
> - `fn` *(Function)*
>
> Registers a middleware, which is a function that gets executed for every incoming `Socket`, and receives as parameters the socket and a function to optionally defer execution to the next registered middleware.
>
> Errors passed to middleware callbacks are sent as special `error` packets to clients.
>
> ```js
> io.use((socket, next) => {
>   if (socket.request.headers.cookie) return next();
>   next(new Error('Authentication error'));
> });
> ```

### socket

A `Socket` is the fundamental class for interacting with browser clients. A `Socket` belongs to a certain `Namespace` (by default `/`) and uses an underlying `Client` to communicate. (= client connection)

> ## socket.emit(eventName`[, …args]` `[, ack]`)
>
> *(overrides `EventEmitter.emit`)*
>
> - `eventName` *(String)*
> - `args`
> - `ack` *(Function)*
> - **Returns** `Socket`
>
> **Emits an event to the socket** identified by the string name. Any other parameters can be included. All serializable datastructures are supported, including `Buffer`.
>
> ```js
> socket.emit('hello', 'world');
> socket.emit('with-binary', 1, '2', { 3: '4', 5: new Buffer(6) });
> ```
>
> The `ack` argument is optional and will be called with the client’s answer.
>
> ```js
> io.on('connection', (socket) => {
>   socket.emit('an event', { some: 'data' });
> 
>   socket.emit('ferret', 'tobi', (data) => {
>     console.log(data); // data will be 'woot'
>   });
> 
>   // the client code
>   // client.on('ferret', (name, fn) => {
>   //   fn('woot');
>   // });
> 
> });
> ```
>
> ## socket.on(eventName, callback)
>
> *(inherited from `EventEmitter`)*
>
> - `eventName` *(String)*
> - `callback` *(Function)*
> - **Returns** `Socket`
>
> Register a new handler for the given event.
>
> ```js
> socket.on('news', (data) => {
>   console.log(data);
> });
> // with several arguments
> socket.on('news', (arg1, arg2, arg3) => {
>   // ...
> });
> // or with acknowledgement
> socket.on('news', (data, callback) => {
>   callback(0);
> });
> ```
>
> ## socket.removeListener(eventName, listener)
>
> ## socket.removeAllListeners([eventName])

### rooms 

= channels

Within each `Namespace`, you can also define arbitrary channels (called `room`) that the (server-side)`Socket` can join and leave. That provides a convenient way to broadcast to a group of `Socket`s 

> ## socket.join(room(s)[, callback])
>
> - `room` *(String)*, `rooms` *(Array)*, 
> - `callback` *(Function)*
> - **Returns** `Socket` for chaining
>
> ```js
> io.on('connection', (socket) => {
>   socket.join('room 237', () => {
>     let rooms = Object.keys(socket.rooms);
>     console.log(rooms); // [ <socket.id>, 'room 237' ]
>     io.to('room 237').emit('a new user has joined the room'); // broadcast to everyone in the room
>   });
> });
> io.on('connection', (socket) => {
>   socket.on('say to someone', (id, msg) => {
>     // send a private message to the socket with the given id
>     socket.to(id).emit('my message', msg);
>   });
> });
> ```
>
> ## socket.leave(room[, callback])
>
> - `room` *(String)*
> - `callback` *(Function)*
> - **Returns** `Socket` for chaining
>
> **Rooms are left automatically upon disconnection**.
>
> ```js
> io.on('connection', (socket) => {
>   socket.leave('room 237', () => {
>     io.to('room 237').emit(`user ${socket.id} has left the room`);
>   });	
> });
> ```
>
> ## socket.to/in(room)
>
> - `room` *(String)*
> - **Returns** `Socket` for chaining
>
> Sets a modifier for a subsequent event emission that the **event** will only be ***broadcasted*** to clients that have **joined** the given `room` (**the socket itself being excluded**).
>
> To emit to multiple rooms, you can call `to` several times.
>
> ```js
> io.on('connection', (socket) => {
> 
>   // to one room
>   socket.to('others').emit('an event', { some: 'data' });
> 
>   // to multiple rooms
>   socket.to('room1').to('room2').emit('hello');
> 
>   // a private message to another socket
>   socket.to(/* another socket id */).emit('hey');
> 
>   // WARNING: `socket.to(socket.id).emit()` will NOT work, as it will send to everyone in the room
>   // named `socket.id` but the sender. Please use the classic `socket.emit()` instead.
> });
> ```
>
> **Note:** acknowledgements are not supported when broadcasting.

### client socket

```js
import openSocket from 'socket.io-client'
const socket = openSocket('http://localhost:8080');
socket.on('posts', data => {
    if (data.action === 'create') this.addPost()
})
```

# Cheat sheet (socket.io)

## Server only

### **Send** an event from the **server** to  socket

```js
socket.emit() //sending socket only
socket.send() //sending socket only
socket.broadcast.emit() //broadcast exclude sending socket
```

**Send** an event from a server **socket** to a **room**:

**broadcast** (exclude sending socket)

```js
socket.to(roomName).emit() //broadcast exclude sending socket
socket.in(roomName).emit() //broadcast exclude sending socket
```

Each **socket has it's own room**, named by it's **socket.id**, a socket can send a message to another socket:

```js
socket.to(anotherSocketId).emit('hey');
socket.in(anotherSocketId).emit('hey');
```

### A **namespace** **send** a message to **room**

```js
io.of(aNamespace).to(roomName).emit() // broadcast to room
io.of(aNamespace).in(roomName).emit() // broadcast to room
```

### A **namespace** can **send** a message to the entire **namespace**

```js
io.emit() //broadcast to namespace
io.of('/').emit() //broadcast to namespace
io.of('/admin').emit() //broadcast to namespace
```

# Chat app flow

![image-20200908102144586](assets/websockets/image-20200908102144586.png)

![image-20200908102741781](assets/websockets/image-20200908102741781.png)
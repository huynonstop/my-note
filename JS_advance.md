# JAVASCRIPT  advance

## hoisting

Hoisting is a term used to explain the behavior of variable declarations in your code. Variables declared or initialized with the `var` keyword will have their declaration "moved" up to the top of their module/function-level scope, which we refer to as hoisting

```javascript
console.log(foo); // undefined
var foo = "foo";

console.log(baz); // ReferenceError: can't access lexical declaration 'baz' before initialization
let baz = "baz";

console.log(bar); // ReferenceError: can't access lexical declaration 'bar' before initialization
const bar = "bar";

// Function Declaration
console.log(foo); // [Function: foo]
foo(); // 'FOOOOO'
function foo() {
  console.log("FOOOOO");
}
console.log(foo); // [Function: foo]

// Function Expression
console.log(bar); // undefined
bar(); // Uncaught TypeError: bar is not a function
var bar = function() {
  console.log("BARRRR");
};
console.log(bar); // [Function: bar]

```

Both function declaration and variable declarations are hoisted to the top of the containing scope. And function declaration takes precedence over variable declarations (but not over variable assignment).![img](https://images.viblo.asia/e70b623f-c197-4435-b1cc-13bde4dd0a6a.png)![img](https://images.viblo.asia/af1f1df2-cd4b-4612-afa3-a1750d79d271.png)

## let vs const

no hoisting

```javascript
x; // undefined
y; // Reference error: y is not defined

var x = "local";
let y = "local";
```

cannot re-declare

```javascript
var foo = "foo";
var foo = "bar";
console.log(foo); // "bar"

let baz = "baz";
let baz = "qux"; // Uncaught SyntaxError: Identifier 'baz' has already been declared
```

const cannot reassigning the variable's value

```javascript
// This is fine.
let foo = "foo";
foo = "bar";

// This causes an exception.
const baz = "baz";
baz = "qux";
```

not exist outside block scope  (if, for)

```javascript
function foo() {
  // All variables are accessible within functions.
  var bar = "bar";
  let baz = "baz";
  const qux = "qux";

  console.log(bar); // bar
  console.log(baz); // baz
  console.log(qux); // qux
}

console.log(bar); // ReferenceError: bar is not defined
console.log(baz); // ReferenceError: baz is not defined
console.log(qux); // ReferenceError: qux is not defined
if (true) {
  var bar = "bar";
  let baz = "baz";
  const qux = "qux";
}

// var declared variables are accessible anywhere in the function scope.
console.log(bar); // bar
// let and const defined variables are not accessible outside of the block they were defined in.
console.log(baz); // ReferenceError: baz is not defined
console.log(qux); // ReferenceError: qux is not defined
```

## Scope

Scope refers to the visibility of variables

In JavaScript, each function gets its own *scope*. Scope is basically a collection of variables as well as the rules for how those variables are accessed by name. Only code inside that function can access that function's scoped variables.

A variable name has to be unique within the same scope. A scope can be nested inside another scope. 

### Global Scope

```javascript
var name = 'Hammad';

console.log(name); // logs 'Hammad'

function logName() {
    console.log(name); // 'name' is accessible here and everywhere else
}
```

### Local Scope / Function scope

```javascript
// Global Scope
function someFunction() {
    // Local Scope #1
    function someOtherFunction() {
        // Local Scope #2
    }
}

// Global Scope
function anotherFunction() {
    // Local Scope #3
}
// Global Scope
```

### Block scope

```javascript
if (true) {
    // this 'if' conditional block doesn't create a scope

    // name is in the global scope because of the 'var' keyword
    var name = 'Hammad';
    // likes is in the local scope because of the 'let' keyword
    let likes = 'Coding';
    // skills is in the local scope because of the 'const' keyword
    const skills = 'JavaScript and PHP';
}
console.log(name); // logs 'Hammad'
console.log(likes); // Uncaught ReferenceError: likes is not defined
console.log(skills); // Uncaught ReferenceError: skills is not defined
while() {}
for() {}
```

### Scope chaining

```js
function A(x) {
  function B(y) {
    function C(z) {
      console.log(x + y + z);
    }
    C(3);
  }
  B(2);
}
A(1); // logs 6 (1 + 2 + 3)
```

In this example, `C` accesses `B`'s `y` and `A`'s `x`. This can be done because:

1. `B` forms a closure including `A`, i.e. `B` can access `A`'s arguments and variables.
2. `C` forms a closure including `B`.
3. Because `B`'s closure includes `A`, `C`'s closure includes `A`, `C` can access both `B` *and* `A`'s arguments and variables. In other words, `C` *chains* the scopes of `B` and `A` in that order.

The reverse, however, is not true. `A` cannot access `C`, because `A` cannot access any argument or variable of `B`, which `C` is a variable of. Thus, `C` remains private to only `B`.

### Lexical Scope/ Static Scope

Lexical Scope means that in a nested group of functions, the inner functions have access to the variables and other resources of their parent scope.

**Lexical Scoping** defines how variable names are resolved in nested functions: **inner functions contain the scope of parent functions even if the parent function has returned**

```javascript
function grandfather() {
    var name = 'Hammad';
    // likes is not accessible here
    function parent() {
        // name is accessible here
        // likes is not accessible here
        function child() {
            // Innermost level of the scope chain
            // name is also accessible here
            var likes = 'Coding';
        }
    }
}
```

## Context

*Context* is used to refer to the value of `this` in some particular part of your code.

1. If the `new` keyword is used when calling the function, `this` inside the function is a brand new object.
2. If `apply`, `call`, or `bind` are used to call/create a function, `this` inside the function is the object that is passed in as the argument.
3. If a function is called as a method, such as `obj.method()` — `this` is the object that the function is a property of.
4. If a function is invoked as a free function invocation, meaning it was invoked without any of the conditions present above, `this` is the global object. In a browser, it is the `window` object. If in strict mode (`'use strict'`), `this` will be `undefined` instead of the global object.
5. If multiple of the above rules apply, the rule that is higher wins and will set the `this` value.
6. If the function is an ES2015 arrow function, it ignores all the rules above and receives the `this` value of its surrounding scope at the time it is created.

Scope refers to the visibility of variables and context refers to the value of `this` ref to the object contain executing code

In the global scope context is always the Window object.

```javascript
// logs: Window {speechSynthesis: SpeechSynthesis, caches: CacheStorage, localStorage: Storage…}
console.log(this);

function logFunction() {
    console.log(this);
}
// logs: Window {speechSynthesis: SpeechSynthesis, caches: CacheStorage, localStorage: Storage…}
// because logFunction() is not a property of an object
logFunction(); 
new logFunction() // foo

var obj = {
    foo: function() {
        return this;   
    }
};
obj.foo() === obj; // true
```

If scope is in the method of an object, context will be the object the method is part of.

```javascript
class User {
    logName() {
        console.log(this);
    }
}

(new User).logName(); // logs User {}

function logFunction() {
    console.log(this);
}

new logFunction(); // logs logFunction {}
```

### Changing Context with .call(), .apply() and .bind()

Both `.call` and `.apply` are used to invoke functions and the first parameter will be used as the value of `this` within the function

`.call` takes in comma-separated arguments as the next arguments while `.apply` takes in an array of arguments as the next argument

The `bind()` method creates a new function that, when called, has its `this` keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called.

```javascript
function introduce(name, interest) {
    console.log('Hi! I\'m '+ name +' and I like '+ interest +'.');
    console.log('The value of this is '+ this +'.')
}

introduce('Hammad', 'Coding'); // the way you usually call it
introduce.call(window, 'Batman', 'to save Gotham'); // pass the arguments one by one after the contextt
introduce.apply('Hi', ['Bruce Wayne', 'businesses']); // pass the arguments in an array after the context
introduce.bind(window, 'Batman', 'to save Gotham')()// not invoke after bind

// Output:
// Hi! I'm Hammad and I like Coding.
// The value of this is [object Window].

// Hi! I'm Batman and I like to save Gotham.
// The value of this is [object Window].

// Hi! I'm Bruce Wayne and I like businesses.
// The value of this is Hi.

// Hi! I'm Hammad and I like Cosmology.
// The value of this is [object Window].
```

### Arrow function context (no context no this)

The `this` within arrow functions is also bound to the enclosing scope which is different compared to regular functions where the `this` is determined by the object calling it. Lexically-scoped `this` is useful when invoking callbacks especially in React components.

The main takeaway here is that `this` can be changed for a normal function, but the context always stays the same for an arrow function

``` 	javascript
var a = {
	name: 'AAA'
	run: function() {
		var run2 = function() {
			console.log(this.name)
		}.bind(this)
		run2()
	} 
}
// Or

var a = {
	name: 'AAA'
	run: function() {
		var run2 = () => {console.log(this.name)
		run2()
	} 
}
```

## Execution Context

JavaScript is a single-threaded language so it can only execute a single task at a time. The rest of the tasks are queued in the Execution Context.

JavaScript interpreter starts to execute your code, the context (scope) is by default set to be global. This global context is appended to your execution context which is actually the first context that starts the execution context.

After that, each function call (invocation) would append its context to the execution context. The same thing happens when an another function is called inside that function or somewhere else.

### Creation Phase

The first phase that is the creation phase is present when a function is called but its code is not yet executed. Three main things that happen in the creation phase are:

- Creation of the Variable (Activation) Object,

  When a function is called, the interpreter scans it for all resources including function arguments, variables, and other declarations. Everything, when packed into a single object, becomes the the Variable Object.

  ```javascript
  'variableObject': {
      // contains function arguments, inner variable and function declarations
  }
  ```

- Creation of the Scope Chain, and

  JavaScript always starts at the innermost level of the code nest and keeps jumping back to the parent scope until it finds the variable or any other resource it is looking for.

  ```javascript
  'scopeChain': {
      // contains its own variable object and other variable objects of the parent execution contexts
  }
  ```

- Setting of the value of context (`this`)

#### The Execution Context Object

```javascript
executionContextObject = {
    'scopeChain': {}, // contains its own variableObject and other variableObject of the parent execution contexts
    'variableObject': {}, // contains function arguments, inner variable and function declarations
    'this': valueOfThis
}
```

### Code Execution Phase

Other values are assigned and the code is finally executed.

## Closure

- The inner function can be accessed only from statements in the outer function.

- The inner function forms a closure: the inner function can use the arguments and variables of the outer function, while the outer function cannot use the arguments and variables of the inner function.

A closure gives you access to an outer function’s scope from an inner function. In JavaScript, closures are created every time a function is created, at function creation time.

To use a closure, define a function inside another function and expose it. To expose a function, return it or pass it to another function. The inner function will have access to the variables in the outer function scope, even after the outer function has returned.

Closures are functions that have access to the outer (enclosing) function's variables—scope chain even after the outer function has returned.

A closure can not only access the variables defined in its outer function but also the arguments of the outer function.

```javascript
function sum(a,b){
    const c = a + b;
    return function() {
        console.log(c) // closure
    }
}

function greet() {
    name = 'Hammad';
    return function () {
        console.log('Hi ' + name);
    }
}

greet(); // nothing happens, no errors
// the returned function from greet() gets saved in greetLetter
greetLetter = greet();
 // calling greetLetter calls the returned function from the greet() function
greetLetter(); // logs 'Hi Hammad'
greet()(); // logs 'Hi Hammad'

function counter() {
  var _counter = 0;
  // return an object with several functions that allow you
  // to modify the private _counter variable
  return {
    add: function(increment) { _counter += increment; },
    retrieve: function() { return 'The counter is currently at: ' + _counter; }
  }
}

// error if we try to access the private variable like below
// _counter;

// usage of our counter function
var c = counter();
c.add(5); 
c.add(9); 

// now we can access the private variable in the following way
c.retrieve(); // => The counter is currently at: 14
```

## Callback

A function is passed as an argument

Delay execution of a function until a timer later on

## Higher order function (receive function as arguments (callback) or return a function)

A **higher order function** is a function that takes a function as an argument, or returns a function

## .forEach() vs .map()

**`forEach`**

- Iterates through the elements in an array.
- Executes a callback for each element.
- Does not return a value.

```javascript
const a = [1, 2, 3];
const doubled = a.forEach((num, index) => {
  // Do something with num and/or index.
});

// doubled = undefined
```

**`map`**

- Iterates through the elements in an array.
- "Maps" each element to a new element by calling the function on each element, creating a new array as a result.

```javascript
const a = [1, 2, 3];
const doubled = a.map(num => {
  return num * 2;
});

// doubled = [2, 4, 6]
```

The main difference between `.forEach` and `.map()` is that `.map()` returns a new array. If you need the result, but do not wish to mutate the original array, `.map()` is the clear choice. If you simply need to iterate over an array, `forEach` is a fine choice.

## Curry function

Currying is a pattern where a function with more than one parameter is broken into multiple functions that, when called in series, will accumulate all of the required parameters one at a time. This technique can be useful for making code written in a functional style easier to read and compose. It's important to note that for a function to be curried, it needs to start out as one function, then broken out into a sequence of functions that each accepts one parameter.

```javascript
function curry(fn) {
  if (fn.length === 0) {
    return fn;
  }

  function _curried(depth, args) {
    return function(newArgument) {
      if (depth - 1 === 0) {
        return fn(...args, newArgument);
      }
      return _curried(depth - 1, [...args, newArgument]);
    };
  }

  return _curried(fn.length, []);
}

function add(a, b) {
  return a + b;
}

var curriedAdd = curry(add);
var addFive = curriedAdd(5);

var result = [0, 1, 2, 3, 4, 5].map(addFive); // [5, 6, 7, 8, 9, 10]
```

## How JS work

+ JavaScript is a synchronous, blocking, single-threaded language. That just means that only one operation can be in progress at a time and all code is executed in a sequence

  + single-threaded: one call stack, one command is being executed at a time

  + blocking: waiting complete then move on

  + synchronous:  one line of code is being executed at a time in order the code appears

    ### Problem?

    When use this on browser to handle requests, the browser stuck, until the request done you can not see the browser render anything. Because if there is some requests in the call stack, it can not do the render stuff, it freeze, the request **block** the stack.

+ It is not JavaScript that does these asynchronous operations; to put it simply, the underlying engine does it.

  ### How we can do asynchronous 

  JS runtime environment provide us **APIs** to work with threads in **parallel**. From **call stack**, if an API call, it will be sent to the **API container** to run and pop it off the stack. When any item in the container is done, the item's **asynchronous callback function** is sent to the end of the **callback queue**. **Event loop** job is when the stack empty, take one item in queue and push to the stack.

  ![image-20200209104121348](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200209104121348.png)

+ JS Engine: callstack, memory heap

+ JS runtime environment: event loop, web api's, task/callback queue, job/micro-task queue

  ![img](https://miro.medium.com/max/700/1*zeKjWCjyAGZ9JN4fvnWsiA.png)

  + Browser:![image-20200209102003696](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200209102003696.png)

  + Node:

    + No script parsing event (<script> tag)

    + No user interaction (clicking on the page)

    + No animation frame callbacks, no render (No DOM **Manipulation**)

      ![image-20200209103048426](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200209103048426.png)

  + Web worker:

    + No script parsing event (<script> tag)
    + No user interaction
    + No DOM **Manipulation**

  

  *AJAX, the DOM tree, and other API’s, are not part of JavaScript (Engine), they are just objects with properties and methods, provided by the browser and made available in the browser’s JS Runtime Environment.*

  Also in the runtime environment is a **JavaScript Engine** that parses the code

  

## Asynchronous

### **Asynchronous** CallBack (phone number to restaurant)

(function run after finish an async job, put in the callback queue) != Callback (function is passed in another function, put in the call stack)

CallBack in CallBack => CallBack hell => make function

**Give callback to third-party service make you lose control**

![image-20200211174140282](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200211174140282.png)

### Promise ( job/micro-task queue)  (buzzer![Kết quả hình ảnh cho buzzer restaurant](https://cdn.hswstatic.com/gif/restaurant-pager-ch.jpg))

A promise is an object that may produce a single value some time in the future: either a resolved value, or a reason that it’s not resolved (an asynchronous task that will eventually finish)

Promises are eager, meaning that a promise will start doing whatever task you give it as soon as the promise constructor is invoked

#### How it work

A promise is an object which can be returned synchronously from an asynchronous function. It will be in one of 3 possible states:

- **Fulfilled:** `onFulfilled()` will be called (e.g., `resolve()` was called)
- **Rejected:** `onRejected()` will be called (e.g., `reject()` was called)
- **Pending:** not yet fulfilled or rejected

Once settled (*not pending* = it has been resolved or rejected), a promise can not be resettled. Calling `resolve()` or `reject()` again will have no effect. The immutability of a settled promise is an important feature.

```javascript
const wait = time => new Promise((resolve) => setTimeout(resolve, time));
wait(3000).then(() => console.log('Hello!')); // 'Hello!'
```

#### Rule

- A promise or “thenable” is an object that supplies a standard-compliant `.then()` method.

  ```javascript
  promise.then(
    onFulfilled?: Function,
    onRejected?: Function
  ) => Promise
  ```

  The `.then()` method must comply with these rules:

  - Both `onFulfilled()` and `onRejected()` are optional.
  - If the arguments supplied are not functions, they must be ignored.
  - `onFulfilled()` will be called after the promise is fulfilled, with the promise’s value as the first argument.
  - `onRejected()` will be called after the promise is rejected, with the reason for rejection as the first argument. The reason may be any valid JavaScript value, but because rejections are essentially synonymous with exceptions, I recommend using Error objects.
  - Neither `onFulfilled()` nor `onRejected()` may be called more than once.
  - `.then()` may be called many times on the same promise. In other words, a promise can be used to aggregate callbacks.
  - `.then()` must return a new promise, `promise2`.
  - If `onFulfilled()` or `onRejected()` return a value `x`, and `x` is a promise, `promise2` will lock in with (assume the same state and value as) `x`. Otherwise, `promise2` will be fulfilled with the value of `x`.
  - If either `onFulfilled` or `onRejected` throws an exception `e`, `promise2` must be rejected with `e` as the reason.
  - If `onFulfilled` is not a function and `promise1` is fulfilled, `promise2` must be fulfilled with the same value as `promise1`.
  - If `onRejected` is not a function and `promise1` is rejected, `promise2` must be rejected with the same reason as `promise1`.

- A pending promise may transition into a fulfilled or rejected state.

- A fulfilled or rejected promise is settled, and must not transition into any other state.

- Once a promise is settled, it must have a value (which may be `undefined`). That value must not change.

![image-20200209110806185](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200209110806185.png)

![image-20200209110928464](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200209110928464.png)

![img](https://firebasestorage.googleapis.com/v0/b/anonystick-83a85.appspot.com/o/img%2F1552272366081?alt=media&token=abccf254-336e-40be-9976-8e30ff511136)

```javascript
console.log('before')
const promise = new Promise(function fn(resolve, reject) {
  console.log('hello')
  // ...
});
console.log('after')
//before
//hello
//after

function get(url) {
  // Return a new promise.
  return new Promise(function(resolve, reject) {
    // Do the usual XHR stuff
    var req = new XMLHttpRequest();
    req.open('GET', url);

    req.onload = function() {
      // This is called even on 404 etc
      // so check the status
      if (req.status == 200) {
        // Resolve the promise with the response text
        resolve(req.response);
      }
      else {
        // Otherwise reject with the status text
        // which will hopefully be a meaningful error
        reject(Error(req.statusText));
      }
    };

    // Handle network errors
    req.onerror = function() {
      reject(Error("Network Error"));
    };

    // Make the request
    req.send();
  });
}
//Call
get('story.json').then(function(response) {
  console.log("Success!", response);
}, function(error) {
  console.error("Failed!", error);
})

//
[promise1, promise2, promise3].reduce(function(currentPromise, promise) {
  return currentPromise.then(promise)
}, Promise.resolve())

// Đoạn ở trên khi được viết dài dòng ra
Promise.resolve().then(promise1).then(promise2).then(promise3)
```

#### Error

```javascript
save()
  .then(handleSuccess)
  .catch(handleError)
;
```

![img](https://miro.medium.com/max/300/1*vRaV9sYpYKdxBj3Ld7KM1Q.png)

#### What are the pros and cons of using Promises instead of callbacks?

**Pros**

- Avoid callback hell which can be unreadable.
- Makes it easy to write sequential asynchronous code that is readable with `.then()`.
- Makes it easy to write parallel asynchronous code with `Promise.all()`.
- With promises, these scenarios which are present in callbacks-only coding, will not happen:
  - Call the callback too early
  - Call the callback too late (or never)
  - Call the callback too few or too many times
  - Fail to pass along any necessary environment/parameters
  - Swallow any errors/exceptions that may happen

**Cons**

- Slightly more complex code (debatable).
- In older browsers where ES2015 is not supported, you need to load a polyfill in order to use it.

### Async/Await 

Async functions are functions that return promises

```javascript
// Async/Await version
async function helloAsync() {
  return "hello";
}// Promises version
function helloAsync() {
  return new Promise(function (resolve) {
    resolve("hello");
  });
}
```

Reuse value of Async method

Write Async code in a sync way

#### How it works

- There are **Async Functions**. These are declared by prepending the word `async` in their declaration `async function doAsyncStuff() { ...code }`
- Your code can be **paused** waiting for an **Async Function** with `await`
- `await` returns whatever the async function returns when it is done.
- `await` can only be used inside an `async function`.
- If an **Async Function** throws an exception, the exception will bubble up to the parent functions just like in normal Javascript, and can be caught with `try/catch`. But there’s a catch *(again, pun unintended)*: Just like in **Promises,** **exceptions will get swallowed** if they are not caught somewhere in the chain. This means that you should always `try/catch`
  wherever a chain of **Async Function** calls begins. It is a good practice to always have one `try/catch` per chain, unless not doing this is absolutely necessary. This will provide one single place to deal with errors while doing async work and will force you to correctly chain your **Async Function** calls.

![image-20200211180859185](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200211180859185.png)

```javascript
function printAll(){
  printString("A")
  .then(() => printString("B"))
  .then(() => printString("C"))
}
printAll()

async function printAll(){
  await printString("A")
  await printString("B")
  await printString("C")
}
printAll()
```

#### Await async function ?

```javascript
// Option 1: now wait
doManyThings();
// Option 2: Call it inside another Async Function wrapped with a try/catch block.
(async function() {
  try {
    await doManyThings();
  } catch (err) {
    console.error(err);
  }
})();
// Option 3: Use it as a Promise.
doManyThings().then((result) => {  // Do the things that need to wait for our function}).catch((err) => {
  throw err;
});
```

## Private scope

In JavaScript, there is no such thing as public or private scope. However, we can emulate this feature using closures.

```javascript
(function () {
  // private scope
})();
```

### The Module Pattern

```javascript
var Module = (function() {
    function privateMethod() {
        // do something
    }

    return {
        publicMethod: function() {
            // can call privateMethod();
        }
    };
})();
Module.publicMethod(); // works
Module.privateMethod(); // Uncaught ReferenceError: privateMethod is not defined

var Module = (function () {
    function _privateMethod() {
        // do something
    }
    function publicMethod() {
        // do something
    }
    return {
        publicMethod: publicMethod,
    }
})();
```

### Anonymous function

They can be used in IIFEs to encapsulate some code within a local scope so that variables declared in it do not leak to the global scope.

```javascript
(function() {
  // Some code here.
})();
```

As a callback that is used once and does not need to be used anywhere else. The code will seem more self-contained and readable when handlers are defined right inside the code calling them, rather than having to search elsewhere to find the function body.

```javascript
setTimeout(function() {
  console.log("Hello world!");
}, 1000);
```

Arguments to functional programming constructs or Lodash (similar to callbacks).

```javascript
const arr = [1, 2, 3];
const double = arr.map(function(el) {
  return el * 2;
});
console.log(double); // [2, 4, 6]
```

### Immediately-Invoked Function Expression (IIFE)

An IIFE is an anonymous function that is created and then immediately invoked. It’s not called from anywhere else (hence why it’s anonymous), but runs just after being created.

```javascript
function foo(){ }();// not working

(function () {return 5;} ());
```

## Composition over inheritance

```
User => need eat(),sleep(),play()
  email
  username
  pets
  friends
  adopt()
  befriend()

Animal
  name
  energy
  eat()
  sleep()
  play()

  Dog
    breed
    bark()

  Cat
    declawed
    meow()
```

```
FarmFantasy => God-obj
  name
  play()
  sleep()
  eat()

  User
    email
    username
    pets
    friends
    adopt()
    befriend()

  Animal
    energy

    Dog
      breed
      bark()

    Cat
      declawed
      meow()
```

Inheritance makes us turn a blind eye to the inevitable fact that our class structure will most likely change in the future, and when it does, our tightly coupled inheritance structure is going to crumble.

=> Rather than thinking in terms of what things **are**, what if we think in terms of what things **do**?

```javascript
//Obj with action
const eater = (state) => ({
  eat(amount) {
    console.log(`${state.name} is eating.`)
    state.energy += amount
  }
})
const sleeper = (state) => ({
  sleep(length) {
    console.log(`${state.name} is sleeping.`)
    state.energy += length
  }
})

const player = (state) => ({
  play() {
    console.log(`${state.name} is playing.`)
    state.energy -= length
  }
})

const barker = (state) => ({
  bark() {
    console.log('Woof Woof!')
    state.energy -= .1
  }
})

const meower = (state) => ({
  meow() {
    console.log('Meow!')
    state.energy -= .1
  }
})

const adopter = (state) => ({
  adopt(pet) {
    state.pets.push(pet)
  }
})

const friender = (state) => ({
  befriend(friend) {
    state.friends.push(friend)
  }
})
//Composing
function Cat (name, energy, declawed) {
  let cat = {
    name,
    energy,
    declawed,
    friends: []
  }

  return Object.assign(
    cat,
    eater(cat),
    sleeper(cat),
    player(cat),
    meower(cat),
  )
}
//OR
function Cat (name, energy, declawed) {
  this.name = name
  this.energy = energy
  this.declawed = declawed
  this.friends = []

  return Object.assign(
    this,
    eater(this),
    sleeper(this),
    player(this),
    meower(this),
  )
}

const charles = new Cat('Charles', 10, false)
```

## Template string 

```javascript
`${name}`
```

## Array-like object  and Arguments

```javascript
// Array-like object
const obj = {
    0: 'a',
    1: 'b',
    2: 'c',
    //index : value
    length: 3
}

for( let i = 0;i < obj.length; i++){
    console.log(obj[i])
}
// Arguments is Array-like object
function sum() {
    let rs = 0
    for (let i = 0; i < arguments.length;i++){
        rs += arguments[i]
    }
    return rs;
}
sum(1,2,3,4,5) // 15
```

## Default parameters

## ...rest (function parameters) => to an array

a shorthand for including an arbitrary number of arguments to be passed to a function. It is like an inverse of the spread syntax, taking data and stuffing it into an array rather than unpacking an array of data, and it works in function arguments, as well as in array and object destructuring assignments.

```javascript
function sum(...nums) {
    return nums.reduce((a,b) => a+b) //nums: [1,2,3,4,5]
}
sum(1,2,3,4,5)

function concat(separator, ...strings) {
    return string.join(separator);
}
```



## ...spread 

very useful when coding in a functional paradigm as we can easily create copies of arrays or objects

```javascript
const a = [1,2,3]
const b = [0,...a,4] // [0,1,2,3,4]

const obj1 = {
    a: 1,
    b: { e: 10}
}
let obj2 = {
    ...obj1 //shallow-cloning
}
obj2.b.e = 1
obj2.a = 2
//obj1={a:1,b:{e:1}}
//obj1={a:2,b:{e:1}}
```

## destructuring 

**Array destructuring**

```javascript
// Variable assignment.
const foo = ["one", "two", "three"];

const [one, two, three] = foo;
console.log(one); // "one"
console.log(two); // "two"
console.log(three); // "three"
// Swapping variables
let a = 1;
let b = 3;

[a, b] = [b, a];
console.log(a); // 3
console.log(b); // 1
```

**Object destructuring**

```javascript
// Variable assignment.
const o = { p: 42, q: true };
const { p, q } = o;

console.log(p); // 42
console.log(q); // true
```

## Immutable 

An immutable object is an object whose state cannot be modified after it is created.

In JavaScript, some built-in types (numbers, strings) are immutable, but custom objects are generally mutable.

Some built-in immutable JavaScript objects are MATH,DATE

#### What are the pros and cons of immutability?

**Pros**

- Easier change detection - Object equality can be determined in a performant and easy manner through referential equality. This is useful for comparing object differences in React and Redux.
- Programs with immutable objects are less complicated to think about, since you don't need to worry about how an object may evolve over time.
- Defensive copies are no longer necessary when immutable objects are returning from or passed to functions, since there is no possibility an immutable object will be modified by it.
- Easy sharing via references - One copy of an object is just as good as another, so you can cache objects or reuse the same object multiple times.
- Thread-safe - Immutable objects can be safely used between threads in a multi-threaded environment since there is no risk of them being modified in other concurrently running threads.
- Using libraries like ImmmutableJS, objects are modified using structural sharing and less memory is needed for having multiple objects with similar structures.

**Cons**

- Naive implementations of immutable data structures and its operations can result in extremely poor performance because new objects are created each time. It is recommended to use libraries for efficient immutable data structures and operations that leverage on structural sharing.
- Allocation (and deallocation) of many small objects rather than modifying existing ones can cause a performance impact. The complexity of either the allocator or the garbage collector usually depends on the number of objects on the heap.
- Cyclic data structures such as graphs are difficult to build. If you have two objects which can't be modified after initialization, how can you get them to point to each other?

```javascript
// Array Example
const arr = [1, 2, 3];
const newArr = [...arr, 4]; // [1, 2, 3, 4]

// Object Example
const human = Object.freeze({ race: "human" });
const john = { ...human, name: "John" }; // {race: "human", name: "John"}
const alienJohn = { ...john, race: "alien" }; // {race: "alien", name: "John"}
```

## 
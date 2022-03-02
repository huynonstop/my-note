# JS advance

[leonardomso/33-js-concepts: 📜 33 concepts every JavaScript developer should know. (github.com)](https://github.com/leonardomso/33-js-concepts)

[lydiahallie/javascript-questions: A long list of (advanced) JavaScript questions, and their explanations (github.com)](https://github.com/lydiahallie/javascript-questions)

[ryanmcdermott/clean-code-javascript: Clean Code concepts adapted for JavaScript (github.com)](https://github.com/ryanmcdermott/clean-code-javascript)

[You-Dont-Know-JS/README.md at 2nd-ed · getify/You-Dont-Know-JS (github.com)](https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/get-started/README.md)

[You-Dont-Know-JS/README.md at 1st-ed · getify/You-Dont-Know-JS (github.com)](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/README.md)

## Context

***Context* is used to refer to the value of `this` in your code.**

If the `new` keyword is used when calling the function, `this` inside the function is a brand new object.

If `apply`, `call`, or `bind` are used to call/create a function, `this` inside the function is the object that is passed in as the argument.

If a function is called as a method, such as `obj.method()` — `this` is the object that the function is a property of.

If a function is invoked as a free function invocation, meaning it was invoked without any of the conditions present above, `this` is the global object. In a browser, it is the `window` object. If in strict mode (`'use strict'`), `this` will be `undefined` instead of the global object.

If multiple of the above rules apply, the rule that is higher wins and will set the `this` value.

If the function is an arrow function, it ignores all the rules above and receives the `this` value of its surrounding scope at the time it is created

```javascript
//In the global scope context is the Window object.
console.log(this);
//Window {speechSynthesis: SpeechSynthesis, caches: CacheStorage, localStorage: Storage…}
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

## Changing Context with method

Both `.call()` and `.apply()` are used to **invoke** functions and the first parameter will be used as the value of `this` within the function

The `bind()` method **creates** a new function that, when called, has its `this` keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called.

```javascript
function introduce(name, interest) {
    console.log('Hi! I\'m '+ name +' and I like '+ interest +'.');
    console.log('The value of this is '+ this +'.')
}

introduce('Hammad', 'Coding'); // the way you usually call it
introduce.call(window, 'Batman', 'to save Gotham'); // pass the arguments one by one after the contextt
introduce.apply('Hi', ['Bruce Wayne', 'businesses']); // pass the arguments in an array after the context
introduce.bind(window, 'Hammad', 'Cosmology')()// not invoke after bind

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

## Arrow function have no context

The main takeaway here is that `this` can be changed for a normal function, but the context always stays the same for an arrow function

```  javascript
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

*Execution context* equates to the 'environment' a function executes in; that is, variable scope (and the *scope chain*, variables in closures from outer scopes), function arguments, and the value of the `this` object.

The *call stack* is a collection of execution contexts.

![JS__advance-2022-02-06-14-11-50](/assets/JS__advance/JS__advance-2022-02-06-14-11-50.png)

![image-20200612164941745](assets/JS__advance/image-20200612164941745.png)

## Creation phase

- JS Engine parses - run through your code & `identifies variables & functions` created by code (which will be used in execution phase)
- Setup memory space for Variables & Functions

![image-20200612165628524](assets/JS__advance/image-20200612165628524.png)

### Hoisting

Hoisting is a term used to explain the behavior of variable declarations in your code. Variables declared or initialized with the `var` keyword will have their declaration "moved" up to the top of their module/function-level scope, which we refer to as hoisting

```javascript
// var
console.log(foo); // undefined
var foo = "foo";
/*
 Code above behavior like this
 
 Creation phase
 Setup memory for: 
 var foo
 => can reference but no value (undefined)
 
 Execute phase
 console.log(foo); (no set value yet => undefined) 
 foo = "foo" => set value here
 => value ready
*/
```

```js
// Function Declaration
console.log(foo); // [Function: foo]
foo(); // 'FOOOOO'
function foo() { 
  console.log("FOOOOO");
}
console.log(foo); // [Function: foo]
/*
 Creation phase
 Setup function memmory (reference and code) for: 
 function foo() { 
  console.log("FOOOOO");
 }
 => ready to call
 
 Execute phase
 console.log(foo); // [Function: foo]
 foo(); // 'FOOOOO'
 console.log(foo); // [Function: foo]
*/
```

```js
// let vs const
console.log(baz); // ReferenceError: can't access before initialization
let baz = "baz";
console.log(bar); // ReferenceError: can't access before initialization
const bar = "bar";
```

## Execution phase

- When the code is executed line-by-line it can access the variables defined inside Execution Context
- variable assignment are done in this phase

Run code in single thread and synchronously

![image-20200612172016928](assets/JS__advance/image-20200612172016928.png)

## Execution/Call stack

Whenever you write a function in JavaScript, the JS engine creates what we call function execution context. Also, each time the JS engine begins, it creates a global execution context that holds the global objects — for example, the `window` object in the browser and the `global` object in Node.js. Both these contexts are handled in JS using a stack data structure also known as the execution stack.

```js
function a() {
  console.log("i am a")
  b()
}

function b() {
  console.log("i am b")
}

a()
```

The JavaScript engine first creates a global execution context and pushes it into the execution stack.

Then it creates a function execution context for the function `a()`. Since `b()` is called inside `a()`, it will create another function execution context for `b()` and push it into the stack.

![image-20200830112444561](assets/JS__advance/image-20200830112444561.png)

When the function `b()` returns, the engine destroys the context of `b()`, and when we exit function `a()`, the context of `a()` is destroyed.

## Scope

[Chính vì Lexical scope là gì? Mà thằng miền trung nói gì thằng Miền Nam không hiểu? (anonystick.com)](https://anonystick.com/blog-developer/chinh-vi-lexical-scope-la-gi-ma-thang-mien-trung-noi-gi-thang-mien-nam-khong-hieu-2020112562757134)

[Scope in javascript - 66 khái niệm cần hiểu khi học lập trình javascript (anonystick.com)](https://anonystick.com/blog-developer/scope-in-javascript-66-khai-niem-can-hieu-khi-hoc-lap-trinh-javascript-2020112314026307)

The **scope** is where a variable is available in your code determines the way your data will travel throughout your application via **statements** and **expressions**.

**Scope** defines the way JavaScript resolves a variable at run time

```js
1: let val1 = 2
2: function multiplyThis(n) {
3:   let ret = n * val1
4:   return ret
5: }
6: let multiplied = multiplyThis(6)
7: console.log('example of scope:', multiplied)
```

we have variables in the local execution context and variables in the global execution context. One intricacy of JavaScript is how it looks for variables. If it can’t find a variable in its *local* execution context, it will look for it in *its* calling context. And if not found there in *its* calling context. Repeatedly, until it is looking in the *global* execution context. (And if it does not find it there, it’s `undefined`)

function can access to variables that are defined in its calling context. The formal name of this phenomenon is the **lexical scope**.

![image-20200612150147376](assets/JS__advance/image-20200612150147376.png)

A variable defined exclusively within the function cannot be accessed from outside the function or within other functions.

## Scope chain

```js
function b() {console.log(myVar)}
function a() {
    var myVar = 2;
    b()
}
var myVar = 1
a()
```

![image-20200612180323636](assets/JS__advance/image-20200612180323636.png)

```js
function a() {
    function b() {
        console.log(myVar)
    }
    // var myVar = 2; // if uncomment this line => 2
    b()
}
var myVar = 1
a() // 1
b() // not defined
```

![image-20200612180830321](assets/JS__advance/image-20200612180830321.png)

**"scope chain" that makes closures possible.**

## Function Arguments

Function arguments (parameters) work as local variables inside functions.

## The Lifetime of Variables

The lifetime of a JavaScript variable starts when it is declared.

Local variables are deleted when the function is completed.

In a web browser, global variables are deleted when you close the browser window (or tab).

## Closure

<https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36>

[Closures Explained in 100 Seconds // Tricky JavaScript Interview Prep - YouTube](https://www.youtube.com/watch?v=vKJpN5FAeF4)

Inner functions contain the scope of parent functions even if the parent function has returned (closure).

```js
 1: function createCounter() {
 2:   let counter = 0
 3:   const myFunction = function() {
 4:     counter = counter + 1
 5:     return counter
 6:   }
 7:   return myFunction
 8: }
 9: const increment = createCounter()
10: const c1 = increment()
11: const c2 = increment()
12: const c3 = increment()
13: console.log('example increment', c1, c2, c3)
```

when a function gets declared, it contains a function definition and a **closure**. The closure contains all the variables that are in scope at the time of creation of the function. It is analogous to a backpack.

A function definition comes with a little backpack. And in its pack it stores all the variables that were in scope at the time that the function definition was created.

So a function, every function in Javascript is a closure because it closes over the variables defined in its environment and it kind of memorizes them.

But since these functions were created in the global scope, they have access to all the variables in the global scope. And the closure concept is not really relevant.

When a function returns a function, that is when the concept of closures becomes more relevant. The returned function has access to variables that are not in the global scope, but they solely exist in its closure.

## Summary

A **closure** is the combination of a function bundled together (enclosed) with references to its surrounding state (the **lexical environment**). In other words, a closure gives you access to an **outer function’s scope** from an inner function even **that outer function has returned**. In JavaScript, closures are created every time a function is created, at function creation time.

![image-20200612211115251](assets/JS__advance/image-20200612211115251.png)

## Function Factory

A function return a function

![image-20200608220359309](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608220359309.png)

![image-20200612211304984](assets/JS__advance/image-20200612211304984.png)

## Higher order function

A **higher order function** is a function that takes a function as an argument, or returns a function

## Curry function

![image-20200612211507617](assets/JS__advance/image-20200612211507617.png)

Change a multiple arguments function into a chain of many single argument function

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
```

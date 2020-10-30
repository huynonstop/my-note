# Behind the scenes of class and object in JavaScript

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Details_of_the_Object_Model

## Constructor function vs factory function

https://medium.com/@chamikakasun/javascript-factory-functions-vs-constructor-functions-585919818afe

## Constructor function vs Classes

![image-20200602210506813](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200602210506813.png)

![image-20200602210528810](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200602210528810.png)

![image-20200602210252909](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200602210252909.png)

![image-20200612212210695](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612212210695.png)

![image-20200612212336865](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612212336865.png)

## new

 The `new` keyword allows us to use a function as a constructor. In addition it clones the `prototype` of the constructor and binds it to the `this` pointer of the constructor, returning `this` if no other object is returned

![image-20200211182818925](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200211182818925.png)

When a constructor call with new key work:

1. A new object will be created and set to this (by javascript engine)
2. this new object will have a prototype (by javascript engine)
3. if not return anything in function, **this** will be return (by javascript engine).

```javascript
function ConstructorExample() {
    // this = {};
    // this.__proto__ = ConstructorExample.prototype;
    console.log(this);
    this.value = 10;
    console.log(this);
    // return this;
}

new ConstructorExample();

// -> ConstructorExample {}
// -> ConstructorExample { value: 10 }
```

![image-20200612212123663](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612212123663.png)

### Difference between: function Person(){}, var person = Person(), and var person = new Person()

```javascript
function Person(name) { //function
  this.name = name;
}

var person = Person("John"); // invokes the Person as a function
console.log(person); // undefined
console.log(person.name); // Uncaught TypeError: Cannot read property 'name' of undefined

var person = new Person("John"); // creates an instance of the Person - constructor call
console.log(person); // Person { name: "John" }
console.log(person.name); // "john"

function Constructor() {}
o = new Constructor();
// is equivalent to: not real code
o = Object.create(Constructor.prototype);
```

## Inherit with Constructor function

`constructor` does not update automatically when a new object is created based on it. When creating a subclass, the correct constructor should be setup manually.

```js
//Base class
function AgedPerson() {}
AgedPerson.prototype.printAge = function() {
  console.log(this.age);
}
//Sub Class
function Person() {
   AgedPerson.call(this);
   this.age = 30;
   this.name = "huy"
}
Person.prototype = Object.create(AgedPerson.prototype);
Person.prototype.greet = function () {
    console.log('Hi, I am ' + this.name + ' and I am ' + this.age + ' years old.');
}

// The prototype has the wrong constructor
// Person.prototype.constructor === AgedPerson   // => true
// Person.prototype.constructor === Person // => false

// Fix the Person prototype constructor
Person.prototype.constructor = Person;

console.log(new Person())
```

![image-20200605212352374](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200605212352374.png)

## Class use  prototype

https://www.codecademy.com/forum_questions/50d3836d5a8a1f5d5c00085c

class is a sugar syntax for Constructor function

###  super keyword

The super keyword is used to call the parent class constructor

```js
class AgedPerson {
  printAge() {
    console.log(this.age);
  }
}

class Person extends AgedPerson {
  name = 'Max';

  constructor() {
    super(); // create an object base on parent and set as prototype of children class instance
    this.age = 30;
  }

  greet() { //this will add a method to Person.prototype 
    console.log('Hi, I am ' + this.name + ' and I am ' + this.age + ' years old.');
  }
}
console.log(new Person())
```

![image-20200603231505454](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200603231505454.png)

## Prototype

https://stackoverflow.com/questions/9959727/proto-vs-prototype-in-javascript

https://stackoverflow.com/questions/572897/how-does-javascript-prototype-work

https://www.javatpoint.com/javascript-oops-prototype-object

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Details_of_the_Object_Model#Object_properties

https://anonystick.com/blog-developer/prototype-la-gi-lap-trinh-huong-doi-tuong-oop-trong-javascript-2020070686855915

**A prototype is just a base blue print of object**

![image-20200602222126942](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200602222126942.png)

JavaScript is often described as a **prototype-based language** — to provide inheritance, objects can have a **`prototype` object**, which acts as a **template** **(fallback)** object that it inherits methods and properties from. 

![image-20200602224007200](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200602224007200.png)

When a function is created in JavaScript, the JavaScript engine adds a `prototype` property to the function. This `prototype` property is an object (called as prototype object) which has a `constructor` property by default. The `constructor` property points back to the function on which `prototype` object is a property. 

```js
function Human(firstName, lastName) {
	this.firstName = firstName,
	this.lastName = lastName,
	this.fullName = function() {
		return this.firstName + " " + this.lastName;
	}
}
```

![image-20200602231222250](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200602231222250.png)

![image-20200602223953825](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200602223953825.png)

This prototype (constructor `prototype` property) then automatically be assigned to the instance of the constructor upon creation (get the prototype object of an instance by using `__proto__ property` or  `Object.getPrototypeOf()`)

```js
var person = new Human("Virat", "Kohli");
Human.prototype === Object.getPrototypeOf(person) // true
Human.prototype === person.__proto__ //true
console.log(person)
```

![image-20200605213852666](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200605213852666.png)

![img](https://miro.medium.com/max/806/1*5qHhF8HTzZD2xdx3p-RLIQ.png)

### `Object` constructor

Default prototype of a function or any object you create on your own with the object literal notation `{}` always uses global object constructor. So any object created with the object literal notation or the object created by Javascript automatically (default prototype of a function) will be based on the object constructor function `new Object()` and will therefore use `Object.prototype` as its fallback value

`Object.prototype` has no other prototype (access `__proto__` will return null)

### Prototype is shared between instance

```js
var person1 = new Human("Virat", "Kohli");
var person2 = new Human("Sachin", "Tendulkar");
person1.__proto__ === person2.__proto__ //true
```

### How prototype chaining

A **prototype** is just an **object** itself. An **object's prototype object** may also have a **prototype object**. This is often referred to as a **prototype chain**, and explains why different objects have properties and methods defined on other objects available to them

![image-20200602230654002](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200602230654002.png)

```js
function doSomething(){}
doSomething.prototype.foo = "bar"; // add a property onto the prototype
var doSomeInstancing = new doSomething();
doSomeInstancing.prop = "some value"; // add a property onto the object
/*
{
    prop: "some value",
    __proto__: { // doSomething.prototype
        foo: "bar",
        constructor: ƒ doSomething(),
        __proto__: { // Object.prototype => base object in JS
            constructor: ƒ Object(),
            hasOwnProperty: ƒ hasOwnProperty(),
            isPrototypeOf: ƒ isPrototypeOf(),
            propertyIsEnumerable: ƒ propertyIsEnumerable(),
            toLocaleString: ƒ toLocaleString(),
            toString: ƒ toString(),
            valueOf: ƒ valueOf()
        }
    }
}
*/
```

When you access a property of `doSomeInstancing`, the browser first looks to see if `doSomeInstancing` has that property.

If `doSomeInstancing` does not have the property, then the browser looks for the property in the `__proto__` of `doSomeInstancing` (a.k.a. doSomething.prototype). If the `__proto__` of doSomeInstancing has the property being looked for, then that property on the `__proto__` of doSomeInstancing is used.

If it still doesn't exits, the `__proto__` of the `__proto__` of doSomeInstancing (a.k.a. the `__proto__` of doSomething.prototype (a.k.a. `Object.prototype`)) is then looked through for the property being searched for.

If JavaScript reach to the end of  **prototype chain** and if it didn't find property, it will return undefined and for the method, it would throw an error

![image-20200612211958126](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612211958126.png)

### Summary

A prototype is an object (let's call it `"P"`) that is linked to another object (let's call it `"O"`) - it (the prototype object) kind of acts as a "**fallback object**" to which the other object (`"O"`) can reach out if you try to work with a property or method that's not defined on the object (`"O"`) itself.

**EVERY object in JavaScript by default has such a fallback object** (i.e. a prototype object)

```js
function User() {
    ... // some logic, doesn't matter => configures which properties etc. user objects will have
}
User.prototype = { age: 30 }; // sets prototype object for "to be created" user objects, NOT for User function object
```

The `User` function here also has a prototype object of course (i.e. a connected fallback object) - **but that is NOT the object the** `prototype` **property**  (`User.prototype`) **points at**. 

Instead, you access the connected fallback/ prototype object via the **special** `__proto__` **property** which **EVERY object** (remember, functions are objects) has.

The `prototype` property (only in **function**) does something different: **It sets the prototype object new objects which you create with this User constructor function will have.**

That means:

```js
const userA = new User();
userA.__proto__ === User.prototype; // true
userA.__proto__ === User.__proto__ // false
```

## Function prototype

https://stackoverflow.com/questions/46129847/why-does-function-proto-return-something-different-than-other-prototypes/46130469

https://stackoverflow.com/questions/383172/correct-prototype-chain-for-function/383366

https://javascript.info/native-prototypes

`Function` is a constructor function, so creating a function with `new Function()` sets its prototype to `Function.prototype`:

```js
new Function().__proto__ === Function.prototype // true
(function() {}).__proto__ == Function.prototype // true

// Function.prototype is a object, Function is a function
Function.__proto__ === Function.prototype
Function instanceof Function //true
// Object and Function are both (constructor) functions, and functions are objects
Object instanceof Function // true
Function instanceof Object // true
```

All functions have this same prototype, including constructor functions. So:

```js
Function.__proto__ == Function.prototype // true
Array.__proto__ == Function.prototype // true
Number.__proto__ == Function.prototype // true
```

`Function.prototype` defines the default behavior of functions, that all functions inherit from, including the ability to call it, so it acts as a function itself:

```js
Function.prototype() // undefined
(new Function())() // undefined
```

## Constructor

The `constructor` property in a prototype is automatically setup to reference the constructor function.

```javascript
function Cat(name) {
  this.name = name;
}
Cat.prototype.getName = function() {
  return this.name;
}
Cat.prototype.clone = function() {
  return new this.constructor(this.name);
}
Cat.prototype.constructor === Cat // => true
```

Because properties are inherited from the prototype, the `constructor` is available on the instance object too.

```javascript
var catInstance = new Cat('Mew');
catInstance.constructor === Cat // => true
```

Even if the object is created from a literal, it inherits the constructor from `Object.prototype`.

```javascript
var simpleObject = {
  weekDay: 'Sunday'
};
simpleObject.prototype === Object // => true
```

## Adding properties at runtime

In JavaScript, you can add properties to any object at run time. You are not constrained to use only the properties provided by the constructor function.

```js
const obj = new BaseClass();
obj.newProperty = "hallelujah"
```

Now, the object has a `newProperty` property,  but no other  `BaseClass`  instances has this property.

If you add a new property to an object that is being used as the prototype for a constructor function, you add that property to all objects that inherit properties from the prototype.

```js
Employee.prototype.sharedProperty = 'we are communist, we share';
```





## Method

Prototype is shared => Add method to prototype (save memory)

![image-20200605205426303](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200605205426303.png)

```js
function Person() {
  this.age = 30;
  this.name = 'Max';
//  this.greet = function() {
//    console.log(
//      'Hi, I am ' + this.name + ' and I am ' + this.age + ' years old.'
//    );
//  };
// => this will created a new function when create an object
// User arrow function will not need to use bind method
}

Person.prototype.greet = function() {
	console.log('Hi, I am ' + this.name + ' and I am ' + this.age + ' years old.');
};

class Person {
//...
//  constructor() {
//      this.greet = function() {...} 
//  }
//  or
//	greet = function() {...} 
//  => Will created a new function when create an object
// User arrow function will not need to use bind method

  greet() { //this will add a method automatically by JavaScript to Person.prototype 
    console.log(
      'Hi, I am ' + this.name + ' and I am ' + this.age + ' years old.'
    );
  }
}
```

```js
const p1 = new Person();
const p2 = new Person();
console.log(p1.greet === p2.greet); // true
```

## OOP in javascript

### Abstraction

https://medium.com/@viktor.kukurba/object-oriented-programming-in-javascript-1-abstraction-c47307c469d1

https://www.javatpoint.com/javascript-oops-abstraction

### Inheritance

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain

https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Inheritance

https://medium.com/@viktor.kukurba/object-oriented-programming-in-javascript-2-inheritance-447368f57a26

https://stackoverflow.com/questions/7486825/javascript-inheritance

https://www.javatpoint.com/javascript-oops-inheritance

### Polymorphism

https://www.javatpoint.com/javascript-oops-polymorphism

https://medium.com/@viktor.kukurba/object-oriented-programming-in-javascript-3-polymorphism-fb564c9f1ce8

https://stackoverflow.com/questions/27642239/what-is-polymorphism-in-javascript

### Encapsulation 

https://stackoverflow.com/questions/32495747/javascript-encapsulation-abstraction

https://medium.com/@viktor.kukurba/object-oriented-programming-in-javascript-4-encapsulation-4f9165cd26f9

https://stackoverflow.com/questions/19948698/encapsulating-in-javascript-does-it-exist

https://medium.com/javascript-scene/encapsulation-in-javascript-26be60e325b4

https://itnext.io/different-ways-to-achieve-encapsulation-in-javascript-es6-7cb938e83f2d

https://www.javatpoint.com/javascript-oops-encapsulation

# Function Invocation

Invoke a function, using `()`

# Context

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

# Execution Context

*Execution context* equates to the 'environment' a function executes in; that is, variable scope (and the *scope chain*, variables in closures from outer scopes), function arguments, and the value of the `this` object.

The *call stack* is a collection of execution contexts.

![image-20200612164941745](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612164941745.png)

> In other words, as we start the program, we start in the global execution context. Some variables are declared within the global execution context. We call these global variables. When the program calls a function:
>
> 1. JavaScript creates a new execution context, a local execution context
> 2. That local execution context will have its own set of variables, these variables will be local to that execution context.
> 3. The new execution context is thrown onto the *execution stack*. Think of the execution stack as a mechanism to keep track of where the program is in its execution
>
> When does the function end? When it encounters a `return` statement or it encounters a closing bracket `}`. When a function ends, the following happens:
>
> 1. The local execution contexts pops off the execution stack
> 2. The functions sends the return value back to the calling context. The calling context is the execution context that called this function, it could be the global execution context or another local execution context. It is up to the calling execution context to deal with the return value at that point. The returned value could be an object, an array, a function, a boolean, anything really. If the function has no `return` statement, `undefined` is returned.
> 3. The local execution context is destroyed. This is important. Destroyed. All the variables that were declared within the local execution context are erased. They are no longer available. That’s why they’re called local variables.
>
> A function definition can be stored in a variable, the function definition is invisible to the program until it gets called. Second, every time a function gets called, a local execution context is (temporarily) created. That execution context vanishes when the function is done. A function is done when it encounters `return` or the closing bracket `}`

## Global Execution Context

No outer environment, this = global object, your global code

Code not inside a function is global

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612164606842.png" alt="image-20200612164606842" style="zoom: 33%;" />

## Creation phase

- JS Engine parses - run through your code & `identifies variables & functions` created by code (which will be used in execution phase)
- Setup memory space for Variables & Functions

![image-20200612165628524](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612165628524.png)

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

![image-20200612172016928](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612172016928.png)

# Execution/Call stack

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

![image-20200830112444561](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200830112444561.png)

When the function `b()` returns, the engine destroys the context of `b()`, and when we exit function `a()`, the context of `a()` is destroyed.

# Scope

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

![image-20200612150147376](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612150147376.png)

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

![image-20200612180323636](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612180323636.png)

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

![image-20200612180830321](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612180830321.png)

**"scope chain" that makes closures possible.**

## Function Arguments

Function arguments (parameters) work as local variables inside functions.

## The Lifetime of Variables

The lifetime of a JavaScript variable starts when it is declared.

Local variables are deleted when the function is completed.

In a web browser, global variables are deleted when you close the browser window (or tab).

# Closure 

https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36

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

![image-20200612211115251](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612211115251.png)

# Function Factory 

A function return a function

![image-20200608220359309](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608220359309.png)

![image-20200612211304984](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612211304984.png)

# Functional programming

https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0

https://www.udemy.com/course/functional-programming-for-beginners-with-javascript/

https://www.freecodecamp.org/news/functional-programming-principles-in-javascript-1b8fc6c3563f/

# Recursion

JS current no have optimize for tail recursion

![image-20200608235527130](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608235527130.png)

![image-20200608235802861](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608235802861.png)

# Pure function

![image-20200608183520088](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608183520088.png)

Should aim for pure function than impure function (reduce impure function), because they are predictable (not do anything behind the scene)

# Higher order function

A **higher order function** is a function that takes a function as an argument, or returns a function

# Curry function

![image-20200612211507617](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612211507617.png)

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

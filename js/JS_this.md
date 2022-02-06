# this

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this

https://codeburst.io/all-about-this-and-new-keywords-in-javascript-38039f71780c

https://blog.bitsrc.io/what-is-this-in-javascript-3b03480514a7

https://www.reddit.com/r/javascript/comments/8oi3ur/what_is_this_in_javascript/

`this` refers to different things, depending on where it's used and how (if used in a function) a function is called.

Generally, `this` refers to the "thing" which call a function (if used inside of a function). That can be the global context, an object or some bound data/ object (e.g. when the browser binds `this` to the button that triggered a click event).

Every javascript function while executing has a reference to its current execution context, called ***this\***. Execution context means here is how the function is called.

## in Global Context

In the global execution context (outside of any function), `this` refers to the global object whether in strict mode or not.

```js
function something() { /*...*/ }
something === window.something // true
console.log(this); // logs global object (window in browser)
console.log(this === window); // true

a = 37;
console.log(window.a); // 37

this.b = "MDN";
console.log(window.b)  // "MDN"
console.log(b)         // "MDN"
```

## function context

Inside a function, the value of `this` depends on how the function is called.

every function created with function keyword or method shortcut has it's own `this` binding (bound to whatever is responsible for executing that function)

- In a function, `this` refers to the **global object**.
- In a function, in strict mode, `this` is `undefined`.

```js
function f1() {
  return this;
}

// In a browser:
f1() === window; // true

// In Node:
f1() === globalThis; // true

function f2() {
  'use strict'; // see strict mode
  return this;
}

f2() === undefined; // true
```

## As a Object method

When a function is called as a method of an object, its `this` is set to the object the method is called on.

- In a method, `this` refers to the **owner object**.

```js
const person = { 
    name: 'Max',
    greet: function() { // or use method shorthand: greet() { ... }
        console.log(this.name);
    }
};

person.greet(); // logs 'Max', "this" refers to the person object

const anotherPerson = { name: 'Manuel' }; // does NOT have a built-in greet method!
anotherPerson.sayHi = person.greet; // greet is NOT called here, it's just assigned to a new property/ method on the "anotherPerson" object
anotherPerson.sayHi(); // logs 'Manuel' because method is called on "anotherPerson" object => "this" refers to the "thing" which called it
```

```js
const members = {
    teamName: 'This wierd',
    names: ['Max','Test'],
    greetAll: function() {
    	this.names.forEach(function(n) {
            console.log(n) // Max , Test
            console.log(this.teamName) // undefined , undefined
            // => this is not about members, this is the global object
        })
    }
}
```

```js
function something() { 
    console.log(this);
}

something(); // logs global object (window in browser) in non-strict mode, undefined in strict mode

// An object can be passed as the first argument to call or apply and this will be bound to it.
var obj = {a: 'Custom'};

// We declare a variable and the variable is assigned to the global window as its property.
var a = 'Global';

function whatsThis() {
  return this.a;  // The value of this is dependent on how the function is called
}

whatsThis();          // 'Global' as this in the function isn't set, so it defaults to the global/window object
whatsThis.call(obj);  // 'Custom' as this in the function is set to obj
whatsThis.apply(obj); // 'Custom' as this in the function is set to obj
```

## As a constructor

The ***new\*** keyword in front of any function turns the function call into constructor call and below things occurred when *new* keyword put in front of function (its `this` is bound to the new object being constructed).

- A brand new empty object gets created
- new empty object gets linked to prototype property of that function
- same new empty object gets bound as ***this\*** keyword for execution context of that function call
- if that function does not return anything then it implicit returns ***this\*** object.

```js
function C() {
  this.a = 37;
}

var o = new C();
console.log(o.a); // 37


function C2() {
  this.a = 37;
  return {a: 38};
}

o = new C2();
console.log(o.a); // 38

function bike() {
  var name = "Ninja";
  this.maker = "Kawasaki";
  console.log(this.name + " " + maker);  // undefined Bajaj
}

var name = "Pulsar";
var maker = "Bajaj";

obj = new bike();
console.log(obj.maker);                  // "Kawasaki"
```

## this in classes

Just like with regular functions, the value of `this` within methods depends on how they are called. Sometimes it is useful to override this behavior so that `this` within classes always refers to the class instance. To achieve this, bind the class methods in the constructor:

```js
class Car {
  constructor() {
    // Bind sayBye but not sayHi to show the difference
    this.sayBye = this.sayBye.bind(this);
  }
  sayHi() {
    console.log(`Hello from ${this.name}`);
  }
  sayBye() {
    console.log(`Bye from ${this.name}`);
  }
  get name() {
    return 'Ferrari';
  }
}

class Bird {
  get name() {
    return 'Tweety';
  }
}

const car = new Car();
const bird = new Bird();

// The value of 'this' in methods depends on their caller
car.sayHi(); // Hello from Ferrari
bird.sayHi = car.sayHi;
bird.sayHi(); // Hello from Tweety

// For bound methods, 'this' doesn't depend on the caller
bird.sayBye = car.sayBye;
bird.sayBye();  // Bye from Ferrari
```

## bind() , call() , apply()

set the value of a function's `this` regardless of how it's called

to preconfigure what `this` will refer to

![image-20200601003113705](assets/JS_this/image-20200601003113705.png)

![image-20200601003438995](assets/JS_this/image-20200601003438995.png)

![image-20200601003540463](assets/JS_this/image-20200601003540463.png)

## arrow function behavior 

https://stackoverflow.com/questions/28798330/arrow-functions-and-this

https://stackoverflow.com/questions/31095710/methods-in-es6-objects-using-arrow-functions

https://www.freecodecamp.org/news/when-and-why-you-should-use-es6-arrow-functions-and-when-you-shouldnt-3d851d7f0b26/

arrow function don't provide their own `this` binding (it retains the `this` value of the enclosing lexical context).

no binding this to anything => this in inside arrow function = this in outside arrow function

`this` points at the nearest bound `this`

```js
const something = () => { 
    console.log(this);
}

something(); // logs global object (window in browser) - ALWAYS (also in strict mode)!

var globalObject = this;
var foo = (() => this);
console.log(foo() === globalObject); // true
var obj = {func: foo};
console.log(obj.func() === globalObject); // true
// Attempt to set this using call
console.log(foo.call(obj) === globalObject); // true
// Attempt to set this using bind
foo = foo.bind(obj);
console.log(foo() === globalObject); // true
```

The same applies to arrow functions created inside other functions: their `this` remains that of the enclosing lexical context.

```js
// Create obj with a method bar that returns a function that
// returns its this. The returned function is created as 
// an arrow function, so its this is permanently bound to the
// this of its enclosing function. The value of bar can be set
// in the call, which in turn sets the value of the 
// returned function.
var obj = {
  bar: function() {
    var x = (() => this);
    return x;
  }
};
var fn = obj.bar();
console.log(fn() === obj); // true

var fn2 = obj.bar;
console.log(fn2()() == window); // true
```

## As a DOM event handler

When a function is used as an event handler, its `this` is set to the element on which the listener is placed

When the code is called from an inline on-event handler its `this` is set to the DOM element on which the listener is placed

`this` is bound to the event's source - the element which is registered the event listener  (not for arrow function)

- In an event, `this` refers to the **element** that received the event.

![image-20200608152759049](assets/JS_this/image-20200608152759049.png)

![image-20200608152825710](assets/JS_this/image-20200608152825710.png)
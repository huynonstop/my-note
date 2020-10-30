# **JavaScript**

https://academind.com/learn/javascript/

## What is JavaScript

JavaScript is a **dynamic**, **weakly typed** **programming language** which is **compiled at runtime**. It can be executed as part of a webpage in a browser or directly on any machine (**“host environment”**).

JavaScript was created to make webpages more dynamic (e.g. change content on a page directly from inside the browser).

JavaScript is a specific version of ECMAScript

![image-20200521234356795](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200521234356795.png)

![image-20200609162703842](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609162703842.png)

For implementation (browser, runtime)![image-20200609162754749](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609162754749.png)

## How JS execute

![image-20200523003553345](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200523003553345.png)

## Dynamic and Weakly-typed

![image-20200523112734856](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200523112734856.png)

## Hosted Language

![image-20200611212206286](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611212206286.png)

## Environment

![image-20200523122656403](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200523122656403.png)

![image-20200523131144520](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200523131144520.png)

### What browser does with JS code (V8)

Browser read html => Hit script tag => Parsing (read JS and load it) and Execute code

![image-20200523222006668](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200523222006668.png)

![image-20200523220200236](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200523220200236.png)

### How code gets executed in the JS engine

![image-20200523222358818](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200523222358818.png)

### How Garbage Collector work

![image-20200523225819599](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200523225819599.png)

When a block of memory (an object, say) is no longer reachable, it is eligible to be reclaimed. When, how, or whether it is reclaimed is entirely up to the implementation, and different implementations do it differently. But at a language level, it's automatic.

## Variable & Constant

### Variable

A "data container" holds data which can change

Key word : var, let (ES6)

Naming: **camelCase** or start with "_", "$"

### Constant

A "data container" holds data which must not change

Key word : const (ES6)

Naming: **CAPITALIZE**

A variable declared outside a function, becomes global variable 

### var vs let vs const

| Key word           | const | let  | var  |
| :----------------- | ----- | ---- | ---- |
| **Global scope**   | YES   | YES  | YES  |
| **Function scope** | YES   | YES  | YES  |
| **Block scope**    | YES   | YES  | NO   |
| **Re-assigned**    | NO    | YES  | YES  |
| **Re-declared**    | NO    | NO   | YES  |
| **Hoisting**       | NO    | NO   | YES  |
| **ES**             | 6+    | 6+   | <5   |

## Operators

[Docs link](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

## Comparison operators

![image-20200523203645095](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200523203645095.png)

Careful with comparing object

```js
1 == '1'; // true
1 == [1]; // true
1 == true; // true
0 == ''; // true
0 == '0'; // true
0 == false; // true
var a = null;
console.log(a == null); // true
console.log(a == undefined); // true
```

![image-20200523204617685](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200523204617685.png)

## Expression

![image-20200612205313041](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612205313041.png)

## Statement

Code that not return value

if, function statement,....

![image-20200612205716918](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612205716918.png)

## Control Structures

![image-20200612145656041](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612145656041.png)

![image-20200612145734885](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612145734885.png)

![image-20200612145746445](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612145746445.png)

![image-20200612145759011](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612145759011.png)

![image-20200612145821016](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612145821016.png)

### Loop

![image-20200523210555960](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200523210555960.png)

break: stop looping

continue: skip this loop

#### Iterable

![image-20200530211212489](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200530211212489.png)

## Datatype

+ Primitive type: Deep copy, store in stack, present a single value

  + Number

  + String

  + Boolean

  + undefined (Something doesn't have value but defined, **auto** set by JS and may set by Dev)

  + null (Something is set to not have value, set by Dev)

    ```javascript
    typeof null; // “object”
    typeof undefined; // “undefined”
    ```

    ![image-20200612205802024](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612205802024.png)

+ Reference type: Shallow copy, store value in heap

  - Array

  - Object ({key: value})

  - Function

    ![image-20200612205815807](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612205815807.png)

![image-20200523224859003](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200523224859003.png)

![image-20200531222813663](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200531222813663.png)

![image-20200609173712566](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609173712566.png)

### Coercion

![image-20200612204624112](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612204624112.png)

### Floating point  (Im)Precision

![image-20200609174146528](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609174146528.png)

## Array

array, element, index, length

An `array` is an object that holds values (of any type) not particularly in named properties/keys, but rather in numerically indexed positions

![image-20200530211734950](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200530211734950.png)

### Array method

+ indexOf(v), lastIndexOf(v), includes(v) : bool => primitive value

+ find(callback : bool) : element, findIndex(callback  : bool) : index => reference value

+ forEach((value,index,array) => {})

+ push,pop,unshift,shift (add,remove element)

+ concat (This method does not change the existing arrays, but instead returns a new array.)

+ slice (return a new array containing the extracted elements from begin to end, not alter the original array )

  ```javascript
  // Return a portion of an existing array
  let fruits = ['Banana', 'Orange', 'Lemon', 'Apple', 'Mango']
  let citrus = fruits.slice(1, 3)
  
  // fruits contains ['Banana', 'Orange', 'Lemon', 'Apple', 'Mango']
  // citrus contains ['Orange','Lemon']
  
  // Create new array
  let myHonda = { color: 'red', wheels: 4, engine: { cylinders: 4, size: 2.2 } }
  let myCar = [myHonda, 2, 'cherry condition', 'purchased 1997']
  let newCar = myCar.slice(0, 2)
  myCar = [{color: 'red', wheels: 4, engine: {cylinders: 4, size: 2.2}}, 2,
           'cherry condition', 'purchased 1997']
  newCar = [{color: 'red', wheels: 4, engine: {cylinders: 4, size: 2.2}}, 2]
  
  // Convert to new array
  function list() {
    return Array.prototype.slice.call(arguments)
  }
  let list1 = list(1, 2, 3) // [1, 2, 3]
  ```

+ splice (removing or replacing existing elements and/or adding new elements return an array containing the deleted elements, if only one element is removed, an array of one element is returned, if no elements are removed, an empty array is returned)

  ```javascript
  // Remove 1 element from index 2, and insert "trumpet"
  let myFish = ['angel', 'clown', 'drum', 'sturgeon']
  let removed = myFish.splice(2, 1, 'trumpet')
  
  // myFish is ["angel", "clown", "trumpet", "sturgeon"]
  // removed is ["drum"]
  ```

+ find (returns the **value** of the **first element** in the provided array that satisfies the provided testing function)

+ reverse

  ```javascript
  const array1 = ['one', 'two', 'three'];
  console.log('array1:', array1);
  // expected output: "array1:" Array ["one", "two", "three"]
  
  const reversed = array1.reverse();
  console.log('reversed:', reversed);
  // expected output: "reversed:" Array ["three", "two", "one"]
  
  // Careful: reverse is destructive -- it changes the original array.
  console.log('array1:', array1);
  // expected output: "array1:" Array ["three", "two", "one"]
  ```

+ sort ( returns the sorted array. The default sort order is ascending, built upon converting the elements into strings , can use compare function)

  ```javascript
  const array1 = [1, 30, 4, 21, 100000];
  array1.sort();
  console.log(array1);
  // expected output: Array [1, 100000, 21, 30, 4]
  
  let numbers = [4, 2, 5, 1, 3];
  numbers.sort((a, b) => a - b);
  console.log(numbers);
  
  // [1, 2, 3, 4, 5]
  ```

+ filter ( **creates a new array** with all elements that pass the test (true) implemented by the provided function)

+ map (transform array to another array)

  ```javascript
  const array1 = [1, 4, 9, 16];
  
  // pass a function to map
  const map1 = array1.map(x => x * 2);
  
  console.log(map1);
  // expected output: Array [2, 8, 18, 32]
  ```

+ reducer (combine array to a simpler value)

  ```javascript
  [0, 1, 2, 3, 4].reduce(function(preValue, currentValue, currentIndex, array) {
    return preValue + currentValue
  },initialValue)
  //if no initialValue skip first iteration and take first value as the initial value
  ```
  
  ![image-20200530220540763](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200530220540763.png)
### split and join

split : string => array

join : array => string

### Spread operator ES6+

```js
const a = [1,2,3]
const b = [0,...a,4] // [0,1,2,3,4]
const c = [...a]
a === c //false
Math.min(a)//NaN
Math.min(...a)//1
```

### Array destructuring

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

###  Array-like-object

![image-20200530211222871](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200530211222871.png)

## Function

![image-20200221180104305](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200221180104305.png)

Functions are one of the fundamental building blocks in JavaScript. A function is a JavaScript object (store your code - a set of statements ) that can be invoked (run) later. 

To use a function, you must define it somewhere in the scope and then you can call it any time and any where you want.

![image-20200612145620350](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612145620350.png)

Function is an object in JavaScript

You can defined a function inside a function

![image-20200524210656777](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200524210656777.png)

![image-20200612204848376](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612204848376.png)

### Anonymous function

A function is used once and does not need to be used anywhere else

They can be used in IIFEs to encapsulate some code within a local scope so that variables declared in it do not leak to the global scope.

```javascript
(function() {
  // Some code here.
})();
```

The code will seem more self-contained and readable when handlers are defined right inside the code calling them, rather than having to search elsewhere to find the function body.

```javascript
setTimeout(function() {
  console.log("Hello world!");
}, 1000);
```

Arguments to functional programming constructs

```javascript
const arr = [1, 2, 3];
const double = arr.map(function(el) {
  return el * 2;
});
console.log(double); // [2, 4, 6]
```

### Function method

#### bind(context[...,arguments ]) method

return a new reference at a function in place which is preconfigured regarding the arguments it receives 

not executed immediately

can append more arguments which is not preconfigured when call the function

#### call and apply method

allow to preconfigured arguments to the function but immediately execute the function

### Arguments variable 

Only used inside a function use function keyword

Arguments is Array-like object

```js
function sum() {
    let rs = 0
    for (let i = 0; i < arguments.length;i++){
        rs += arguments[i]
    }
    return rs;
}
sum(1,2,3,4,5) // 15
```

### Rest parameters ES6+

Take all arguments and merges to an array

Must be the last argument and 1 rest parameters

```js
function sum(...nums) {
    return nums.reduce((a,b) => a+b) //nums: [1,2,3,4,5]
}
sum(1,2,3,4,5) // 15
```

### Arrow function ES6+

![image-20200524212628601](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200524212628601.png)

It is an anonymous function

## Object

![image-20200531222729940](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200531222729940.png)

### 	Native objects 

​	Objects that are part of the JavaScript language defined by the ECMAScript specification, such as `String`, `Math`, `RegExp`, `Object`, `Function`, etc.

 [JS_NativeObj.md](JS_NativeObj.md) 

### 	Host objects

​	Host objects are provided by the runtime environment (browser or Node), such as `window`, `XMLHTTPRequest`, etc.

​	The object type refers to a compound value where you can set properties (named locations) that each hold their own values of any type.

### Create object

There are 3 ways to create objects.

1. By object literal
2. By creating instance of Object directly (using new keyword)
3. By using an object constructor (using new keyword)

```javascript
//obj literals
var obj = {
    key: 'value', // number or string or 'special string'
    method : function() {}
}
// key order is insertion order or sort if key have number only
//ES6 obj literal
const name = 'abc'
const cat = {
    name,
    run() {
        console.log(this.name + "running")
    }
}
```

```javascript
//Using Object.create() method:
var a = Object.create(null); 

let person2 = Object.create(person1);
person2.__proto__ === person1
//creates a new object extending the prototype object passed as a parameter.
prototypeObject = {
	fullName: function(){
		return this.firstName + " " + this.lastName		
	}
}
var person = Object.create(prototypeObject)
var person2 = Object.create(prototypeObject, {
      'firstName': {
	value: "Virat", 
	writable: true, 
	enumerable: true
      },
      'lastName': {
	value: "Kohli",
	writable: true,
	enumerable: true
      }
})
```

### Access object

```js
//Dot notation
obj.key 
obj.method()
//Added New properties
obj.age = 27
obj.getAge = function(){
	return this.age;
}
//Square bracket notation
obj['key'] 
obj['method'] 
let property = "key";
console.log(obj[property]) // Output: value
Console.log(obj.property) //Output: undefined
//Name
obj["date of birth"] = "Nov 28";
obj[12] = 12;
obj.12 = 12; //gives error
//Check Property Existance
'key' in obj
//Delete
delete obj.key; // return true
```

### Spread operator ES6+

very useful when coding in a functional paradigm as we can easily create copies of arrays or objects

```javascript
const obj1 = {
    a: 1,
    b: { e: 10}
}
let obj2 = {
    ...obj1
}
obj2.b.e = 1
obj2.a = 2
//obj1={a:1,b:{e:1}}
//obj2={a:2,b:{e:1}}

//other way
Object.assign({},obj1)
```

### Object destructuring

```javascript
// Variable assignment.
const o = { p: 42, q: true };
const { p, q } = o;

console.log(p); // 42
console.log(q); // true
```

### this

`this` refers to different things, depending on where it's used and how (if used in a function) a function is called.

Generally, `this` refers to the "thing" which call a function (if used inside of a function). That can be the global context, an object or some bound data/ object (e.g. when the browser binds `this` to the button that triggered a click event).

![image-20200601001943153](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601001943153.png)

![image-20200601002806023](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601002806023.png)

#### `this` in Global Context (i.e. outside of any function)

```js
function something() { ... }
something === window.something // true
console.log(this); // logs global object (window in browser)
```

#### function behavior 

every function created with function keyword or method shortcut has it's own this binding (bound to whatever is responsible for executing that function)

**Called on an object**

```js
const person = { 
    name: 'Max',
    greet: function() { // or use method shorthand: greet() { ... }
        console.log(this.name);
    }
};

person.greet(); // logs 'Max', "this" refers to the person object
```

**Called in the global context**

![image-20200601010338863](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601010338863.png)

this become window because function is defined same as **defined function outside** and it is **called without binding**

```js
function something() { 
    console.log(this);
}

something(); // logs global object (window in browser) in non-strict mode, undefined in strict mode
```

#### arrow function behavior 

no binding this to anything => this in inside arrow function = this in outside arrow function

**Called on an object**

```js
const person = { 
    name: 'Max',
    greet: () => {
        console.log(this.name);
    }
};

person.greet(); 
/* 
**logs nothing (or some global name on window object), "this" refers to **global (window) object, even in strict mode
**this can refer to unexpected things if you call it on some other object, **e.g.:
*/
const person = { 
    name: 'Max',
    greet() {
        console.log(this.name);
    }
};

const anotherPerson = { name: 'Manuel' }; // does NOT have a built-in greet method!

anotherPerson.sayHi = person.greet; // greet is NOT called here, it's just assigned to a new property/ method on the "anotherPerson" object

anotherPerson.sayHi(); // logs 'Manuel' because method is called on "anotherPerson" object => "this" refers to the "thing" which called it
```

![image-20200601010226898](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601010226898.png)

**Called in the global context**

```js
const something = () => { 
    console.log(this);
}

something(); // logs global object (window in browser) - ALWAYS (also in strict mode)!
```

#### browser behavior with event

**whatever called that function** (if not arrow function)

![image-20200601003853418](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601003853418.png)

#### bind() , call() , apply()

to preconfigure what this will refer to

![image-20200601003113705](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601003113705.png)

![image-20200601003438995](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601003438995.png)

![image-20200601003540463](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601003540463.png)

### Getter & Setter

![image-20200601210340382](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601210340382.png)

### Descriptor

![image-20200602004653502](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200602004653502.png)

![image-20200602004806899](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200602004806899.png)

![image-20200602005053500](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200602005053500.png)

## document & window

document: give access to your loaded HTML document

window: give access to browser's core APIs (just the active tab)

![image-20200525233250502](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200525233250502.png)

## Class 

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes

JavaScript classes, introduced in ECMAScript 2015, are primarily syntactical sugar over JavaScript's existing prototype-based inheritance. The class syntax *does not* introduce a new object-oriented inheritance model to JavaScript.

Classes are in fact "special functions"

### Constructor method

```js
// ES6 Class
class Person {
  constructor(name) {
    this.name = name; //this = object is being created by using the new keyword
  }
}
const a = new Person("hi")// this = a
```

### Fields vs Properties

![image-20200601212615544](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601212615544.png)

```js
class MyClass() {
  prop1 = 1;
  prop2; 
  /* 
   	Don't depend on the constructor
  	This is a property that this class instance will have, created when constructor is called and after super is executed completely
  */
  constructor(someArg) {
    this.prop2 = someArg;
  }

  /* methods are separated from the rest of the properties and construction logic */
  method1 = () => {}
}
```

![image-20200602003702552](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200602003702552.png)

```js
class Private { 
    #privateField = "private" 
    #privateMethod() {}
    //new syntax becareful
}
```

### Binding class method 

![image-20200601214159413](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601214159413.png)

![image-20200601214143079](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601214143079.png)

![image-20200602003253632](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200602003253632.png)

![image-20200602003417752](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200602003417752.png)

### static

![image-20200601233442514](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601233442514.png)

Static class members (properties/methods) are not tied to a specific instance of a class and have the same value regardless of which instance is referring to it. Static properties are typically configuration variables and static methods are usually pure utility functions which do not depend on the state of the instance.

### Getter & Setter

![image-20200601235032694](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601235032694.png)

### Inheritance

![image-20200601235316105](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601235316105.png)

```javascript
class Polygon {
  constructor(height, width) {
    this.name = 'Polygon';
    this.height = height;
    this.width = width;
  }
}

class Square extends Polygon {
  constructor(length) {
    super(length, length); 
      //Must call constructor of parrent class (super.function())
      //allow accessing `this` inside child contructor 
    this.name = 'Square';
  }
}
```

## Event

![image-20200608132055539](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608132055539.png)

### Browser Event Objects

![image-20200608132201928](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608132201928.png)

Whatever causes an event provides you some data along with such an event to describe it to help you control it

![image-20200608142509047](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608142509047.png)

![image-20200608142527917](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608142527917.png)

### Adding event

HTML `on` attributes => mix HTML and JS code

element `on` property => only 1 event handler

element `addEventListener()` => most flexibles ways (can add and remove event listener later)

### Remove event

![image-20200608134906555](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608134906555.png)

can only remove exact the same function (not work direct with anonymous function or `bind()`)

### preventDefault()

form submit to server or link navigation

### Bubbling & Capturing, Propagation

![image-20200608144128162](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608144128162.png)

Capturing => Bubbling

By default all event listeners are registered in the bubbling phase 

```html
<div>
    <button>
    </button>
</div>
```

![image-20200608144923627](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608144923627.png)

![image-20200608145005127](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608145005127.png)

![image-20200608145139650](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608145139650.png)

![image-20200608145211518](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608145211518.png)

The way event occurred on this button but it's listenable on all ancestors called **Event Propagation** (some event are not propagated)

![image-20200608145736200](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608145736200.png)

![image-20200608150018545](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608150018545.png)

### Event delegation pattern

```html
<ul id="parent-list">
	<li id="post-1">Item 1</li>
	<li id="post-2">Item 2</li>
	<li id="post-3">Item 3</li>
	<li id="post-4">Item 4</li>
	<li id="post-5">Item 5</li>
</ul>
```

Event delegation is a technique involving adding event listeners to a **parent** element instead of adding them to the child elements. 

![image-20200608150822633](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608150822633.png)

The listener will fire whenever the event is triggered on the descendant elements due to **event bubbling up** the DOM. The benefits of this technique are:

- Only have 1 listener
- There is no need to unbind the handler from elements that are removed and to bind the event for new elements.

```js
document.getElementById("parent-list").addEventListener("click", 			function(e) {
		// e.target is the clicked element!
   		event.target.closest('li').classList.toggle('highlight');
    }
 );
```

### Trigger event programmatically

use element method (submit(),click(),...)

### `this` in event handler

`this` is bound to the event's source - the element which is registered the event listener  (not for arrow function)

![image-20200608152759049](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608152759049.png)

![image-20200608152825710](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608152825710.png)

### Observer Pattern

![image-20200623095423563](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200623095423563.png)

- **subject** – This is the object that will send out a notification to all of the ‘observers’ who want/need to know that the subject was updated.
- **observers** – These are the objects that want to know when the subject has changed

```js
// Observable or Subject
class Observable {
  constructor() {
    this.observers = [];
  }
  subscribe(f) {
    this.observers.push(f);
  }
  unsubscribe(f) {
    this.observers = this.observers.filter(subscriber => subscriber !== f);
  }
  notify(data) {
    this.observers.forEach(observer => observer.update(data));
  }
}
// Observer
class Observer {
   constructor(element,update) {
    this.element = element;
    this.update = update;
  }
}
//--------------------------------------------------------------------
const headingsObserver = new Observable();
// DOM 
const p1 = document.querySelector('.p1')
const p2 = document.querySelector('.p2')
document.querySelector('.button').addEventListener('click', e => {
  // notify all observers about new data on event
  headingsObserver.notify("clicked");
});

const o1 = new Observer(p1,text => p1.textContent = text)
const o2 = new Observer(p2,text => p2.textContent = text)

headingsObserver.subscribe(o1);
headingsObserver.subscribe(o2);

headingsObserver.unsubscribe(o2)
```

## Immediately-Invoked Function Expression (IIFE)

An IIFE is an anonymous function that is created and then immediately invoked. It’s not called from anywhere else (hence why it’s anonymous), but runs just after being created.

![image-20200612210709261](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612210709261.png)

```javascript
function foo(){ }();// not working
(function () {return 5;} ()); //invokes when created
```

## Modules 

Each file content will be locked down (not add to global, but in it's own module scope, thing defined in a module is not shared unless you export it) and each file will be downloaded when needed (by HTTP request to a server)

Module run in strict mode

Can use global object (window, globalThis, ...) to share global data 

```js
//app.js
window.SOME_GLOBAL_VALUE ="abc"
//abc.js
console.log(window.SOME_GLOBAL_VALUE)
//order is matter be careful
```

![image-20200611133605096](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611133605096.png)

![image-20200611132342219](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611132342219.png)

![image-20200611131250853](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611131250853.png)

To share `export`

```js
// component.js
export class Component {} // name export

export default class Component {} // default export
```

To use exported module `import`

```js
import { name } form './file'
import { name as anotherName } form './file'
import * as alias form './file'
import defaultContent form './file'
```

```js
// app.js
import { Component } form './component'
```

Dynamic import 

Import code in a later time, not when your web app load but only when we need that code

![image-20200611133255822](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611133255822.png)

**Need a web server environment to bypass security issue** 



# What is JavaScript

<https://academind.com/learn/javascript/>

JavaScript is a **dynamic**, **weakly typed** **programming language** which is **compiled at runtime**. It can be executed as part of a webpage in a browser or directly on any machine (**“host environment”**).

JavaScript was created to make webpages more dynamic (e.g. change content on a page directly from inside the browser).

JavaScript is a specific version of ECMAScript

![image-20200521234356795](assets/JS__basic/image-20200521234356795.png)

![image-20200609162703842](assets/JS__basic/image-20200609162703842.png)

![image-20200609162754749](assets/JS__basic/image-20200609162754749.png)

## How JS execute

![image-20200523003553345](assets/JS__basic/image-20200523003553345.png)

## Dynamic and Weakly-typed

![image-20200523112734856](assets/JS__basic/image-20200523112734856.png)

## Hosted Language

![image-20200611212206286](assets/JS__basic/image-20200611212206286.png)

## Environment

![image-20200523122656403](assets/JS__basic/image-20200523122656403.png)

![image-20200523131144520](assets/JS__basic/image-20200523131144520.png)

## What browser does with JS code (V8)

Browser read html => Hit script tag => Parsing (read JS and load it) and Execute code

![image-20200523222006668](assets/JS__basic/image-20200523222006668.png)

![image-20200523220200236](assets/JS__basic/image-20200523220200236.png)

## How code gets executed in the JS engine

![image-20200523222358818](assets/JS__basic/image-20200523222358818.png)

## How Garbage Collector work

![image-20200523225819599](assets/JS__basic/image-20200523225819599.png)

When a block of memory (an object, say) is no longer reachable, it is eligible to be reclaimed. When, how, or whether it is reclaimed is entirely up to the implementation, and different implementations do it differently. But at a language level, it's automatic.

## Variable

A "data container" holds data which can change

Key word : var, let (ES6)

Naming: **camelCase** or start with "_", "$"

## Constant

A "data container" holds data which must not change

Key word : const (ES6)

Naming: **CAPITALIZE**

A variable declared outside a function, becomes global variable

## var vs let vs const

| Key word           | const | let  | var  |
| :----------------- | ----- | ---- | ---- |
| **Global scope**   | YES   | YES  | YES  |
| **Function scope** | YES   | YES  | YES  |
| **Block scope**    | YES   | YES  | NO   |
| **Re-assigned**    | NO    | YES  | YES  |
| **Re-declared**    | NO    | NO   | YES  |
| **Hoisting**       | NO    | NO   | YES  |
| **ES**             | 6+    | 6+   | <5   |

## Comparison operators

![image-20200523203645095](assets/JS__basic/image-20200523203645095.png)

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

![image-20200523204617685](assets/JS__basic/image-20200523204617685.png)

## Expression

![image-20200612205313041](assets/JS__basic/image-20200612205313041.png)

## Statement

Code that not return value

if, function statement,....

![image-20200612205716918](assets/JS__basic/image-20200612205716918.png)

## Loop

![image-20200523210555960](assets/JS__basic/image-20200523210555960.png)

break: stop looping

continue: skip this loop

### Iterable

![image-20200530211212489](assets/JS__basic/image-20200530211212489.png)

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

    ![image-20200612205802024](assets/JS__basic/image-20200612205802024.png)

+ Reference type: Shallow copy, store value in heap

  + Array

  + Object ({key: value})

  + Function

    ![image-20200612205815807](assets/JS__basic/image-20200612205815807.png)

![image-20200523224859003](assets/JS__basic/image-20200523224859003.png)

![image-20200531222813663](assets/JS__basic/image-20200531222813663.png)

![image-20200609173712566](assets/JS__basic/image-20200609173712566.png)

## Coercion

![image-20200612204624112](assets/JS__basic/image-20200612204624112.png)

## Floating point  (Im)Precision

![image-20200609174146528](assets/JS__basic/image-20200609174146528.png)

## Array

array, element, index, length

An `array` is an object that holds values (of any type) not particularly in named properties/keys, but rather in numerically indexed positions

![image-20200530211734950](assets/JS__basic/image-20200530211734950.png)

## Array method

+ `indexOf(v`), `lastIndexOf(v)`, includes(v) : bool => primitive value

+ find(callback : bool) : element, `findIndex(callback  : bool)` : index => reference value

+ `forEach((value,index,array) => {})`

+ `push`,`pop`,`unshift`,`shift`,`splice` (add,remove element) => mutation

+ `concat` ,`slice` (add,remove element) => im-mutation

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

  ```javascript
  // removing or replacing existing elements and/or adding new elements return an array containing the deleted elements, if only one element is removed, an array of one element is returned, if no elements are removed, an empty array is returned
  // Remove 1 element from index 2, and insert "trumpet"
  let myFish = ['angel', 'clown', 'drum', 'sturgeon']
  let removed = myFish.splice(2, 1, 'trumpet')
  
  // myFish is ["angel", "clown", "trumpet", "sturgeon"]
  // removed is ["drum"]
  ```

+ reverse

+ sort ( returns the sorted array. The default sort order is ascending, built upon converting the elements into strings , can use compare function)

  ```javascript
  const array1 = ['one', 'two', 'three'];
  console.log('array1:', array1);
  // expected output: "array1:" Array ["one", "two", "three"]
  
  const reversed = array1.reverse();
  console.log('reversed:', reversed);
  // expected output: "reversed:" Array ["three", "two", "one"]
  console.log('array1:', array1);
  // expected output: "array1:" Array ["three", "two", "one"]
  
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

+ reducer (combine array to a simpler value)

  ```javascript
  const array1 = [1, 4, 9, 16];
  
  // pass a function to map
  const map1 = array1.map(x => x * 2);
  
  console.log(map1);
  // expected output: Array [2, 8, 18, 32]
  [0, 1, 2, 3, 4].reduce(function(preValue, currentValue, currentIndex, array) {
    return preValue + currentValue
  },initialValue)
  //if no initialValue skip first iteration and take first value as the initial value
  ```

+ split : string => array

+ join : array => string

## Array Spread operator ES6+

```js
const a = [1,2,3]
const b = [0,...a,4] // [0,1,2,3,4]
const c = [...a]
a === c //false
Math.min(a)//NaN
Math.min(...a)//1
```

## Array destructuring

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

## Array-like-object

![image-20200530211222871](assets/JS__basic/image-20200530211222871.png)

## Function

![image-20200221180104305](assets/JS__basic/image-20200221180104305.png)

Functions are one of the fundamental building blocks in JavaScript. A function is a JavaScript object (store your code - a set of statements ) that can be invoked (run) later.

To use a function, you must define it somewhere in the scope and then you can call it any time and any where you want.

![image-20200612145620350](assets/JS__basic/image-20200612145620350.png)

Function is an object in JavaScript

You can defined a function inside a function

![image-20200524210656777](assets/JS__basic/image-20200524210656777.png)

![image-20200612204848376](assets/JS__basic/image-20200612204848376.png)

## Anonymous function

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

## bind method

bind(context[...,arguments ])

return a new reference at a function in place which is preconfigured regarding the arguments it receives

not executed immediately

can append more arguments which is not preconfigured when call the function

## call and apply method

allow to preconfigured arguments to the function but immediately execute the function

## Arguments variable

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

## Rest parameters ES6+

Take all arguments and merges to an array

Must be the last argument and 1 rest parameters

```js
function sum(...nums) {
    return nums.reduce((a,b) => a+b) //nums: [1,2,3,4,5]
}
sum(1,2,3,4,5) // 15
```

## Arrow function ES6+

![image-20200524212628601](assets/JS__basic/image-20200524212628601.png)

## Object

![image-20200531222729940](assets/JS__basic/image-20200531222729940.png)

## Native objects

​ Objects that are part of the JavaScript language defined by the ECMAScript specification, such as `String`, `Math`, `RegExp`, `Object`, `Function`, etc.

## Host objects

​ Host objects are provided by the runtime environment (browser or Node), such as `window`, `XMLHTTPRequest`, etc.

​ The object type refers to a compound value where you can set properties (named locations) that each hold their own values of any type.

## Create object

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

## Access object

Dot notation

```js
//Dot notation
obj.key 
obj.method()
//Added New properties
obj.age = 27
obj.getAge = function(){
 return this.age;
}
```

Square bracket notation

```js
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

## Object Spread operator ES6+

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

## Object destructuring

```javascript
// Variable assignment.
const o = { p: 42, q: true };
const { p, q } = o;

console.log(p); // 42
console.log(q); // true
```

## Descriptor

![image-20200602004653502](assets/JS__basic/image-20200602004653502.png)

![image-20200602004806899](assets/JS__basic/image-20200602004806899.png)

![image-20200602005053500](assets/JS__basic/image-20200602005053500.png)

## document & window

document: give access to your loaded HTML document

window: give access to browser's core APIs (just the active tab)

![image-20200525233250502](assets/JS__basic/image-20200525233250502.png)

## Class

<https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes>

JavaScript classes, introduced in ECMAScript 2015, are primarily syntactical sugar over JavaScript's existing prototype-based inheritance. The class syntax *does not* introduce a new object-oriented inheritance model to JavaScript.

Classes are in fact "special functions"

## Constructor method

```js
// ES6 Class
class Person {
  constructor(name) {
    this.name = name; //this = object is being created by using the new keyword
  }
}
const a = new Person("hi")// this = a
```

## Fields vs Properties

![image-20200601212615544](assets/JS__basic/image-20200601212615544.png)

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

![image-20200602003702552](assets/JS__basic/image-20200602003702552.png)

```js
class Private { 
    #privateField = "private" 
    #privateMethod() {}
    //new syntax becareful
}
```

## Binding class method

![image-20200601214159413](assets/JS__basic/image-20200601214159413.png)

![image-20200601214143079](assets/JS__basic/image-20200601214143079.png)

![image-20200602003253632](assets/JS__basic/image-20200602003253632.png)

![image-20200602003417752](assets/JS__basic/image-20200602003417752.png)

## static

![image-20200601233442514](assets/JS__basic/image-20200601233442514.png)

Static class members (properties/methods) are not tied to a specific instance of a class and have the same value regardless of which instance is referring to it. Static properties are typically configuration variables and static methods are usually pure utility functions which do not depend on the state of the instance.

## Getter & Setter

![image-20200601235032694](assets/JS__basic/image-20200601235032694.png)

## Inheritance

<https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/super>

![image-20200601235316105](assets/JS__basic/image-20200601235316105.png)

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

## Immediately-Invoked Function Expression (IIFE)

An IIFE is an anonymous function that is created and then immediately invoked. It’s not called from anywhere else (hence why it’s anonymous), but runs just after being created.

![image-20200612210709261](assets/JS__basic/image-20200612210709261.png)

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

![image-20200611133605096](assets/JS__basic/image-20200611133605096.png)

![image-20200611132342219](assets/JS__basic/image-20200611132342219.png)

![image-20200611131250853](assets/JS__basic/image-20200611131250853.png)

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

## Dynamic import

Import code in a later time, not when your web app load but only when we need that code

![image-20200611133255822](assets/JS__basic/image-20200611133255822.png)

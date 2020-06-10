# **JavaScript**

## What is JavaScript

JavaScript is a **dynamic**, **weakly typed** **programming language** which is **compiled at runtime**. It can be executed as part of a webpage in a browser or directly on any machine (**“host environment”**).

JavaScript was created to make webpages more dynamic (e.g. change content on a page directly from inside the browser).

It  has **nothing in common with Java** and **no alternatives to JS in the browser**

JavaScript is a specific version of ECMAScript

![image-20200521234356795](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200521234356795.png)

![image-20200609162703842](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609162703842.png)

For implementation (browser, runtime)![image-20200609162754749](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609162754749.png)

## How JS execute

![image-20200523003553345](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200523003553345.png)

## Dynamic and Weakly-typed

![image-20200523112734856](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200523112734856.png)

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



## Datatype

+ Primitive type: Deep copy, store in stack
  + Number
  
  + String
  
  + Boolean
  
  + undefined (Something hasn't been initialized auto set by JS)
  
  + null (Something is currently unavailable set by Dev)
  
    ```javascript
    typeof null; // “object”
    typeof undefined; // “undefined”
    ```
  
+ Reference type: Shallow copy, store value in heap
  - Array
  - Object ({key: value})
  - Function

![image-20200523224859003](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200523224859003.png)

![image-20200531222813663](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200531222813663.png)

![image-20200609173712566](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609173712566.png)

### Floating point  (Im)Precision

![image-20200609174146528](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609174146528.png)

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

## Loop

![image-20200523210555960](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200523210555960.png)

break: stop looping

continue: skip this loop

## Function

![image-20200221180104305](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200221180104305.png)

Functions are one of the fundamental building blocks in JavaScript. A function is a JavaScript procedure—a set of statements that performs a task or calculates a value. To use a function, you must define it somewhere in the scope and call it any time and any where you want.

Function just be an object in javascript 

You can defined a function inside a function

![image-20200524210656777](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200524210656777.png)

### Anonymous function

One time used no named function

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

##  Array-like-object

![image-20200530211222871](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200530211222871.png)

## Iterable

![image-20200530211212489](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200530211212489.png)

## Set

![image-20200530223213192](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200530223213192.png)

```js
var array = [1, 2, 3, 5, 1, 5, 9, 1, 2, 8];
Array.from(new Set(array)); // [1, 2, 3, 5, 9, 8]
```

![image-20200530224402270](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200530224402270.png)

## Map

![image-20200530223719341](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200530223719341.png)

![image-20200530223740138](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200530223740138.png)

```js
let myMap = new Map()
myMap.set('bla','blaa')
myMap.set('bla2','blaa2')
console.log(myMap)  // Map { 'bla' => 'blaa', 'bla2' => 'blaa2' }

myMap.has('bla')    // true
myMap.delete('bla') // true
console.log(myMap)  // Map { 'bla2' => 'blaa2' }
```

![image-20200530224451404](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200530224451404.png)

**for obj:**![image-20200530223811758](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200530223811758.png)

## Difference

![image-20200530222255014](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200530222255014.png)

![image-20200530222307367](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200530222307367.png)

## Object

![image-20200531222729940](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200531222729940.png)

### 	Native objects 

​	Objects that are part of the JavaScript language defined by the ECMAScript specification, such as `String`, `Math`, `RegExp`, `Object`, `Function`, etc.

 [JS_NativeObj.md](JS_NativeObj.md) 

### 	Host objects

​	Host objects are provided by the runtime environment (browser or Node), such as `window`, `XMLHTTPRequest`, etc.

​	The object type refers to a compound value where you can set properties (named locations) that each hold their own values of any type.

### Create object

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

Generally, `this` refers to the "thing" which called a function (if used inside of a function). That can be the global context, an object or some bound data/ object (e.g. when the browser binds `this` to the button that triggered a click event).

![image-20200601001943153](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601001943153.png)

![image-20200601002806023](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601002806023.png)

#### `this` in Global Context (i.e. outside of any function)

```js
function something() { ... }
something === window.something // true
console.log(this); // logs global object (window in browser) - ALWAYS (also in strict mode)!
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

## OOP

![image-20200601211040597](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601211040597.png)

![image-20200601211116941](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601211116941.png)

![image-20200601211007877](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601211007877.png)

## Class 

![image-20200601211422042](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601211422042.png)

![image-20200601234636997](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601234636997.png)

### Fields vs Properties

![image-20200601212615544](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200601212615544.png)

```js
class MyClass() {
  /* properties - don't depend on the constructor*/
  prop1 = 1;
  prop2; 
  /* 
  	This is a property that this class instance will have but
  	Created when constructor is called but after super is executed completely
  */

  /* easy to see what the constructor does that is only about *constructing* the object */
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
class Student extends Person {
  constructor(name, studentId) {
    super(name);
    this.studentId = studentId;
  }
}

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
      //call constructor of parrent class (super.function())
      //must call before accessing 'this'
    this.name = 'Square';
  }
}
```

## Behind the scenes of class and object in JavaScript

### Constructor function vs Classes

![image-20200602210506813](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200602210506813.png)

![image-20200602210528810](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200602210528810.png)

![image-20200602210252909](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200602210252909.png)

### new

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

### Prototype

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

#### `Object` constructor

Default prototype of a function or any object you create on your own with the object literal notation `{}` always uses global object constructor. So any object created with the object literal notation or the object created by Javascript automatically (default prototype of a function) will be based on the object constructor function `new Object()` and will therefore use `Object.prototype` as its fallback value

`Object.prototype` has no other prototype (access `__proto__` will return null)

#### Prototype is shared between instance

```js
var person1 = new Human("Virat", "Kohli");
var person2 = new Human("Sachin", "Tendulkar");
person1.__proto__ === person2.__proto__ //true
```

#### Summary

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

**A prototype is just a base class of object**

### Constructor

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

### Inherit with function

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
Person.prototype.constructor === AgedPerson   // => true
Person.prototype.constructor === Person // => false

//Fix the Person prototype constructor
Person.prototype.constructor = Person;

console.log(new Person())
```

![image-20200605212352374](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200605212352374.png)

### Class use  prototype

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

### Method

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

### Built-in prototype

Array.prototype

String.prototype

## Browser API

### DOM

Read more [JS_DOM.md](JS_DOM.md) 

### Timers & intervals

![image-20200608121224915](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608121224915.png)

![image-20200608121324990](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608121324990.png)

![image-20200608121503051](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608121503051.png)

![image-20200608121524339](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608121524339.png)

**minimum** time to run the callback (waiting for the callstack is empty)

### location (URL and page we're on)

![image-20200608124840159](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608124840159.png)

### history

![image-20200608124758794](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608124758794.png)

### navigator

![image-20200608125128292](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608125128292.png)

### Date

![image-20200608125559605](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608125559605.png)

### Error

![image-20200608125712784](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608125712784.png)

### JSON 

JSON.stringify(object)

JSON.parse(JSONstring)

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

Event delegation is a technique involving adding event listeners to a **parent** element instead of adding them to the child elements. 

![image-20200608150822633](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608150822633.png)

The listener will fire whenever the event is triggered on the descendant elements due to **event bubbling up** the DOM. The benefits of this technique are:

- Only have 1 listener
- There is no need to unbind the handler from elements that are removed and to bind the event for new elements.

```html
<ul id="parent-list">
	<li id="post-1">Item 1</li>
	<li id="post-2">Item 2</li>
	<li id="post-3">Item 3</li>
	<li id="post-4">Item 4</li>
	<li id="post-5">Item 5</li>
</ul>
```

```js
document.getElementById("parent-list").addEventListener("click", 			function(e) {
		// e.target is the clicked element!
   		event.target.closest('li').classList.toggle('highlight');
    }
 );
```

### Trigger event programmatically

use element method (submit(),click(),...)

### this in event handler

this is bound to the event's source - the element which is registered the event listener  (not for arrow function)

![image-20200608152759049](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608152759049.png)

![image-20200608152825710](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608152825710.png)

## Hoisting

Hoisting is a term used to explain the behavior of variable declarations in your code. Variables declared or initialized with the `var` keyword will have their declaration "moved" up to the top of their module/function-level scope, which we refer to as hoisting

```javascript
console.log(foo); // undefined
var foo = "foo";
//code above behavior like this
var foo
console.log(foo); // undefined
foo = "foo";

console.log(baz); // ReferenceError: can't access before initialization
let baz = "baz";

console.log(bar); // ReferenceError: can't access before initialization
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

![img](https://images.viblo.asia/e70b623f-c197-4435-b1cc-13bde4dd0a6a.png)![img](https://images.viblo.asia/af1f1df2-cd4b-4612-afa3-a1750d79d271.png)

## More about function

### Pure function

![image-20200608183520088](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608183520088.png)

Should aim for pure function than impure function (reduce impure function), because they are predictable (not do anything behind the scene)

### Higher order function

A **higher order function** is a function that takes a function as an argument, or returns a function

### Factory function

A function return a function

![image-20200608220359309](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608220359309.png)

### Closure 

If you have a function in a function, that **inner function** can use all the variables or parameters of the outer function and all variables that are defined globally (access outer environment), the **outer function** can not access the inner functions specific constants or variables.

When a function is created as we do it here with the function keyword by using this function declaration, then this function creates a new **lexical environment** and registers any variables it has access to inside of this environment.

**Every function** closes over the surrounding environment which means it **registers** the **surrounding environment** and the variables registered there and it **memorizes the values of these variables**. If you then change a variable, well that is **reflected** inside of the function.

So a function, every function in Javascript is a closure because it closes over the variables defined in its environment and it kind of memorizes them, so that they're not thrown away when you don't need them in the surrounding context anymore and you can still use them from inside of the inner function

#### Summary

Functions remember the surrounding variables

```javascript
let name = "Max"
function greet() { // lexical environment
    console.log(name) // access name in outer environment
}
//greet() => Max
name = "Huy"
greet() //=> Huy
```

```js
let name = "Max"
function greet() { // lexical environment
    let userName = name // store name, part of the inner evironment
    console.log(userName) 
}
//greet() => Max
name = "Huy"
greet() //=> Huy
```

```js
function greet() { // lexical environment
    console.log(userName) 
}
let name = "Max"
greet() //=> Max
```

### Scope /  Lexical environment

Scope is the accessibility of variables, functions, and objects in some particular part of your code during runtime. In other words, scope determines the visibility of variables and other resources in areas of your code. (lexical environment for the curly braces)

#### Global Scope

```javascript
var name = 'Hammad';

console.log(name); // logs 'Hammad'

function logName() {
    console.log(name); // 'name' is accessible here and everywhere else
}
```

#### Local Scope

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

```javascript
if (true) {
    // if conditional block or while() {} or for() {} also create a scope
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
```

Global scope lives as long as your application lives. Local Scope lives as long as your functions are called and executed.

#### Function Arguments

Function arguments (parameters) work as local variables inside functions.

#### The Lifetime of Variables

The lifetime of a JavaScript variable starts when it is declared.

Local variables are deleted when the function is completed.

In a web browser, global variables are deleted when you close the browser window (or tab).

### Recursion

![image-20200608235527130](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608235527130.png)

![image-20200608235802861](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608235802861.png)

### Context

*Context* is used to refer to the value of `this` in some particular part of your code.

1. If the `new` keyword is used when calling the function, `this` inside the function is a brand new object.
2. If `apply`, `call`, or `bind` are used to call/create a function, `this` inside the function is the object that is passed in as the argument.
3. If a function is called as a method, such as `obj.method()` — `this` is the object that the function is a property of.
4. If a function is invoked as a free function invocation, meaning it was invoked without any of the conditions present above, `this` is the global object. In a browser, it is the `window` object. If in strict mode (`'use strict'`), `this` will be `undefined` instead of the global object.
5. If multiple of the above rules apply, the rule that is higher wins and will set the `this` value.
6. If the function is an ES2015 arrow function, it ignores all the rules above and receives the `this` value of its surrounding scope at the time it is created

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

#### Changing Context with method

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

#### Arrow function have no context

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

### Execution Context (CallStack)

JavaScript is a single-threaded language so it can only execute a single task at a time. The rest of the tasks are queued in the Execution Context.

JavaScript interpreter starts to execute your code, the context (scope) is by default set to be global. This global context is appended to your execution context which is actually the first context that starts the execution context.

After that, each function call (invocation) would append its context to the execution context. The same thing happens when an another function is called inside that function or somewhere else.

### Curry function

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
```

## Asynchronous code

### JS runtime environment

event loop, API's, task/callback queue, job/micro-task queue

![img](https://miro.medium.com/max/700/1*zeKjWCjyAGZ9JN4fvnWsiA.png)

*AJAX, the DOM tree, and other API’s, are not part of JavaScript (Engine), they are just objects with properties and methods, provided by the browser and made available in the browser’s JS Runtime Environment.*

Also in the runtime environment is a **JavaScript Engine (callstack, memory heap)** that parses and run the code

### JS is single-threaded

![image-20200609200301370](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609200301370.png)

### Some code can't be finished immediately

timeout, http request, event listener,...

![image-20200609200626297](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609200626297.png)

=> Blocking code execution

![image-20200609201736970](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609201736970.png)

=> **Runtime environment** is responsible for asynchronous action by using **multiple threads**, so JS code will not blocked in main thread.

### How we can do asynchronous  action without blocking

JS runtime environment provide us **APIs** to work with threads in **parallel**. 

From **call stack**, if an asynchronous action pop off the stack, it will be handed to the **API container (environment)** to run in **another thread**. When any action is done or an event occurred, the **callback handler function** is sent to the end of the **callback (message) queue**. 

**Event loop** job is waiting to the stack empty, take one item in queue, push to the **call stack** and then the **JS engine** will handle the callback code

![image-20200209104121348](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200209104121348.png)

**TLDR**: Handing the asynchronous (long) task to the environment and give function to do when done task (call-back), then we continue in JavaScript so that Javascript code is **never blocked** (if there is no really long JS operations like for loops, heavy calculation,...)

#### Event loop in actions

![image-20200609221539480](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609221539480.png)

=> Browser handler setTimeout, JS continue exec code

![image-20200609222017101](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609222017101.png)

=> done timer

![image-20200609222401163](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609222401163.png)

=> When the call stack is empty, the event loop executes and push a waiting messages to the call stack and JS engine will executes it 

![image-20200609223824366](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609223824366.png)

![image-20200609221007727](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609221007727.png)

![image-20200609221404314](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609221404314.png)

##### Browser![image-20200209102003696](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200209102003696.png)

##### Node

No script parsing event (<script> tag)

No user interaction (clicking on the page)

No animation frame callbacks, no render (No DOM **Manipulation**)

![image-20200209103048426](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200209103048426.png)

##### Web worker

No script parsing event (<script> tag)

No user interaction

No DOM **Manipulation**

### Asynchronous Callback

Function run after finish an async job, put in the callback queue != function is passed to another simple function (not an API of browser), put in the call stack

![image-20200610201745892](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200610201745892.png)

Task depend on before task => Callback in Callback => Callback hell

### Promise 

![image-20200610201843824](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200610201843824.png)

A promise is an object that may produce a single value some time in the future

Promises are eager, meaning that a promise will start doing the task you give it (the function passed to the promise) as soon as the promise constructor is invoked

![image-20200610203008825](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200610203008825.png)

![image-20200610203929118](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200610203929118.png)

![image-20200610204057687](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200610204057687.png)

![image-20200610204452121](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200610204452121.png)

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

#### Then()

```javascript
promise.then(
  onFulfilled?: Function,
  onRejected?: Function
) => Promise
```

The `.then()` method must comply with these rules:

- Both `onFulfilled()` and `onRejected()` are optional.

- If the arguments supplied are not functions, they must be ignored.

- `onFulfilled()` will be called after the **promise** is fulfilled, with the promise’s value as the first argument.v

- `onRejected()` will be called after the **promise** is rejected, with the reason for rejection as the first argument. The reason may be any valid JavaScript value recommend using Error objects (rejections = exceptions)

- Neither `onFulfilled()` nor `onRejected()` may be called more than once.

- `.then()` may be called many times on the same promise. In other words, a promise can be used to aggregate callbacks.

- `.then()` will return a new promise, `promise2`.

- when `onFulfilled()` or `onRejected()` return something.  If you return a value `x`, the next `then()` is called  (`promise2` **will be fulfilled**) with that value `x`. However, if you return something promise-like `p`, the next `then()` waits on it (`promise2` **resolve**  **whatever `p` resolves**) , and is only called when that promise settles (succeeds/fails).

  ```js
  let p1 = new Promise(function(resolve, reject) {
      resolve(42);
  });
  
  let p2 = new Promise(function(resolve, reject) {
      resolve(43);
  });
  
  let p3 = p1.then(function(value) {
      // first fulfillment handler
      console.log(value);     // 42
      return p2;
  });
  
  p3.then(function(value) {
      // second fulfillment handler
      console.log(value);     // 43
  });
  ```

- If either `onFulfilled` or `onRejected` throws an exception `e`, `promise2` will be rejected with `e` as the reason.

- If `onFulfilled` is not a function and `promise1` is fulfilled, `promise2` will be fulfilled with the same value as `promise1`.

- If `onRejected` is not a function and `promise1` is rejected, `promise2` will be rejected with the same reason as `promise1`.

- A pending promise may transition into a fulfilled or rejected state.

- A fulfilled or rejected promise is settled, and must not transition into any other state.

- Once a promise is settled, it must have a value (which may be `undefined`). That value must not change.

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

[promise1, promise2, promise3].reduce(function(currentPromise, promise) {
  return currentPromise.then(promise)
}, Promise.resolve())

// Same
Promise.resolve().then(promise1).then(promise2).then(promise3)
```

#### Error

If a promise in chain reject, **all** then() will be **skipped** until meet a catch() 

```javascript
promise 
  .then(handleSuccess)
  .catch(handleError)
  .then(continueHandler)
```

#### Promises instead of callbacks?

- Avoid callback hell which can be unreadable.
- Makes it easy to write sequential asynchronous code that is readable (1 nested level) with `.then()`.
- Makes it easy to write parallel asynchronous code with `Promise.all()`.
- With promises, these scenarios which are present in callbacks-only coding, will not happen:
  - Call the callback too early
  - Call the callback too late (or never)
  - Call the callback too few or too many times
  - Fail to pass along any necessary environment/parameters
  - Swallow any errors/exceptions that may happen

### Async/Await 

Async functions are functions that auto return a promises

```javascript
// Async/Await version
async function helloAsync() {
  return "hello";
}
// Promises version
function helloAsync() {
  return new Promise(function (resolve) {
    resolve("hello");
  });
}
```

Write async code a bit more like synchronous code

![image-20200610221055590](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200610221055590.png)

#### How it works

-  **Async Functions** are declared by prepending the word `async` in their declaration `async function doAsyncStuff() { ...code }`

   It wraps everything inside of the async function into one big promise.

- Your code can waiting for a **promise settled**  with `await` and next line code can only execute after that **promise settled** (just code in the async function)

  It replicate `then` behind the scene, return that promise and get result of the promise 

- `await` can only be used inside an `async` function.

  ```js
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

  ![image-20200211180859185](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200211180859185.png)

  ![image-20200610222512399](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200610222512399.png)

#### Error handle

- If a **promise** throws an exception or `reject()` , we can handle by using `try/catch`. **exceptions will get swallowed** if they are not caught somewhere in the async function chain. 
- It is a good practice to always have one `try/catch` per chain. This will provide one single place to deal with errors while doing async work and will force you to correctly chain your **Async Function** calls.

![image-20200610223453174](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200610223453174.png)

#### Use an async function

```javascript
// Option 1:
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

## Anonymous function

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

## Immediately-Invoked Function Expression (IIFE)

An IIFE is an anonymous function that is created and then immediately invoked. It’s not called from anywhere else (hence why it’s anonymous), but runs just after being created.

```javascript
function foo(){ }();// not working

(function () {return 5;} ());
```

## Best practice

### .forEach() vs .map()

**`forEach`**

- Iterates through the elements in an array.
- Executes a callback for each element.
- Does not return a value.

```javascript
const a = [1, 2, 3];
a.forEach((num, index) => {
  // Do something with num and/or index.
});
```

If you simply need to iterate over an array, `forEach` is a fine choice.

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

If you need the result, but do not wish to mutate the original array, `.map()` is the clear choice. 

### Drag & Drop

https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API

https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API/Recommended_drag_types

https://developer.mozilla.org/en-US/docs/Web/API/DataTransfer/effectAllowed

![image-20200608161618766](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608161618766.png)

![image-20200608171018463](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608171018463.png)

![image-20200608180617462](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608180617462.png)

![image-20200608172954427](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608172954427.png)

![image-20200608180315371](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200608180315371.png)

### Infinitive scrolling

```js
let curElementNumber = 0;

function scrollHandler() {
    const distanceToBottom = document.body.getBoundingClientRect().bottom;

    if (distanceToBottom < document.documentElement.clientHeight + 150) {
        const newDataElement = document.createElement('div');
        curElementNumber++;
        newDataElement.innerHTML = `<p>Element ${curElementNumber}</p>`;
        document.body.append(newDataElement);
    }
}

window.addEventListener('scroll', scrollHandler);
```

**So what's happening here?**

At the very bottom, we register the `scrollHandler` function as a handler for the `'scroll'` event on our window object.

Inside that function, we first of all **measure the total distance** between our viewport (top left corner of what we currently see) and the end of the page (**not** just the end of our currently visible area) => Stored in `distanceToBottom`.

*For example, if our browser window has a height of* `500px`*, then distanceToBottom could be* `684px`*, assuming that we got some content we can scroll to.*

Next, we **compare the distance** to the bottom of our overall content (`distanceToBottom`) **to the window height + a certain threshold** (in this example `150px`). `document.documentElement.clientHeight` is preferable to `window.innerHeight` because it respects potential scroll bars.

If we have **less than** **150px** **to the end of our page content**, we make it into the if-block (where we append new data).

Inside of the if-statement, we then create a new `<div>` element and populate it with a `<p>` element which in turn outputs an incrementing counter value.

### Difference between console.dir and console.log

The **console.log()** returns the object in its string representation and **console.dir()** recognizes the object just as an object and outputs its properties. Both log() and dir() returns the string just as a string.

### Boolean trick

![image-20200523210404436](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200523210404436.png)

### import external "\<script>" to html

Download both script and HTML

Run script after browser parsed and rendered all the HTML

- Put script to end of body: have to wait downloading and parsing HTML complete then begin downloading and executing script
- If async is present: The script is executed asynchronously with the rest of the page (the script will be executed while the page continues the parsing)
- If async is not present and defer is present: The script is executed when the page has finished parsing
- If neither async or defer is present: The script is fetched and executed immediately, before the browser continues parsing the page

### Obj equality

```javascript
function isEquivalent(a, b) {
    // Create arrays of property names
    var aProps = Object.getOwnPropertyNames(a);
    var bProps = Object.getOwnPropertyNames(b);

    // If number of properties is different,
    // objects are not equivalent
    if (aProps.length != bProps.length) {
        return false;
    }

    for (var i = 0; i < aProps.length; i++) {
        var propName = aProps[i];

        // If values of same property are not equal,
        // objects are not equivalent
        if (a[propName] !== b[propName]) {
            return false;
        }
    }

    // If we made it this far, objects
    // are considered equivalent
    return true;
}
```

### Remove duplicates of an array and return an array of only unique elements

```javascript
// ES6 Implementation
var array = [1, 2, 3, 5, 1, 5, 9, 1, 2, 8];
Array.from(new Set(array)); // [1, 2, 3, 5, 9, 8]
// ES5 Implementation
var array = [1, 2, 3, 5, 1, 5, 9, 1, 2, 8];
uniqueArray(array); // [1, 2, 3, 5, 9, 8]
function uniqueArray(array) {
  var hashmap = {};
  var unique = [];
  for(var i = 0; i < array.length; i++) {
    // If key returns undefined (unique), it is evaluated as false.
    if(!hashmap[array[i]]) {
      hashmap[array[i]] = 1;
      unique.push(array[i]);
    }
  }
  console.log(unique);
}
```

### Reverse string 

```javascript
var string = "Welcome to this Javascript Guide!";

// Output becomes !ediuG tpircsavaJ siht ot emocleW
var reverseEntireSentence = reverseBySeparator(string, "");

// Output becomes emocleW ot siht tpircsavaJ !ediuG
var reverseEachWord = reverseBySeparator(reverseEntireSentence, " ");

function reverseBySeparator(string, separator) {
  return string.split(separator).reverse().join(separator);
}
```

### Empty array

```javascript
var arrayList = ['a', 'b', 'c', 'd', 'e', 'f']; // Created array
var anotherArrayList = arrayList;  // Referenced arrayList by another variable
arrayList = []; // Empty the array
console.log(anotherArrayList); // Output ['a', 'b', 'c', 'd', 'e', 'f']

var arrayList = ['a', 'b', 'c', 'd', 'e', 'f']; // Created array
var anotherArrayList = arrayList;  // Referenced arrayList by another variable
arrayList.length = 0; // Empty the array by setting length to 0
console.log(anotherArrayList); // Output []

var arrayList = ['a', 'b', 'c', 'd', 'e', 'f']; // Created array
var anotherArrayList = arrayList;  // Referenced arrayList by another variable
arrayList.splice(0, arrayList.length); // Empty the array by setting length to 0
console.log(anotherArrayList); // Output []

while(arrayList.length) {
  arrayList.pop();
}
```

### Is Array

```javascript
Array.isArray(arrayList);
```

### Closure private counter

```js
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

### Set prototype

![image-20200605214747603](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200605214747603.png)

### Check integer

```js
function isInt(num) {
  return num % 1 === 0;
}
```

### Iterating

For objects:

- `for` loops - `for (var property in obj) { console.log(property); }`. However, this will also iterate through its inherited properties, and you will add an `obj.hasOwnProperty(property)` check before using it.
- `Object.keys()` - `Object.keys(obj).forEach(function (property) { ... })`. `Object.keys()` is a static method that will lists all enumerable properties of the object that you pass it.
- `Object.getOwnPropertyNames()` - `Object.getOwnPropertyNames(obj).forEach(function (property) { ... })`. `Object.getOwnPropertyNames()` is a static method that will lists all enumerable and non-enumerable properties of the object that you pass it.

For arrays:

- `for` loops - `for (var i = 0; i < arr.length; i++)`. The common pitfall here is that `var` is in the function scope and not the block scope and most of the time you would want block scoped iterator variable. ES2015 introduces `let` which has block scope and it is recommended to use that instead. So this becomes: `for (let i = 0; i < arr.length; i++)`.
- `forEach` - `arr.forEach(function (el, index) { ... })`. This construct can be more convenient at times because you do not have to use the `index` if all you need is the array elements. There are also the `every` and `some` methods which will allow you to terminate the iteration early.

### Difference between document `load` event and document `DOMContentLoaded` event?

The `load` event fires at the end of the document loading process. At this point, all of the objects in the document are in the DOM, and all the images, scripts, links and sub-frames have finished loading.

The DOM event `DOMContentLoaded` will fire after the DOM for the page has been constructed, but do not wait for other resources to finish loading. This is preferred in certain cases when you do not need the full page to be loaded before initializing.

### **FizzBuzz Challenge** 

Create a for loop that iterates up to `100` while outputting **"fizz"** at multiples of `3`, **"buzz"** at multiples of `5` and **"fizzbuzz"** at multiples of `3` and `5`.

```js
for (let i = 1; i <= 100; i++) {
  let f = i % 3 == 0,
    b = i % 5 == 0;
  console.log(f ? (b ? 'FizzBuzz' : 'Fizz') : b ? 'Buzz' : i);
}
```

### Error

```javascript
var err1 = Error('message');
var err2 = new Error('message');
```

### What's the difference between an "attribute" and a "property"?

Attributes are defined on the HTML markup but properties are defined on the DOM

```javascript
const input = document.querySelector("input");
console.log(input.getAttribute("value")); // Hello
console.log(input.value); // Hello
```

But after you change the value of the text field by adding "World!" to it, this becomes:

```javascript
console.log(input.getAttribute("value")); // Hello
console.log(input.value); // Hello World!
```

### Singleton

```js
class Singleton {
    constructor() {
        const instance = this.constructor.instance;
        if (instance) {
            return instance;
        }
        this.constructor.instance = this;
    }
    foo() {
        // ...
    }
}
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

### Private scope

In JavaScript, there is no such thing as public or private scope. However, we can emulate this feature using closures.

```javascript
(function () {
  // private scope
})();
```

### HTTP request

```js
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
```


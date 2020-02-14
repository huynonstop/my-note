# JavaScript Basic

## Variable

JS is dynamically typed ( >< statically type)

Key word : var, let, const (A variable declared outside a function, becomes **GLOBAL**) 

![Kết quả hình ảnh cho let const var difference](https://kickoff.tech/wp-content/uploads/2019/04/const-vs-let-vs-var-scope.png)

## Datatype

+ Primitive type: Deep copy
  + Number
  + String
  + Boolean
  
  + undefined (Something hasn't been initialized)
  
  + null (Something is currently unavailable)
  
    ```javascript
    typeof null; // “object”
    typeof undefined; // “undefined”
    ```
  
+ Reference type: Shallow copy
  - Array
  - Object ({key: value})
  - Function

## Array

array, element, index, length

An `array` is an object that holds values (of any type) not particularly in named properties/keys, but rather in numerically indexed positions

### Array method

+ Return shallow copy:  Both the original and new array refer to the same object, copy the values of strings and numbers into the new array

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

  + filter ( **creates a new array** with all elements that pass the test implemented by the provided function)

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

  + map 

    ```javascript
    const array1 = [1, 4, 9, 16];
    
    // pass a function to map
    const map1 = array1.map(x => x * 2);
    
    console.log(map1);
    // expected output: Array [2, 8, 18, 32]
    ```

  + reducer

    ```javascript
    [0, 1, 2, 3, 4].reduce(function(accumulator, currentValue, currentIndex, array) {
      return accumulator + currentValue
    },initialValue)
    ```

    | `callback` iteration | `accumulator` | `currentValue` | `currentIndex` | `array`           | return value |
    | :------------------- | :------------ | :------------- | :------------- | :---------------- | :----------- |
    | first call           | `10`          | `0`            | `0`            | `[0, 1, 2, 3, 4]` | `10`         |
    | second call          | `10`          | `1`            | `1`            | `[0, 1, 2, 3, 4]` | `11`         |
    | third call           | `11`          | `2`            | `2`            | `[0, 1, 2, 3, 4]` | `13`         |
    | fourth call          | `13`          | `3`            | `3`            | `[0, 1, 2, 3, 4]` | `16`         |
    | fifth call           | `16`          | `4`            | `4`            | `[0, 1, 2, 3, 4]` | `20`         |

+ Return new length, change length of array:

  + push (add to the end, return new length)
  + unshift (add to the beginning, return new length)

+ Return element, change length of array:

  + pop (remove the last element, return that element)
  + shift (remove the first element, return that element)

```javascript
var a = [2,4,6]
var b = [4,5]

var c = b.concat(a) // c: [4,5,2,4,6]
a.push(7) // a: [2,4,6,7]
a.pop() // a: [2,4]
a.shift() // a: [4,6]
a.unshift(7) // a: [7,2,4,6]

```

### Set

```js
// ES6 Implementation
var array = [1, 2, 3, 5, 1, 5, 9, 1, 2, 8];

Array.from(new Set(array)); // [1, 2, 3, 5, 9, 8]
```

## Function

Functions are one of the fundamental building blocks in JavaScript. A function is a JavaScript procedure—a set of statements that performs a task or calculates a value. To use a function, you must define it somewhere in the scope from which you wish to call it.

```javascript
//// Function Declaration
function doSmth(input1,input2,...) {
	//DO SMTH
	return something
}
// Function Expression
var doSmth = function() {
    return something
}
//mul
console.log(mul(2)(3)(4)); // output : 24
```

## Object

### 	Native objects 

​	objects that are part of the JavaScript language defined by the ECMAScript specification, such as `String`, `Math`, `RegExp`, `Object`, `Function`, etc.

### 	Host objects

​	Host objects are provided by the runtime environment (browser or Node), such as `window`, `XMLHTTPRequest`, etc.

​	The object type refers to a compound value where you can set properties (named locations) that each hold their own values of any type.

```javascript
//1. obj literals
var obj = {
    key: value,
    method : function() {}
}
//Dot notation
obj.key // =value
obj.method()
//added New properties
obj.age = 27
obj.getAge = function(){
	return this.age;
}
//Square bracket notation
obj['key'] 
obj['method'] 
obj["weight"] = 65
obj.getWeight = function(){
	return this.weight;
}
//Accessed using a variable
var property = "key";
console.log(obj[property]) // Output: value
Console.log(obj.property) //Output: undefined
//Name
obj["date of birth"] = "Nov 28";
obj[12] = 12;
obj.12 = 12; //gives error
//Delete
delete obj.key; // return true

//advance obj literal
const name = 'abc'
const cat = {
    name,
    run() {
        console.log(this.name + "running")
    }
}
```

```javascript
//2. new operator => new + function => contrustor
//a) Constructor function + prototype

//Define the object specific properties inside the constructor
function Human(name, age){
	this.name = name,
	this.age = age,
	this.friends = ["Jadeja", "Vijay"]
}
//Define the shared properties and methods using the prototype
Human.prototype.sayName = function(){
	console.log(this.name);
}
//Create two objects using the Human constructor function
var person1 = new Human("Virat", 31);
var person2 = new Human("Sachin", 40);

//Lets check if person1 and person2 have points to the same instance of the sayName function
console.log(person1.sayName === person2.sayName) // true

//Let's modify friends property and check
person1.friends.push("Amit");

console.log(person1.friends)// Output: "Jadeja, Vijay, Amit"
console.log(person2.friends)//Output: "Jadeja, Vijay"

//b) Class ES6
class Cat {
    constructor(name){
        this.name = name;
    }
    run() {
    	console.log(this.name + "running")
    }
}
//3. Using the Object() constructor: 
var d = new Object(); //now  discouraged.
//4. Using Object.create() method:
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

//5. Singleton pattern:
var l = new function(){
  this.name = "hello";
}
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
```

![image-20200211182818925](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200211182818925.png)

Khi có constructor call, những hành động sau sẽ được thực hiện:

1. Một object mới được tạo ra (hay constructed) (by javascript engine)

   ```javascript
   function ConstructorExample() {
       // this = {};
       // this.__proto__ = PersonConstructor.prototype;
       console.log(this);
       this.value = 10;
       console.log(this);
       // return this;
   }
   
   new ConstructorExample();
   
   // -> ConstructorExample {}
   // -> ConstructorExample { value: 10 }
   ```

2. object mới này được gắn prototype

3. object mới được set thành this cho hàm gọi

4. nếu hàm không trả về object nào khác của nó, thì cái object mới tạo ra kia sẽ được trả về.

## Prototype

![img](https://i.stack.imgur.com/4pCXC.png)

![enter image description here](https://i.stack.imgur.com/UfXRZ.png)

![prototype là gì](https://topdev.vn/blog/wp-content/uploads/2019/06/prototype-la-gi.png)

`_proto__` is the actual object that is used in the lookup chain to resolve methods, etc. `prototype` is the object that is used to build `__proto__` when you create an object with `new`:

```js
( new Foo ).__proto__ === Foo.prototype;
( new Foo ).prototype === undefined;
//prototype is only available on functions since they are derived from Function, Function, and Object but in anything else it is not. However, __proto__ is available everywhere.
 Foo.__proto__ === Foo.constructor.prototype 
```

Prototype is a property on a function that points to an object.

JavaScript is often described as a **prototype-based language** — to provide inheritance, objects can have a **`prototype` object**, which acts as a template object that it inherits methods and properties from.

An object's prototype object may also have a prototype object, which it inherits methods and properties from, and so on. This is often referred to as a **prototype chain**, and explains why different objects have properties and methods defined on other objects available to them.

In JavaScript, a link is made between the object instance and its prototype (its `__proto__` property, which is derived from the `prototype` property on the constructor), and the properties and methods are found by walking up the chain of prototypes.

![img](https://miro.medium.com/max/806/1*5qHhF8HTzZD2xdx3p-RLIQ.png)

```javascript
function Human(firstName, lastName) {
	this.firstName = firstName,
	this.lastName = lastName,
	this.fullName = function() {
		return this.firstName + " " + this.lastName;
	}
}
var person1 = new Human("Virat", "Kohli");
var person2 = new Human("Sachin", "Tendulkar");

Human.prototype === person1.__proto__ //true
person1.__proto__ === person2.__proto__ //true

//Dot notation
Human.prototype.name = "Ashwin";
console.log(Human.prototype.name)//Output: Ashwin

//Square bracket notation
Human.prototype["age"] = 26;
console.log(Human.prototype["age"]); //Output: 26
```



When a function is created in JavaScript, the JavaScript engine adds a `prototype` property to the function. This `prototype` property is an object (called as prototype object) which has a `constructor` property by default. The `constructor` property points back to the function on which `prototype` object is a property. We can access the function’s prototype property using `functionName.prototype`.

**Prototype object is shared among all the objects created using new.**

### Problem

1. Problem with the constructor function: Every object has its own instance of the function

   ![img](https://miro.medium.com/max/838/1*3ftgUPoTz5nSmGPwTjf7VA.png)

    It doesn’t make sense to have two instances of function `fullName` that do the same thing

2. Problem with the prototype: Modifying a property using one object reflects the other object also

   ```javascript
   Human.prototype.friends = ['Jadeja', 'Vijay']
   var person1 = new Human("Virat", "Kohli");
   var person2 = new Human("Sachin", "Tendulkar");
   person1.friends.push("Amit");
   console.log(person1.friends);// Output: "Jadeja, Vijay, Amit"
   console.log(person2.friends);// Output: "Jadeja, Vijay, Amit"
   ```

### Prototype Chaining

```js
function doSomething(){}
doSomething.prototype.foo = "bar"; // add a property onto the prototype
var doSomeInstancing = new doSomething();
doSomeInstancing.prop = "some value"; // add a property onto the object
console.log( doSomeInstancing );
```

```js
{
    prop: "some value",
    __proto__: {
        foo: "bar",
        constructor: ƒ doSomething(),
        __proto__: {
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
```

As seen above, the `__proto__` of `doSomeInstancing` is `doSomething.prototype`. But, what does this do? When you access a property of `doSomeInstancing`, the browser first looks to see if `doSomeInstancing` has that property.

If `doSomeInstancing` does not have the property, then the browser looks for the property in the `__proto__` of `doSomeInstancing` (a.k.a. doSomething.prototype). If the `__proto__` of doSomeInstancing has the property being looked for, then that property on the `__proto__` of doSomeInstancing is used.

If it still doesn't exits, the `__proto__` of the `__proto__` of doSomeInstancing (a.k.a. the `__proto__` of doSomething.prototype (a.k.a. `Object.prototype`)) is then looked through for the property being searched for.

### Prototype  Inherit

When a property is accessed on an object and if the property is not found on that object, the JavaScript engine looks at the object's `prototype`, and the `prototype`'s `prototype` and so on, until it finds the property defined on one of the `prototype`s or until it reaches the end of the prototype chain

![img](https://miro.medium.com/max/798/1*KAFq0uLa-GlTdT22e5YWLw.png)

```javascript

//SuperType constructor function
function SuperType(firstName, lastName){
	this.firstName = firstName,
	this.lastName = lastName,
	this.friends = ["Ashwin", "Jadeja"]
}
SuperType.prototype.getSuperName = function(){
	return this.firstName + " " + this.lastName;
}

//SubType prototype function
function SubType(firstName, lastName, age){
	//Inherit instance properties
	SuperType.call(this, firstName, lastName);
	this.age = age;
}

//Inherit the properties from SuperType
SubType.prototype = new SuperType(); 
//or
SubType.prototype = Object.create(SuperType.prototype);
SubType.prototype.constructor = SubType;
//Add new property to SubType prototype after the inheritance because inheritance overwrites the existing prototype of SubType
SubType.prototype.getSubAge = function(){
	return this.age;
}
//Create SubType objects
var subTypeObj1= new SubType("Virat", "Kohli", 26);
var subTypeObj2 = new SubType("Sachin", "Tendulkar", 39);

//Modify the friends property using the subTypeObj1
subTypeObj1.friends.push("Amit");

console.log(subTypeObj1.friends);//["Ahswin", "Jadega", "Amit"]
console.log(subTypeObj2.friends);//["Ashwin", "Jadega"]

//subTypeObj1
console.log(subTypeObj1.firstName); //Output: Virat
console.log(subTypeObj1.age); //Output: 26
console.log(subTypeObj1.getSuperName()); //Output: Virat Kohli
console.log(subTypeObj1.getSubAge()); //Output: 26

//subTypeObj2
console.log(subTypeObj2.firstName); //Output: Sachin
console.log(subTypeObj2.age); //Output: 39
console.log(subTypeObj2.getSuperName()); //Output: Sachin Tendulkar
console.log(subTypeObj2.getSubAge()); //Output: 39
```

![image-20200209221606111](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200209221606111.png)

## Class

**Classes are functions, and functions are objects in JS**

```javascript
// ES6 Class
class Person {
  constructor(name) {
    this.name = name;
  }
}
```

### Inherit

```javascript
// ES5 Function Constructor
function Student(name, studentId) {
  // Call constructor of superclass to initialize superclass-derived members.
  Person.call(this, name);

  // Initialize subclass's own members.
  this.studentId = studentId;
}

Student.prototype = Object.create(Person.prototype);
Student.prototype.constructor = Student;

// ES6 Class
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
    super(length, length); //call constructor of parrent class (super.function())
    this.name = 'Square';
  }
}
```

### static

Static class members (properties/methods) are not tied to a specific instance of a class and have the same value regardless of which instance is referring to it. Static properties are typically configuration variables and static methods are usually pure utility functions which do not depend on the state of the instance.

## Coercion

 conversion between different two build-in types

## Comparison operators

== — Equal to (with *coercion* allowed)

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

=== — Equal value and equal type (without allowing *coercion*)

 != — Not equal 

!== — Not equal value or not equal type   

< — Less than > — Greater than

<= — Less than or equal to >= — Greater than

## Loop

for,while, do ... while

for ... of (element of array)

for ... in (key in object)

## JSON 

JSON.stringify(object)

JSON.parse(JSONstring)

## Event

eventTarget.addEventListener(type,listener,[,useCapture]);

### **Event capturing**

When you use event capturing

```js
               | |
---------------| |-----------------
| element1     | |                |
|   -----------| |-----------     |
|   |element2  \ /          |     |
|   -------------------------     |
|        Event CAPTURING          |
-----------------------------------
```

the event handler of element1 fires first, the event handler of element2 fires last.

### Event bubbling

When you use event bubbling

```js
               / \
---------------| |-----------------
| element1     | |                |
|   -----------| |-----------     |
|   |element2  | |          |     |
|   -------------------------     |
|        Event BUBBLING           |
-----------------------------------
```

the event handler of element2 fires first, the event handler of element1 fires last.

### Event delegation

Event delegation is a technique involving adding event listeners to a parent element instead of adding them to the descendant elements. The listener will fire whenever the event is triggered on the descendant elements due to event bubbling up the DOM. The benefits of this technique are:

- Memory footprint goes down because only one single handler is needed on the parent element, rather than having to attach event handlers on each descendant.
- There is no need to unbind the handler from elements that are removed and to bind the event for new elements.

```html
<ul id="parent-list">
<li id="post-1">Item 1</li>
<li id="post-2">Item 2</li>
<li id="post-3">Item 3</li>
<li id="post-4">Item 4</li>
<li id="post-5">Item 5</li>
<li id="post-6">Item 6</li>
```

```js
document.getElementById("parent-list").addEventListener("click", function(e) {
// e.target is the clicked element!
// If it was a list item
if(e.target && e.target.nodeName == "LI") {
    // List item found!  Output the ID!
    console.log("List item ", e.target.id.replace("post-"), " was clicked!");
       }
 });
```

## Best practice

## Obj equality

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

### **Remove duplicates of an array and return an array of only unique elements**

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

### **Reverse string** 

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
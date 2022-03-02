# Behind the scenes of class and object in JavaScript

<https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Details_of_the_Object_Model>

## Constructor function vs factory function

<https://medium.com/@chamikakasun/javascript-factory-functions-vs-constructor-functions-585919818afe>

## Constructor function vs Classes

![image-20200602210506813](assets/JS__advance/image-20200602210506813.png)

![image-20200602210528810](assets/JS__advance/image-20200602210528810.png)

![image-20200602210252909](assets/JS__advance/image-20200602210252909.png)

![image-20200612212210695](assets/JS__advance/image-20200612212210695.png)

![image-20200612212336865](assets/JS__advance/image-20200612212336865.png)

## new

 The `new` keyword allows us to use a function as a constructor. In addition it clones the `prototype` of the constructor and binds it to the `this` pointer of the constructor, returning `this` if no other object is returned

![image-20200211182818925](assets/JS__advance/image-20200211182818925.png)

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

![image-20200612212123663](assets/JS__advance/image-20200612212123663.png)

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

![image-20200605212352374](assets/JS__advance/image-20200605212352374.png)

## Class use  prototype

<https://www.codecademy.com/forum_questions/50d3836d5a8a1f5d5c00085c>

class is a sugar syntax for Constructor function

### super keyword

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

![image-20200603231505454](assets/JS__advance/image-20200603231505454.png)

## Prototype

<https://stackoverflow.com/questions/9959727/proto-vs-prototype-in-javascript>

<https://stackoverflow.com/questions/572897/how-does-javascript-prototype-work>

<https://www.javatpoint.com/javascript-oops-prototype-object>

<https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Details_of_the_Object_Model#Object_properties>

<https://anonystick.com/blog-developer/prototype-la-gi-lap-trinh-huong-doi-tuong-oop-trong-javascript-2020070686855915>

[What Makes JavaScript JavaScript? Prototypal Inheritance (dmitripavlutin.com)](https://dmitripavlutin.com/javascript-prototypal-inheritance/)

A prototype is just a base blue print of object

![image-20200602222126942](assets/JS__advance/image-20200602222126942.png)

JavaScript is often described as a **prototype-based language** — to provide inheritance, objects can have a **`prototype` object**, which acts as a **template** **(fallback)** object that it inherits methods and properties from.

![image-20200602224007200](assets/JS__advance/image-20200602224007200.png)

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

![image-20200602231222250](assets/JS__advance/image-20200602231222250.png)

![image-20200602223953825](assets/JS__advance/image-20200602223953825.png)

This prototype (constructor `prototype` property) then automatically be assigned to the instance of the constructor upon creation (get the prototype object of an instance by using `__proto__ property` or  `Object.getPrototypeOf()`)

```js
var person = new Human("Virat", "Kohli");
Human.prototype === Object.getPrototypeOf(person) // true
Human.prototype === person.__proto__ //true
console.log(person)
```

![image-20200605213852666](assets/JS__advance/image-20200605213852666.png)

![image-20201208091643389](assets/JS__advance/image-20201208091643389.png)

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

![image-20200602230654002](assets/JS__advance/image-20200602230654002.png)

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

![image-20200612211958126](assets/JS__advance/image-20200612211958126.png)

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

<https://stackoverflow.com/questions/46129847/why-does-function-proto-return-something-different-than-other-prototypes/46130469>

<https://stackoverflow.com/questions/383172/correct-prototype-chain-for-function/383366>

<https://javascript.info/native-prototypes>

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

![image-20200605205426303](assets/JS__advance/image-20200605205426303.png)

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
// greet = function() {...} 
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

<https://medium.com/@viktor.kukurba/object-oriented-programming-in-javascript-1-abstraction-c47307c469d1>

<https://www.javatpoint.com/javascript-oops-abstraction>

### Inheritance

<https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain>

<https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Inheritance>

<https://medium.com/@viktor.kukurba/object-oriented-programming-in-javascript-2-inheritance-447368f57a26>

<https://stackoverflow.com/questions/7486825/javascript-inheritance>

<https://www.javatpoint.com/javascript-oops-inheritance>

### Polymorphism

<https://www.javatpoint.com/javascript-oops-polymorphism>

<https://medium.com/@viktor.kukurba/object-oriented-programming-in-javascript-3-polymorphism-fb564c9f1ce8>

<https://stackoverflow.com/questions/27642239/what-is-polymorphism-in-javascript>

### Encapsulation

<https://stackoverflow.com/questions/32495747/javascript-encapsulation-abstraction>

<https://medium.com/@viktor.kukurba/object-oriented-programming-in-javascript-4-encapsulation-4f9165cd26f9>

<https://stackoverflow.com/questions/19948698/encapsulating-in-javascript-does-it-exist>

<https://medium.com/javascript-scene/encapsulation-in-javascript-26be60e325b4>

<https://itnext.io/different-ways-to-achieve-encapsulation-in-javascript-es6-7cb938e83f2d>

<https://www.javatpoint.com/javascript-oops-encapsulation>

## Prototype-based

<https://www.techopedia.com/definition/30040/prototype-based-programming>

<https://stackoverflow.com/questions/186244/what-does-it-mean-that-javascript-is-a-prototype-based-language>

<https://en.wikipedia.org/wiki/Prototype-based_programming>

<https://medium.com/javascript-scene/master-the-javascript-interview-what-s-the-difference-between-class-prototypal-inheritance-e4cd0a7562e9>

Some current prototype-oriented languages are JavaScript

A prototype-based language, such as JavaScript, does not make this distinction: it simply has objects. A prototype-based language has the notion of a *prototypical object*, an object used as a template from which to get the initial properties for a new object. Any object can specify its own properties, either when you create it or at run time. In addition, any object can be associated as the *prototype* for another object, allowing the second object to share the first object's properties.

### Prototype defining a class

JavaScript follows a similar model, but does not have a class definition separate from the constructor. Instead, you define a constructor function to create objects with a particular initial set of properties and values. Any JavaScript function can be used as a constructor. You use the `new` operator with a constructor function to create a new object.

### Prototype subclasses and inheritance

JavaScript implements inheritance by allowing you to associate a prototypical object with any constructor function. So, you can create exactly the `Employee` — `Manager` example, but you use slightly different terminology.

- First you define the `Employee` constructor function, specifying the `name` and `dept` properties.
- Next, you define the `Manager` constructor function, calling the `Employee` constructor and specifying the `reports` property.
- Then, you assign a new object derived from `Employee.prototype` as the `prototype` for the `Manager` constructor function.
- Finally, when you create a new `Manager`, it inherits the `name` and `dept` properties from the `Employee` object.

### Prototype adding and removing properties

In JavaScript, however, at run time you can add or remove properties of any object. If you add a property to an object that is used as the prototype for a set of objects, the objects for which it is the prototype also get the new property.

## Class-based

<https://en.wikipedia.org/wiki/Class-based_programming>

Class-based object-oriented languages, such as Java and C++, are founded on the concept of two distinct entities: classes and instances.

- A *class* defines all of the properties that characterize a certain set of objects (considering methods and fields in Java, or members in C++, to be properties). A class is abstract rather than any particular member in a set of objects it describes. For example, the `Employee` class could represent the set of all employees.
- An *instance*, on the other hand, is the instantiation of a class; that is. For example, `Victoria` could be an instance of the `Employee` class, representing a particular individual as an employee. An instance has exactly the same properties of its parent class (no more, no less).

### Defining a class

In class-based languages, you define a class in a separate *class definition*. In that definition you can specify special methods, called *constructors*, to create instances of the class. A constructor method can specify initial values for the instance's properties and perform other processing appropriate at creation time. You use the `new` operator in association with the constructor method to create class instances.

### Subclasses and inheritance

In a class-based language, you create a hierarchy of classes through the class definitions. In a class definition, you can specify that the new class is a *subclass* of an already existing class. The subclass inherits all the properties of the superclass and additionally can add new properties or modify the inherited ones. For example, assume the `Employee` class includes only the `name` and `dept` properties, and `Manager` is a subclass of `Employee` that adds the `reports` property. In this case, an instance of the `Manager` class would have all three properties: `name`, `dept`, and `reports`.

### Adding and removing properties

In class-based languages, you typically create a class at compile time and then you instantiate instances of the class either at compile time or at run time. You cannot change the number or the type of properties of a class after you define the class.

## Summary of differences

<https://stackoverflow.com/questions/816071/prototype-based-vs-class-based-inheritance>

<https://stackoverflow.com/questions/11620582/the-difference-between-a-class-based-language-like-java-or-python-and-a-protot>

The following table gives a short summary of some of these differences. The rest of this chapter describes the details of using JavaScript constructors and prototypes to create an object hierarchy and compares this to how you would do it in Java.

| Category                         | Class-based (Java)                                           | Prototype-based (JavaScript)                                 |
| :------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| Class vs. Instance               | Class and instance are distinct entities.                    | All objects can inherit from another object.                 |
| Definition                       | Define a class with a class definition; instantiate a class with constructor methods. | Define and create a set of objects with constructor functions. |
| Creation of new object           | Create a single object with the `new` operator.              | Same.                                                        |
| Construction of object hierarchy | Construct an object hierarchy by using class definitions to define subclasses of existing classes. | Construct an object hierarchy by assigning an object as the prototype associated with a constructor function. |
| Inheritance model                | Inherit properties by following the class chain.             | Inherit properties by following the prototype chain.         |
| Extension of properties          | Class definition specifies *all* properties of all instances of a class. Cannot add properties dynamically at run time. | Constructor function or prototype specifies an *initial set* of properties. Can add or remove properties dynamically to individual objects or to the entire set of objects. |

## The employee example

<https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Details_of_the_Object_Model#Creating_the_hierarchy>

![image-20201028163952141](assets/PrototypeBase-ClassBase/image-20201028163952141.png)

The following Java and JavaScript (base class) `Employee` definitions are similar.

```js
class Employee {
  constructor() {
    this.name = '';
    this.dept = 'general';
  }
}
// or
function Employee() {
    this.name = '';
    this.dept = 'general';
}
```

```java
public class Employee {
   public String name = "";
   public String dept = "general";
}
```

The `Manager` and `WorkerBee` definitions show the difference in how to specify the next object higher in the inheritance chain.

```js
// In JavaScript, you add a prototypical instance as the value of the prototype property of the constructor function, then override the prototype.constructor to the constructor function. You can do so at any time after you define the constructor.
function Manager() {
  Employee.call(this);
  this.reports = [];
}
Manager.prototype = Object.create(Employee.prototype);
Manager.prototype.constructor = Manager;

function WorkerBee() {
  Employee.call(this);
  this.projects = [];
}
WorkerBee.prototype = Object.create(Employee.prototype);
WorkerBee.prototype.constructor = WorkerBee;
// ES6 Class extends is sugar syntax for this
```

```java
// In Java, you specify the superclass within the class definition. You cannot change the superclass outside the class definition.
public class Manager extends Employee {
   public Employee[] reports = 
       new Employee[0];
}

public class WorkerBee extends Employee {
   public String[] projects = new String[0];
}
```

The `Engineer` and `SalesPerson` definitions create objects that descend from `WorkerBee` and hence from `Employee`. An object of these types has properties of all the objects above it in the chain. In addition, these definitions override the inherited value of the `dept` property with new values specific to these objects.

```js
function SalesPerson() {
   WorkerBee.call(this);
   this.dept = 'sales';
   this.quota = 100;
}
SalesPerson.prototype = Object.create(WorkerBee.prototype);
SalesPerson.prototype.constructor = SalesPerson;

function Engineer() {
   WorkerBee.call(this);
   this.dept = 'engineering';
   this.machine = '';
}
Engineer.prototype = Object.create(WorkerBee.prototype)
Engineer.prototype.constructor = Engineer;
```

```java
public class SalesPerson extends WorkerBee {
   public String dept = "sales";
   public double quota = 100.0;
}

public class Engineer extends WorkerBee {
   public String dept = "engineering";
   public String machine = "";
}
```

# Prototype-based

https://www.techopedia.com/definition/30040/prototype-based-programming

https://stackoverflow.com/questions/186244/what-does-it-mean-that-javascript-is-a-prototype-based-language

https://en.wikipedia.org/wiki/Prototype-based_programming

https://medium.com/javascript-scene/master-the-javascript-interview-what-s-the-difference-between-class-prototypal-inheritance-e4cd0a7562e9

Some current prototype-oriented languages are JavaScript

A prototype-based language, such as JavaScript, does not make this distinction: it simply has objects. A prototype-based language has the notion of a *prototypical object*, an object used as a template from which to get the initial properties for a new object. Any object can specify its own properties, either when you create it or at run time. In addition, any object can be associated as the *prototype* for another object, allowing the second object to share the first object's properties.

## Defining a class

JavaScript follows a similar model, but does not have a class definition separate from the constructor. Instead, you define a constructor function to create objects with a particular initial set of properties and values. Any JavaScript function can be used as a constructor. You use the `new` operator with a constructor function to create a new object.

## Subclasses and inheritance

JavaScript implements inheritance by allowing you to associate a prototypical object with any constructor function. So, you can create exactly the `Employee` — `Manager` example, but you use slightly different terminology.

- First you define the `Employee` constructor function, specifying the `name` and `dept` properties.
- Next, you define the `Manager` constructor function, calling the `Employee` constructor and specifying the `reports` property.
- Then, you assign a new object derived from `Employee.prototype` as the `prototype` for the `Manager` constructor function.
- Finally, when you create a new `Manager`, it inherits the `name` and `dept` properties from the `Employee` object.

## Adding and removing properties

In JavaScript, however, at run time you can add or remove properties of any object. If you add a property to an object that is used as the prototype for a set of objects, the objects for which it is the prototype also get the new property.

# Class-based

https://en.wikipedia.org/wiki/Class-based_programming

Class-based object-oriented languages, such as Java and C++, are founded on the concept of two distinct entities: classes and instances.

- A *class* defines all of the properties that characterize a certain set of objects (considering methods and fields in Java, or members in C++, to be properties). A class is abstract rather than any particular member in a set of objects it describes. For example, the `Employee` class could represent the set of all employees.
- An *instance*, on the other hand, is the instantiation of a class; that is. For example, `Victoria` could be an instance of the `Employee` class, representing a particular individual as an employee. An instance has exactly the same properties of its parent class (no more, no less).

## Defining a class

In class-based languages, you define a class in a separate *class definition*. In that definition you can specify special methods, called *constructors*, to create instances of the class. A constructor method can specify initial values for the instance's properties and perform other processing appropriate at creation time. You use the `new` operator in association with the constructor method to create class instances.

## Subclasses and inheritance

In a class-based language, you create a hierarchy of classes through the class definitions. In a class definition, you can specify that the new class is a *subclass* of an already existing class. The subclass inherits all the properties of the superclass and additionally can add new properties or modify the inherited ones. For example, assume the `Employee` class includes only the `name` and `dept` properties, and `Manager` is a subclass of `Employee` that adds the `reports` property. In this case, an instance of the `Manager` class would have all three properties: `name`, `dept`, and `reports`.

## Adding and removing properties

In class-based languages, you typically create a class at compile time and then you instantiate instances of the class either at compile time or at run time. You cannot change the number or the type of properties of a class after you define the class.

# Summary of differences

https://stackoverflow.com/questions/816071/prototype-based-vs-class-based-inheritance

https://stackoverflow.com/questions/11620582/the-difference-between-a-class-based-language-like-java-or-python-and-a-protot

The following table gives a short summary of some of these differences. The rest of this chapter describes the details of using JavaScript constructors and prototypes to create an object hierarchy and compares this to how you would do it in Java.

| Category                         | Class-based (Java)                                           | Prototype-based (JavaScript)                                 |
| :------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| Class vs. Instance               | Class and instance are distinct entities.                    | All objects can inherit from another object.                 |
| Definition                       | Define a class with a class definition; instantiate a class with constructor methods. | Define and create a set of objects with constructor functions. |
| Creation of new object           | Create a single object with the `new` operator.              | Same.                                                        |
| Construction of object hierarchy | Construct an object hierarchy by using class definitions to define subclasses of existing classes. | Construct an object hierarchy by assigning an object as the prototype associated with a constructor function. |
| Inheritance model                | Inherit properties by following the class chain.             | Inherit properties by following the prototype chain.         |
| Extension of properties          | Class definition specifies *all* properties of all instances of a class. Cannot add properties dynamically at run time. | Constructor function or prototype specifies an *initial set* of properties. Can add or remove properties dynamically to individual objects or to the entire set of objects. |

# The employee example

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Details_of_the_Object_Model#Creating_the_hierarchy

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
# OOP

## Principles of OOP

![img](https://images.viblo.asia/e8e24a71-54dd-4b1d-94c0-3862a46683b9.png)

### Abstraction

Applying abstraction means that each object should **only** expose a high-level mechanism for using it.

Abstract means a concept or an Idea which is not associated with any particular instance. Using abstract class/Interface we express the intent of the class rather than the actual implementation. In a way, one class should not know the inner details of another in order to use it, just knowing the interfaces should be good enough.

This mechanism should hide internal implementation details. It should only reveal operations relevant for the other objects.

Think — a coffee machine. It does a lot of stuff and makes quirky noises under the hood. But all you have to do is put in coffee and press a button.

Preferably, this mechanism should be easy to use and should rarely change over time. Think of it as a small set of public methods which any other class can call without “knowing” how they work.

### Encapsulation

Encapsulation is the mechanism of hiding of data implementation by restricting access to public things (data and methods) and don't allow to access to private things

Only inside of object's class can access to **private** things . And the object only expose public things for the outside.

### Inheritance

This reuse the common logic

It means that you create a (child) class by deriving from another (parent) class. This way, we form a hierarchy.

The child class reuses all fields and methods of the parent class (common part) and can implement its own (unique part).

<img src="https://cdn-media-1.freecodecamp.org/images/ZIm7lFjlrKeMWxcH8fqBapNkuSJIxW9-t9yf" alt="img" style="zoom:100%; background: white" />

### Polymorphism

 polymorphism gives a way to use a class exactly like its parent so there’s no confusion with mixing types. But each child class keeps its own implement methods as they are.

<img src="https://cdn-media-1.freecodecamp.org/images/8GySv1U8Kh9nVVyiTqv5cDuWZC7p0uARVeF0" alt="img" style="zoom:100%; background: white" />

## SOLID

### **Single responsibility principle**

The **single responsibility principle** (SRP) states that every class or module in a program should have **responsibility** for just a **single** piece of that program's functionality

### **Open/closed principle**

*open for extension, but closed for modification*

```c#
public abstract class Shape
{
    public abstract double Area();
}
```

Inheriting from Shape the Rectangle and Circle classes now looks like this:

```C#
public class Rectangle : Shape
{
    public double Width { get; set; }
    public double Height { get; set; }
    public override double Area()
    {
        return Width*Height;
    }
}

public class Circle : Shape
{
    public double Radius { get; set; }
    public override double Area()
    {
        return Radius*Radius*Math.PI;
    }
}
```

```c#
public double Area(Shape[] shapes)
{
    double area = 0;
    foreach (var shape in shapes)
    {
        area += shape.Area();
    }

    return area;
}
```

### **Liskov Substitution Principle**

objects of a superclass shall be replaceable with objects of its subclasses without breaking the application.

### **Interface Segregation Principle**

1 interface 1 purpose, not use big interface

### **Dependency inversion principle**

1. High-level modules should not depend on low-level modules. Both should depend on abstractions (e.g. interfaces).
2. Abstractions should not depend on details. Details (concrete implementations) should depend on abstractions.

## When abstract when interface

1. Class abstract for “IS – A” (Ôtô là Xe)
2. Interface for “like – A” (Ô tô có thể chuyển động).
3. class abstract for instruction sub class
4. Interface khi chúng ta cung cấp các hành vi bổ sung cho class cụ thể và những hành vì này không bắt buộc đối với clas đó.

## Inheritance

![image-20200612211739276](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612211739276.png)

![image-20200612211759007](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612211759007.png)

![image-20200612211808601](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200612211808601.png)

## Composition over inheritance

https://www.youtube.com/watch?v=wfMtDGfHWpA

https://www.youtube.com/watch?v=7HolHe7Gqbw

![image-20201021232918308](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20201021232918308.png)

=> God object

```js
Animal
	eat()
	sound()
FlyAnimal extend Aninal
	fly()
SwimAnimal extend Animal
	swim()
MoveAnimal extend Animal
	move()
MoveAndFlyAnimal extend ?
```

Inheritance makes us turn a blind eye to the inevitable fact that our class structure will most likely change in the future, and when it does, our tightly coupled inheritance structure is going to crumble.

![image-20201021233015497](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20201021233015497.png)

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

# Functional Programming

## Immutable 

An immutable object is an object whose state cannot be modified after it is created.

In JavaScript, some built-in types (numbers, strings) are immutable, but custom objects are generally mutable.

Some built-in immutable JavaScript objects are MATH,DATE

### **Pros**

- Easier change detection - Object equality can be determined in a performant and easy manner through referential equality. This is useful for comparing object differences in React and Redux.
- Programs with immutable objects are less complicated to think about, since you don't need to worry about how an object may evolve over time.
- Defensive copies are no longer necessary when immutable objects are returning from or passed to functions, since there is no possibility an immutable object will be modified by it.
- Easy sharing via references - One copy of an object is just as good as another, so you can cache objects or reuse the same object multiple times.
- Thread-safe - Immutable objects can be safely used between threads in a multi-threaded environment since there is no risk of them being modified in other concurrently running threads.
- Using libraries like ImmutableJS, objects are modified using structural sharing and less memory is needed for having multiple objects with similar structures.

### **Cons**

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

# Declarative Programming

What you want

Declarative programming is when you write your code in such a way that it describes what you want to do, and not how you want to do it. It is left up to the compiler to figure out the how.

```js
let array = [1,2,3,4]
array.map((v) => v*2)
```

# Imperative Programming

How you do

In this imperative program, I have told you the exact steps to take in order to do the task. These instructions aren’t the most detailed in the world, but I have told you all the steps you need to take in order to arrive at a finished product.

```js
let array = [1,2,3,4]
for(let i=0;i<length;i++) {
    array[i] = array[i] * 2
}
```


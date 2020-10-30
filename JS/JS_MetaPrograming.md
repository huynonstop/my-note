## Meta-Programming

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Meta_programming

https://stackoverflow.com/questions/514644/what-exactly-is-metaprogramming

https://medium.com/jspoint/a-brief-introduction-to-metaprogramming-in-javascript-88d13ed407b5

Configuring our code to behave in a certain way when other people use it

![image-20200611211554298](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611211554298.png)

- [More about Symbols (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)
- [List of Well-Known Symbols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol#Well-known_symbols)
- [More about Iterators & Generators (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_Generators)
- [More about the Reflect API (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect)
- [More about the Proxy API (MDN)]( https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)
- [List of all Proxy API Traps]( https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy#A_complete_traps_list_example)

### Symbol

https://medium.com/intrinsic/javascript-symbols-but-why-6b02768f4a5c

Unique identifiers for your objects

![image-20200611181703613](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611181703613.png)

![image-20200611181716785](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611181716785.png)

```js
Symbol("abc") === Symbol("abc") //false
```

#### Well-know symbols

![image-20200611182645270](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611182645270.png)

### Iterator

Iterator is an object with next() method

```js
const company = {
    curEmployee: 0,
    employees: ["Max","Manu","Anna"],
    next() { // Iterator is an object with next() method
        if(this.curEmployee >= this.employees.length) {
            return {value: this.curEmployee, done: true}
        }
       const returnValue = {
           value: this.employees[this.curEmployee],
           done: false
       }
       this.curEmployee++
       return returnValue
    }
}
company.next()
company.next()
company.next()
company.next()
company.next()
```

![image-20200611192846023](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611192846023.png)

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611193123041.png" alt="image-20200611193123041" style="zoom:150%;" /><img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611193146309.png" alt="image-20200611193146309" style="zoom:150%;" />

### Generator

A special function create an iterator object (object with a next() method)

```js
const employees = ["Max","Manu","Anna"]
function* employeeGenerator() {
    let current = 0
    while(current < employees.length) {
        yield employees[current++] //Javascript save the current state of execution this iterator and will continue from that point when call the next() method
    }
}
const it = employeeGenerator()
```

![image-20200611195527229](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611195527229.png)

```js
const company = {
    employees: ["Max","Manu","Anna"],
    //You can tap in native JS features to implement your own loop logic
    [Symbol.iterator]: function* employeeGenerator() {
        let current = 0
        while(current < this.employees.length) {
            yield this.employees[current++] 
        }
	}
}
for(let i of company) {
    console.log(i)
}
//Max
//Manu
//Anna
console.log([...company])
//Max
//Manu
//Anna
```

### Reflect API

Provide extra thing to work with (change) object on a meta level

```js
const course = {
    title: 'Javascript - The Complete Guide'
}
Reflect.setPrototypeOf(course, {
    toString() {
        return this.title
    }
})
console.log(course.toString()) // 'Javascript - The Complete Guide'
```

#### When error behavior

Object: fail silently or return undefined

Reflect: better error, return true or false, ...

#### Reflect group all method you need to work with object

Refelect: have deleProperty method and much more

### Proxy API

Help you 'trap' a certain operations of someone  and execute your own code 

Help you control how your code is used

```js
const course = {
    title: 'Javascript - The Complete Guide'
}
const courseHandler = {
    get(object, propertyName) {
        console.log(propertyName)
        return object[propertyName]
    }
}
const pCourse = new Proxy(course,courseHandler)
console.log(pCourse.title)
```

![image-20200611211112467](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611211112467.png)
# Paradigm

## Declarative Programming

What you want

Declarative programming is when you write your code in such a way that it describes what you want to do, and not how you want to do it. It is left up to the compiler to figure out the how.

```js
let array = [1,2,3,4]
array.map((v) => v*2)
```

## Imperative Programming

How you do

In this imperative program, I have told you the exact steps to take in order to do the task. These instructions aren’t the most detailed in the world, but I have told you all the steps you need to take in order to arrive at a finished product.

```js
let array = [1,2,3,4]
for(let i=0;i<length;i++) {
    array[i] = array[i] * 2
}
```

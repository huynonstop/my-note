# 	Native objects 

​	Objects that are part of the JavaScript language defined by the ECMAScript specification, such as `String`, `Math`, `RegExp`, `Object`, `Function`, etc.

## String

![image-20200609180009212](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609180009212.png)

### Template string literal

```javascript
`${name}`
```

### Tag template string

```js
function productDescription(strings, productName, productPrice) {
  console.log(strings);
  console.log(productName);
  console.log(productPrice);
  let priceCategory = 'pretty cheap regarding its price';
  if (productPrice > 20) {
    priceCategory = 'fairly priced';
  }
  return `${strings[0]}${productName}${strings[1]}${priceCategory}${
    strings[2]
  }`;
  // can return whatever
  //return {name: productName, price: productPrice}; 
}

const prodName = 'JavaScript Course';
const prodPrice = 29.99;

const productOutput = productDescription`This product (${prodName}) is ${prodPrice}.`;
console.log(productOutput);

```

## RegEx

[More](https://www.youtube.com/watch?v=0LKdKixl5Ug&list=PL55RiY5tL51ryV3MhCbH8bLl7O_RZGUUE)

![image-20200609190413777](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609190413777.png)

![image-20200609190427518](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609190427518.png)

![image-20200609191042611](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609191042611.png)

![image-20200609191057759](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609191057759.png)

![image-20200609191519659](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609191519659.png)

![image-20200609191451011](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609191451011.png)

![image-20200609191548441](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609191548441.png)

## Math

![image-20200609175715677](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609175715677.png)

## Number

![image-20200609175107482](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609175107482.png)

## BigInt (no mixing)

`BigInt()`

![image-20200609174601261](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200609174601261.png)

### 	
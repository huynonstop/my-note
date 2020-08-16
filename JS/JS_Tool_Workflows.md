# Tooling & Workflows

![image-20200611140405097](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611140405097.png)

![image-20200611140427459](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611140427459.png)

![image-20200611140629104](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611140629104.png)

## ESLint

To fully understand all options you can configure in `.eslintrc.json`, 

https://eslint.org/docs/user-guide/configuring

https://eslint.org/docs/rules/

Want to use a preset? Here you go: https://www.npmjs.com/search?q=eslint-config (just click on one of the results and follow the instructions provided there)

https://eslint.org/docs/user-guide/getting-started

## Webpack

#### Bundle and optimize code

### `webpack.config.js`

![image-20200611143200356](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611143200356.png)

![image-20200611142536194](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611142536194.png)

#### Webpack dev server (hot reload)

![image-20200611143529885](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611143529885.png)

![image-20200611143559885](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611143559885.png)

### Webpack dev tool

![image-20200611143901367](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611143901367.png)

### Production

`webpack.config.prod.js`

![image-20200611144206834](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611144206834.png)

![image-20200611144431991](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611144431991.png)

#### Clean webpack plugin

clean-webpack-plugin

![image-20200611144817778](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611144817778.png)

![image-20200611144835668](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611144835668.png)

#### New name for file (no old cache on browser)

![image-20200611145119738](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611145119738.png)

change import script in the html file

# Browser support

![image-20200611164923403](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611164923403.png)

![image-20200611164943405](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611164943405.png)

![image-20200611165220488](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611165220488.png)

![image-20200611165334323](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611165334323.png)

![image-20200611165410604](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611165410604.png)

![image-20200611165617262](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611165617262.png)

## Polyfill

![image-20200611165738554](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611165738554.png)

## Transpilation

![image-20200611165900311](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611165900311.png)

### babel loader + webpack + polyfill

![image-20200611170031012](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611170031012.png)

![image-20200611170138191](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611170138191.png)

![image-20200611170534203](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611170534203.png)

https://babeljs.io/docs/en/

https://babeljs.io/docs/en/babel-preset-env

https://github.com/babel/babel-loader

https://github.com/browserslist/browserslist#full-list

install `core-js` or preset like that

![image-20200611171758128](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611171758128.png)

## Disable JS

![image-20200611172441016](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611172441016.png)

# non-Browser support

![image-20200611172211838](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611172211838.png)
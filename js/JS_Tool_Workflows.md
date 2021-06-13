# Tooling & Workflows

![image-20200611140405097](assets/JS_Tool_Workflows/image-20200611140405097.png)

![image-20200611140427459](assets/JS_Tool_Workflows/image-20200611140427459.png)

![image-20200611140629104](assets/JS_Tool_Workflows/image-20200611140629104.png)

## ESLint

To fully understand all options you can configure in `.eslintrc.json`, 

https://eslint.org/docs/user-guide/configuring

https://eslint.org/docs/rules/

Want to use a preset? Here you go: https://www.npmjs.com/search?q=eslint-config (just click on one of the results and follow the instructions provided there)

https://eslint.org/docs/user-guide/getting-started

## Module bundlers

### Webpack

https://webpack.js.org/concepts/

https://academind.com/learn/webpack/webpack-2-the-basics/

Webpack is a static module bundler for JavaScript applications — it takes all the code from your application and makes it usable in a web browser.

When Webpack processes your application, it builds a dependency graph which maps out the modules that your project needs and generates one or more **bundles**. A bundle is a distinct grouping of connected code that has been compiled and transformed for the browser.

If one file depends on another (it uses the code from a separate file), Webpack treats this as a dependency. Webpack also takes your non-code assets (images, fonts, styles, etc.) and converts them to dependencies for your application.

![image-20201024123431671](assets/JS_Tool_Workflows/image-20201024123431671.png)

![image-20201024122747596](assets/JS_Tool_Workflows/image-20201024122747596.png)

Loaders: compile files not JS

webpack.config.js: entry, output, module (rules => match files to loaders, plugins => a tap into the bundler life cycle, Dev Server =>  watch and serve files (auto recompile want have change))  



> (legacy)
>
> Bundle and optimize code
>
> `webpack.config.js`
>
> ![image-20200611143200356](assets/JS_Tool_Workflows/image-20200611143200356.png)
>
> ![image-20200611142536194](assets/JS_Tool_Workflows/image-20200611142536194.png)
>
> Webpack dev server (hot reload)
>
> ![image-20200611143529885](assets/JS_Tool_Workflows/image-20200611143529885.png)
>
> ![image-20200611143559885](assets/JS_Tool_Workflows/image-20200611143559885.png)
>
> Webpack dev tool
>
> ![image-20200611143901367](assets/JS_Tool_Workflows/image-20200611143901367.png)
>
> Production
>
> `webpack.config.prod.js`
>
> ![image-20200611144206834](assets/JS_Tool_Workflows/image-20200611144206834.png)
>
> ![image-20200611144431991](assets/JS_Tool_Workflows/image-20200611144431991.png)
>
> Clean webpack plugin
>
> clean-webpack-plugin
>
> ![image-20200611144817778](assets/JS_Tool_Workflows/image-20200611144817778.png)
>
> ![image-20200611144835668](assets/JS_Tool_Workflows/image-20200611144835668.png)
>
> New name for file (no old cache on browser)
>
> ![image-20200611145119738](assets/JS_Tool_Workflows/image-20200611145119738.png)
>
> change import script in the html file
>

### Snowpack

https://www.snowpack.dev/

# Browser support

![image-20200611164923403](assets/JS_Tool_Workflows/image-20200611164923403.png)

![image-20200611164943405](assets/JS_Tool_Workflows/image-20200611164943405.png)

![image-20200611165220488](assets/JS_Tool_Workflows/image-20200611165220488.png)

![image-20200611165334323](assets/JS_Tool_Workflows/image-20200611165334323.png)

![image-20200611165410604](assets/JS_Tool_Workflows/image-20200611165410604.png)

![image-20200611165617262](assets/JS_Tool_Workflows/image-20200611165617262.png)

## Polyfill

![image-20200611165738554](assets/JS_Tool_Workflows/image-20200611165738554.png)

## Transpilation

![image-20200611165900311](assets/JS_Tool_Workflows/image-20200611165900311.png)

### babel loader + webpack + polyfill

![image-20200611170031012](assets/JS_Tool_Workflows/image-20200611170031012.png)

![image-20200611170138191](assets/JS_Tool_Workflows/image-20200611170138191.png)

![image-20200611170534203](assets/JS_Tool_Workflows/image-20200611170534203.png)

https://babeljs.io/docs/en/

https://babeljs.io/docs/en/babel-preset-env

https://github.com/babel/babel-loader

https://github.com/browserslist/browserslist#full-list

install `core-js` or preset like that

![image-20200611171758128](assets/JS_Tool_Workflows/image-20200611171758128.png)

# Disable JS

![image-20200611172441016](assets/JS_Tool_Workflows/image-20200611172441016.png)

# non-Browser support

![image-20200611172211838](assets/JS_Tool_Workflows/image-20200611172211838.png)
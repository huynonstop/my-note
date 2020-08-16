https://www.vuemastery.com/

https://vuejs.org/v2/guide/

https://vuejs.org/v2/api/

# Reactivity in Depth

https://vuejs.org/v2/guide/reactivity.html

![Reactivity Cycle](https://vuejs.org/images/data.png)

# How Vue updating the DOM

![image-20200628011238581](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200628011238581.png)

# Vue Syntax

https://vuejs.org/v2/guide/syntax.html

# Vue template

## Interpolations

### Text

The most basic form of data binding is text interpolation using the “**Mustache**” syntax (**double curly braces**):

```html
<span>Message: {{ msg }}</span>
```

```html
<span v-once>This will never change: {{ msg }}</span>
```

### Raw HTML

The double mustaches interprets the data as plain text, not HTML. In order to output real HTML, you will need to use the [`v-html` directive](https://vuejs.org/v2/api/#v-html):

```
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

Using mustaches: `<span style="color: red">This should be red.</span>`

Using v-html directive: This should be red.

> Dynamically rendering arbitrary HTML on your website can be very dangerous because it can easily lead to [XSS vulnerabilities](https://en.wikipedia.org/wiki/Cross-site_scripting)

### Attributes

Mustaches cannot be used inside HTML attributes. Instead, use a [`v-bind` directive](https://vuejs.org/v2/api/#v-bind):

```html
<div v-bind:id="dynamicId"></div>
```

In the case of boolean attributes, where their mere existence implies `true`, `v-bind` works a little differently. In this example:

```html
<button v-bind:disabled="isButtonDisabled">Button</button>
```

If `isButtonDisabled` has the value of `null`, `undefined`, or `false`, the `disabled` attribute will not even be included in the rendered `<button>` element.

### JS expressions

```html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

```html
<!-- this is a statement, not an expression: -->
{{ var a = 1 }}

<!-- flow control won't work either, use ternary expressions -->
{{ if (ok) { return message } }}
```

## Directives

https://vuejs.org/v2/guide/syntax.html#Directives

https://vuejs.org/v2/guide/custom-directive.html

Directives are special attributes with the `v-` prefix. Directive attribute values are expected to be **a single JavaScript expression** (with the exception of `v-for`)

`v-bind` directive is used to reactively update an HTML attribute:

```html
<a v-bind:href="url"> ... </a>
```

Here `href` is the **argument**, which tells the `v-bind` directive to bind the element’s `href` attribute to the value of the expression `url`.

Another example is the `v-on` directive, which listens to DOM events:

```html
<a v-on:click="doSomething"> ... </a>
```

Here the argument is the event name to listen to.

Dynamically

```html
<a v-bind:[attributeName]="url"> ... </a>
```

# Vue instance

https://vuejs.org/v2/guide/instance.html

middle man between DOM and bussiness logic

```js
const vm = new Vue({
	el: "#app", // => refer to the template, html of this instance
	data,
	methods,
	computed,
    watch
})
console.log(vm) // === this in instance methods
console.log(vm.$el) // === div.#app
console.log(vm.$refs) 
/*
	{
		refInTemplate: htmlElemnt
	}
*/
```

```js
const vm = new Vue({
	//el: "#app" remove this
	data: somedata,
	methods,
	computed,
    watch
})
vm.$mount("#app")
```

```js
//html
<div id="app"/>
//js
const vm = new Vue({
	template: "<h1>Hello</h1>" //harder for multi-line, no IDE support
	data: somedata,
	methods,
	computed,
    watch
})
vm.$mount("#app")
//same energy 
document.getElementById('app3').appendChild(vm3.$el)
```

can have mutli instance in the DOM

Vue instance get all properties of object **passed** to the **Vue constructor**  with a watcher (observer) layer warping around. So it can recongizes that something changed and do Vue job

## Vue instance data

```js
const vm = new Vue({
	el
	data: somedata,
	methods,
	computed,
    watch
})
console.log(vm.$data === somedata) //true
```

## Vue instance methods

## Life cycle

<img src="https://vuejs.org/images/lifecycle.png" alt="The Vue Instance Lifecycle" style="zoom:50%;" />





# Vue computed properties

pure function

# Vue watcher

can run asynchronous

# Conditional Rendering

## v-if, v-else, v-else-if

warp `<template>`

```html
<tempalte v-if="show">
	<h1>
        haha
    </h1>
    <p>
        hehe
    </p>
</tempalte>
```

## v-show

# List rendering

## array

## object

List rendering with Components

```html
<ul>
    <li v-for="(item,index) in itemlist" :key="item">
        <div v-for="(value,key,i) in item" :key="i">
            {{key}} : {{value}} ({{i}})
        </div>
        {{item}} {{index}}
    </li>
    <div v-for="n in 10" :key="n">
        {{ 10 }}
    </div>
</ul>


<ul>
    <template v-for="(item,index) in itemlist" :key="item.id">
		<h1>
            {{item}}
        </h1>
		<p>
           {{index}}
        </p>
    </template>
</ul>
```

# Dynamic Clas and Styling

https://vuejs.org/v2/guide/class-and-style.html

```vue
<template>
	<div class="demo" :class="{red: !attatchRed}" @click="attachRed = !attachRed"/>
	<div class="demo" :class="divClass"/>
	<div class="demo" :class="inputColor"/>
	<div class="demo" :class="[inputColor,{red: !attatchRed}]"/>
	<input type="text" v-model="inputColor">
</template>
<script>
    export default  {
        data: {
            attatchRed: false,
            inputColor: ""
        },
        computed: {
            divClass: function() {
                return {
                    red: this.attatchRed,
                    blue: !this.attatchRed
                }
            }
        }
    }
</script>
```

```vue
<template>
	<div class="demo" :style="{backgroundColor: color}"/>
	<div class="demo" :style="divStyle"/>
	<div class="demo" :style="[divStyle,{height: widthThat + 'px'}]"/>
</template>
<script>
    export default  {
        data: {
            color: "blue",
            widthThat: 200
        },
        computed: {
            divStyle: function() {
                return {
                    backgroundColor: this.color,
                    width: "100px"
                }
            }
        }
    }
</script>
```

# Event handling

## Listening to Events

We can use the `v-on`

## Event object

## Event modifider

https://vuejs.org/v2/guide/events.html#Event-Modifiers

```html
<form v-on:submit.prevent="onSubmit"> ... </form>
```

## [Key Modifiers](https://vuejs.org/v2/guide/events.html#Key-Modifiers)

When listening for keyboard events, we often need to check for specific keys. Vue allows adding key modifiers for `v-on` when listening for key events:

```html
<!-- only call `vm.submit()` when the `key` is `Enter` -->
<input v-on:keyup.enter="submit">
```

## [Why Listeners in HTML?](https://vuejs.org/v2/guide/events.html#Why-Listeners-in-HTML)

You might be concerned that this whole event listening approach violates the good old rules about “separation of concerns”. Rest assured - since all Vue handler functions and expressions are strictly bound to the ViewModel that’s handling the current view, it won’t cause any maintenance difficulty. In fact, there are several benefits in using `v-on`:

1. It’s easier to locate the handler function implementations within your JS code by skimming the HTML template.
2. Since you don’t have to manually attach event listeners in JS, your ViewModel code can be pure logic and DOM-free. This makes it easier to test.
3. When a ViewModel is destroyed, all event listeners are automatically removed. You don’t need to worry about cleaning it up yourself..

# Form Input Bindings

https://vuejs.org/v2/guide/forms.html

# Vue components

https://vuejs.org/v2/guide/components.html

```js
Vue.component('hello', {
    template: "<h1>hello</h1>"
})
//html
<hello/>
<Hello/>
```

https://vuejs.org/v2/guide/components-edge-cases.html

## `data` Must Be a Function

## Props

https://vuejs.org/v2/guide/components.html#Passing-Data-to-Child-Components-with-Props

https://vuejs.org/v2/guide/components-props.html

```html
<blog-post title="My journey with Vue"></blog-post>
<blog-post title="Blogging with Vue"></blog-post>
<blog-post title="Why Vue is so fun"></blog-post>
```

```js
Vue.component('blog-post', {
  props: ['title'],
  template: '<h3>{{ title }}</h3>'
})
```

## A Single Root Element

## Listening to Child Components Events

https://vuejs.org/v2/guide/components.html#Listening-to-Child-Components-Events

https://vuejs.org/v2/guide/components-custom-events.html

## Slots

https://vuejs.org/v2/guide/components.html#Content-Distribution-with-Slots

https://vuejs.org/v2/guide/components-slots.html

## Dynamically

https://vuejs.org/v2/guide/components.html#Dynamic-Components

https://vuejs.org/v2/guide/components-dynamic-async.html

```html
<component v-bind:is="currentTabComponent"></component>
```

## DOM Template Parsing Caveats

```html
<table>   
    <tr is="blog-post-row"></tr> 
</table>
```

It should be noted that **this limitation does \*not\* apply if you are using string templates from one of the following sources**:

- String templates (e.g. `template: '...'`)
- Single-file (`.vue`) components
- `<script type="text/x-template">`

## Component Registration

https://vuejs.org/v2/guide/components-registration.html

### [Global Registration](https://vuejs.org/v2/guide/components-registration.html#Global-Registration)

```js
Vue.component('my-component-name', {
  // ... options ...
})
```

### [Local Registration](https://vuejs.org/v2/guide/components-registration.html#Local-Registration)

```js
new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```

# Single File Components

https://vuejs.org/v2/guide/single-file-components.html

# Transitions & Animation

https://vuejs.org/v2/guide/transitions.html

https://vuejs.org/v2/guide/transitioning-state.html

Vue provides a `transition` wrapper component, allowing you to add entering/leaving transitions for any element or component in the following contexts:

- Conditional rendering (using `v-if`)
- Conditional display (using `v-show`)
- Dynamic components
- Component root nodes

# Mixins

https://vuejs.org/v2/guide/mixins.html

# Plugin

https://vuejs.org/v2/guide/plugins.html

# Filters

https://vuejs.org/v2/guide/filters.html

# Production Deployment

https://vuejs.org/v2/guide/deployment.html

# Routing

https://vuejs.org/v2/guide/routing.html

https://router.vuejs.org/

## [Simple Routing From Scratch](https://vuejs.org/v2/guide/routing.html#Simple-Routing-From-Scratch)

If you only need very simple routing and do not wish to involve a full-featured router library, you can do so by dynamically rendering a page-level component like this:

```js
const NotFound = { template: '<p>Page not found</p>' }
const Home = { template: '<p>home page</p>' }
const About = { template: '<p>about page</p>' }

const routes = {
  '/': Home,
  '/about': About
}

new Vue({
  el: '#app',
  data: {
    currentRoute: window.location.pathname
  },
  computed: {
    ViewComponent () {
      return routes[this.currentRoute] || NotFound
    }
  },
  render (h) { return h(this.ViewComponent) }
})
```

Combined with the HTML5 History API, you can build a very basic but fully-functional client-side router. To see that in practice, check out [this example app](https://github.com/chrisvfritz/vue-2.0-simple-routing-example).

# State Management

https://vuex.vuejs.org/

![img](https://raw.githubusercontent.com/vuejs/vuex/dev/docs/.vuepress/public/vuex.png)
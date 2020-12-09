# Mixins

Mixins distribute reusable functionalities for Vue components. A mixin object can contain any component options. When a component uses a mixin, all options in the mixin will be "mixed" into the component's own options.

```js
// define a mixin object
const myMixin = {
  created() {
    this.hello()
  },
  methods: {
    hello() {
      console.log('hello from mixin!')
    }
  }
}

// define an app that uses this mixin
const app = Vue.createApp({
  mixins: [myMixin]
})

app.mount('#mixins-basic') // => "hello from mixin!"
```

## Option Merging

When a mixin and the component itself contain overlapping options, they will be "merged" using appropriate strategies.

```js
// Data objects undergo a recursive merge, with the component's data taking priority in cases of conflicts
const myMixin = {
  data() {
    return {
      message: 'hello',
      foo: 'abc'
    }
  }
}

const app = Vue.createApp({
  mixins: [myMixin],
  data() {
    return {
      message: 'goodbye',
      bar: 'def'
    }
  },
  created() {
    console.log(this.$data) // => { message: "goodbye", foo: "abc", bar: "def" }
  }
})
```

```js
// Hook functions with the same name are merged into an array so that all of them will be called. Mixin hooks will be called before the component's own hooks.
const myMixin = {
  created() {
    console.log('mixin hook called')
  }
}

const app = Vue.createApp({
  mixins: [myMixin],
  created() {
    console.log('component hook called')
  }
})

// => "mixin hook called"
// => "component hook called"
```

```js
// Options that expect object values, for example methods, components and directives, will be merged into the same object. The component's options will take priority when there are conflicting keys in these objects
const myMixin = {
  methods: {
    foo() {
      console.log('foo')
    },
    conflicting() {
      console.log('from mixin')
    }
  }
}

const app = Vue.createApp({
  mixins: [myMixin],
  methods: {
    bar() {
      console.log('bar')
    },
    conflicting() {
      console.log('from self')
    }
  }
})

const vm = app.mount('#mixins-basic')

vm.foo() // => "foo"
vm.bar() // => "bar"
vm.conflicting() // => "from self"
```

## Global Mixin

You can also apply a mixin globally for a Vue application:

```js
const app = Vue.createApp({
  myOption: 'hello!'
})

// inject a handler for `myOption` custom option
app.mixin({
  created() {
    const myOption = this.$options.myOption
    if (myOption) {
      console.log(myOption)
    }
  }
})

app.mount('#mixins-global') // => "hello!"
// Use with caution! Once you apply a mixin globally, it will affect every component instance created afterwards in the given app (for example, child components):

// add myOption also to child component
app.component('test-component', {
  myOption: 'hello from component!'
})

app.mount('#mixins-global')
// => "hello!"
// => "hello from component!"
```

## Custom Option Merge Strategies

When custom options are merged, they use the default strategy which overwrites the existing value. If you want a custom option to be merged using custom logic, you need to attach a function to `app.config.optionMergeStrategies`:

```js
const app = Vue.createApp({})

app.config.optionMergeStrategies.customOption = (toVal, fromVal) => {
  // return mergedVal
}
```

The merge strategy receives the value of that option defined on the parent and child instances as the first and second arguments, respectively. Let's try to check what do we have in these parameters when we use a mixin:

```js
const app = Vue.createApp({
  custom: 'hello!'
})

app.config.optionMergeStrategies.custom = (toVal, fromVal) => {
  console.log(fromVal, toVal)
  // => "goodbye!", undefined
  // => "hello", "goodbye!"
  return fromVal || toVal
}

app.mixin({
  custom: 'goodbye!',
  created() {
    console.log(this.$options.custom) // => "hello!"
  }
})
// As you can see, in the console we have toVal and fromVal printed first from the mixin and then from the app. We always return fromVal if it exists, that's why this.$options.custom is set to hello! in the end. Let's try to change a strategy to always return a value from the child instance:
const app = Vue.createApp({
  custom: 'hello!'
})

app.config.optionMergeStrategies.custom = (toVal, fromVal) => toVal || fromVal

app.mixin({
  custom: 'goodbye!',
  created() {
    console.log(this.$options.custom) // => "goodbye!"
  }
})
```

##  Precautions

In Vue 2, mixins were the primary tool to abstract parts of component logic into reusable chunks. However, they have a few issues:

- Mixins are conflict-prone: Since properties from each feature are merged into the same component, you still have to know about every other feature to avoid property name conflicts and for debugging.
- Reusability is limited: we cannot pass any parameters to the mixin to change its logic which reduces their flexibility in terms of abstracting logic

=> Composition API

# Custom Directives

[Application API | Vue.js](https://v3.vuejs.org/api/application-api.html#directive)

In addition to the default set of directives shipped in core (like `v-model` or `v-show`), Vue also allows you to register your own custom directives. 

Note that in Vue, the primary form of code reuse and abstraction is components - however, there may be cases where you need some low-level DOM access on plain elements, and this is where custom directives would still be useful.

Now let's build the directive that accomplishes When the page loads, that element gains focus:

```js
const app = Vue.createApp({})
// Register a global custom directive called `v-focus`
app.directive('focus', {
  // When the bound element is mounted into the DOM...
  mounted(el) {
    // Focus the element
    el.focus()
  }
})
```

If you want to register a directive locally instead, components also accept a `directives` option:

```js
directives: {
  focus: {
    // directive definition
    mounted(el) {
      el.focus()
    }
  }
}
```

Then in a template, you can use the new `v-focus` attribute on any element, like this:

```html
<input v-focus />
```

## Hook Functions

A directive definition object can provide several hook functions (all optional):

- `beforeMount`: called when the directive is first bound to the element and before parent component is mounted. This is where you can do one-time setup work.
- `mounted`: called when the bound element's parent component is mounted.
- `beforeUpdate`: called before the containing component's VNode is updated

- `updated`: called after the containing component's VNode **and the VNodes of its children** have updated.
- `beforeUnmount`: called before the bound element's parent component is unmounted
- `unmounted`: called only once, when the directive is unbound from the element and the parent component is unmounted.

You can check the arguments passed into these hooks (i.e. `el`, `binding`, `vnode`, and `prevVnode`)

### Dynamic Directive Arguments

Directive arguments can be dynamic. For example, in `v-mydirective:[argument]="value"`, the `argument` can be updated based on data properties in our component instance! This makes our custom directives flexible for use throughout our application

```vue
<script>
	// Let's say you want to make a custom directive that allows you to pin elements to your page using fixed positioning. We could create a custom directive where the value updates the vertical positioning in pixels, like this
</script>

<div id="dynamic-arguments-example" class="demo">
  <p>Scroll down the page</p>
  <p v-pin="200">Stick me 200px from the top of the page</p>
</div>

<script>
const app = Vue.createApp({})

app.directive('pin', {
  mounted(el, binding) {
    el.style.position = 'fixed'
    // binding.value is the value we pass to directive - in this case, it's 200
    el.style.top = binding.value + 'px'
  }
})

app.mount('#dynamic-arguments-example')
</script>
```

This would pin the element 200px from the top of the page. But what happens if we run into a scenario when we need to pin the element from the left, instead of the top? Here's where a dynamic argument that can be updated per component instance comes in very handy:

```vue
<div id="dynamicexample">
  <h3>Scroll down inside this section ↓</h3>
  <p v-pin:[direction]="200">I am pinned onto the page at 200px to the left.</p>
</div>
```

```js
const app = Vue.createApp({
  data() {
    return {
      direction: 'right'
    }
  }
})

app.directive('pin', {
  mounted(el, binding) {
    el.style.position = 'fixed'
    // binding.arg is an argument we pass to directive
    const s = binding.arg || 'top'
    el.style[s] = binding.value + 'px'
  }
})

app.mount('#dynamic-arguments-example')
```

Our custom directive is now flexible enough to support a few different use cases. To make it even more dynamic, we can also allow to modify a bound value. Let's create an additional property `pinPadding` and bind it to the `<input type="range">`

```vue
<div id="dynamicexample">
  <h2>Scroll down the page</h2>
  <input type="range" min="0" max="500" v-model="pinPadding">
  <p v-pin:[direction]="pinPadding">Stick me {{ pinPadding + 'px' }} from the {{ direction }} of the page</p>
</div>
```

```js
const app = Vue.createApp({
  data() {
    return {
      direction: 'right',
      pinPadding: 200
    }
  }
})
```

Now let's extend our directive logic to recalculate the distance to pin on component update:

```js
app.directive('pin', {
  mounted(el, binding) {
    el.style.position = 'fixed'
    const s = binding.arg || 'top'
    el.style[s] = binding.value + 'px'
  },
  updated(el, binding) {
    const s = binding.arg || 'top'
    el.style[s] = binding.value + 'px'
  }
})
```

## Function Shorthand

In previous example, you may want the same behavior on `mounted` and `updated`, but don't care about the other hooks. You can do it by passing the callback to directive:

```js
app.directive('pin', (el, binding) => {
  el.style.position = 'fixed'
  const s = binding.arg || 'top'
  el.style[s] = binding.value + 'px'
})
```

## Object Literals

If your directive needs multiple values, you can also pass in a JavaScript object literal. Remember, directives can take any valid JavaScript expression.

```vue
<div v-demo="{ color: 'white', text: 'hello!' }"></div>
```

```js
app.directive('demo', (el, binding) => {
  console.log(binding.value.color) // => "white"
  console.log(binding.value.text) // => "hello!"
})
```

##  Usage on Components

When used on components, custom directive will always apply to component's root node, similarly to non-prop attributes.

```vue
<my-component v-demo="test"></my-component>
```

```js
app.component('my-component', {
  template: `
    <div> // v-demo directive will be applied here
      <span>My component content</span>
    </div>
  `
})
```

Unlike attributes, directives can't be passed to a different element with `v-bind="$attrs"`.

With [fragments](https://v3.vuejs.org/guide/migration/fragments.html#overview) support, components can potentially have more than one root nodes. When applied to a multi-root component, directive will be ignored and the warning will be thrown.

# Render Functions

# Teleport

https://v3.vuejs.org/guide/teleport.html

https://vueschool.io/lessons/vue-3-teleport?friend=vuejs

https://v3.vuejs.org/api/built-in-components.html#teleport

Vue encourages us to build our UIs by encapsulating UI and related behavior into components. We can nest them inside one another to build a tree that makes up an application UI.

However, sometimes a part of a component's template belongs to this component logically, while from a technical point of view, it would be preferable to move this part of the template somewhere else in the DOM, outside of the Vue app.

A common scenario for this is creating a component that includes a full-screen modal. In most cases, you'd want the modal's logic to live within the component, but the positioning of the modal quickly becomes difficult to solve through CSS, or requires a change in component composition.

```html
<body>
  <div style="position: relative;">
    <h3>Tooltips with Vue 3 Teleport</h3>
    <div>
      <modal-button></modal-button>
    </div>
  </div>
</body>
```

```js
const app = Vue.createApp({});
// The component will have a button element to trigger the opening of the modal, and a div element with a class of .modal, which will contain the modal's content and a button to self-close.
app.component('modal-button', {
  template: `
    <button @click="modalOpen = true">
        Open full screen modal!
    </button>

    <div v-if="modalOpen" class="modal">
      <div>
        I'm a modal! 
        <button @click="modalOpen = false">
          Close
        </button>
      </div>
    </div>
  `,
  data() {
    return { 
      modalOpen: false
    }
  }
})
// When using this component inside the initial HTML structure, we can see a problem - the modal is being rendered inside the deeply nested div and the position: absolute of the modal takes the parent relatively positioned div as reference.
```

Teleport provides a clean way to allow us to control under which parent in our DOM we want a piece of HTML to be rendered, without having to resort to global state or splitting this into two components.

```js
// Let's modify our modal-button to use <teleport> and tell Vue "teleport this HTML to the "body" tag".
app.component('modal-button', {
  template: `
    <button @click="modalOpen = true">
        Open full screen modal! (With teleport!)
    </button>

    <teleport to="body">
      <div v-if="modalOpen" class="modal">
        <div>
          I'm a teleported modal! 
          (My parent is "body")
          <button @click="modalOpen = false">
            Close
          </button>
        </div>
      </div>
    </teleport>
  `,
  data() {
    return { 
      modalOpen: false
    }
  }
})
// => once we click the button to open the modal, Vue will correctly render the modal's content as a child of the body tag.
```

## Using with Vue components

If `<teleport>` contains a Vue component, it will remain a logical child component of the `<teleport>`'s parent

```js
const app = Vue.createApp({
  template: `
    <h1>Root instance</h1>
    <parent-component />
  `
})

app.component('parent-component', {
  template: `
    <h2>This is a parent component</h2>
    <teleport to="#endofbody">
      <child-component name="John" />
    </teleport>
  `
})

app.component('child-component', {
  props: ['name'],
  template: `
    <div>Hello, {{ name }}</div>
  `
})
// => In this case, even when child-component is rendered in the different place, it will remain a child of parent-component and will receive a name prop from it.
// This also means that injections from a parent component work as expected, and that the child component will be nested below the parent component in the Vue Devtools, instead of being placed where the actual content moved to
```

## Using multiple teleports on the same target

A common use case scenario would be a reusable `<Modal>` component of which there might be multiple instances active at the same time. For this kind of scenario, multiple `<teleport>` components can mount their content to the same target element. The order will be a simple append - later mounts will be located after earlier ones within the target element.

```html
<teleport to="#modals">
  <div>A</div>
</teleport>
<teleport to="#modals">
  <div>B</div>
</teleport>

<!-- result-->
<div id="modals">
  <div>A</div>
  <div>B</div>
</div>
```

# Plugins

[Application API | Vue.js](https://v3.vuejs.org/api/application-api.html)

Plugins are self-contained code that usually add global-level functionality to Vue. It is either an `object` that exposes an `install()` method, or a `function`.

There is no strictly defined scope for a plugin, but common scenarios where plugins are useful include:

1. Add some global methods or properties, e.g. [vue-custom-element ](https://github.com/karol-f/vue-custom-element).
2. Add one or more global assets: directives/filters/transitions etc. (e.g. [vue-touch ](https://github.com/vuejs/vue-touch)).
3. Add some component options by global mixin (e.g. [vue-router](https://github.com/vuejs/vue-router)).
4. Add some global instance methods by attaching them to `config.globalProperties`.
5. A library that provides an API of its own, while at the same time injecting some combination of the above (e.g. [vue-router](https://github.com/vuejs/vue-router))

## Writing a Plugin

Whenever this plugin is added to an application, the `install` method will be called if it is an object. If it is a `function`, the function itself will be called. In both cases, it will receive two parameters - the `app` object resulting from Vue's `createApp`, and the options passed in by the user.

```js
// plugins/i18n.js
export default {
  install: (app, options) => {
    app.config.globalProperties.$translate = (key) => {
      return key.split('.')
        .reduce((o, i) => { if (o) return o[i] }, options)
    }

    app.provide('i18n', options)

    app.directive('my-directive', {
      mounted (el, binding, vnode, oldVnode) {
        // some logic ...
      }
      ...
    })

    app.mixin({
      created() {
        // some logic ...
      }
      ...
    })
  }
}
```

## Using a Plugin

After a Vue app has been initialized with `createApp()`, you can add a plugin to your application by calling the `use()` method.

The `use()` method takes two parameters. The first one is the plugin to be installed, in this case `i18nPlugin`.

It also automatically prevents you from using the same plugin more than once, so calling it multiple times on the same plugin will install the plugin only once.

The second parameter is optional, and depends on each particular plugin. In the case of the demo `i18nPlugin`, it is an object with the translated strings.

```js
import { createApp } from 'vue'
import Root from './App.vue'
import i18nPlugin from './plugins/i18n'

const app = createApp(Root)
const i18nStrings = {
  greetings: {
    hi: 'Hallo!'
  }
}

app.use(i18nPlugin, i18nStrings)
app.mount('#app')
```
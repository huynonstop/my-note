# Components basic

https://v3.vuejs.org/guide/class-and-style.html#with-components

https://v3.vuejs.org/guide/forms.html#v-model-with-components

https://v3.vuejs.org/guide/list.html#v-for-with-a-component

```js
// Create a Vue application
const app = Vue.createApp({})

// Define a new global component called button-counter
app.component('button-counter', {
  data() {
    return {
      count: 0
    }
  },
  template: `
    <button @click="count++">
      You clicked me {{ count }} times.
    </button>`
})
//in a typical Vue application we use Single File Components instead of a string template
```

Components are reusable instances with a name: in this case, `<button-counter>`. We can use this component as a custom element inside a root instance:

```html
<div id="components-demo">
  <button-counter></button-counter>
</div>
```

```js
app.mount('#components-demo')
```

Since components are reusable instances, they accept the same options as a root instance, such as `data`, `computed`, `watch`, `methods`, and lifecycle hooks. The only exceptions are a few root-specific options like `el`

## Reusing Components

Components can be reused as many times as you want:

```html
<div id="components-demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>
```

Notice that when clicking on the buttons, each one maintains its own, separate `count`. That's because each time you use a component, a new **instance** of it is created.

## Organizing Components

It's common for an app to be organized into a tree of nested components

For example, you might have components for a header, sidebar, and content area, each typically containing other components for navigation links, blog posts, etc.

## Comunication between Component

![image-20201022173111114](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20201022173111114.png)

## Passing Data to Child Components with Props

The problem is, that a component for blog posts won't be useful unless you can pass data to it, such as the title and content of the specific post we want to display. That's where props come in.

Props are custom attributes you can register on a component. When a value is passed to a prop attribute, it becomes a property on that component instance. To pass a title to our blog post component, we can include it in the list of props this component accepts, using a `props` option

A component can have as many props as you'd like and by default, any value can be passed to any prop and we can access this value on the component instance, just like with `data`

```js
const app = Vue.createApp({})

app.component('blog-post', {
  props: ['title'],
  template: `<h4>{{ title }}</h4>`
})

app.mount('#blog-post-demo')
```

Once a prop is registered, you can pass data to it as a custom attribute

```html
<div id="blog-post-demo" class="demo">
  <blog-post title="My journey with Vue"></blog-post>
  <blog-post title="Blogging with Vue"></blog-post>
  <blog-post title="Why Vue is so fun"></blog-post>
</div>
```

In a typical app, however, you'll likely have an array of posts in `data`:

```js
const App = {
  data() {
    return {
      posts: [
        { id: 1, title: 'My journey with Vue' },
        { id: 2, title: 'Blogging with Vue' },
        { id: 3, title: 'Why Vue is so fun' }
      ]
    }
  }
}

const app = Vue.createApp(App)

app.component('blog-post', {
  props: ['title'],
  template: `<h4>{{ title }}</h4>`
})

app.mount('#blog-posts-demo')
```

Then want to render a component for each one:

```html
<div id="blog-posts-demo">
  <blog-post
    v-for="post in posts"
    :key="post.id"
    :title="post.title"
  ></blog-post>
</div>
```

Above, you'll see that we can use `v-bind` to dynamically pass props. This is especially useful when you don't know the exact content you're going to render ahead of time.

## Listening to Child Components Events

As we develop our component, some features may require communicating back up to the parent

 For example, we may decide to include an accessibility feature to enlarge the text of blog posts, while leaving the rest of the page its default size.

In the parent, we can support this feature by adding a `postFontSize` data property:

```js
const App = {
  data() {
    return {
      posts: [
        /* ... */
      ],
      postFontSize: 1
    }
  }
}
```

```html
<div id="blog-posts-events-demo">
  <div v-bind:style="{ fontSize: postFontSize + 'em' }">
    <blog-post v-for="post in posts" :key="post.id" :title="title"></blog-post>
  </div>
</div>
```

Now let's add a button to enlarge the text right before the content of every post:

```js
app.component('blog-post', {
  props: ['title'],
  template: `
    <div class="blog-post">
      <h4>{{ title }}</h4>
      <button>
        Enlarge text
      </button>
    </div>
  `
})
// => this button doesn't do anything
```

When we click on the button, we need to communicate to the parent that it should enlarge the text of all posts

Fortunately, component instances provide a custom events system to solve this problem. The parent can choose to listen to any event on the child component instance with `v-on` or `@`, just as we would with a native DOM event:

```html
<blog-post ... @enlarge-text="postFontSize += 0.1"></blog-post>
```

Then the child component can emit an event on itself by calling the built-in **`$emit`** method, passing the name of the event:

```html
<button @click="$emit('enlarge-text')">
  Enlarge text
</button>
```

Thanks to the `@enlarge-text="postFontSize += 0.1"` listener, the parent will receive the event and update `postFontSize` value.

We can list emitted events in the component's `emits` option.

```js
app.component('blog-post', {
  props: ['title'],
  emits: ['enlarge-text']
})
```

This will allow you to check all the events component emits and optionally validate them

###  Emitting a Value With an Event

we can use `$emit`'s 2nd parameter to provide a specific value with an event:

```html
<button @click="$emit('enlarge-text', 0.1)">
  Enlarge text
</button>
```

Then when we listen to the event in the parent, we can access the emitted event's value with `$event`:

```html
<blog-post ... @enlarge-text="postFontSize += $event"></blog-post>
```

Or, if the event handler is a method:

```html
<blog-post ... @enlarge-text="onEnlargeText"></blog-post>
```

Then the value will be passed as the first parameter of that method:

```js
methods: {
  onEnlargeText(enlargeAmount) {
    this.postFontSize += enlargeAmount
  }
}
```

### Using `v-model` on Components

Custom events can also be used to create custom inputs that work with `v-model`. Remember that:

```html
<input v-model="searchText" />
```

does the same thing as:

```html
<input :value="searchText" @input="searchText = $event.target.value" />
```

When used on a component, `v-model` instead does this:

```html
<custom-input
  :model-value="searchText"
  @update:model-value="searchText = $event"
></custom-input>
```

For this to actually work though, the `<input>` inside the component must:

- Bind the `value` attribute to a `modelValue` prop
- On `input`, emit an `update:modelValue` event with the new value

Here's that in action:

```js
app.component('custom-input', {
  props: ['modelValue'],
  template: `
    <input
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
    >
  `
})
```

Now `v-model` should work perfectly with this component:

```html
<custom-input v-model="searchText"></custom-input>
```



Another way of creating the `v-model` capability within a custom component is to use the ability of `computed` properties' to define a getter and setter.

In the following example, we refactor the `custom-input` component using a computed property.

Keep in mind, the `get` method should return the `modelValue` property, or whichever property is being using for binding, and the `set` method should fire off the corresponding `$emit` for that property.

```js
app.component('custom-input', {
  props: ['modelValue'],
  template: `
    <input v-model="value">
  `,
  computed: {
    value: {
      get() {
        return this.modelValue
      },
      set(value) {
        this.$emit('update:modelValue', value)
      }
    }
  }
})
```

## Content Distribution with Slots

Just like with HTML elements, it's often useful to be able to pass content to a component, like this:

```html
<alert-box>
  Something bad happened.
</alert-box>
```

```js
app.component('alert-box', {
  template: `
    <div class="demo-alert-box">
      <strong>Error!</strong>
      <slot></slot>
    </div>
  `
})
// => we just add the slot where we want it to go
```

##  DOM Template Parsing Caveats

Some HTML elements, such as `<ul>`, `<ol>`, `<table>` and `<select>` have restrictions on what elements can appear inside them, and some elements such as `<li>`, `<tr>`, and `<option>` can only appear inside certain other elements.

This will lead to issues when using components with elements that have such restrictions. For example:

```html
<table>
  <blog-post-row></blog-post-row>
</table>
```

The custom component `<blog-post-row>` will be hoisted out as invalid content, causing errors in the eventual rendered output. Fortunately, we can use `v-is` special directive as a workaround:

```html
<table>
  <tr v-is="'blog-post-row'"></tr>
</table>

<!-- Incorrect, nothing will be rendered -->
<tr v-is="blog-post-row"></tr>
```

HTML attribute names are case-insensitive, so browsers will interpret any uppercase characters as lowercase. That means when you’re using in-DOM templates, camelCased prop names and event handler parameters need to use their kebab-cased (hyphen-delimited) equivalents:

```js
// camelCase in JavaScript

app.component('blog-post', {
  props: ['postTitle'],
  template: `
    <h3>{{ postTitle }}</h3>
  `
})
```

```html
<!-- kebab-case in HTML -->

<blog-post post-title="hello!"></blog-post>
```

It should be noted that **these limitations do \*not\* apply if you are using string templates from one of the following sources**:

- String templates (e.g. `template: '...'`)
- [Single-file (`.vue`) components](https://v3.vuejs.org/guide/single-file-component.html)
- `<script type="text/x-template">`

# Component Registration

To use these components in templates, they must be registered so that Vue knows about them. There are two types of component registration: **global** and **local**

## Component Names

When registering a component, it will always be given a name. For example, in the global registration we've seen so far:

```js
const app = Vue.createApp({...})

app.component('my-component-name', {
  /* ... */
})
```

The component's name is the first argument of `app.component`. In the example above, the component's name is "my-component-name".

1. All lowercase
2. Contains a hyphen (i.e., has multiple words connected with the hyphen symbol)

### Name Casing

When defining components in a string template or a single-file component, you have two options when defining component names:

```js
//With kebab-case
app.component('my-component-name', {
  /* ... */
})
// When defining a component with kebab-case, you must also use kebab-case when referencing its custom element, such as in `<my-component-name>`.
```

```js
//With PascalCase
app.component('MyComponentName', {
  /* ... */
})
// When defining a component with PascalCase, you can use either case when referencing its custom element. That means both `<my-component-name>` and `<MyComponentName>` are acceptable. Note, however, that only kebab-case names are valid directly in the DOM (i.e. non-string templates).
```

## Global Registration

So far, we've only created components using `app.component`:

```js
Vue.createApp({...}).component('my-component-name', {
  // ... options ...
})
```

These components are **globally registered** for the application. That means they can be used in the template of any component instance within this application:

```js
const app = Vue.createApp({})

app.component('component-a', {
  /* ... */
})
app.component('component-b', {
  /* ... */
})
app.component('component-c', {
  /* ... */
})

app.mount('#app')
```

```html
<div id="app">
  <component-a></component-a>
  <component-b></component-b>
  <component-c></component-c>
</div>
```

This even applies to all subcomponents, meaning all three of these components will also be available *inside each other*.

## Local Registration

Global registration often isn't ideal. For example, if you're using a build system like Webpack, globally registering all components means that even if you stop using a component, it could still be included in your final build. This unnecessarily increases the amount of JavaScript your users have to download.

In these cases, you can define your components as plain JavaScript objects:

```js
const ComponentA = {
  /* ... */
}
const ComponentB = {
  /* ... */
}
const ComponentC = {
  /* ... */
}
```

Then define the components you'd like to use in a `components` option:

```js
const app = Vue.createApp({
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```

For each property in the `components` object, the key will be the name of the custom element, while the value will contain the options object for the component.

Note that **locally registered components are \*not\* also available in subcomponents**. For example, if you wanted `ComponentA` to be available in `ComponentB`, you'd have to use:

```js
const ComponentA = {
  /* ... */
}

const ComponentB = {
  components: {
    'component-a': ComponentA
  }
  // ...
}
// Or if you're using ES2015 modules, such as through Babel and Webpack, that might look more like:
import ComponentA from './ComponentA.vue'

export default {
  components: {
    ComponentA // same ComponentA: ComponentA
  }
  // ...
}
// the name of the ComponentA is both:
// - the custom element name to use in the template, and
// - the name of the variable containing the component options
```

### Module Systems (Local Registration)

We recommend creating a `components` directory, with each component in its own file.

```js
import ComponentA from './ComponentA'
import ComponentC from './ComponentC'
// => Then you'll need to import each component you'd like to use, before you locally register it. For example, in a hypothetical `ComponentB.js` or `ComponentB.vue` file:

export default {
  components: {
    ComponentA,
    ComponentC
  }
  // ...
}
// => Now both `ComponentA` and `ComponentC` can be used inside `ComponentB`'s template.
```

# Props

## Prop Types

We can use props listed as an array of strings

```js
props: ['title', 'likes', 'isPublished', 'commentIds', 'author']
```

Usually though, you'll want every prop to be a specific type of value. In these cases, you can list props as an object, where the properties' names and values contain the prop names and types, respectively:

```js
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise // or any other constructor
}
```

## Passing Static or Dynamic Props

props passed a static value

```html
<blog-post title="My journey with Vue"></blog-post>
```

props assigned dynamically with `v-bind` or its shortcut, the `:` character

```html
<!-- Dynamically assign the value of a variable -->
<blog-post :title="post.title"></blog-post>

<!-- Dynamically assign the value of a complex expression -->
<blog-post :title="post.title + ' by ' + post.author.name"></blog-post>
```

any value can be passed as props

```html
<!-- Passing Number -->
<!-- Even though `42` is static, we need v-bind to tell Vue that -->
<!-- this is a JavaScript expression rather than a string.       -->
<blog-post :likes="42"></blog-post>

<!-- Dynamically assign to the value of a variable. -->
<blog-post :likes="post.likes"></blog-post>


<!-- Passing Boolean -->
<!-- Including the prop with no value will imply `true`. -->
<blog-post is-published></blog-post>

<!-- Even though `false` is static, we need v-bind to tell Vue that -->
<!-- this is a JavaScript expression rather than a string.          -->
<blog-post :is-published="false"></blog-post>

<!-- Dynamically assign to the value of a variable. -->
<blog-post :is-published="post.isPublished"></blog-post>


<!-- Passing Array -->
<!-- Even though the array is static, we need v-bind to tell Vue that -->
<!-- this is a JavaScript expression rather than a string.            -->
<blog-post :comment-ids="[234, 266, 273]"></blog-post>

<!-- Dynamically assign to the value of a variable. -->
<blog-post :comment-ids="post.commentIds"></blog-post>


<!-- Passing Object -->
<!-- Even though the object is static, we need v-bind to tell Vue that -->
<!-- this is a JavaScript expression rather than a string.             -->
<blog-post
  :author="{
    name: 'Veronica',
    company: 'Veridian Dynamics'
  }"
></blog-post>

<!-- Dynamically assign to the value of a variable. -->
<blog-post :author="post.author"></blog-post>
```

```js
// Passing the Properties of an Object
post: {
  id: 1,
  title: 'My Journey with Vue'
}
// If you want to pass all the properties of an object as props, you can use v-bind without an argument (v-bind instead of :prop-name). For example, given a post object:
```

```html
<blog-post v-bind="post"></blog-post>
<!-- Will be equivalent to -->
<blog-post v-bind:id="post.id" v-bind:title="post.title"></blog-post>
```

## One-Way Data Flow

All props form a **one-way-down binding** between the child property and the parent one: when the parent property updates, it will flow down to the child, but not the other way around. This prevents child components from accidentally mutating the parent's state, which can make your app's data flow harder to understand.

In addition, every time the parent component is updated, all props in the child component will be refreshed with the latest value. This means you should **not** attempt to mutate a prop inside a child component.

if the prop is an array or object, mutating the object or array itself inside the child component **will** affect parent state.

There are usually two cases where it's tempting to mutate a prop:

```js
// **The prop is used to pass in an initial value; the child component wants to use it as a local data property afterwards.** In this case, it's best to define a local data property that uses the prop as its initial value:
props: ['initialCounter'],
data() {
  return {
    counter: this.initialCounter
  }
}
```

```js
// **The prop is passed in as a raw value that needs to be transformed.** In this case, it's best to define a computed property using the prop's value:
props: ['size'],
computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}
```

## Prop Validation

Components can specify requirements for their props, such as the types you've already seen. If a requirement isn't met, Vue will warn you in the browser's JavaScript console

To specify prop validations, you can provide an object with validation requirements to the value of `props`, instead of an array of strings. For example:

```js
app.component('my-component', {
  props: {
    // Basic type check (`null` and `undefined` values will pass any type validation)
    propA: Number,
    // Multiple possible types
    propB: [String, Number],
    // Required string
    propC: {
      type: String,
      required: true
    },
    // Number with a default value
    propD: {
      type: Number,
      default: 100
    },
    // Object with a default value
    propE: {
      type: Object,
      // Object or array defaults must be returned from
      // a factory function
      default: function() {
        return { message: 'hello' }
      }
    },
    // Custom validator function
    propF: {
      validator: function(value) {
        // The value must match one of these strings
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    },
    // Function with a default value
    propG: {
      type: Function,
      // Unlike object or array default, this is not a factory function - this is a function to serve as a default value
      default: function() {
        return 'Default function'
      }
    }
  }
})
/*
When prop validation fails, Vue will produce a console warning (if using the development build).

Note that props are validated before a component instance is created, so instance properties (e.g. data, computed, etc) will not be available inside default or validator functions.
*/
```

The `type` can be one of the following native constructors: String, Number, Boolean, Array, Object, Date, Function, Symbol

In addition, `type` can also be a custom constructor function  (built-in ones like `Date` or custom ones).  and the assertion will be made with an `instanceof` check. For example, given the following constructor function exists:

```js
function Person(firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
}
```

You could use:

```js
app.component('blog-post', {
  props: {
    author: Person
  }
})
```

to validate that the value of the `author` prop was created with `new Person`

## Prop Casing (camelCase vs kebab-case)

HTML attribute names are case-insensitive, so browsers will interpret any uppercase characters as lowercase. That means when you're using in-DOM templates, camelCased prop names need to use their kebab-cased (hyphen-delimited) equivalents:

```js
const app = Vue.createApp({})

app.component('blog-post', {
  // camelCase in JavaScript
  props: ['postTitle'],
  template: '<h3>{{ postTitle }}</h3>'
})
```

```html
<!-- kebab-case in HTML -->
<blog-post post-title="hello!"></blog-post>
```

Again, if you're using string templates, this limitation does not apply.

# Non-Prop Attributes

A component non-prop attribute is an attribute or event listener that is passed to a component, but does not have a corresponding property defined in props or emits. Common examples of this include `class`, `style`, and `id` attributes. You can access those attributes via `$attrs` property.

## Attribute Inheritance

When a component returns a single root node, non-prop attributes will automatically be added to the root node's attributes

```js
app.component('date-picker', {
  template: `
    <div class="date-picker">
      <input type="datetime" />
    </div>
  `
})
```

```html
<!-- Date-picker component with a non-prop attribute -->
<date-picker data-status="activated"></date-picker>

<!-- Rendered date-picker component -->
<div class="date-picker" data-status="activated">
  <input type="datetime" />
</div>
```

Same rule applies to the event listeners:

```html
<date-picker @change="submitChange"></date-picker>
```

```js
app.component('date-picker', {
  created() {
    console.log(this.$attrs) // { onChange: () => {}  }
  }
})
```

This might be helpful when we have an HTML element with `change` event as a root element of `date-picker`.

```js
app.component('date-picker', {
  template: `
    <select>
      <option value="1">Yesterday</option>
      <option value="2">Today</option>
      <option value="3">Tomorrow</option>
    </select>
  `
})
```

In this case, `change` event listener is passed from the parent component to the child and it will be triggered on native `<select>` `change` event. We won't need to emit an event from the `date-picker` explicitly:

```html
<div id="date-picker" class="demo">
  <date-picker @change="showChange"></date-picker>
</div>
```

```js
const app = Vue.createApp({
  methods: {
    showChange(event) {
      console.log(event.target.value) // will log a value of the selected option
    }
  }
})
```

## Disabling Attribute Inheritance

If you do **not** want a component to automatically inherit attributes, you can set `inheritAttrs: false` in the component's options.

The common scenario for disabling an attribute inheritance is when attributes need to be applied to other elements besides the root node.

By setting the `inheritAttrs` option to `false`, you can control to apply to other elements attributes to use the component's `$attrs` property, which includes all attributes not included to component `props` and `emits` properties (e.g., `class`, `style`, `v-on` listeners, etc.).

Using our date-picker component example from the previous section, in the event we need to apply all non-prop attributes to the `input` element rather than the root `div` element, this can be accomplished by using the `v-bind` shortcut.

```js
app.component('date-picker', {
  inheritAttrs: false,
  template: `
    <div class="date-picker">
      <input type="datetime" v-bind="$attrs" />
    </div>
  `
})
```

With this new configuration, our `data-status` attribute will be applied to our `input` element!

```html
<!-- Date-picker component with a non-prop attribute -->
<date-picker data-status="activated"></date-picker>

<!-- Rendered date-picker component -->
<div class="date-picker">
  <input type="datetime" data-status="activated" />
</div>
```

## Attribute Inheritance on Multiple Root Nodes

Unlike single root node components, components with multiple root nodes do not have an automatic attribute fallthrough behavior. If `$attrs` are not bound explicitly, a runtime warning will be issued.

```html
<custom-layout id="custom-layout" @click="changeValue"></custom-layout>
```

```js
// This will raise a warning
app.component('custom-layout', {
  template: `
    <header>...</header>
    <main>...</main>
    <footer>...</footer>
  `
})

// No warnings, $attrs are passed to <main> element
app.component('custom-layout', {
  template: `
    <header>...</header>
    <main v-bind="$attrs">...</main>
    <footer>...</footer>
  `
})
```

# Custom Events

## Event Names

Unlike components and props, event names don't provide any automatic case transformation. Instead, the name of an emitted event must exactly match the name used to listen to that event.

```js
this.$emit('myEvent')
```

```html
<!-- Won't work -->
<my-component @my-event="doSomething"></my-component>
```

Event names will never be used as variable or property names in JavaScript, there is no reason to use camelCase or PascalCase. Additionally, `v-on` event listeners inside DOM templates will be automatically transformed to lowercase (due to HTML's case-insensitivity), so `@myEvent` would become `@myevent` -- making `myEvent` impossible to listen to.

For these reasons, we recommend you **always use kebab-case for event names**.

## Defining Custom Events

Emitted events can be defined on the component via the `emits` option.

```js
app.component('custom-form', {
  emits: ['in-focus', 'submit']
})
```

When a native event (e.g., `click`) is defined in the `emits` option, the component event will be used **instead** of a native event listener.

### Validate Emitted Events

Similar to prop type validation, an emitted event can be validated if it is defined with the Object syntax instead of the Array syntax.

To add validation, the event is assigned a function that receives the arguments passed to the `$emit` call and returns a boolean to indicate whether the event is valid or not.

```js
app.component('custom-form', {
  emits: {
    // No validation
    click: null,

    // Validate submit event
    submit: ({ email, password }) => {
      if (email && password) {
        return true
      } else {
        console.warn('Invalid submit event payload!')
        return false
      }
    }
  },
  methods: {
    submitForm() {
      this.$emit('submit', { email, password })
    }
  }
})
```

## `v-model` arguments

By default, `v-model` on a component uses `modelValue` as the prop and `update:modelValue` as the event. We can modify these names passing an argument to `v-model`:

```html
<my-component v-model:title="bookTitle"></my-component>
```

1

In this case, child component will expect a `title` prop and emits `update:title` event to sync:

```js
const app = Vue.createApp({})

app.component('my-component', {
  props: {
    title: String
  },
  template: `
    <input 
      type="text"
      :value="title"
      @input="$emit('update:title', $event.target.value)">
  `
})
```

```html
<my-component v-model:title="bookTitle"></my-component>
```

## Multiple `v-model` bindings

By leveraging the ability to target a particular prop and event as we learned before with [`v-model` arguments](https://v3.vuejs.org/guide/component-custom-events.html#v-model-arguments), we can now create multiple v-model bindings on a single component instance.

Each v-model will sync to a different prop, without the need for extra options in the component:

```html
<user-name
  v-model:first-name="firstName"
  v-model:last-name="lastName"
></user-name>
```

```js
const app = Vue.createApp({})

app.component('user-name', {
  props: {
    firstName: String,
    lastName: String
  },
  template: `
    <input 
      type="text"
      :value="firstName"
      @input="$emit('update:firstName', $event.target.value)">

    <input
      type="text"
      :value="lastName"
      @input="$emit('update:lastName', $event.target.value)">
  `
})
```

## Handling `v-model` modifiers

When we were learning about form input bindings, we saw that `v-model` has built-in modifiers - `.trim`, `.number` and `.lazy`. In some cases, however, you might also want to add your own custom modifiers.

Modifiers added to a component `v-model` will be provided to the component via the `modelModifiers` prop. In the below example, we have created a component that contains a `modelModifiers` prop that defaults to an empty object.

```html
<my-component v-model.capitalize="bar"></my-component>
```

```js
app.component('my-component', {
  props: {
    modelValue: String,
    modelModifiers: {
      default: () => ({})
    }
  },
  template: `
    <input type="text" 
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)">
  `,
  created() {
    // Notice that when the component's created lifecycle hook triggers, the modelModifiers prop contains capitalize and its value is true - due to it being set on the v-model binding v-model.capitalize="bar".
    console.log(this.modelModifiers) // { capitalize: true }
  }
})
```

Now that we have our prop set up, we can check the `modelModifiers` object keys and write a handler to change the emitted value. In the code below we will capitalize the string whenever the `<input />` element fires an `input` event.

```html
<div id="app">
  <my-component v-model.capitalize="myText"></my-component>
  {{ myText }}
</div>
```

```js
const app = Vue.createApp({
  data() {
    return {
      myText: ''
    }
  }
})

app.component('my-component', {
  props: {
    modelValue: String,
    modelModifiers: {
      default: () => ({})
    }
  },
  methods: {
    emitValue(e) {
      let value = e.target.value
      if (this.modelModifiers.capitalize) {
        value = value.charAt(0).toUpperCase() + value.slice(1)
      }
      this.$emit('update:modelValue', value)
    }
  },
  template: `<input
    type="text"
    :value="modelValue"
    @input="emitValue">`
})

app.mount('#app')
```

For `v-model` bindings with arguments, the generated prop name will be `arg + "Modifiers"`:

```html
<my-component v-model:foo.capitalize="bar"></my-component>
```

```js
app.component('my-component', {
  props: ['foo', 'fooModifiers'],
  template: `
    <input type="text" 
      :value="foo"
      @input="$emit('update:foo', $event.target.value)">
  `,
  created() {
    console.log(this.fooModifiers) // { capitalize: true }
  }
})
```

#  Slots

##  Slot Content

Vue  using the `<slot>` element to serve as distribution outlets for content.

This allows you to compose components like this:

```html
<todo-button>
  Add todo
</todo-button>
```

Then in the template for `<todo-button>`, you might have:

```html
<!-- todo-button component template -->
<button class="btn-primary">
  <slot></slot>
</button>
```

When the component renders, `<slot></slot>` will be replaced by "Add Todo".

```html
<!-- rendered HTML -->
<button class="btn-primary">
  Add todo
</button>
```

Slots can also contain any template code, including HTML:

```html
<todo-button>
  <!-- Add a Font Awesome icon -->
  <i class="fas fa-plus"></i>
  Add todo
</todo-button>
```

Or even other components:

```html
<todo-button>
  <!-- Use a component to add an icon -->
  <font-awesome-icon name="plus"></font-awesome-icon>
  Add todo
</todo-button>
```

If `<todo-button>`'s template did **not** contain a `<slot>` element, any content provided between its opening and closing tag would be discarded.

```html
<!-- todo-button component template -->

<button class="btn-primary">
  Create a new item
</button>
```

```html
<todo-button>
  <!-- Following text won't be rendered -->
  Add todo
</todo-button>
```

## Render Scope

When you want to use data inside a slot, such as in:

```html
<todo-button>
  Delete a {{ item.name }}
</todo-button>
```

That slot has access to the same instance properties (i.e. the same "scope") as the rest of the template.

![image-20201020190933518](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20201020190933518.png)

The slot does **not** have access to `<todo-button>`'s scope. For example, trying to access `action` would not work:

```html
<todo-button action="delete">
  Clicking here will {{ action }} an item
  <!--
  The `action` will be undefined, because this content is passed
  _to_ <todo-button>, rather than defined _inside_ the
  <todo-button> component.
  -->
</todo-button>
```

As a rule, remember that:

> Everything in the parent template is compiled in parent scope; everything in the child template is compiled in the child scope.

## Fallback (default) Content

There are cases when it's useful to specify fallback (i.e. default) content for a slot, to be rendered only when no content is provided. For example, in a `<submit-button>` component:

```html
<button type="submit">
  <slot></slot>
</button>
```

We might want the text "Submit" to be rendered inside the `<button>` most of the time. To make "Submit" the fallback content, we can place it in between the `<slot>` tags:

```html
<button type="submit">
  <slot>Submit</slot>
</button>
```

Now when we use `<submit-button>` in a parent component, providing no content for the slot:

```html
<submit-button></submit-button>
```

will render the fallback content, "Submit":

```html
<button type="submit">
  Submit
</button>
```

But if we provide content:

```html
<submit-button>
  Save
</submit-button>
```

Then the provided content will be rendered instead:

```html
<button type="submit">
  Save
</button>
```

## Named Slots

There are times when it's useful to have multiple slots. For example, in a `<base-layout>` component with the following template:

```html
<div class="container">
  <header>
    <!-- We want header content here -->
  </header>
  <main>
    <!-- We want main content here -->
  </main>
  <footer>
    <!-- We want footer content here -->
  </footer>
</div>
```

For these cases, the `<slot>` element has a special attribute, `name`, which can be used to assign a unique ID to different slots so you can determine where content should be rendered:

A `<slot>` outlet without `name` implicitly has the name "default".

```html
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

To provide content to named slots, we need to use the `v-slot` directive on a `<template>` element, providing the name of the slot as `v-slot`'s argument:

```html
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <template v-slot:default>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

Now everything inside the `<template>` elements will be passed to the corresponding slots.

The rendered HTML will be:

```html
<div class="container">
  <header>
    <h1>Here might be a page title</h1>
  </header>
  <main>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </main>
  <footer>
    <p>Here's some contact info</p>
  </footer>
</div>
```

### Not render empty warper element when empty slot

```vue
<template>
  <div class="base-card">
    <header v-if="$slot.header">
      <slot name="header"></slot>
    </header>
    <slot></slot>
  </div>
</template>
```

Note that **`v-slot` can only be added to a `<template> `** and  with 1 exception below:

## Scoped Slots

Sometimes, it's useful for slot content to have access to data only available in the child component. It's a common case when a component is used to render an array of items, and we want to be able to customize the way each item is rendered.

For example, we have a component, containing a list of todo-items.

```js
app.component('todo-list', {
  data() {
    return {
      items: ['Feed a cat', 'Buy milk']
    }
  },
  template: `
    <ul>
      <li v-for="(item, index) in items">
        {{ item }}
      </li>
    </ul>
  `
})
```

We might want to replace the slot to customize it on parent component:

```html
<todo-list>
  <i class="fas fa-check"></i>
  <span class="green">{{ item }}</span>
</todo-list>
<!-- This won't work, however, because only the `<todo-list>` component has access to the `item` and we are providing the slot content from its parent. -->
```

To make `item` available to the slot content provided by the parent, we can add a `<slot>` element and bind it as an attribute:

```html
<ul>
  <li v-for="( item, index ) in items">
    <slot :item="item"></slot>
  </li>
</ul>
```

Attributes bound to a `<slot>` element are called **slot props**. Now, in the parent scope, we can use `v-slot` with a value to define a name for the slot props we've been provided:

```html
<todo-list>
  <template v-slot:default="slotProps">
    <i class="fas fa-check"></i>
    <span class="green">{{ slotProps.item }}</span>
  </template>
</todo-list>
```

![image-20201020192121682](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20201020192121682.png)

### Abbreviated Syntax for Lone Default Slots

when *only* the default slot is provided content, the component's tags can be used as the slot's template. This allows us to use `v-slot` directly on the component:

```html
<todo-list v-slot:default="slotProps">
  <i class="fas fa-check"></i>
  <span class="green">{{ slotProps.item }}</span>
</todo-list>
<!-- OR -->
<todo-list v-slot="slotProps">
  <i class="fas fa-check"></i>
  <span class="green">{{ slotProps.item }}</span>
</todo-list>

<!-- INVALID,  syntax for default slot cannot be mixed with named slots, will result in warning -->
<todo-list v-slot="slotProps">
  <todo-list v-slot:default="slotProps">
    <i class="fas fa-check"></i>
    <span class="green">{{ slotProps.item }}</span>
  </todo-list>
  <template v-slot:other="otherSlotProps">
    slotProps is NOT available here
  </template>
</todo-list>

<!-- Whenever there are multiple slots, use the full <template> based syntax for all slots: -->
<todo-list>
  <template v-slot:default="slotProps">
    <i class="fas fa-check"></i>
    <span class="green">{{ slotProps.item }}</span>
  </template>

  <template v-slot:other="otherSlotProps">
    ...
  </template>
</todo-list>
```

### Destructuring Slot Props

Internally, scoped slots work by wrapping your slot content in a function passed a single argument:

```js
function (slotProps) {
  // ... slot content ...
}
```

That means the value of `v-slot` can actually accept any valid JavaScript expression that can appear in the argument position of a function definition. So you can destructuring slot props by using ES2015 syntax:

```html
<todo-list v-slot="{ item }">
  <i class="fas fa-check"></i>
  <span class="green">{{ item }}</span>
</todo-list>
```

This can make the template much cleaner, especially when the slot provides many props. It also opens other possibilities, such as renaming props, e.g. `item` to `todo`:

```html
<todo-list v-slot="{ item: todo }">
  <i class="fas fa-check"></i>
  <span class="green">{{ todo }}</span>
</todo-list>
```

You can even define fallbacks, to be used in case a slot prop is undefined:

```html
<todo-list v-slot="{ item = 'Placeholder' }">
  <i class="fas fa-check"></i>
  <span class="green">{{ item }}</span>
</todo-list>
```

## Dynamic Slot Names

[Dynamic directive arguments](https://v3.vuejs.org/guide/template-syntax.html#dynamic-arguments) also work on `v-slot`, allowing the definition of dynamic slot names:

```html
<base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>
</base-layout>
```

## Named Slots Shorthand

Similar to `v-on` and `v-bind`, `v-slot` also has a shorthand, replacing everything before the argument (`v-slot:`) with the special symbol `#`. For example, `v-slot:header` can be rewritten as `#header`:

```html
<base-layout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <template #default>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

However, just as with other directives, the shorthand is only available when an argument is provided. That means the following syntax is invalid:

```html
<!-- This will trigger a warning -->

<todo-list #="{ item }">
  <i class="fas fa-check"></i>
  <span class="green">{{ item }}</span>
</todo-list>
```

Instead, you must always specify the name of the slot if you wish to use the shorthand:

```html
<todo-list #default="{ item }">
  <i class="fas fa-check"></i>
  <span class="green">{{ item }}</span>
</todo-list>
```

# Provide / inject

Usually, when we need to pass data from the parent to child component, we use [props](https://v3.vuejs.org/guide/component-props.html). Imagine the structure where you have some deeply nested components and you only need something from the parent component in the deep nested child. In this case, you still need to pass the prop down the whole component chain which might be annoying.

For such cases, we can use the `provide` and `inject` pair. Parent components can serve as dependency provider for all its children, regardless how deep the component hierarchy is. This feature works on two parts: parent component has a `provide` option to provide data and child component has an `inject` option to start using this data.

![image-20201020192700267](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20201020192700267.png)

For example, if we have a hierarchy like this:

```text
Root
└─ TodoList
   ├─ TodoItem
   └─ TodoListFooter
      ├─ ClearTodosButton
      └─ TodoListStatistics
```

If we want to pass the length of todo-items directly to `TodoListStatistics`, we would pass the prop down the hierarchy: `TodoList` -> `TodoListFooter` -> `TodoListStatistics`. With provide/inject approach, we can do this directly:

```js
const app = Vue.createApp({})

app.component('todo-list', {
  data() {
    return {
      todos: ['Feed a cat', 'Buy tickets']
    }
  },
  provide: {
    user: 'John Doe'
  },
  template: `
    <div>
      {{ todos.length }}
      <!-- rest of the template -->
    </div>
  `
})

app.component('todo-list-statistics', {
  inject: ['user'],
  created() {
    console.log(`Injected property: ${this.user}`) // > Injected property: John Doe
  }
})

//However, this won't work if we try to provide some component instance property here:
app.component('todo-list', {
  data() {
    return {
      todos: ['Feed a cat', 'Buy tickets']
    }
  },
  provide: {
    todoLength: this.todos.length // this will result in error 'Cannot read property 'length' of undefined`
  },
  template: `
    ...
  `
})

// To access component instance properties, we need to convert provide to be a function returning an object
app.component('todo-list', {
  data() {
    return {
      todos: ['Feed a cat', 'Buy tickets']
    }
  },
  provide() {
    return {
      todoLength: this.todos.length
    }
  },
  template: `
    ...
  `
})
// => This allows us to more safely keep developing that component, without fear that we might change/remove something that a child component is relying on. The interface between these components remains clearly defined, just as with props.

// Provide a method, instead passing custom event
app.component('todo-list', {
  provide() {
    return {
      doSomething: this.doSomething
    }
  },
  methods: {
    doSomething() {
        console.log("something")
    }  
  },
  template: `
    ...
  `
})
```

In fact, you can think of dependency injection as sort of “long-range props”, except:

- parent components don’t need to know which descendants use the properties it provides
- child components don’t need to know where injected properties are coming from

## Working with reactivity

In the example above, if we change the list of `todos`, this change won't be reflected in the injected `todoLength` property. This is because `provide/inject` bindings are *not* reactive by default. We can change this behavior by passing a `ref` property or `reactive` object to `provide`. In our case, if we wanted to react to changes in the ancestor component, we would need to assign a Composition API `computed` property to our provided `todoLength`:

```js
app.component('todo-list', {
  // ...
  provide() {
    return {
      todoLength: Vue.computed(() => this.todos.length)
    }
  }
})
```

In this, any change to `todos.length` will be reflected correctly in the components, where `todoLength` is injected. Read more about `reactive` provide/inject in the [Composition API section](https://v3.vuejs.org/guide/composition-api-provide-inject.html#reactivity)

#  Dynamic & Async Components

## Dynamic Components

```html
<!-- Component changes when currentTabComponent changes -->
<component :is="currentTabComponent"></component>
```

In the example above, `currentTabComponent` can contain either:

- the name of a registered component, or
- a component's options object

## `keep-alive` Dynamic Components

https://v3.vuejs.org/api/built-in-components.html#keep-alive

Earlier, we used the `is` attribute to switch between components in a tabbed interface:

```vue
<component :is="currentTabComponent"></component>
```

When switching between these components though, you'll sometimes want to maintain their state or avoid re-rendering for performance reasons

```vue
<div id="dynamic-component-demo" class="demo">
  <button
     v-for="tab in tabs"
     :key="tab"
     :class="['tab-button', { active: currentTab === tab }]"
     @click="currentTab = tab"
   >
    {{ tab }}
  </button>

  <component :is="currentTabComponent" class="tab"></component>
</div>

const app = Vue.createApp({
  data() {
    return {
      currentTab: 'Home',
      tabs: ['Home', 'Posts', 'Archive']
    }
  },
  computed: {
    currentTabComponent() {
      return 'tab-' + this.currentTab.toLowerCase()
    }
  }
})

app.component('tab-home', {
  template: `<div class="demo-tab">Home component</div>`
})
app.component('tab-posts', {
  template: `
     <div class="dynamic-component-demo-posts-tab">
        <ul class="dynamic-component-demo-posts-sidebar">
          <li
            v-for="post in posts"
            :key="post.id"
            :class="{
              'dynamic-component-demo-active': post === selectedPost
            }"
            @click="selectedPost = post"
          >
            {{ post.title }}
          </li>
        </ul>
        <div class="dynamic-component-demo-post-container">
          <div v-if="selectedPost" class="dynamic-component-demo-post">
            <h3>{{ selectedPost.title }}</h3>
            <div v-html="selectedPost.content"></div>
          </div>
          <strong v-else>
            Click on a blog title to the left to view it.
          </strong>
        </div>
      </div>
    `,
    data() {
    return {
      posts: [
        {
          id: 1,
          title: 'Cat Ipsum',
          content:
            '<p>Cat Ipsum</p>'
        },
        {
          id: 2,
          title: 'Hipster Ipsum',
          content:
            '<p>Hipster Ipsum</p>'
        },
        {
          id: 3,
          title: 'Cupcake Ipsum',
          content:
            '<p>Cupcake Ipsum</p>'
        }
      ],
      selectedPost: null
    }
  }
})
app.component('tab-archive', {
  template: `<div class="demo-tab">Archive component</div>`
})

app.mount('#dynamic-component-demo')
```

 if you select a post, switch to the *Archive* tab, then switch back to *Posts*, it's no longer showing the post you selected. That's because each time you switch to a new tab, Vue creates a new instance of the `currentTabComponent`

Recreating dynamic components is normally useful behavior, but in this case, we'd really like those tab component instances to be cached once they're created for the first time. To solve this problem, we can wrap our dynamic component with a `<keep-alive>` element:

```vue
<!-- Inactive components will be cached! -->
<keep-alive>
  <component :is="currentTabComponent"></component>
</keep-alive>
```

Now the *Posts* tab maintains its state (the selected post) even when it's not rendered.

## Async Components

https://v3.vuejs.org/api/global-api.html#defineasynccomponent

In large applications, we may need to divide the app into smaller chunks and only load a component from the server when it's needed. To make that possible, Vue has a `defineAsyncComponent` method:

```js
const app = Vue.createApp({})

const AsyncComp = Vue.defineAsyncComponent(
  () =>
    new Promise((resolve, reject) => {
      resolve({
        template: '<div>I am async!</div>'
      })
    })
)
// => this method accepts a factory function returning a Promise. Promise's resolve callback should be called when you have retrieved your component definition from the server. You can also call reject(reason) to indicate the load has failed.
app.component('async-example', AsyncComp)
```

You can also return a `Promise` in the factory function, so with Webpack 2 or later and ES2015 syntax you can do:

```js
import { defineAsyncComponent } from 'vue'

const AsyncComp = defineAsyncComponent(() =>
  import('./components/AsyncComponent.vue')
)

app.component('async-component', AsyncComp)
```

You can also use `defineAsyncComponent` when registering a component locally

```js
import { createApp, defineAsyncComponent } from 'vue'

createApp({
  // ...
  components: {
    AsyncComponent: defineAsyncComponent(() =>
      import('./components/AsyncComponent.vue')
    )
  }
})
```

### Using with Suspense

https://vueschool.io/articles/vuejs-tutorials/suspense-new-feature-in-vue-3/

Async components are *suspensible* by default. This means if it has a `<Suspense>` in the parent chain, it will be treated as an async dependency of that `<Suspense>`. In this case, the loading state will be controlled by the `<Suspense>`, and the component's own loading, error, delay and timeout options will be ignored.

The async component can opt-out of `Suspense` control and let the component always control its own loading state by specifying `suspensible: false` in its options.

#  Template refs

Despite the existence of props and events, sometimes you might still need to directly access a child component in JavaScript. To achieve this you can assign a reference ID to the child component or HTML element using the `ref` attribute. For example:

```html
<input ref="input" />
```

This may be useful when you want to, for example, programmatically focus this input on component mount:

```js
const app = Vue.createApp({})

app.component('base-input', {
  template: `
    <input ref="input" />
  `,
  methods: {
    focusInput() {
      this.$refs.input.focus()
    }
  },
  mounted() {
    this.focusInput()
  }
})
```

Also, you can add another `ref` to the component itself and use it to trigger `focusInput` event from the parent component:

```html
<base-input ref="usernameInput"></base-input>
```

```js
this.$refs.usernameInput.focusInput()
```

WARNING

`$refs` are only populated after the component has been rendered. It is only meant as an escape hatch for direct child manipulation - you should avoid accessing `$refs` from within templates or computed properties.

# Handling Edge Cases

## Controlling Updates

### Forcing an Update

If you find yourself needing to force an update in Vue, in 99.99% of cases, you've made a mistake somewhere. For example, you may be relying on state that isn't tracked by Vue's reactivity system, e.g. with `data` property added after component creation.

However, if you've ruled out the above and find yourself in this extremely rare situation of having to manually force an update, you can do so with [`$forceUpdate`](https://v3.vuejs.org/api/instance-methods.html#forceupdate)

### Cheap Static Components with `v-once`

Rendering plain HTML elements is very fast in Vue, but sometimes you might have a component that contains **a lot** of static content. In these cases, you can ensure that it's only evaluated once and then cached by adding the `v-once` directive to the root element, like this:

```js
app.component('terms-of-service', {
  template: `
    <div v-once>
      <h1>Terms of Service</h1>
      ... a lot of static content ...
    </div>
  `
})
// => Once again, try not to overuse this pattern. While convenient in those rare cases when you have to render a lot of static content, it's simply not necessary unless you actually notice slow rendering - plus, it could cause a lot of confusion later. For example, imagine another developer who's not familiar with v-once or simply misses it in the template. They might spend hours trying to figure out why the template isn't updating correctly.
```

# Multiple Vue Apps vs Multiple Components

You can use Vue.js to control **parts** of (possibly multiple HTML) pages OR you use it to build so-called "**Single Page Applications**" (**SPA**s).

If you control multiple, independent parts of HTML pages, you will often work with **multiple Vue apps** (i.e. you create multiple apps by calling `createApp()` more than once).

On the other hand, if you're building a SPA, you typically work with just **one "root app"** (i.e. `createApp()` is only used once in your entire codebase) and you instead **build up a user interface with multiple components**.

You absolutely are allowed to also use components in cases where you have multiple Vue apps but you typically won't use multiple Vue apps if you build one big connected user interface.

Why?

Because **Vue apps are independent from each other** - they **can't really communicate** with each other. You might find "hacks" to make it work but there's no great "official" way of sharing data between apps, updating something in app A in case something happens in app B etc.

**Components** on the other hand - as you will learn soon - **DO offer certain communication mechanisms** that allow you to exchange data between them. Hence you can build one connected UI if you work with one root app that holds multiple components.

# Single File Components

https://v3.vuejs.org/guide/single-file-component.html

In many Vue projects, global components will be defined using `app.component()`, followed by `app.mount('#app')` to target a container element in the body of every page.

This can work very well for small to medium-sized projects, where JavaScript is only used to enhance certain views. In more complex projects however, or when your frontend is entirely driven by JavaScript, these disadvantages become apparent:

- **Global definitions** force unique names for every component
- **String templates** lack syntax highlighting and require ugly slashes for multiline HTML
- **No CSS support** means that while HTML and JavaScript are modularized into components, CSS is conspicuously left out
- **No build step** restricts us to HTML and ES5 JavaScript, rather than preprocessors like Pug (formerly Jade) and Babel

All of these are solved by **single-file components** with a `.vue` extension, made possible with build tools such as Webpack or Browserify.
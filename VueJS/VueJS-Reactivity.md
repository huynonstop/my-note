# Reactivity

https://v3.vuejs.org/guide/reactivity.html

https://www.vuemastery.com/courses/vue-3-reactivity/vue3-reactivity/

https://www.youtube.com/watch?v=HezB8UEU5Rg

https://www.youtube.com/watch?v=5HxOx_uyV3A

One of Vue’s most distinct features is the unobtrusive reactivity system. Models are proxied JavaScript objects. When you modify them, the view updates.

Reactivity is a programming paradigm that allows us to adjust to changes in a declarative manner. 

The canonical example that people usually show, because it’s a great one, is an **excel spreadsheet**.

JavaScript doesn’t usually work like this -- If we were to write something comparable in JavaScript:

```js
var val1 = 2
var val2 = 3
var sum = val1 + val2

// sum
// 5

//If we update the first value, the sum is not adjusted.
val1 = 3

// sum
// 5
```

So how would we do Reactivity in JavaScript?

- Detect when there’s a change in one of the values
- Track the function that changes it
- Trigger the function so it can update the final value

## How Vue Tracks These Changes

https://v3.vuejs.org/guide/reactivity.html#how-vue-tracks-these-changes

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy

When you pass a plain JavaScript object to an application or component instance as its `data` option, Vue will walk through all of its properties and convert them to Proxies using a handler with getters and setters. This is an ES6-only feature, but we offer a version of Vue 3 that uses the older `Object.defineProperty` to support IE browsers. Both have the same surface API, but the Proxy version is slimmer and offers improved performance.

**Proxy is an object that encases another object or function and allows you to intercept it.**

we can also intercept this object while we wrap it in the Proxy. This interception is called a trap.

```js
const dinner = {
  meal: 'tacos'
}

const handler = {
  get(target, prop) {
    // Beyond a console log, we could do anything here we wish. We could even not return the real value if we wanted to.
    console.log('intercepted!')
    return target[prop]
  }
}

const proxy = new Proxy(dinner, handler)
console.log(proxy.meal)

// intercepted!
// tacos
```

Furthermore, there’s another feature Proxies offer us. Rather than just returning the value like this: `target[prop]`, we could take this a step further and use a feature called `Reflect`, which allows us to do proper `this` binding

We mentioned before that in order to have an API that updates a final value when something changes, we’re going to have to set new values when something changes. We do this in the handler, in a function called `track`, where we pass in the `target` and `key`

Finally, we also set new values when something changes. For this, we’re going to set the changes on our new proxy, by triggering those changes

```js
const dinner = {
  meal: 'tacos'
}

const handler = {
  get(target, prop, receiver) {
    track(target, prop)
    return Reflect.get(...arguments)
  },
  set(target, key, value, receiver) {
    trigger(target, key)
    return Reflect.set(...arguments)
  }
}

const proxy = new Proxy(dinner, handler)
console.log(proxy.meal)

// tacos
```

how Vue handles Reactivity:

- ~~Detect when there’s a change in one of the values~~: we no longer have to do this, as Proxies allow us to intercept it
- **Track the function that changes it**: We do this in a getter within the proxy, called `effect`
- **Trigger the function so it can update the final value**: We do in a setter within the proxy, called `trigger`

The proxied object is invisible to the user, but under the hood they enable Vue to perform dependency-tracking and change-notification when properties are accessed or modified

### Proxied Objects

Vue internally tracks all objects that have been made reactive, so it always returns the same proxy for the same object.

When a nested object is accessed from a reactive proxy, that object is *also* converted into a proxy before being returned:

```js
const handler = {
  get(target, prop, receiver) {
    track(target, prop)
    const value = Reflect.get(...arguments)
    if (isObject(value)) {
      return reactive(value)
    } else {
      return value
    }
  }
  // ...
}
```

### Proxy vs. original identity

The use of Proxy does introduce a new caveat to be aware with: the proxied object is not equal to the original object in terms of identity comparison (`===`). For example:

```js
const obj = {}
const wrapped = new Proxy(obj, handlers)

console.log(obj === wrapped) // false
```

The original and the wrapped version will behave the same in most cases, but be aware that they will fail operations that rely on strong identity comparisons, such as `.filter()` or `.map()`. This caveat is unlikely to come up when using the options API, because all reactive state is accessed from `this` and guaranteed to already be proxies.

However, when using the composition API to explicitly create reactive objects, the best practice is to never hold a reference to the original raw object and only work with the reactive version:

```js
const obj = reactive({
  count: 0
}) // no reference to original
```

## Watchers

Every component instance has a corresponding watcher instance, which records any properties "touched" during the component’s render as dependencies. Later on when a dependency’s setter is triggered, it notifies the watcher, which in turn causes the component to re-render.

When you pass an object to a component instance as data, Vue converts it to a proxy. This proxy enables Vue to perform dependency-tracking and change-notification when properties are accessed or modified. Each property is considered a dependency.

After the first render, a component would have tracked a list of dependencies — the properties it accessed during the render. Conversely, the component becomes a subscriber to each of these properties. When a proxy intercepts a set operation, the property will notify all of its subscribed components to re-render.

# Reactivity Fundamentals

[Reactivity API | Vue.js](https://v3.vuejs.org/api/reactivity-api.html)

https://v3.vuejs.org/guide/reactivity-fundamentals.html

## Declaring Reactive State

To create a reactive state from a JavaScript object, we can use a `reactive` method:

```js
import { reactive } from 'vue'

// reactive state
const state = reactive({
  count: 0
})
```

`reactive` is the equivalent of the `Vue.observable()` API in Vue 2.x

Here, the returned state is a reactive object. The reactive conversion is "deep" - it affects all nested properties of the passed object.

The essential use case for reactive state in Vue is that we can use it during render. Thanks to dependency tracking, the view automatically updates when reactive state changes.

This is the very essence of Vue's reactivity system. When you return an object from `data()` in a component, it is internally made reactive by `reactive()`

The template is compiled into a [render function](https://v3.vuejs.org/guide/render-function.html) that makes use of these reactive properties.

## Creating Standalone Reactive Values as `refs`

Imagine the case where we have a standalone primitive value (for example, a string) and we want to make it reactive. Of course, we could make an object with a single property equal to our string, and pass it to `reactive`

```js
import { ref } from 'vue'

const count = ref(0)
```

`ref` will return a reactive and mutable object that serves as a reactive **ref**erence to the internal value it is holding - that's where the name comes from. This object contains the only one property named `value`:

```js
import { ref } from 'vue'

const count = ref(0)
console.log(count.value) // 0

count.value++
console.log(count.value) // 1
```

###  Ref Unwrapping

```vue
<template>
  <div>
    <span>{{ count }}</span>
    <button @click="count ++">Increment count</button>
  </div>
</template>

<script>
  import { ref } from 'vue'
  export default {
    setup() {
      const count = ref(0)
      // When a ref is returned as a property on the render context (the object returned from setup()) and accessed in the template, it automatically unwraps to the inner value. There is no need to append .value in the template:
      return {
        count
      }
    }
  }
</script>
```

### Access in Reactive Objects

When a `ref` is accessed or mutated as a property of a reactive object, it automatically unwraps to the inner value so it behaves like a normal property:

```js
const count = ref(0)
const state = reactive({
  count
})

console.log(state.count) // 0

state.count = 1
console.log(count.value) // 1
```

If a new ref is assigned to a property linked to an existing ref, it will replace the old ref:

```js
const otherCount = ref(2)

state.count = otherCount
console.log(state.count) // 2
console.log(count.value) // 1
```

Ref unwrapping only happens when nested inside a reactive `Object`. There is no unwrapping performed when the ref is accessed from an `Array` or a native collection type like `Map`

```js
const books = reactive([ref('Vue 3 Guide')])
// need .value here
console.log(books[0].value)

const map = reactive(new Map([['count', ref(0)]]))
// need .value here
console.log(map.get('count').value)
```

## Destructuring Reactive State

```js
import { reactive } from 'vue'

const book = reactive({
  author: 'Vue Team',
  year: '2020',
  title: 'Vue 3 Guide',
  description: 'You are reading this book right now ;)',
  price: 'free'
})
// When we want to use a few properties of the large reactive object, it could be tempting to use ES6 destructuring (opens new window)to get properties we want
let { author, title } = book
// Unfortunately, with such a destructuring the reactivity for both properties would be lost. For such a case, we need to convert our reactive object to a set of refs. These refs will retain the reactive connection to the source object
let { author, title } = toRefs(book)

title.value = 'Vue 3 Detailed Guide' // we need to use .value as title is a ref now
console.log(book.title) // 'Vue 3 Detailed Guide'
```

## Prevent Mutating Reactive Objects with `readonly`

Sometimes we want to track changes of the reactive object (`ref` or `reactive`) but we also want prevent changing it from a certain place of the application. For example, when we have a [provided](https://v3.vuejs.org/guide/component-provide-inject.html) reactive object, we want to prevent mutating it where it's injected. To do so, we can create a readonly proxy to the original object:

```js
import { reactive, readonly } from 'vue'

const original = reactive({ count: 0 })

const copy = readonly(original)

// mutating original will trigger watchers relying on the copy
original.count++

// mutating the copy will fail and result in a warning
copy.count++ // warning: "Set operation on key 'count' failed: target is readonly."
```

# Computed and Watch

https://v3.vuejs.org/guide/reactivity-computed-watchers.html

## Computed values

Sometimes we need state that depends on other state - in Vue this is handled with component computed properties

To directly create a computed value, we can use the `computed` method: it takes a getter function and returns an immutable reactive [ref](https://v3.vuejs.org/guide/reactivity-fundamentals.html#creating-standalone-reactive-values-as-refs) object for the returned value from the getter.

```js
const count = ref(1)
const plusOne = computed(() => count.value + 1)

console.log(plusOne.value) // 2
plusOne.value++ // error

// Alternatively, it can take an object with get and set functions to create a writable ref object.
const count = ref(1)
const plusOne = computed({
  get: () => count.value + 1,
  set: val => {
    count.value = val - 1
  }
})

plusOne.value = 1
console.log(count.value) // 0
```

## `watchEffect`

To apply and *automatically re-apply* a side effect based on reactive state, we can use the `watchEffect` method. It runs a function immediately while reactively tracking its dependencies and re-runs it whenever the dependencies are changed.

```js
const count = ref(0)

watchEffect(() => console.log(count.value))
// -> logs 0

setTimeout(() => {
  count.value++
  // -> logs 1
}, 100)
```

### Stopping the Watcher

When `watchEffect` is called during a component's [setup()](https://v3.vuejs.org/guide/composition-api-setup.html) function or [lifecycle hooks](https://v3.vuejs.org/guide/composition-api-lifecycle-hooks.html), the watcher is linked to the component's lifecycle and will be automatically stopped when the component is unmounted.

In other cases, it returns a stop handle which can be called to explicitly stop the watcher:

```js
const stop = watchEffect(() => {
  /* ... */
})

// later
stop()
```

### Side Effect Invalidation

Sometimes the watched effect function will perform asynchronous side effects that need to be cleaned up when it is invalidated (i.e. state changed before the effects can be completed). The effect function receives an `onInvalidate` function that can be used to register an invalidation callback. This invalidation callback is called when:

- the effect is about to re-run
- the watcher is stopped (i.e. when the component is unmounted if `watchEffect` is used inside `setup()` or lifecycle hooks)

```js
watchEffect(onInvalidate => {
  const token = performAsyncOperation(id.value)
  onInvalidate(() => {
    // id has changed or watcher is stopped.
    // invalidate previously pending async operation
    token.cancel()
  })
})
```

We are registering the invalidation callback via a passed-in function instead of returning it from the callback because the return value is important for async error handling. It is very common for the effect function to be an async function when performing data fetching:

```js
const data = ref(null)
watchEffect(async (onInvalidate) => {
  onInvalidate(() => { /* ... */ }) // we register cleanup function before Promise resolves
  data.value = await fetchData(props.id)
})
```

An async function implicitly returns a Promise, but the cleanup function needs to be registered immediately before the Promise resolves. In addition, Vue relies on the returned Promise to automatically handle potential errors in the Promise chain.

### Effect Flush Timing

Vue's reactivity system buffers invalidated effects and flushes them asynchronously to avoid unnecessary duplicate invocation when there are many state mutations happening in the same "tick". Internally, a component's `update` function is also a watched effect. When a user effect is queued, it is by default invoked **before** all component `update` effects

### Watcher Debugging

The `onTrack` and `onTrigger` options can be used to debug a watcher's behavior.

## `watch`

The `watch` API is the exact equivalent of the component [watch](https://v3.vuejs.org/guide/computed.html#watchers) property. `watch` requires watching a specific data source and applies side effects in a separate callback function. It also is lazy by default - i.e. the callback is only called when the watched source has changed.

Compared to `watchEffect`, `watch` allows us to:

- Perform the side effect lazily;
- Be more specific about what state should trigger the watcher to re-run;
- Access both the previous and current value of the watched state.

```js
// watching a getter
const state = reactive({ count: 0 })
watch(
  () => state.count,
  (count, prevCount) => {
    /* ... */
  }
)

// directly watching a ref
const count = ref(0)
watch(count, (count, prevCount) => {
  /* ... */
})
```

```js
const firstName = ref('');
const lastName= ref('');

watch([firstName, lastName], (newValues, prevValues) => {
  console.log(newValues, prevValues);
})

firstName.value = "John"; // logs: ["John",""] ["", ""]
lastName.value = "Smith"; // logs: ["John", "Smith"] ["John", ""]
```

`watch` shares behavior with `watchEffect` in terms of manual stoppage, side effect invalidation (with `onInvalidate` passed to the callback as the 3rd argument instead), flush timing and debugging.

# Change Detection Caveats in Vue 2

[Reactivity in Depth — Vue.js](https://vuejs.org/v2/guide/reactivity.html)

https://v3.vuejs.org/guide/change-detection.html
# Reactivity

https://v3.vuejs.org/guide/reactivity.html

https://www.vuemastery.com/courses/vue-3-reactivity/vue3-reactivity/

https://v3.vuejs.org/guide/reactivity-fundamentals.html

https://v3.vuejs.org/guide/reactivity-computed-watchers.html

https://www.youtube.com/watch?v=HezB8UEU5Rg

One of Vue’s most distinct features is the unobtrusive reactivity system. Models are proxied JavaScript objects. When you modify them, the view updates.

Reactivity is a programming paradigm that allows us to adjust to changes in a declarative manner. The canonical example that people usually show, because it’s a great one, is an **excel spreadsheet**.

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

So how would we do this in JavaScript?

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

# Change Detection Caveats in Vue 2

https://v3.vuejs.org/guide/change-detection.html
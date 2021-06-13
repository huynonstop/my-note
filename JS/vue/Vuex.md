https://vuex.vuejs.org/

https://scrimba.com/playlist/pnyzgAP

Vuex is a **state management pattern + library** for Vue.js applications. It serves as a centralized store for all the components in an application, with rules ensuring that the state can only be mutated in a predictable fashion

![image-20201110224306229](assets/Vuex/image-20201110224306229.png)

![image-20201110225213402](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20201110225213402.png)

# What is a "State Management Pattern"?

```js
const Counter = {
  // state the source of truth that drives our app;
  data () {
    return {
      count: 0
    }
  },
  // view a declarative mapping of the state
  template: `
    <div>{{ count }}</div>
  `,
  // actions the possible ways the state could change in reaction to user inputs from the view
  methods: {
    increment () {
      this.count++
    }
  }
}

createApp(Counter).mount('#app')
```

![image-20201110194551629](assets/Vuex/image-20201110194551629.png)

However, the simplicity quickly breaks down when we have **multiple components that share a common state**:

- Multiple views may depend on the same piece of state.
- Actions from different views may need to mutate the same piece of state.

> For **problem one**, passing props can be tedious for deeply nested components, and simply doesn't work for sibling components. For **problem two**, we often find ourselves resorting to solutions such as reaching for direct parent/child instance references or trying to mutate and synchronize multiple copies of the state via events. Both of these patterns are brittle and quickly lead to **unmaintainable code**.

So why don't we extract the shared state out of the components, and manage it in a global singleton? With this, our component tree becomes a big "view", and any component can access the state or trigger actions, no matter where they are in the tree!

![image-20201110195007073](assets/Vuex/image-20201110195007073.png)

This is the basic idea behind Vuex, inspired by [Flux](https://facebook.github.io/flux/docs/overview), [Redux](http://redux.js.org/) and [The Elm Architecture](https://guide.elm-lang.org/architecture/)

##  Simple State Management from Scratch

https://v3.vuejs.org/guide/state-management.html#official-flux-like-implementation

#  Installation

Vuex requires [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises).

```bash
yarn add vuex@next --save
```

# Getting Started

At the center of every Vuex application is the **store**. A "store" is basically a container that holds your application **state**. There are two things that make a Vuex store different from a plain global object:

1. Vuex stores are reactive. When Vue components retrieve state from it, they will reactively and efficiently update if the store's state changes.
2. You cannot directly mutate the store's state. The only way to change a store's state is by explicitly **committing mutations**. This ensures every state change leaves a track-able record, and enables tooling that helps us better understand our applications.

```js
import { createApp } from 'vue'
import { createStore } from 'vuex'

// Create a new store instance.
const store = createStore({
  state () {
    return {
      count: 1
    }
  }
})

const app = createApp({ /* your root component */ })

// Install the store instance as a plugin
app.use(store)

// Now, you can access the state object as store.state, and trigger a state change with the store.commit method:
store.commit('increment')
console.log(store.state.count) // -> 1

// In a Vue component, you can access the store as this.$store. Now we can commit a mutation using a component method:
methods: {
  increment() {
    this.$store.commit('increment')
    console.log(this.$store.state.count)
  }
}
```

#  State

## Single State Tree

Vuex uses a **single state tree** - that is, this single object contains all your application level state and serves as the "single source of truth." This also means usually you will have only one store for each application. A single state tree makes it straightforward to locate a specific piece of state, and allows us to easily take snapshots of the current app state for debugging purposes.

## Getting Vuex State into Vue Components

 Since Vuex stores are reactive, the simplest way to "retrieve" state from it is simply returning some store state from within a computed property:

```js
// let's create a Counter component
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return store.state.count
    }
  }
}
// Whenever store.state.count changes, it will cause the computed property to re-evaluate, and trigger associated DOM updates.
```

However, this pattern causes the component to rely on the global store singleton. When using a module system, it requires importing the store in every component that uses store state, and also requires mocking when testing the component.

Vuex "injects" the store into all child components from the root component through Vue's plugin system, and will be available on them as `this.$store`

```js
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return this.$store.state.count
    }
  }
}
```

## The `mapState` Helper

When a component needs to make use of multiple store state properties or getters, we can make use of the `mapState` helper which generates computed getter functions for us

```js
// in full builds helpers are exposed as Vuex.mapState
import { mapState } from 'vuex'

export default {
  // ...
  computed: mapState({
    // arrow functions can make the code very succinct!
    count: state => state.count,

    // passing the string value 'count' is same as `state => state.count`
    countAlias: 'count',

    // to access local state with `this`, a normal function must be used
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
}

// We can also pass a string array to mapState when the name of a mapped computed property is the same as a state sub tree name.
computed: mapState([
  // map this.count to store.state.count
  'count'
])

// combination with other local computed properties
computed: {
  localComputed () { /* ... */ },
  // mix this into the outer object with the object spread operator
  ...mapState({
    // ...
  })
}
```

# Getters

Sometimes we may need to compute derived state based on store state, for example filtering through a list of items and counting them:

```js
computed: {
  doneTodosCount () {
    return this.$store.state.todos.filter(todo => todo.done).length
  }
}
```

If more than one component needs to make use of this, we have to either duplicate the function, or extract it into a shared helper and import it in multiple places - both are less than ideal.

Vuex allows us to define "getters" in the store. You can think of them as computed properties for stores. Like computed properties, a getter's result is cached based on its dependencies, and will only re-evaluate when some of its dependencies have changed.

```js
const store = createStore({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos (state) { // Getters will receive the state as their 1st argument
      return state.todos.filter(todo => todo.done)
    }
  }
})
```

## Property-Style Access

The getters will be exposed on the `store.getters` object, and you access values as properties

```js
getters: {
  // ...
  doneTodosCount (state, getters) { // Getters will also receive other getters as the 2nd argument
    return getters.doneTodos.length
  }
}

// We can now easily make use of it inside any component:
computed: {
  doneTodosCount () {
    return this.$store.getters.doneTodosCount
  }
}
// or store instance
store.getters.doneTodosCount // -> 1
```

Note that getters accessed as properties are **cached** as part of Vue's reactivity system.

## Method-Style Access

```js
getters: {
  // ...
  getTodoById: (state) => (id) => { // You can also pass arguments to getters by returning a function.
    return state.todos.find(todo => todo.id === id)
  }
}

store.getters.getTodoById(2) // -> { id: 2, text: '...', done: false }
```

Note that getters accessed via methods will run each time you call them, and the **result is not cached**.

## The `mapGetters` Helper

```js
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
    // mix the getters into computed with object spread operator
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
}

// If you want to map a getter to a different name, use an object
...mapGetters({
  // map `this.doneCount` to `this.$store.getters.doneTodosCount`
  doneCount: 'doneTodosCount'
})
```

# Mutations

The only way to actually change state in a Vuex store is by committing a mutation. Vuex mutations are very similar to events: each mutation has a string **type** and a **handler**. The handler function is where we perform actual state modifications, and it will receive the state as the first argument

```js
const store = createStore({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {
      // mutate state
      state.count++
    }
  }
})
// You cannot directly call a mutation handler. Think of it more like event registration: "When a mutation with type increment is triggered, call this handler." To invoke a mutation handler, you need to call store.commit with its type:
store.commit('increment')

// Commit with Payload
// ...
mutations: {
  increment (state, n) {
    state.count += n
  }
}
store.commit('increment', 10)
// Or use an object payload
// ...
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
store.commit('increment', {
  amount: 10
})

// Object-Style Commit
// An alternative way to commit a mutation is by directly using an object that has a type property
store.commit({
  type: 'increment',
  amount: 10
})

// ...
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
```

Asynchronicity combined with state mutation can make your program very hard to reason about. For example, when you call two methods both with async callbacks that mutate the state, how do you know when they are called and which callback was called first?

In Vuex, **mutations are synchronous transactions**

## Using Constants for Mutation Types

```js
// mutation-types.js
export const SOME_MUTATION = 'SOME_MUTATION'
// store.js
import { createStore } from 'vuex'
import { SOME_MUTATION } from './mutation-types'

const store = createStore({
  state: { ... },
  mutations: {
    // we can use the ES2015 computed property name feature
    // to use a constant as the function name
    [SOME_MUTATION] (state) {
      // mutate state
    }
  }
})
```

## Mutations Must Be Synchronous

https://next.vuex.vuejs.org/guide/mutations.html#mutations-must-be-synchronous

One important rule to remember is that **mutation handler functions must be synchronous**. 

## Committing Mutations in Components

You can commit mutations in components with `this.$store.commit('xxx')`, or use the `mapMutations` helper which maps component methods to `store.commit` calls (requires root `store` injection)

```js
import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations([
      'increment', // map `this.increment()` to `this.$store.commit('increment')`

      // `mapMutations` also supports payloads:
      'incrementBy' // map `this.incrementBy(amount)` to `this.$store.commit('incrementBy', amount)`
    ]),
    ...mapMutations({
      add: 'increment' // map `this.add()` to `this.$store.commit('increment')`
    })
  }
}
```

#  Actions

Actions are similar to mutations, the differences being that:

- Instead of mutating the state, actions commit mutations.
- Actions can contain arbitrary asynchronous operations.

```js
const store = createStore({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
      context.commit('increment')
    }
  }
})
//ES6
actions: {
  increment ({ commit }) {
    commit('increment')
  }
}
```

Action handlers receive a context object which exposes the same set of methods/properties on the store instance, so you can call `context.commit` to commit a mutation, or access the state and getters via `context.state` and `context.getters`. We can even call other actions with `context.dispatch`.

**context object is not the store instance itself**

## Dispatching Actions

Actions are triggered with the `store.dispatch` method:

```js
store.dispatch('increment')
```

Remember that **mutations have to be synchronous**. Actions don't. We can perform **asynchronous** operations inside an action

```js
actions: {
  incrementAsync ({ commit }) {
    setTimeout(() => {
      commit('increment')
    }, 1000)
  }
}

// dispatch with a payload
store.dispatch('incrementAsync', {
  amount: 10
})

// dispatch with an object
store.dispatch({
  type: 'incrementAsync',
  amount: 10
})
```

```js
actions: {
  checkout ({ commit, state }, products) {
    // save the items currently in the cart
    const savedCartItems = [...state.cart.added]
    // send out checkout request, and optimistically
    // clear the cart
    commit(types.CHECKOUT_REQUEST)
    // the shop API accepts a success callback and a failure callback
    shop.buyProducts(
      products,
      // handle success
      () => commit(types.CHECKOUT_SUCCESS),
      // handle failure
      () => commit(types.CHECKOUT_FAILURE, savedCartItems)
    )
  }
}
```

Note we are performing a flow of asynchronous operations, and recording the side effects (state mutations) of the action by committing them.

## Dispatching Actions in Components

You can dispatch actions in components with `this.$store.dispatch('xxx')`, or use the `mapActions` helper which maps component methods to `store.dispatch` calls (requires root `store` injection):

```js
import { mapActions } from 'vuex'

export default {
  // ...
  methods: {
    ...mapActions([
      'increment', // map `this.increment()` to `this.$store.dispatch('increment')`

      // `mapActions` also supports payloads:
      'incrementBy' // map `this.incrementBy(amount)` to `this.$store.dispatch('incrementBy', amount)`
    ]),
    ...mapActions({
      add: 'increment' // map `this.add()` to `this.$store.dispatch('increment')`
    })
  }
}
```

## Composing Actions

Actions are often asynchronous, so how do we know when an action is done? And more importantly, how can we compose multiple actions together to handle more complex async flows?

The first thing to know is that `store.dispatch` can handle Promise returned by the triggered action handler and it also returns Promise:

```js
actions: {
  actionA ({ commit }) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        commit('someMutation')
        resolve()
      }, 1000)
    })
  }
}
```

Now you can do:

```js
store.dispatch('actionA').then(() => {
  // ...
})
```

And also in another action:

```js
actions: {
  // ...
  actionB ({ dispatch, commit }) {
    return dispatch('actionA').then(() => {
      commit('someOtherMutation')
    })
  }
}

// Aysnc await
// assuming `getData()` and `getOtherData()` return Promises
actions: {
  async actionA ({ commit }) {
    commit('gotData', await getData())
  },
  async actionB ({ dispatch, commit }) {
    await dispatch('actionA') // wait for `actionA` to finish
    commit('gotOtherData', await getOtherData())
  }
}
```

It's possible for a `store.dispatch` to trigger multiple action handlers in different modules. In such a case the returned value will be a Promise that resolves when all triggered handlers have been resolved.

# Modules

Due to using a single state tree, all states of our application are contained inside one big object. However, as our application grows in scale, the store can get really bloated.

Vuex allows us to divide our store into **modules**. Each module can contain its own state, mutations, actions, getters, and even nested modules - it's fractal all the way down

```js
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = createStore({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> `moduleA`'s state
store.state.b // -> `moduleB`'s state
```

## Module Local State

Inside a module's mutations and getters, the first argument received will be **the module's local state**.

```js
const moduleA = {
  state: () => ({
    count: 0
  }),
  mutations: {
    increment (state) {
      // `state` is the local module state
      state.count++
    }
  },
  getters: {
    doubleCount (state) {
      return state.count * 2
    }
  }
}
```

Also, inside module getters, the root state will be exposed as their 3rd argument:

```js
const moduleA = {
  // ...
  getters: {
    sumWithRootCount (state, getters, rootState) {
      return state.count + rootState.count
    }
  }
}
```

Similarly, inside module actions, `context.state` will expose the local state, and root state will be exposed as `context.rootState`:

```js
const moduleA = {
  // ...
  actions: {
    incrementIfOddOnRootSum ({ state, commit, rootState }) {
      if ((state.count + rootState.count) % 2 === 1) {
        commit('increment')
      }
    }
  }
}
```

## Namespacing

https://next.vuex.vuejs.org/guide/modules.html#namespacing

By default, actions, mutations and getters inside modules are still registered under the **global namespace** - this allows multiple modules to react to the same mutation/action type.

If you want your modules to be more self-contained or reusable, you can mark it as namespaced with `namespaced: true`. When the module is registered, all of its getters, actions and mutations will be automatically namespaced based on the path the module is registered at.

```js
const store = createStore({
  modules: {
    account: {
      namespaced: true,

      // module assets
      state: () => ({ ... }), // module state is already nested and not affected by namespace option
      getters: {
        isAdmin () { ... } // -> getters['account/isAdmin']
      },
      actions: {
        login () { ... } // -> dispatch('account/login')
      },
      mutations: {
        login () { ... } // -> commit('account/login')
      },

      // nested modules
      modules: {
        // inherits the namespace from parent module
        myPage: {
          state: () => ({ ... }),
          getters: {
            profile () { ... } // -> getters['account/profile']
          }
        },

        // further nest the namespace
        posts: {
          namespaced: true,

          state: () => ({ ... }),
          getters: {
            popular () { ... } // -> getters['account/posts/popular']
          }
        }
      }
    }
  }
})
```

Namespaced getters and actions will receive localized `getters`, `dispatch` and `commit`. In other words, you can use the module assets without writing prefix in the same module. Toggling between namespaced or not does not affect the code inside the module.

##  Dynamic Module Registration

You can register a module **after** the store has been created with the `store.registerModule` method:

```js
import { createStore } from 'vuex'

const store = createStore({ /* options */ })

// register a module `myModule`
store.registerModule('myModule', {
  // ...
})

// register a nested module `nested/myModule`
store.registerModule(['nested', 'myModule'], {
  // ...
})
```

The module's state will be exposed as `store.state.myModule` and `store.state.nested.myModule`.

> Dynamic module registration makes it possible for other Vue plugins to also leverage Vuex for state management by attaching a module to the application's store. For example, the [`vuex-router-sync`](https://github.com/vuejs/vuex-router-sync) library integrates vue-router with vuex by managing the application's route state in a dynamically attached module.

You can also remove a dynamically registered module with `store.unregisterModule(moduleName)`. Note you cannot remove static modules (declared at store creation) with this method.

Note that you may check if the module is already registered to the store or not via `store.hasModule(moduleName)` method.

### Preserving state

It may be likely that you want to preserve the previous state when registering a new module, such as preserving state from a Server Side Rendered app. You can achieve this with `preserveState` option: `store.registerModule('a', module, { preserveState: true })`

When you set `preserveState: true`, the module is registered, actions, mutations and getters are added to the store, but the state is not. It's assumed that your store state already contains state for that module and you don't want to overwrite it.

## Module Reuse

Sometimes we may need to create multiple instances of a module, for example:

- Creating multiple stores that use the same module (e.g. To [avoid stateful singletons in the SSR](https://ssr.vuejs.org/en/structure.html#avoid-stateful-singletons) when the `runInNewContext` option is `false` or `'once'`);
- Register the same module multiple times in the same store.

If we use a plain object to declare the state of the module, then that state object will be shared by reference and cause cross store/module state pollution when it's mutated.

This is actually the exact same problem with `data` inside Vue components. So the solution is also the same - use a function for declaring module state (supported in 2.3.0+):

```js
const MyReusableModule = {
  state: () => ({
    foo: 'bar'
  }),
  // mutations, actions, getters...
}
```

# Application Structure

Vuex doesn't really restrict how you structure your code. Rather, it enforces a set of high-level principles:

1. Application-level state is centralized in the store.
2. The only way to mutate the state is by committing **mutations**, which are synchronous transactions.
3. Asynchronous logic should be encapsulated in, and can be composed with **actions**.

As long as you follow these rules, it's up to you how to structure your project. If your store file gets too big, simply start splitting the actions, mutations and getters into separate files.

Sample:

```text
├── index.html
├── main.js
├── api
│   └── ... # abstractions for making API requests
├── components
│   ├── App.vue
│   └── ...
└── store
    ├── index.js          # where we assemble modules and export the store
    ├── actions.js        # root actions
    ├── mutations.js      # root mutations
    └── modules
        ├── cart.js       # cart module
        └── products.js   # products module
```

https://github.com/vuejs/vuex/tree/4.0/examples/classic/shopping-cart

# Composition API

https://next.vuex.vuejs.org/guide/composition-api.html

#  Plugins

https://next.vuex.vuejs.org/guide/plugins.html#plugins

# Strict Mode

To enable strict mode, simply pass in `strict: true` when creating a Vuex store:

```js
const store = createStore({
  // ...
  strict: true
})
```

In strict mode, whenever Vuex state is mutated outside of mutation handlers, an error will be thrown. This ensures that all state mutations can be explicitly tracked by debugging tools.

## Development vs. Production

**Do not enable strict mode when deploying for production!** Strict mode runs a synchronous deep watcher on the state tree for detecting inappropriate mutations, and it can be quite expensive when you make large amount of mutations to the state. Make sure to turn it off in production to avoid the performance cost.

Similar to plugins, we can let the build tools handle that:

```js
const store = createStore({
  // ...
  strict: process.env.NODE_ENV !== 'production'
})
```

# Form Handling

When using Vuex in strict mode, it could be a bit tricky to use `v-model` on a piece of state that belongs to Vuex:

```html
<input v-model="obj.message">
```

Assuming `obj` is a computed property that returns an Object from the store, the `v-model` here will attempt to directly mutate `obj.message` when the user types in the input. In strict mode, this will result in an error because the mutation is not performed inside an explicit Vuex mutation handler.

The "Vuex way" to deal with it is binding the `<input>`'s value and call a method on the `input` or `change` event:

```vue
<input :value="message" @input="updateMessage">
// ...
computed: {
  ...mapState({
    message: state => state.obj.message
  })
},
methods: {
  updateMessage (e) {
    this.$store.commit('updateMessage', e.target.value)
  }
}
// ...
mutations: {
  updateMessage (state, message) {
    state.obj.message = message
  }
}
```

## Two-way Computed Property

Admittedly, the above is quite a bit more verbose than `v-model` + local state, and we lose some of the useful features from `v-model` as well. An alternative approach is using a two-way computed property with a setter:

```vue
<input v-model="message">
// ...
computed: {
  message: {
    get () {
      return this.$store.state.obj.message
    },
    set (value) {
      this.$store.commit('updateMessage', value)
    }
  }
}
```

# Hot Reloading

https://next.vuex.vuejs.org/guide/hot-reload.html#dynamic-module-hot-reloading

# TypeScript Support

https://next.vuex.vuejs.org/guide/typescript-support.html#typescript-support

# Migrating to 4.0 from 3.x

https://next.vuex.vuejs.org/guide/migrating-to-4-0-from-3-x.html#migrating-to-4-0-from-3-x

## Installation process

```js
import { createStore } from 'vuex'

export const store = createStore({
  state () {
    return {
      count: 1
    }
  }
})

import { createApp } from 'vue'
import { store } from './store'
import App from './App.vue'

const app = createApp(App)

app.use(store)

app.mount('#app')
```

## `createLogger` function

```js
import { createLogger } from 'vuex'
```

## New `useStore` composition function

```js
import { useStore } from 'vuex'

export default {
  setup () {
    const store = useStore()
  }
}
```
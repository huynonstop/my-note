https://v3.vuejs.org/guide/composition-api-introduction.html

https://www.youtube.com/watch?v=bwItFdPt-6M

[Why the Composition API - Vue 3 Composition API | Vue Mastery](https://www.vuemastery.com/courses/vue-3-essentials/why-the-composition-api/)

[Composition API | Vue.js](https://v3.vuejs.org/api/composition-api.html)

[Reactivity API | Vue.js](https://v3.vuejs.org/api/reactivity-api.html)

![image-20201211231753281](assets/Vue-3-Composition-API/image-20201211231753281.png)

# limit of options api

![image-20200923135459550](assets/Vue-3-Composition-API/image-20200923135459550.png)

# Why Composition API?

[why-composition-api](https://v3.vuejs.org/guide/composition-api-introduction.html#why-composition-api)

# Basics of Composition API

## `setup` Component Option

The new `setup` component option is executed **before** the component is created, once the `props` are resolved, and serves as the entry point for composition API's.

Because the component instance is not yet created when `setup` is executed, there is no `this` inside a `setup` option. This means, with the exception of `props`, you won't be able to access any properties declared in the component – **local state**, **computed properties** or **methods**.

The `setup` option should be a function that accepts `props` and `context`

Additionally, everything that we return from `setup` will be exposed to the rest of our component (computed properties, methods, lifecycle hooks and so on) as well as to the component's template.

```js
// src/components/UserRepositories.vue

export default {
  components: { RepositoriesFilters, RepositoriesSortBy, RepositoriesList },
  props: {
    user: {
      type: String,
      required: true
    }
  },
  setup(props) {
    console.log(props) // { user: '' }

    return {} // anything returned here will be available for the rest of the component
  }
  // the "rest" of the component
}
```

This component has several responsibilities:

1. Getting repositories from a presumedly external API for that user name and refreshing it whenever the user changes
2. Searching for repositories using a `searchQuery` string
3. Filtering repositories using a `filters` object

## Reactive Variables with `ref`

In Vue 3.0 we can make any variable reactive anywhere with a new `ref` function, like this:

```js
import { ref } from 'vue'

const counter = ref(0)
// ref takes the argument and returns it wrapped within an object with a value property, which can then be used to access or mutate the value of the reactive variable

console.log(counter) // { value: 0 }
console.log(counter.value) // 0

counter.value++
console.log(counter.value) // 1
// Having a wrapper object around any value allows us to safely pass it across our whole app without worrying about losing its reactivity somewhere along the way
```

In other words, `ref` creates a **Reactive Reference** to our value. The concept of working with **References** will be used often throughout the Composition API.

```js
// src/components/UserRepositories.vue `setup` function
import { fetchUserRepositories } from '@/api/repositories'
import { ref } from 'vue'

// in our component
setup (props) {
  const repositories = ref([])
  const getUserRepositories = async () => {
    repositories.value = await fetchUserRepositories(props.user)
  }

  return {
    repositories,
    getUserRepositories
  }
}
// Done! Now whenever we call getUserRepositories, repositories will be mutated and the view will be updated to reflect the change.
```

## Lifecycle Hook Registration Inside `setup`

This is possible thanks to several new functions exported from Vue. Lifecycle hooks on composition API have the same name as for Options API but are prefixed with `on`

These functions accept a callback that will be executed when the hook is called by the component.

```js
// src/components/UserRepositories.vue `setup` function
import { fetchUserRepositories } from '@/api/repositories'
import { ref, onMounted } from 'vue'

// in our component
setup (props) {
  const repositories = ref([])
  const getUserRepositories = async () => {
    repositories.value = await fetchUserRepositories(props.user)
  }

  onMounted(getUserRepositories) // on `mounted` call `getUserRepositories`

  return {
    repositories,
    getUserRepositories
  }
}
```

## Reacting to Changes with `watch`

Just like how we set up a watcher on the `user` property inside our component using the `watch` option, we can do the same using the `watch` function imported from Vue. It accepts 3 arguments:

- A **Reactive Reference** or getter function that we want to watch
- A callback
- Optional configuration options

```js
import { ref, watch } from 'vue'

const counter = ref(0)
watch(counter, (newValue, oldValue) => {
  console.log('The new counter value is: ' + counter.value)
})

// Options API
export default {
  data() {
    return {
      counter: 0
    }
  },
  watch: {
    counter(newValue, oldValue) {
      console.log('The new counter value is: ' + this.counter)
    }
  }
}
```

```js
// src/components/UserRepositories.vue `setup` function
import { fetchUserRepositories } from '@/api/repositories'
import { ref, onMounted, watch, toRefs } from 'vue'

// in our component
setup (props) {
  // using `toRefs` to create a Reactive Reference to the `user` property of props
  const { user } = toRefs(props)

  const repositories = ref([])
  const getUserRepositories = async () => {
    // update `props.user` to `user.value` to access the Reference value
    repositories.value = await fetchUserRepositories(user.value)
  }

  onMounted(getUserRepositories)

  // set a watcher on the Reactive Reference to user prop
  watch(user, getUserRepositories)

  return {
    repositories,
    getUserRepositories
  }
}
```

## Standalone `computed` properties

Similar to `ref` and `watch`, computed properties can also be created outside of a Vue component with the `computed` function imported from Vue. Let’s get back to our counter example:

```js
import { ref, computed } from 'vue'

const counter = ref(0)
const twiceTheCounter = computed(() => counter.value * 2)

counter.value++
console.log(counter.value) // 1
console.log(twiceTheCounter.value) // 2
```

Here, the computed function returns a **read-only Reactive Reference** to the output of the getter-like callback passed as the first argument to computed. In order to access the value of the newly-created computed variable, we need to use the .value property just like with ref.

```js
// src/components/UserRepositories.vue `setup` function
import { fetchUserRepositories } from '@/api/repositories'
import { ref, onMounted, watch, toRefs, computed } from 'vue'

// in our component
setup (props) {
  // using `toRefs` to create a Reactive Reference to the `user` property of props
  const { user } = toRefs(props)

  const repositories = ref([])
  const getUserRepositories = async () => {
    // update `props.user` to `user.value` to access the Reference value
    repositories.value = await fetchUserRepositories(user.value)
  }

  onMounted(getUserRepositories)

  // set a watcher on the Reactive Reference to user prop
  watch(user, getUserRepositories)

  const searchQuery = ref('')
  const repositoriesMatchingSearchQuery = computed(() => {
    return repositories.value.filter(
      repository => repository.name.includes(searchQuery.value)
    )
  })

  return {
    repositories,
    getUserRepositories,
    searchQuery,
    repositoriesMatchingSearchQuery
  }
}
```

## composition function

```js
// src/composables/useUserRepositories.js

import { fetchUserRepositories } from '@/api/repositories'
import { ref, onMounted, watch } from 'vue'

export default function useUserRepositories(user) {
  const repositories = ref([])
  const getUserRepositories = async () => {
    repositories.value = await fetchUserRepositories(user.value)
  }

  onMounted(getUserRepositories)
  watch(user, getUserRepositories)

  return {
    repositories,
    getUserRepositories
  }
}
```

```js
// src/composables/useRepositoryNameSearch.js

import { ref, computed } from 'vue'

export default function useRepositoryNameSearch(repositories) {
  const searchQuery = ref('')
  const repositoriesMatchingSearchQuery = computed(() => {
    return repositories.value.filter(repository => {
      return repository.name.includes(searchQuery.value)
    })
  })

  return {
    searchQuery,
    repositoriesMatchingSearchQuery
  }
}
```

```js
// src/components/UserRepositories.vue
import { toRefs } from 'vue'
import useUserRepositories from '@/composables/useUserRepositories'
import useRepositoryNameSearch from '@/composables/useRepositoryNameSearch'
import useRepositoryFilters from '@/composables/useRepositoryFilters'

export default {
  components: { RepositoriesFilters, RepositoriesSortBy, RepositoriesList },
  props: {
    user: {
      type: String,
      required: true
    }
  },
  setup(props) {
    const { user } = toRefs(props)

    const { repositories, getUserRepositories } = useUserRepositories(user)

    const {
      searchQuery,
      repositoriesMatchingSearchQuery
    } = useRepositoryNameSearch(repositories)

    const {
      filters,
      updateFilters,
      filteredRepositories
    } = useRepositoryFilters(repositoriesMatchingSearchQuery)

    return {
      // Since we don’t really care about the unfiltered repositories
      // we can expose the end results under the `repositories` name
      repositories: filteredRepositories,
      getUserRepositories,
      searchQuery,
      filters,
      updateFilters
    }
  }
}
```

#  Setup

## Arguments

When using the `setup` function, it will take two arguments:

### Props

`props` inside of a `setup` function are reactive and will be updated when new props are passed in.

```js
// MyBook.vue

export default {
  props: {
    title: String
  },
  setup(props) {
    console.log(props.title)
  }
}
```

However, because `props` are reactive, you **cannot use ES6 destructuring** because it will remove props reactivity.

If you need to destructure your props, you can do this by utilizing the `toRefs` inside of the setup function:

```js
// MyBook.vue

import { toRefs } from 'vue'

setup(props) {
	const { title } = toRefs(props)

	console.log(title.value)
}
```

If `title` is an optional prop, it could be missing from `props`. In that case, `toRefs` won't create a ref for `title`. Instead you'd need to use `toRef`:

```js
// MyBook.vue

import { toRef } from 'vue'

setup(props) {
	const title = toRef(props, 'title')

	console.log(title.value)
}
```

### Context

The `context` is a normal JavaScript object that exposes three component properties:

```js
// MyBook.vue

export default {
  setup(props, context) {
    // Attributes (Non-reactive object)
    console.log(context.attrs)

    // Slots (Non-reactive object)
    console.log(context.slots)

    // Emit Events (Method)
    console.log(context.emit)
  }
}
// The context object is a normal JavaScript object, i.e., it is not reactive, this means you can safely use ES6 destructuring on context.

// MyBook.vue
export default {
  setup(props, { attrs, slots, emit }) {
    ...
  }
}
```

`attrs` and `slots` are stateful objects that are always updated when the component itself is updated. This means you should avoid destructuring them and always reference properties as `attrs.x` or `slots.x`. Also note that unlike `props`, `attrs` and `slots` are **not** reactive. If you intend to apply side effects based on `attrs` or `slots` changes, you should do so inside an `onUpdated` lifecycle hook.

#### Emits

```js
export default {
  setup(props, { attrs, slots, emit }) {
    emit('custome-event') // this.$emit('custome-event')
  }
}
```



## Accessing Component Properties

When `setup` is executed, the component instance has not been created yet. As a result, you will only be able to access the following properties:

- `props`
- `attrs`
- `slots`
- `emit`

In other words, you **will not have access** to the following component options:

- `data`
- `computed`
- `methods`

## Usage with Templates

If `setup` returns an object, the properties on the object can be accessed in the component's template, as well as the properties of the `props` passed into `setup`:

```vue
<!-- MyBook.vue -->
<template>
  <div>{{ collectionName }}: {{ readersNumber }} {{ book.title }}</div>
</template>

<script>
  import { ref, reactive } from 'vue'

  export default {
    props: {
      collectionName: String
    },
    setup(props) {
      const readersNumber = ref(0)
      const book = reactive({ title: 'Vue 3 Guide' })

      // expose to template
      return {
        readersNumber,
        book
      }
    }
  }
  // Note that refs returned from setup are automatically unwrapped when accessed in the template so you shouldn't use .value in templates.
</script>
```

## Usage with Render Functions

`setup` can also return a render function which can directly make use of the reactive state declared in the same scope:

```js
// MyBook.vue

import { h, ref, reactive } from 'vue'

export default {
  setup() {
    const readersNumber = ref(0)
    const book = reactive({ title: 'Vue 3 Guide' })
    // Please note that we need to explicitly expose ref value here
    return () => h('div', [readersNumber.value, book.title])
  }
}
```

## Usage of `this`

**Inside `setup()`, `this` won't be a reference to the current active instance** Since `setup()` is called before other component options are resolved, `this` inside `setup()` will behave quite differently from `this` in other options. This might cause confusions when using `setup()` along other Options API.

# Lifecycle Hooks

![image-20201209135752214](assets/Vue-3-Composition-API/image-20201209135752214.png)

Because `setup` is run around the `beforeCreate` and `created` lifecycle hooks, you do not need to explicitly define them. In other words, any code that would be written inside those hooks should be written directly in the `setup` function.

These functions accept a callback that will be executed when the hook is called by the component:

```js
// MyBook.vue

export default {
  setup() {
    // mounted
    onMounted(() => {
      console.log('Component is mounted!')
    })
  }
}
```

# Provide / Inject

##  Scenario Background

Let's assume that we want to rewrite the following code, which contains a `MyMap` component that provides a `MyMarker` component with the user's location, using the Composition API.

```vue
<!-- src/components/MyMap.vue -->
<template>
  <MyMarker />
</template>

<script>
import MyMarker from './MyMarker.vue'

export default {
  components: {
    MyMarker
  },
  provide: {
    location: 'North Pole',
    geolocation: {
      longitude: 90,
      latitude: 135
    }
  }
}
</script>
<!-- src/components/MyMarker.vue -->
<script>
export default {
  inject: ['location', 'geolocation']
}
</script>
```

## Using Provide

When using `provide` in `setup()`, we start by explicitly importing the method from `vue`. This allows us to define each property with its own invocation of `provide`.

The `provide` function allows you to define the property through two parameters:

1. The property's name (`<String>` type)
2. The property's value

Using our `MyMap` component, our provided values can be refactored as the following:

```vue
<!-- src/components/MyMap.vue -->
<template>
  <MyMarker />
</template>

<script>
import { provide } from 'vue'
import MyMarker from './MyMarker.vue

export default {
  components: {
    MyMarker
  },
  setup() {
    provide('location', 'North Pole')
    provide('geolocation', {
      longitude: 90,
      latitude: 135
    })
  }
}
</script>
```

## Using Inject

When using `inject` in `setup()`, we also need to explicitly import it from `vue`. Once we do so, this allows us to invoke it to define how we want to expose it to our component.

The `inject` function takes two parameters:

1. The name of the property to inject
2. A default value (**Optional**)

Using our `MyMarker` component, we can refactor it with the following code:

```vue
<!-- src/components/MyMarker.vue -->
<script>
import { inject } from 'vue'

export default {
  setup() {
    const userLocation = inject('location', 'The Universe')
    const userGeolocation = inject('geolocation')

    return {
      userLocation,
      userGeolocation
    }
  }
}
</script>
```

## Reactivity

### Adding Reactivity

To add reactivity between provided and injected values, we can use a ref or reactive when providing a value.

Using our `MyMap` component, our code can be updated as follows:

```vue
<!-- src/components/MyMap.vue -->
<template>
  <MyMarker />
</template>

<script>
import { provide, reactive, ref } from 'vue'
import MyMarker from './MyMarker.vue

export default {
  components: {
    MyMarker
  },
  setup() {
    const location = ref('North Pole')
    const geolocation = reactive({
      longitude: 90,
      latitude: 135
    })

    provide('location', location)
    provide('geolocation', geolocation)
  }
}
</script>
```

Now, if anything changes in either property, the `MyMarker` component will automatically be updated as well!

### Mutating Reactive Properties

When using reactive provide / inject values, **it is recommended to keep any mutations to reactive properties inside of the \*provider\* whenever possible**.

For example, in the event we needed to change the user's location, we would ideally do this inside of our `MyMap` component.

```vue
<!-- src/components/MyMap.vue -->
<template>
  <MyMarker />
</template>

<script>
import { provide, reactive, ref } from 'vue'
import MyMarker from './MyMarker.vue

export default {
  components: {
    MyMarker
  },
  setup() {
    const location = ref('North Pole')
    const geolocation = reactive({
      longitude: 90,
      latitude: 135
    })

    provide('location', location)
    provide('geolocation', geolocation)

    return {
      location
    }
  },
  methods: {
    updateLocation() {
      this.location = 'South Pole'
    }
  }
}
</script>
```

However, there are times where we need to update the data inside of the component where the data is injected. In this scenario, we recommend providing a method that is responsible for mutating the reactive property.

```vue
<!-- src/components/MyMap.vue -->
<template>
  <MyMarker />
</template>

<script>
import { provide, reactive, ref } from 'vue'
import MyMarker from './MyMarker.vue

export default {
  components: {
    MyMarker
  },
  setup() {
    const location = ref('North Pole')
    const geolocation = reactive({
      longitude: 90,
      latitude: 135
    })

    const updateLocation = () => {
      location.value = 'South Pole'
    }

    provide('location', location)
    provide('geolocation', geolocation)
    provide('updateLocation', updateLocation)
  }
}
</script>
<!-- src/components/MyMarker.vue -->
<script>
import { inject } from 'vue'

export default {
  setup() {
    const userLocation = inject('location', 'The Universe')
    const userGeolocation = inject('geolocation')
    const updateUserLocation = inject('updateLocation')

    return {
      userLocation,
      userGeolocation,
      updateUserLocation
    }
  }
}
</script>
```

Finally, we recommend using `readonly` on provided property if you want to ensure that the data passed through `provide` cannot be mutated by the injected component.

```vue
<!-- src/components/MyMap.vue -->
<template>
  <MyMarker />
</template>

<script>
import { provide, reactive, readonly, ref } from 'vue'
import MyMarker from './MyMarker.vue

export default {
  components: {
    MyMarker
  },
  setup() {
    const location = ref('North Pole')
    const geolocation = reactive({
      longitude: 90,
      latitude: 135
    })

    const updateLocation = () => {
      location.value = 'South Pole'
    }

    provide('location', readonly(location))
    provide('geolocation', readonly(geolocation))
    provide('updateLocation', updateLocation)
  }
}
</script>
```

#  Template Refs

When using the Composition API, the concept of reactive refs and template refs are unified. In order to obtain a reference to an in-template element or component instance, we can declare a ref as usual and return it from setup():

```html
<template>
  <div ref="root">This is a root element</div>
</template>

<script>
  import { ref, onMounted } from 'vue'

  export default {
    setup() {
      const root = ref(null)

      onMounted(() => {
        // the DOM element will be assigned to the ref after initial render
        console.log(root.value) // <div>This is a root element</div>
      })

      return {
        root
      }
    }
  }
</script>
```

Here we are exposing `root` on the render context and binding it to the div as its ref via `ref="root"`. In the Virtual DOM patching algorithm, if a VNode's `ref` key corresponds to a ref on the render context, the VNode's corresponding element or component instance will be assigned to the value of that ref. This is performed during the Virtual DOM mount / patch process, so template refs will only get assigned values after the initial render.

Refs used as templates refs behave just like any other refs: they are reactive and can be passed into (or returned from) composition functions.

## Usage with JSX

```js
export default {
  setup() {
    const root = ref(null)

    return () =>
      h('div', {
        ref: root
      })

    // with JSX
    return () => <div ref={root} />
  }
}
```

## Usage inside `v-for`

Composition API template refs do not have special handling when used inside `v-for`. Instead, use function refs to perform custom handling:

```html
<template>
  <div v-for="(item, i) in list" :ref="el => { if (el) divs[i] = el }">
    {{ item }}
  </div>
</template>

<script>
  import { ref, reactive, onBeforeUpdate } from 'vue'

  export default {
    setup() {
      const list = reactive([1, 2, 3])
      const divs = ref([])

      // make sure to reset the refs before each update
      onBeforeUpdate(() => {
        divs.value = []
      })

      return {
        list,
        divs
      }
    }
  }
</script>
```

# Custom hooks

![image-20201211233617951](assets/Vue-3-Composition-API/image-20201211233617951.png)

![image-20201211233631674](assets/Vue-3-Composition-API/image-20201211233631674.png)

# Router

## props params

![image-20201211230917426](assets/Vue-3-Composition-API/image-20201211230917426.png)

## Router & route object

```js
import { useRoute, useRouter } from 'vue-router'
```

# Vuex

```js
import { useStore } from 'vuex'
```



# Migrate from V2

![image-20201211224026360](assets/Vue-3-Composition-API/image-20201211224026360.png)

![image-20201211224053833](assets/Vue-3-Composition-API/image-20201211224053833.png)
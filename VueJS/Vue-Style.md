https://v3.vuejs.org/style-guide/

# Priority A Rules: Essential (Error Prevention)

## Multi-word component names

This prevents conflicts with existing and future HTML elements, since all HTML elements are a single word.

```js
app.component('todo-item', {
  // ...
})
```

```js
export default {
  name: 'TodoItem',
  // ...
}
```

## Prop definitions detail

In committed code, prop definitions should always be as detailed as possible, specifying at least type(s).

## Keyed `v-for`

https://v3.vuejs.org/style-guide/#keyed-v-for-essential

**Always use `key` with `v-for`.**

`key` with `v-for` is *always* required on components, in order to maintain internal component state down the subtree. Even for elements though, it's a good practice to maintain predictable behavior, such as [object constancy](https://bost.ocks.org/mike/constancy/) in animations.

## Avoid `v-if` with `v-for`

**Never use `v-if` on the same element as `v-for`.**

There are two common cases where this can be tempting:

- To filter items in a list (e.g. `v-for="user in users" v-if="user.isActive"`). In these cases, replace `users` with a new computed property that returns your filtered list (e.g. `activeUsers`).
- To avoid rendering a list if it should be hidden (e.g. `v-for="user in users" v-if="shouldShowUsers"`). In these cases, move the `v-if` to a container element (e.g. `ul`, `ol`).

## Component style scoping

https://v3.vuejs.org/style-guide/#component-style-scoping-essential

## Private property names

https://v3.vuejs.org/style-guide/#private-property-names-essential

# Priority B Rules: Strongly Recommended (Improving Readability)

# Priority C Rules: Recommended (Minimizing Arbitrary Choices and Cognitive Overhead)

# Priority D Rules: Use with Caution (Potentially Dangerous Patterns)
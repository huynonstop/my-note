# Overview

https://v3.vuejs.org/guide/transitions-overview.html#performance

https://v3.vuejs.org/guide/transitions-overview.html#timing

https://v3.vuejs.org/guide/transitions-overview.html#easing

Designing Interface Animation: Improving the User Experience Through Animation Kindle Edition by Val Head (Author): https://www.amazon.com/dp/B01J4NKSZA/

ANIMATION AT WORK by Rachel Nabors: https://abookapart.com/products/animation-at-work

Vue offers some abstractions that can help work with transitions and animations, particularly in response to something changing. Some of these abstractions include:

- Hooks for components entering and leaving the DOM, in both CSS and JS, using the built-in `<transition>` component.
- Transition Modes so that you can orchestrate ordering during a transition.
- Hooks for when multiple elements are updating in position, with FLIP techniques applied under the hood to increase performance, using the `<transition-group>` component.
- Transitioning different states in an application, with `watchers`.

## Class-based Animations & Transitions

you can also activate an animation without mounting a component, by adding a conditional class.

```html
<div id="demo">
  Push this button to do something you shouldn't be doing:<br />

  <div :class="{ shake: noActivated }">
    <button @click="noActivated = true">Click me</button>
    <span v-if="noActivated">Oh no!</span>
  </div>
</div>
```

```css
.shake {
  animation: shake 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
  transform: translate3d(0, 0, 0);
  backface-visibility: hidden;
  perspective: 1000px;
}

@keyframes shake {
  10%,
  90% {
    transform: translate3d(-1px, 0, 0);
  }

  20%,
  80% {
    transform: translate3d(2px, 0, 0);
  }

  30%,
  50%,
  70% {
    transform: translate3d(-4px, 0, 0);
  }

  40%,
  60% {
    transform: translate3d(4px, 0, 0);
  }
}
```

```js
const Demo = {
  data() {
    return {
      noActivated: false
    }
  }
}

Vue.createApp(Demo).mount('#demo')
```

## Transitions with Style Bindings

Some transition affects can be applied by interpolating values, for instance by binding a style to an element while an interaction occurs. Take this example for instance:

```html
<div id="demo">
  <div
    @mousemove="xCoordinate"
    :style="{ backgroundColor: `hsl(${x}, 80%, 50%)` }"
    class="movearea"
  >
    <h3>Move your mouse across the screen...</h3>
    <p>x: {{x}}</p>
  </div>
</div>
```

```css
.movearea {
  transition: 0.2s background-color ease;
}
```

```js
const Demo = {
  data() {
    return {
      x: 0
    }
  },
  methods: {
    xCoordinate(e) {
      this.x = e.clientX
    }
  }
}

Vue.createApp(Demo).mount('#demo')
```

# Enter & Leave Transitions

Vue provides a variety of ways to apply transition effects when items are inserted, updated, or removed from the DOM. This includes tools to:

- automatically apply classes for CSS transitions and animations
- integrate 3rd-party CSS animation libraries, such as [Animate.css](https://animate.style/)
- use JavaScript to directly manipulate the DOM during transition hooks
- integrate 3rd-party JavaScript animation libraries

https://v3.vuejs.org/guide/transitions-enterleave.html

# List Transitions

So what about for when we have a whole list of items we want to render simultaneously, for example with `v-for`? In this case, we'll use the `<transition-group>` component. Before we dive into an example though, there are a few things that are important to know about this component:

- Unlike `<transition>`, it renders an actual element: a `<span>` by default. You can change the element that's rendered with the `tag` attribute.
- Transition modes are not available, because we are no longer alternating between mutually exclusive elements.
- Elements inside are **always required** to have a unique `key` attribute.
- CSS transition classes will be applied to inner elements and not to the group/container itself.

https://v3.vuejs.org/guide/transitions-list.html#list-transitions

# State Transitions

Vue's transition system offers many simple ways to animate entering, leaving, and lists, but what about animating your data itself? For example:

- numbers and calculations
- colors displayed
- the positions of SVG nodes
- the sizes and other properties of elements

All of these are either already stored as raw numbers or can be converted into numbers. Once we do that, we can animate these state changes using 3rd-party libraries to tween state, in combination with Vue's reactivity and component systems.

https://v3.vuejs.org/guide/transitions-state.html
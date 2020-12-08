# Routing

 [Vue-routing.md](Vue-routing.md) 

# State Management

https://v3.vuejs.org/guide/state-management.html#state-management

https://vuex.vuejs.org/

# Server-Side Rendering

https://ssr.vuejs.org/

https://nuxtjs.org/

https://quasar.dev/

# Material Design Framework

https://vuetifyjs.com/en/

https://academind.com/learn/firebase/a-comprehensive-project-with-vuetify-firebase/

# Vue-powered Static Site Generator

https://vuepress.vuejs.org/

# Vue CLI

https://cli.vuejs.org/

https://www.youtube.com/watch?v=xht56VLMCY8

# Vue loader

https://vue-loader.vuejs.org/

## Scoped CSS

https://vue-loader.vuejs.org/guide/scoped-css.html

When a `<style>` tag has the `scoped` attribute, its CSS will apply to elements of the current component only. 

This is similar to the style encapsulation found in Shadow DOM

It is achieved by using PostCSS to transform the following:

```html
<style scoped>
.example {
  color: red;
}
</style>

<template>
  <div class="example">hi</div>
</template>
```

Into the following:

```html
<style>
.example[data-v-f3f3eg9] {
  color: red;
}
</style>

<template>
  <div class="example" data-v-f3f3eg9>hi</div>
</template>
```

## CSS module

https://vue-loader.vuejs.org/guide/css-modules.html

# Vue Devtools

https://github.com/vuejs/vue-devtools

# Vue installing

https://v3.vuejs.org/guide/installation.html

# Vue awesome

https://github.com/vuejs/awesome-vue

# Vetur

https://www.youtube.com/watch?v=6G-YI8lSmz8

# Vite

https://www.youtube.com/watch?v=xXrhg26VCSc
# Introduction

https://v3.vuejs.org/guide/routing.html

https://next.router.vuejs.org/guide

https://next.router.vuejs.org/api/

https://www.youtube.com/watch?v=0whOTp7nH5U

Features include:

- Nested routes mapping
- Dynamic Routing
- Modular, component-based router configuration
- Route params, query, wildcards
- View transition effects powered by Vue.js' transition system
- Fine-grained navigation control
- Links with automatic active CSS classes
- HTML5 history mode or hash mode
- Customizable Scroll Behavior
- Proper encoding for URLs

# Installation

## Direct Download / CDN

https://unpkg.com/vue-router@next

## npm

```bash
npm install vue-router@next
```

#  Getting Started

```html
<script src="https://unpkg.com/vue@next"></script>
<script src="https://unpkg.com/vue-router@next"></script>

<div id="app">
  <h1>Hello App!</h1>
  <p>
    <!-- use the router-link component for navigation. -->
    <!-- specify the link by passing the `to` prop. -->
    <!-- `<router-link>` will render an `<a>` tag with the correct `href` attribute -->
    <router-link to="/">Go to Home</router-link>
    <router-link to="/about">Go to About</router-link>
  </p>
  <!-- route outlet -->
  <!-- component matched by the route will render here -->
  <router-view></router-view>
</div>
```

`router-link`
Note how instead of using regular `a` tags, we use a custom component `router-link` to create links. This allows Vue Router to change the URL without reloading the page, handle URL generation as well as its encoding. We will see later how to benefit from these features.

`router-view`
`router-view` will display the component that corresponds to the url. You can put it anywhere to adapt it to your layout.

```js
// 1. Define route components.
// These can be imported from other files
const Home = { template: '<div>Home</div>' }
const About = { template: '<div>About</div>' }

// 2. Define some routes
// Each route should map to a component.
// We'll talk about nested routes later.
const routes = [
  { path: '/', component: Home },
  { path: '/about', component: About },
]

// 3. Create the router instance and pass the `routes` option
// You can pass in additional options here, but let's
// keep it simple for now.
const router = VueRouter.createRouter({
  // 4. Provide the history implementation to use. We are using the hash history for simplicity here.
  history: VueRouter.createWebHashHistory(),
  routes, // short for `routes: routes`
})

// 5. Create and mount the root instance.
const app = Vue.createApp({})
// Make sure to _use_ the router instance to make the
// whole app router-aware.
app.use(router)

app.mount('#app')

// Now the app has started!
```

By calling `app.use(router)`, we get access to it as `this.$router` as well as the current route as `this.$route` inside of any component:

```js
// Home.vue
export default {
  computed: {
    username() {
      // We will see what `params` is shortly
      return this.$route.params.username
    },
  },
  methods: {
    goToDashboard() {
      if (isAuthenticated) {
        this.$router.push('/dashboard')
      } else {
        this.$router.push('/login')
      }
    },
  },
}
```

To access the router or the route inside the `setup` function, call the `useRouter` or `useRoute` functions

https://next.router.vuejs.org/guide/advanced/composition-api.html#accessing-the-router-and-current-route-inside-setup

Keep in mind that `this.$router` is exactly the same as directly using the `router` instance created through `createRouter`. The reason we use `this.$router` is because we don't want to import the router in every single component that needs to manipulate routing.

# Dynamic Route Matching with Params

https://next.router.vuejs.org/api/#routelocationnormalized

https://codesandbox.io/s/route-params-vue-router-examples-mlb14?from-embed&initialpath=%2Fusers%2Feduardo%2Fposts%2F1

Very often we will need to map routes with the given pattern to the same component. For example we may have a `User` component which should be rendered for all users but with different user IDs. In Vue Router we can use a dynamic segment in the path to achieve that, we call that a *param*

```js
const User = {
  template: '<div>User</div>',
}

// these are passed to `createRouter`
const routes = [
  // dynamic segments start with a colon
  { path: '/users/:id', component: User },
]
```

Now URLs like `/users/johnny` and `/users/jolyne` will both map to the same route.

A *param* is denoted by a colon `:`. When a route is matched, the value of its *params* will be exposed as `this.$route.params` in every component.

```js
const User = {
  template: '<div>User {{ $route.params.id }}</div>',
}
```

You can have multiple *params* in the same route, and they will map to corresponding fields on `$route.params`. Examples:

| pattern                        | matched path             | $route.params                            |
| ------------------------------ | ------------------------ | ---------------------------------------- |
| /users/:username               | /users/eduardo           | `{ username: 'eduardo' }`                |
| /users/:username/posts/:postId | /users/eduardo/posts/123 | `{ username: 'eduardo', postId: '123' }` |

In addition to `$route.params`, the `$route` object also exposes other useful information such as `$route.query` (if there is a query in the URL), `$route.hash`

## Reacting to Params Changes

when using routes with params is that when the user navigates from `/users/johnny` to `/users/jolyne`, **the same component instance will be reused**. Since both routes render the same component, this is more efficient than destroying the old instance and then creating a new one. **However, this also means that the lifecycle hooks of the component will not be called**.

To react to params changes in the same component, you can simply watch anything on the `$route` object, in this scenario, the `$route.params`:

```js
const User = {
  template: '...',
  created() {
    this.$watch(
      () => this.$route.params,
      (toParams, previousParams) => {
        // react to route changes...
      }
    )
  },
}
```

Or, use the `beforeRouteUpdate`navigation guard, which also allows to cancel the navigation:

```js
const User = {
  template: '...',
  async beforeRouteUpdate(to, from) {
    // react to route changes...
    this.userData = await fetchUser(to.params.id)
  },
}
```

## Catch all / 404 Not found Route

Regular params will only match characters in between url fragments, separated by `/`. If we want to match **anything**, we can use a custom *param* regexp by adding the regexp inside parentheses right after the *param*

```js
const routes = [
  // will match everything and put it under `$route.params.pathMatch`
  { path: '/:pathMatch(.*)*', name: 'NotFound', component: NotFound },
  // will match anything starting with `/user-` and put it under `$route.params.afterUser`
  { path: '/user-:afterUser(.*)', component: UserGeneric },
]
```

In this specific scenario we are using a [custom regexp](https://next.router.vuejs.org/guide/advanced/path-matching.html#custom-regexp) between parentheses and marking the `pathMatch` param as [optionally repeatable](https://next.router.vuejs.org/guide/advanced/path-matching.html#zero-or-more). This is to allows us to directly navigate to the route if we need to by splitting the `path` into an array

```js
this.$router.push({
  name: 'NotFound',
  params: { pathMatch: this.$route.path.split('/') },
})
```

See more in the [repeated params](https://next.router.vuejs.org/guide/advanced/path-matching.html#zero-or-more) section.

## Advanced Matching Patterns

https://next.router.vuejs.org/guide/essentials/route-matching-syntax.html#custom-regexp-in-params

Vue Router uses its own path matching, inspired to the one used by `express`, so it supports many advanced matching patterns such as optional params, zero or more / one or more requirements, and even custom regex patterns

# Route Matching Syntax

Most applications will use static routes like `/about` and dynamic routes like `/users/:userId`, but Vue Router has much more to offer!

## Custom Regexp in params

When defining a param like `:userId`, we internally use the following regexp `([^/]+)` (at least one character that isn't a slash `/`) to extract params from URLs.

This works well unless you need to differentiate two routes based on the param content.

Imagine two routes `/:orderId` and `/:productName`, both would match the exact same URLs, so we need a way to differentiate them. The easiest way would be to add a static section to the path that differentiates them:

```js
const routes = [
  // matches /o/3549
  { path: '/o/:orderId' },
  // matches /p/books
  { path: '/p/:productName' },
]
```

But in some scenarios we don't want to add that static section `/o`/`p`. However, `orderId` is always a number while `productName` can be anything, so we can specify a custom regexp for a param in parentheses:

```js
const routes = [
  // /:orderId -> matches only numbers
  { path: '/:orderId(\\d+)' },
  // /:productName -> matches anything else
  { path: '/:productName' },
]
```

Make sure to **escape backslashes `\`** like we did with `\d` to actually pass the backslash character to a string in JavaScript.

## Repeatable params

If you need to match routes with multiple sections like `/first/second/third`, you can mark a param as repeatable with `*` (0 or more) and `+` (1 or more):

```js
const routes = [
  // /:chapters -> matches /one, /one/two, /one/two/three, etc
  { path: '/:chapters+' },
  // /:chapters -> matches /, /one, /one/two, /one/two/three, etc
  { path: '/:chapters*' },
]
```

This will give you an array of params instead of a string and will also require you to pass an array when using named routes:

```js
// given { path: '/:chapters*', name: 'chapters' },
router.resolve({ name: 'chapters', params: { chapters: [] } }).href
// produces /
router.resolve({ name: 'chapters', params: { chapters: ['a', 'b'] } }).href
// produces /a/b

// given { path: '/:chapters+', name: 'chapters' },
router.resolve({ name: 'chapters', params: { chapters: [] } }).href
// throws an Error because `chapters` is empty
```

These can also be combined with custom Regexp by adding them **after the closing parentheses**:

```js
const routes = [
  // only match numbers
  { path: '/:chapters(\\d+)+' },
  { path: '/:chapters(\\d+)*' },
]
```

## Optional parameters

You can also mark a parameter as optional by using the `?` modifier:

```js
const routes = [
  // will match /users and /users/posva
  { path: '/users/:userId?' },
  // will match /users and /users/42
  { path: '/users/:userId(\\d+)?' },
]
```

Note that `*` also marks the parameter as optional but cannot be repeated

## Debugging

If you need to dig how your routes are transformed into Regexp to understand why a route isn't being matched or, to report a bug, you can use the [path ranker tool](https://paths.esm.dev/?p=AAMeJSyAwR4UbFDAFxAcAGAIJXMAAA..#). It supports sharing your routes through the URL.

# Nested Routes

https://codesandbox.io/s/nested-views-vue-router-4-examples-hl326?initialpath=%2Fusers%2Feduardo

Some application's UIs are composed of components that are nested multiple levels deep.

```text
/user/johnny/profile                     /user/johnny/posts
+------------------+                  +-----------------+
| User             |                  | User            |
| +--------------+ |                  | +-------------+ |
| | Profile      | |  +------------>  | | Posts       | |
| |              | |                  | |             | |
| +--------------+ |                  | +-------------+ |
+------------------+                  +-----------------+
```

With Vue Router, you can express this relationship using nested route configurations.

```html
<div id="app">
  <router-view></router-view>
</div>
```

```js
const User = {
  template: `
    <div class="user">
      <h2>User {{ $route.params.id }}</h2>
      <router-view></router-view>
    </div>
  `,
}

// these are passed to `createRouter`
// To render components into this nested router-view, we need to use the children option in any of the routes:
const routes = [
  {
    path: '/user/:id',
    component: User,
    children: [
      {
        // UserProfile will be rendered inside User's <router-view>
        // when /user/:id/profile is matched
        path: 'profile',
        component: UserProfile,
      },
      {
        // UserPosts will be rendered inside User's <router-view>
        // when /user/:id/posts is matched
        path: 'posts',
        component: UserPosts,
      },
    ],
  },
]
// when you visit /user/eduardo, nothing will be rendered inside User's router-view, because no nested route is matched. Maybe you do want to render something there. In such case you can provide an empty nested path:
const routes = [
  {
    path: '/user/:id',
    component: User,
    children: [
      // UserHome will be rendered inside User's <router-view>
      // when /user/:id is matched
      { path: '', component: UserHome },

      // ...other sub routes
    ],
  },
]
```

**Note that nested paths that start with `/` will be treated as a root path. This allows you to leverage the component nesting without having to use a nested URL.**

As you can see the `children` option is just another Array of routes like `routes` itself. Therefore, you can keep nesting views as much as you need.

# Programmatic Navigation

Aside from using `<router-link>` to create anchor tags for declarative navigation, we can do this programmatically using the router's instance methods.

## Navigate 

https://next.router.vuejs.org/api/#routelocationraw

**Note: Inside of a Vue instance, you have access to the router instance as `$router`. You can therefore call `this.$router.push`.**

To navigate to a different URL, use `router.push`. This method pushes a new entry into the history stack, so when the user clicks the browser back button they will be taken to the previous URL.

This is the method called internally when you click a `<router-link>`, so clicking `<router-link :to="...">` is the equivalent of calling `router.push(...)`.

```js
// literal string path
router.push('/users/eduardo')

// object with path | Location Objects
router.push({ path: '/users/eduardo' })

// named route with params to let the router build the url
router.push({ name: 'user', params: { username: 'eduardo' } })

// with query, resulting in /register?plan=private
router.push({ path: '/register', query: { plan: 'private' } })

// with hash, resulting in /about#team
router.push({ path: '/about', hash: '#team' })

// Note: params are ignored if a path is provided, which is not the case for query, as shown in the example above. Instead, you need to provide the name of the route or manually specify the whole path with any parameter:

const username = 'eduardo'
// we can manually build the url but we will have to handle the encoding ourselves
router.push(`/user/${username}`) // -> /user/eduardo
// same as
router.push({ path: `/user/${username}` }) // -> /user/eduardo
// if possible use `name` and `params` to benefit from automatic URL encoding
router.push({ name: 'user', params: { username } }) // -> /user/eduardo

// `params` cannot be used alongside `path`
router.push({ path: '/user', params: { username } }) // -> /user
```

Since the prop `to` accepts the same kind of object as `router.push`, the exact same rules apply to both of them.

`router.push` and all the other navigation methods return a *Promise* that allows us to wait til the navigation is finished and to know if it succeeded or failed

## Replace 

It acts like `router.push`, the only difference is that it navigates without pushing a new history entry, as its name suggests - it replaces the current entry.

clicking `<router-link :to="..." replace>` is the equivalent of calling `router.push(...)`

It's also possible to directly add a property `replace: true` to the `routeLocation` that is passed to `router.push`:

```js
router.push({ path: '/home', replace: true })
// equivalent to
router.replace({ path: '/home' })
```

## Traverse history

This method takes a single integer as parameter that indicates by how many steps to go forward or go backward in the history stack, similar to `window.history.go(n)`.

```js
// go forward by one record, the same as router.forward()
router.go(1)

// go back by one record, the same as router.back()
router.go(-1)

// go forward by 3 records
router.go(3)

// fails silently if there aren't that many records
router.go(-100)
router.go(100)
```

## History Manipulation

https://developer.mozilla.org/en-US/docs/Web/API/History

It is worth mentioning that Vue Router navigation methods (`push`, `replace`, `go`) work consistently no matter the kind of [`history` option](https://next.router.vuejs.org/api/#history) is passed when creating the router instance.

# Named Routes

https://github.com/vuejs/vue-router/blob/dev/examples/named-routes/app.js

Alongside the `path`, you can provide a `name` to any route. This has the following advantages:

- No hardcoded URLs
- Automatic encoding/decoding of `params`
- Prevents you from having a typo in the url
- Bypassing path ranking (e.g. to display a )

```js
const routes = [
  {
    path: '/user/:username',
    name: 'user',
    component: User
  }
]
```

To link to a named route, you can pass an object to the `router-link` component's `to` prop:

```html
<router-link :to="{ name: 'user', params: { username: 'erina' }}">
  User
</router-link>
```

This is the exact same object used programmatically with `router.push()`:

```js
router.push({ name: 'user', params: { username: 'erina' } })
```

In both cases, the router will navigate to the path `/user/erina`.

# Named Views

https://codesandbox.io/s/named-views-vue-router-4-examples-rd20l

Sometimes you need to display multiple views at the same time instead of nesting them, e.g. creating a layout with a `sidebar` view and a `main` view.

This is where named views come in handy. Instead of having one single outlet in your view, you can have multiple and give each of them a name. A `router-view` without a name will be given `default` as its name.

```html
<router-view class="view left-sidebar" name="LeftSidebar"></router-view>
<router-view class="view main-content"></router-view>
<router-view class="view right-sidebar" name="RightSidebar"></router-view>
```

A view is rendered by using a component, therefore multiple views require multiple components for the same route.

the same route. Make sure to use the `components` (with an **s**) option:

```js
const router = createRouter({
  history: createWebHashHistory(),
  routes: [
    {
      path: '/',
      components: {
        default: Home,
        // short for LeftSidebar: LeftSidebar
        LeftSidebar,
        // they match the `name` attribute on `<router-view>`
        RightSidebar,
      },
    },
  ],
})
```

## Nested Named Views

https://codesandbox.io/s/nested-named-views-vue-router-4-examples-re9yl?&initialpath=%2Fsettings%2Femails

It is possible to create complex layouts using named views with nested views. When doing so, you will also need to give nested `router-view` a name. Let's take a Settings panel example:

```text
/settings/emails                                       /settings/profile
+-----------------------------------+                  +------------------------------+
| UserSettings                      |                  | UserSettings                 |
| +-----+-------------------------+ |                  | +-----+--------------------+ |
| | Nav | UserEmailsSubscriptions | |  +------------>  | | Nav | UserProfile        | |
| |     +-------------------------+ |                  | |     +--------------------+ |
| |     |                         | |                  | |     | UserProfilePreview | |
| +-----+-------------------------+ |                  | +-----+--------------------+ |
+-----------------------------------+                  +------------------------------+
```

- `Nav` is just a regular component
- `UserSettings` is the parent view component
- `UserEmailsSubscriptions`, `UserProfile`, `UserProfilePreview` are nested view components

```html
<!-- UserSettings.vue -->
<div>
  <h1>User Settings</h1>
  <NavBar />
  <router-view />
  <router-view name="helper" />
</div>
```

Then you can achieve the layout above with this route configuration:

```js
{
  path: '/settings',
  // You could also have named views at the top
  component: UserSettings,
  children: [{
    path: 'emails',
    component: UserEmailsSubscriptions
  }, {
    path: 'profile',
    components: {
      default: UserProfile,
      helper: UserProfilePreview
    }
  }]
}
```

# Redirect and Alias

## Redirect

A redirect means when the user visits `/home`, the URL will be replaced by `/`, and then matched as `/`

Redirecting is also done in the `routes` configuration. To redirect from `/a` to `/b`:

```js
const routes = [{ path: '/home', redirect: '/' }]
```

The redirect can also be targeting a named route:

```js
const routes = [{ path: '/home', redirect: { name: 'homepage' } }]
```

Or even use a function for dynamic redirecting:

```js
const routes = [
  {
    // /search/screens -> /search?q=screens
    path: '/search/:searchText',
    redirect: to => {
      // the function receives the target route as the argument
      // we return a redirect path/location here.
      return { path: '/search', query: { q: to.params.searchText } }
    },
  },
  {
    path: '/search',
    // ...
  },
]
```

Note that **[Navigation Guards](https://next.router.vuejs.org/guide/advanced/navigation-guards.html) are not applied on the route that redirects, only on its target**. e.g. In the example below, adding a `beforeEnter` guard to the `/home` route would not have any effect.

When writing a `redirect`, you can omit the `component` option because it is never directly reached so there is no component to render. The only exception are **nested routes**: if a route record has `children` and a `redirect` property, it should also have a `component` property.

### Relative redirecting

It's also possible to redirect to a relative location:

```js
const routes = [
  {
    path: '/users/:id/posts',
    redirect: to => {
      // the function receives the target route as the argument
      // return redirect path/location here.
    },
  },
]
```

## Alias

An alias of `/` as `/home` means when the user visits `/home`, the URL remains `/home`, but it will be matched as if the user is visiting `/`.

The above can be expressed in the route configuration as:

```js
const routes = [{ path: '/', component: Homepage, alias: '/home' }]
```

An alias gives you the freedom to map a UI structure to an arbitrary URL, instead of being constrained by the configuration's nesting structure. Make the alias start with a `/` to make the path absolute in nested routes. You can even combine both and provide multiple aliases with an array:

```js
const routes = [
  {
    path: '/users',
    component: UsersLayout,
    children: [
      // this will render the UserList for these 3 URLs
      // - /users
      // - /users/list
      // - /people
      { path: '', component: UserList, alias: ['/people', 'list'] },
    ],
  },
]
```

If your route has parameters, make sure to include them in any absolute alias:

```js
const routes = [
  {
    path: '/users/:id',
    component: UsersByIdLayout,
    children: [
      // this will render the UserDetails for these 3 URLs
      // - /users/24
      // - /users/24/profile
      // - /24
      { path: 'profile', component: UserDetails, alias: ['/:id', ''] },
    ],
  },
]
```

**Note about SEO**: when using aliases, make sure to [define canonical links](https://support.google.com/webmasters/answer/139066?hl=en).

# Passing Props to Route Components

https://github.com/vuejs/vue-router/blob/dev/examples/route-props/app.js

Using `$route` in your component creates a tight coupling with the route which limits the flexibility of the component as it can only be used on certain URLs. While this is not necessarily a bad thing, we can decouple this behavior with a `props` option

We can replace

```js
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
const routes = [{ path: '/user/:id', component: User }]
```

with

```js
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}
const routes = [{ path: '/user/:id', component: User, props: true }]
```

This allows you to use the component anywhere, which makes the component easier to reuse and test.

## Boolean mode

When `props` is set to `true`, the `route.params` will be set as the component props.

## Named views

For routes with named views, you have to define the `props` option for each named view:

```js
const routes = [
  {
    path: '/user/:id',
    components: { default: User, sidebar: Sidebar },
    props: { default: true, sidebar: false }
  }
]
```

## Object mode

When `props` is an object, this will be set as the component props as-is. Useful for when the props are static.

```js
const routes = [
  {
    path: '/promotion/from-newsletter',
    component: Promotion,
    props: { newsletterPopup: false }
  }
]
```

## Function mode

You can create a function that returns props. This allows you to cast parameters into other types, combine static values with route-based values, etc.

```
const routes = [
  {
    path: '/search',
    component: SearchUser,
    props: route => ({ query: route.query.q })
  }
]
```

The URL `/search?q=vue` would pass `{query: 'vue'}` as props to the `SearchUser` component.

Try to keep the `props` function stateless, as it's only evaluated on route changes. Use a wrapper component if you need state to define the props, that way vue can react to state changes.

# Different History modes

The `history` option when creating the router instance allows us to choose among different history modes.

## Hash Mode

The hash history mode is created with `createWebHashHistory()`:

```js
import { createRouter, createWebHashHistory } from 'vue-router'

const router = createRouter({
  history: createWebHashHistory(),
  routes: [
    //...
  ],
})
```

It uses a hash character (`#`) before the actual URL that is internally passed. Because this section of the URL is never sent to the server, it doesn't require any special treatment on the server level. **It does however have a bad impact in SEO**. If that's a concern for you, use the HTML5 history mode.

## HTML5 Mode

The HTML5 mode is created with `createWebHistory()` and is the recommend mode:

```js
import { createRouter, createWebHistory } from 'vue-router'

const router = createRouter({
  history: createWebHistory(),
  routes: [
    //...
  ],
})
```

When using history mode, the URL will look "normal," e.g. `https://example.com/user/id`. Beautiful!

Here comes a problem, though: Since our app is a single page client side app, without a proper server configuration, the users will get a 404 error if they access `https://example.com/user/id` directly in their browser.

Not to worry: To fix the issue, all you need to do is add a simple catch-all fallback route to your server. If the URL doesn't match any static assets, it should serve the same `index.html` page that your app lives in.

## Example Server Configurations

https://next.router.vuejs.org/guide/essentials/history-mode.html#example-server-configurations

## Caveat

There is a caveat to this: Your server will no longer report 404 errors as all not-found paths now serve up your `index.html` file. To get around the issue, you should implement a catch-all route within your Vue app to show a 404 page:

```ja
const router = createRouter({
  history: createWebHistory(),
  routes: [{ path: '/:pathMatch(.*)', component: NotFoundComponent }],
})
```

Alternatively, if you are using a Node.js server, you can implement the fallback by using the router on the server side to match the incoming URL and respond with 404 if no route is matched.

Check out the [Vue server side rendering documentation](https://ssr.vuejs.org/en/) for more information.

# Navigation Guards

https://next.router.vuejs.org/guide/advanced/navigation-guards.html

the navigation guards provided by Vue router are primarily used to guard navigations either by redirecting it or canceling it. There are a number of ways to hook into the route navigation process: globally, per-route, or in-component.

## Global Before Guards

https://next.router.vuejs.org/api/#routelocationnormalized

You can register global before guards using `router.beforeEach`:

```js
const router = createRouter({ ... })

router.beforeEach((to, from) => {
  // ...
  // explicitly return false to cancel the navigation
  return false
})
```

Global before guards are called in creation order, whenever a navigation is triggered. Guards may be resolved asynchronously, and the navigation is considered **pending** before all hooks have been resolved.

Every guard function receives two arguments and can optionally return any of the following values:

- `false`: cancel the current navigation. If the browser URL was changed (either manually by the user or via back button), it will be reset to that of the `from` route.
- A [Route Location](https://next.router.vuejs.org/api/#routelocationraw): Redirect to a different location by passing a route location as if you were calling [`router.push()`](https://next.router.vuejs.org/api/#push), which allows you to pass options like `replace: true` or `name: 'home'`. The current navigation is dropped and a new one is created with the same `from`.

It's also possible to throw an `Error` if an unexpected situation was met. This will also cancel the navigation and call any callback registered via [`router.onError()`](https://next.router.vuejs.org/api/#onerror).

If nothing, `undefined` or `true` is returned, **the navigation is validated**, and the next navigation guard is called.

All of the the things above **work the same way with `async` functions** and Promises:

```js
router.beforeEach(async (to, from) => {
  // canUserAccess() returns `true` or `false`
  return await canUserAccess(to)
})
```

### Optional third argument `next`

https://next.router.vuejs.org/guide/advanced/navigation-guards.html#optional-third-argument-next

**you must call `next` exactly once** in any given pass through a navigation guard

## Global Resolve Guards

You can register a global guard with `router.beforeResolve`. This is similar to `router.beforeEach` because it triggers on **every navigation**, but resolve guards are called right before the navigation is confirmed, **after all in-component guards and async route components are resolved**.

```js
router.beforeResolve(async to => {
  if (to.meta.requiresCamera) {
    try {
      await askForCameraPermission()
    } catch (error) {
      if (error instanceof NotAllowedError) {
        // ... handle the error and then cancel the navigation
        return false
      } else {
        // unexpected error, cancel the navigation and pass the error to the global handler
        throw error
      }
    }
  }
})
```

`router.beforeResolve` is the ideal spot to fetch data or do any other operation that you want to avoid doing if the user cannot enter a page.

## Global After Hooks

You can also register global after hooks, however unlike guards, these hooks do not get a `next` function and cannot affect the navigation:

```js
router.afterEach((to, from) => {
  sendToAnalytics(to.fullPath)
})
```

They are useful for analytics, changing the title of the page, accessibility features like announcing the page and many other things.

They also reflect [navigation failures](https://next.router.vuejs.org/guide/advanced/navigation-failures.html) as the third argument:

```js
router.afterEach((to, from, failure) => {
  if (!failure) sendToAnalytics(to.fullPath)
})
```

##  Per-Route Guard

You can define `beforeEnter` guards directly on a route's configuration object:

```js
const routes = [
  {
    path: '/users/:id',
    component: UserDetails,
    beforeEnter: (to, from) => {
      // reject the navigation
      return false
    },
  },
]
```

`beforeEnter` guards **only trigger when entering the route**, they don't trigger when the `params`, `query` or `hash` change e.g. going from `/users/2` to `/users/3` or going from `/users/2#info` to `/users/2#projects`. They are only triggered when navigating **from a different** route.

You can also pass an array of functions to `beforeEnter`, this is useful when reusing guards for different routes:

```js
function removeQueryParams(to) {
  if (Object.keys(to.query).length)
    return { path: to.path, query: {}, hash: to.hash }
}

function removeHash(to) {
  if (to.hash) return { path: to.path, query: to.query, hash: '' }
}

const routes = [
  {
    path: '/users/:id',
    component: UserDetails,
    beforeEnter: [removeQueryParams, removeHash],
  },
  {
    path: '/about',
    component: UserDetails,
    beforeEnter: [removeQueryParams],
  },
]
```

##  In-Component Guards

### Using the options API

```js
const UserDetails = {
  template: `...`,
  beforeRouteEnter(to, from) {
    // called before the route that renders this component is confirmed.
    // does NOT have access to `this` component instance,
    // because it has not been created yet when this guard is called!
  },
  beforeRouteUpdate(to, from) {
    // called when the route that renders this component has changed,
    // but this component is reused in the new route.
    // For example, given a route with params `/users/:id`, when we
    // navigate between `/users/1` and `/users/2`, the same `UserDetails` component instance
    // will be reused, and this hook will be called when that happens.
    // Because the component is mounted while this happens, the navigation guard has access to `this` component instance.
  },
  beforeRouteLeave(to, from) {
    // called when the route that renders this component is about to
    // be navigated away from.
    // As with `beforeRouteUpdate`, it has access to `this` component instance.
  },
}
```

```js
// The `beforeRouteEnter` guard does **NOT** have access to `this`, because the guard is called before the navigation is confirmed, thus the new entering component has not even been created yet.
// However, you can access the instance by passing a callback to `next`. The callback will be called when the navigation is confirmed, and the component instance will be passed to the callback as the argument:
beforeRouteEnter (to, from, next) {
  next(vm => {
    // access to component public instance via `vm`
  })
}
```

```js
// Note that `beforeRouteEnter` is the only guard that supports passing a callback to `next`. For `beforeRouteUpdate` and `beforeRouteLeave`, `this` is already available, so passing a callback is unnecessary and therefore *not supported*:
beforeRouteUpdate (to, from) {
  // just use `this`
  this.name = to.params.name
}
```

```js
// The **leave guard** is usually used to prevent the user from accidentally leaving the route with unsaved edits. The navigation can be canceled by returning `false`.
beforeRouteLeave (to, from) {
  const answer = window.confirm('Do you really want to leave? you have unsaved changes!')
  if (!answer) return false
}
```

## The Full Navigation Resolution Flow

1. Navigation triggered.
2. Call `beforeRouteLeave` guards in deactivated components.
3. Call global `beforeEach` guards.
4. Call `beforeRouteUpdate` guards in reused components.
5. Call `beforeEnter` in route configs.
6. Resolve async route components.
7. Call `beforeRouteEnter` in activated components.
8. Call global `beforeResolve` guards.
9. Navigation is confirmed.
10. Call global `afterEach` hooks.
11. DOM updates triggered.
12. Call callbacks passed to `next` in `beforeRouteEnter` guards with instantiated instances.

#  Route Meta Fields

You can include a `meta` field when defining a route with any arbitrary information:

```js
const routes = [
  {
    path: '/posts',
    component: PostsLayout,
    children: [
      {
        path: 'new',
        component: PostsNew,
        // only authenticated users can create posts
        meta: { requiresAuth: true }
      },
      {
        path: ':id',
        component: PostsDetail
        // anybody can read a post
        meta: { requiresAuth: false }
      }
    ]
  }
]
```

First, each route object in the `routes` configuration is called a **route record**. Route records may be nested. Therefore when a route is matched, it can potentially match more than one route record.

For example, with the above route config, the URL `/posts/new` will match both the parent route record (`path: '/posts'`) and the child route record (`path: 'new'`).

All route records matched by a route are exposed on the `$route` object (and also route objects in navigation guards) as the `$route.matched` Array. We could loop through that array to check all `meta` fields, but Vue Router also provides you a `$route.meta` that is a non-recursive merge of **all `meta`** fields from parent to child.

```js
router.beforeEach((to, from) => {
  // instead of having to check every route record with
  // to.matched.some(record => record.meta.requiresAuth)
  if (to.meta.requiresAuth && !auth.isLoggedIn()) {
    // this route requires auth, check if logged in
    // if not, redirect to login page.
    return {
      path: '/login',
      // save the location we were at to come back later
      query: { redirect: to.fullPath },
    }
  }
})
```

# Scroll Behavior

**Note: this feature only works if the browser supports `history.pushState`.**

When creating the router instance, you can provide the `scrollBehavior` function:

```js
const router = createRouter({
  history: createWebHashHistory(),
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    // return desired position
  }
})
```

The third argument, `savedPosition`, is only available if this is a `popstate` navigation (triggered by the browser's back/forward buttons).

The function can return a [`ScrollToOptions`](https://developer.mozilla.org/en-US/docs/Web/API/ScrollToOptions) position object:

```js
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    // always scroll to top
    return { top: 0 }
  },
})
```

You can also pass a CSS selector or a DOM element via `el`. In that scenario, `top` and `left` will be treated as relative offsets to that element.

```js
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    // always scroll 10px above the element #main
    return {
      // could also be
      // el: document.getElementById('main'),
      el: '#main',
      top: -10,
    }
  },
})
```

If a falsy value or an empty object is returned, no scrolling will happen.

Returning the `savedPosition` will result in a native-like behavior when navigating with back/forward buttons:

```js
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    if (savedPosition) {
      return savedPosition
    } else {
      return { top: 0 }
    }
  },
})
```

If you want to simulate the "scroll to anchor" behavior:

```js
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    if (to.hash) {
      return {
        el: to.hash,
      }
    }
  },
})
```

If your browser supports [scroll behavior](https://developer.mozilla.org/en-US/docs/Web/API/ScrollToOptions/behavior), you can make it smooth:

```js
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    if (to.hash) {
      return {
        el: to.hash
        behavior: 'smooth',
      }
    }
  }
})
```

## Delaying the scroll

Sometimes we need to wait a bit before scrolling in the page. For example, when dealing with transitions, we want to wait for the transition to finish before scrolling. To do this you can return a Promise that returns the desired position descriptor.

```js
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve({ left: 0, top: 0 })
      }, 500)
    })
  },
})
```

# Transitions

https://next.router.vuejs.org/guide/advanced/transitions.html

# Migrating from Vue 2
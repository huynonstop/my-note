# Routing

![image-20200626200729737](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200626200729737.png)

![image-20200626200713192](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200626200713192.png)

# React Router

React Router is a powerful routing library built on top of React that helps you add new screens and flows to your application incredibly quickly, all while keeping the URL in sync with what's being displayed on the page.

React Router is a wrapper around the `history` library which handles interaction with the browser's `window.history` with its browser and hash histories. It also provides memory history which is useful for environments that don't have global history, such as mobile app development (React Native) and unit testing with Node.

React Router Docs: https://reacttraining.com/react-router/web/guides/philosophy

## `<Router>` components

React Router v4 provides below 3 `<Router>` components:

1. `<BrowserRouter>`
2. `<HashRouter>`
3. `<MemoryRouter>`

The above components will create browser, hash, and memory history instances. React Router v4 makes the properties and methods of the history instance associated with your router available through the context in the router object.

## `push()` and `replace()` methods of `history`

A history instance has two methods for navigation purpose.

1. `push()` will add a new location to the array (can back)
2. `replace()` will replace the current location in the array with the new one.

## `<Link>` to switch page

```jsx
<Link to="/somepath"></Link>
<Link to={{
        pathname: "/somepath",
        hash: "#somehash",
        search: "?query=true"
    }}></Link>
```



## Routing-props

![image-20200626201851218](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200626201851218.png)

You can accessing Routing-props when:

Passing Routing-props to child component

Using the `withRouter()` higher-order function

Using props of component rendered by a `<Route>`

## Absolute Paths

By **default**, if you just enter `to="/some-path"` or `to="some-path"` , that's an **absolute path**. 

**Absolute path** means that it's **always appended right after your domain**. Therefore, both syntaxes (with and without leading slash) lead to `example.com/some-path` .

## Relative Paths

If you're on a component loaded via `/posts` , `to="new"` would lead to `example.com/new` , **NOT** `example.com/posts/new` . 

To change this behavior, you have to find out which path you're on and add the new fragment to that existing path. You can do that with the `url` property of `props.match`

```jsx
<Link to={props.match.url + '/new'}>
{/*
	This will lead to example.com/posts/new  when placing this link in a component loaded on /posts
	If you'd use the same <Link>  in a component loaded via /all-posts , the link would point to /all-posts/new .
*/}
```

## Active link

```jsx
<NavLink to="/" activeClassName="active"></NavLink> {/*active is the default class name for active Link*/}
```

## Passing route parameters

```jsx
<Route path="/:id" component={Posts}/>
```

```js
//Access by
this.props.match.params.id
```

## query parameters

You can also use `URLSearchParams` if you want something native:

```jsx
const params = new URLSearchParams(props.location.search)
const foo = params.get('name')
```

## Fragment

```jsx
<Link to="/my-path#start-position">Go to Start</Link>
```

or

```jsx
<Link 
    to={{
        pathname: '/my-path',
        hash: 'start-position'
    }}
    >Go to Start</Link>
```

React router makes it easy to extract the fragment. You can simply access `props.location.hash` .

## programmatically navigate

There are three different ways to achieve programmatic routing/navigation within components.

1. Using the `withRouter()` higher-order function:

   The `withRouter()` higher-order function will inject the history object as a prop of the component. This object provides `push()` and `replace()` methods to avoid the usage of context.

   ```jsx
   import { withRouter } from 'react-router-dom' // this also works with 'react-router-native'
   
   const Button = withRouter(({ history }) => (
     <button
       type='button'
       onClick={() => { history.push('/new-location') }}
     >
       {'Click Me!'}
     </button>
   ))
   ```

2. Using props of `component` and `render` 

   The component passes the same props as `withRouter()`, so you will be able to access the history methods through the history prop.

   ```jsx
   import { Route } from 'react-router-dom'
   
   const Button = () => (
     <Route render={({ history }) => (
       <button
         type='button'
         onClick={() => { history.push('/new-location') }}
       >
         {'Click Me!'}
       </button>
     )} />
   )
   ```

3. Using context:

   This option is not recommended and treated as unstable API.

   ```jsx
   const Button = (props, context) => (
     <button
       type='button'
       onClick={() => {
         context.history.push('/new-location')
       }}
     >
       {'Click Me!'}
     </button>
   )
   
   Button.contextTypes = {
     history: React.PropTypes.shape({
       push: React.PropTypes.func.isRequired
     })
   }
   ```

## Render 1 route

You have to wrap your Route's in a `` block because `` is unique in that it renders a route exclusively.

At first you need to add `Switch` to your imports:

```jsx
import { Switch, Route } from 'react-router'
```

Then define the routes within `Router` block:

```jsx
  <Switch>
      {/*render only the first match Route*/}
    <Route {/* ... */} />
    <Route {/* ... */} />
  </Switch>
```

## pass params to `history.push` method

While navigating you can pass props to the `history` object:

```jsx
this.props.history.push({
  pathname: '/template',
  search: '?name=sudheer',
  state: { detail: response.data }
})
```

The `search` property is used to pass query params in `push()` method.

## *default* or *NotFound* page

A `` renders the first child `` that matches. A `Route` with no path always matches. So you just need to simply drop path attribute as below

```jsx
<Switch>
  <Route exact path="/" component={Home}/>
  <Route path="/user" component={User}/>
  <Route component={NotFound} />
</Switch>
```

## get `history `

1. Create a module that exports a `history` object and import this module across the project.

   For example, create `history.js` file:

   ```jsx
   import { createBrowserHistory } from 'history'
   
   export default createBrowserHistory({
     /* pass a configuration object here if needed */
   })
   //history.js
   ```

2. You should use the `Router` component 

   ```jsx
   import { Router } from 'react-router-dom'
   import history from './history'
   import App from './App'
   
   ReactDOM.render((
     <Router history={history}>
       <App />
     </Router>
   ), holder)
   //index.js
   ```

3. You can also use push method of `history` object similar to built-in history object:

   ```jsx
   // some-other-file.js
   import history from './history'
   
   history.push('/go-here')
   ```

## Programming redirect 

The `react-router` package provides `` component in React Router. Rendering a `` will navigate to a new location. Like server-side redirects, the new location will override the current location in the history stack.

```js
import React, { Component } from 'react'
import { Redirect } from 'react-router'

export default class LoginComponent extends Component {
  render() {
    if (this.state.isLoggedIn === true) {
      return <Redirect to="/your/redirect/page" />
    } else {
      return <div>{'Login Please'}</div>
    }
  }
}
```

# Server deployment

![image-20200626204520572](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200626204520572.png)

```jsx
//index.js
<BrowserRouter basename="/">
</BrowserRouter>
```

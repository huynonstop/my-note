# Best practice

## Error boundary

https://reactjs.org/docs/error-boundaries.html

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // Update state so the next render will show the fallback UI.
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // You can also log the error to an error reporting service
    logErrorToMyService(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children; 
  }
}
```

## CSS Modules Stylesheet

https://github.com/css-modules/css-modules

https://css-tricks.com/css-modules-part-3-react/

https://medium.com/nulogy/how-to-use-css-modules-with-create-react-app-9e44bec2b5c2

https://create-react-app.dev/docs/adding-a-css-modules-stylesheet/

with `react-scripts@2.0.0` and higher 

```css
/*Button.module.css*/
.error {
  background-color: red;
}
/*another-stylesheet.css*/
.error {
  color: red;
}
```

```jsx
import React, { Component } from 'react';
import styles from './Button.module.css'; // Import css modules stylesheet as styles
import './another-stylesheet.css'; // Import regular stylesheet
class Button extends Component {
  render() {
    // reference as a js object
    return <button className={styles.error}>Error Button</button>;
  }
}
```

```html
<!-- This button has red background but not red text -->
<button class="Button_error_ax7yz">Error Button</button>
```

## conditional JSX

![image-20200623230956314](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200623230956314.png)

## use `innerHTML ` in React

The `dangerouslySetInnerHTML` attribute is React replacement for using `innerHTML` in the browser DOM. Just like `innerHTML`, it is risky to use this attribute considering cross-site scripting (XSS) attacks. You just need to pass a `__html` object as key and HTML text as value.

```jsx
function createMarkup() {
  return { __html: 'First &middot; Second' }
}

function MyComponent() {
  return <div dangerouslySetInnerHTML={createMarkup()} />
}
```

## Deploy to GitHub pages

add remote to github

`yarn add gh-page`

add `homepage` (github page URL) to `package.json`

add `"predeploy": "yarn build"` script to `package.json` 

add `"deploy": "gh-pages -d build"` script to `package.json` 

`yarn deploy`

## ordering of methods

Recommended ordering of methods from mounting to render stage:

```js
static methods
constructor()
getChildContext()
componentWillMount()
componentDidMount()
componentWillReceiveProps()
shouldComponentUpdate()
componentWillUpdate()
componentDidUpdate()
componentWillUnmount()
click handlers or event handlers 
getter methods 
optional render methods 
render()
```

## Why Custom event delegation is discourage?

The implementation of custom event delegation looks elegant, but it is against React’s philosophy.

### New data flow is created

One way data flow is one of the core idea of React. In custom event delegation, data is passing to parent by events. Event bubbling become another data flow.

When the application grow large, it is difficult to maintain two data flows. Also, data in event cannot be capture by React developer tool, it is very difficult to debug error in event bubbling.

### Implicit dependency is introduced

React try to make every dependency explicit, even at the cost of writing more code. Custom event delegation introduce an implicit dependency between parent and the children.

Take the cards example mentioned above, the `App` component need to know :

1. There is a custom field `title` in click event
2. The clicked card title need to be alert

Even `App` component does not passing any function to `Card` components, they are logically coupled, because `App` need to know the behaviour of `Card`.

Not using custom event delegation, we can either pass click handler from `App` or defined the click handler inside `Card`. Both solution show the dependency explicitly. The code is easier to understand and maintain.

## switching component

A *switching component* is a component that renders one of many components. We need to use object to map prop values to components.

For example, a switching component to display different pages based on `page` prop:

```jsx
import HomePage from './HomePage'
import AboutPage from './AboutPage'
import ServicesPage from './ServicesPage'
import ContactPage from './ContactPage'

const PAGES = {
  home: HomePage,
  about: AboutPage,
  services: ServicesPage,
  contact: ContactPage
}

const Page = (props) => {
  const Handler = PAGES[props.page] || ContactPage

  return <Handler {...props} />
}

// The keys of the PAGES object can be used in the prop types to catch dev-time errors.
Page.propTypes = {
  page: PropTypes.oneOf(Object.keys(PAGES)).isRequired
}
```

## React label 

```jsx
<label htmlFor={'user'}>{'User'}</label>
<input type={'text'} id={'user'} />
```

## listen to resize event

You can listen to the `resize` event in `componentDidMount()` and then update the dimensions (`width` and `height`). You should remove the listener in `componentWillUnmount()` method.

```jsx
class WindowDimensions extends React.Component {
  constructor(props){
    super(props);
    this.updateDimensions = this.updateDimensions.bind(this);
  }
   
  componentWillMount() {
    this.updateDimensions()
  }

  componentDidMount() {
    window.addEventListener('resize', this.updateDimensions)
  }

  componentWillUnmount() {
    window.removeEventListener('resize', this.updateDimensions)
  }

  updateDimensions() {
    this.setState({width: window.innerWidth, height: window.innerHeight})
  }

  render() {
    return <span>{this.state.width} x {this.state.height}</span>
  }
}
```

## How to listen to state changes?

The following lifecycle methods will be called when state changes. You can compare provided state and props values with current state and props to determine if something meaningful changed.

```js
componentWillUpdate(object nextProps, object nextState)
componentDidUpdate(object prevProps, object prevState)
```

## removing an array element in React state

The better approach is to use `Array.prototype.filter()` method.

For example, let's create a `removeItem()` method for updating the state.

```jsx
removeItem(index) {
  this.setState({
    data: this.state.data.filter((item, i) => i !== index)
  })
}
```

## React thing without render HTML?

It is possible with latest version (>=16.2). Below are the possible options:

```jsx
render() {
  return false
}
render() {
  return null
}
render() {
  return []
}
render() {
  return <React.Fragment></React.Fragment>
}
render() {
  return <></>
}
```

## focus an element on page load

You can do it by creating *ref* for `input` element and using it in `componentDidMount()`:

```jsx
class App extends React.Component{
  componentDidMount() {
    this.nameInput.focus()
  }

  render() {
    return (
      <div>
        <input
          defaultValue={'Won\'t focus'}
        />
        <input
          ref={(input) => this.nameInput = input}
          defaultValue={'Will focus'}
        />
      </div>
    )
  }
}

ReactDOM.render(<App />, document.getElementById('app'))
```

## updating objects in state

1. **Calling `setState()` with an object to merge with state:**

   - Using `Object.assign()` to create a copy of the object:

     ```
     const user = Object.assign({}, this.state.user, { age: 42 })
     this.setState({ user })
     ```

   - Using *spread operator*:

     ```
     const user = { ...this.state.user, age: 42 }
     this.setState({ user })
     ```

2. **Calling `setState()` with a function:**

   React may batch multiple `setState()` calls into a single update for performance. Because `this.props` and `this.state` may be updated asynchronously, you should not rely on their values for calculating the next state.

   ```jsx
   this.setState(prevState => ({
     user: {
       ...prevState.user,
       age: 42
     }
   }))
   
   this.setState((prevState, props) => ({
     counter: prevState.counter + props.increment
   }))
   ```

## How to use https instead of http in create-react-app?

You just need to use `HTTPS=true` configuration. You can edit your `package.json` scripts section:

```json
"scripts": {
  "start": "set HTTPS=true && react-scripts start"
}
```

## setInterval in a component

You need to use `setInterval()` to trigger the change, but you also need to clear the timer when the component unmounts to prevent errors and memory leaks.

```jsx
componentDidMount() {
  this.interval = setInterval(() => this.setState({ time: Date.now() }), 1000)
}

componentWillUnmount() {
  clearInterval(this.interval)
}
```

##  programmatically trigger click event

You could use the ref prop to acquire a reference to the underlying `HTMLInputElement` object through a callback, store the reference as a class property, then use that reference to later trigger a click from your event handlers using the `HTMLElement.click` method. This can be done in two steps:

1. Create ref in render method:

   ```jsx
   <input ref={input => this.inputElement = input} />
   ```

2. Apply click event in your event handler:

   ```jsx
   this.inputElement.click()
   ```

## Folder structures for React

There are two common practices for React project file structure.

1. **Grouping by features or routes:**

   One common way to structure projects is locate CSS, JS, and tests together, grouped by feature or route.

   ```
   common/
   ├─ Avatar.js
   ├─ Avatar.css
   ├─ APIUtils.js
   └─ APIUtils.test.js
   feed/
   ├─ index.js
   ├─ Feed.js
   ├─ Feed.css
   ├─ FeedStory.js
   ├─ FeedStory.test.js
   └─ FeedAPI.js
   profile/
   ├─ index.js
   ├─ Profile.js
   ├─ ProfileHeader.js
   ├─ ProfileHeader.css
   └─ ProfileAPI.js
   ```

2. **Grouping by file type:**

   Another popular way to structure projects is to group similar files together.

   ```
   api/
   ├─ APIUtils.js
   ├─ APIUtils.test.js
   ├─ ProfileAPI.js
   └─ UserAPI.js
   components/
   ├─ Avatar.js
   ├─ Avatar.css
   ├─ Feed.js
   ├─ Feed.css
   ├─ FeedStory.js
   ├─ FeedStory.test.js
   ├─ Profile.js
   ├─ ProfileHeader.js
   └─ ProfileHeader.css
   ```

## Where to fetch data

You can use AJAX libraries such as Axios, jQuery AJAX, and the browser built-in `fetch`. You should fetch data in the `componentDidMount()` lifecycle method. This is so you can use `setState()` to update your component when the data is retrieved.

For example, the employees list fetched from API and set local state:

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      employees: [],
      error: null
    }
  }

  componentDidMount() {
    fetch('https://api.example.com/items')
      .then(res => res.json())
      .then(
        (result) => {
          this.setState({
            employees: result.employees
          })
        },
        (error) => {
          this.setState({ error })
        }
      )
  }

  render() {
    const { error, employees } = this.state
    if (error) {
      return <div>Error: {error.message}</div>;
    } else {
      return (
        <ul>
          {employees.map(item => (
            <li key={employee.name}>
              {employee.name}-{employees.experience}
            </li>
          ))}
        </ul>
      )
    }
  }
}
```

## What are render props?

**Render Props** is a simple technique for sharing code between components using a prop whose value is a function. The below component uses render prop which returns a React element.

```jsx
<DataProvider render={data => (
  <h1>{`Hello ${data.target}`}</h1>
)}/>
```

Libraries such as React Router and DownShift are using this pattern.
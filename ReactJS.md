# **React is a JavaScript “library” for building user interfaces**

React is an open-source JavaScript library created by Facebook for building complex, interactive UIs in web and mobile applications. React’s core purpose is to build UI components; it is often referred to as just the “V” (View) in an “MVC” architecture.

React creates a virtual DOM. When state changes in a component it firstly runs a "diffing" algorithm, which identifies what has changed in the virtual DOM. The second step is reconciliation, where it updates the DOM with the results of diff.

- It uses **VirtualDOM** instead RealDOM considering that RealDOM manipulations are expensive.
- Supports **server-side rendering**
- Follows **Unidirectional** data flow or data binding
- Uses **reusable/composable** UI components to develop the view

# Advantages of ReactJS

1. Increases the application’s performance with Virtual DOM
2. JSX makes code is easy to read and write. It is also really easy to see the layout, or how components are plugged/combined with each other.
3. It renders both on client and server side. This enables improves SEO.
4. Easy to integrate with other frameworks (Angular, BackboneJS) since it is only a view library
5. Easy to write UI Test cases and integration with tools such as JEST

# Below are the list of limitations:

1. React is just a view library, not a full-blown framework
2. There is a learning curve for beginners who are new to web development.
3. Integrating React.js into a traditional MVC framework requires some additional configuration
4. The code complexity increases with inline templating and JSX.
5. Too many smaller components leading to over-engineering or boilerplate

# Composition vs Inheritance

React has a powerful composition model, and we recommend using composition instead of inheritance to reuse code between components.

# One-way data binding

# ![img](https://viblo.asia/uploads/29fea837-c6e8-416f-a11f-d708b33c2a78.png)

# What is the Virtual DOM?

The virtual DOM (VDOM) is a programming concept where an ideal, or “virtual”, representation of a UI is kept in memory and synced with the “real” DOM by a library such as ReactDOM. 

# Reconciliation

When a component’s props or state change, React decides whether an actual DOM update is necessary by comparing the newly returned element with the previously rendered one. When they are not equal, React will update the DOM. This process is called “reconciliation”.

## The Diffing Algorithm of React

React implements a heuristic O(n) algorithm based on two assumptions:

1. Two elements of different types will produce different trees.
2. The developer can hint at which child elements may be stable across different renders with a `key` prop.

When we tell React to render a tree of elements in the browser, it first generates a virtual representation of that tree and keeps it around in memory for later. Then it’ll proceed to perform the DOM operations that will make the tree show up in the browser.

When we tell React to update the tree of elements it previously rendered, it generates a new virtual representation of the updated tree. Now React has 2 versions of the tree in memory!

To render the updated tree in the browser, React does not discard what has already been rendered. Instead, it will compare the 2 virtual versions of the tree that it has in memory, compute the differences between them, figure out what sub-trees in the main tree need to be updated, and only update these sub-trees in the browser.

This process is what’s known as the tree reconciliation algorithm and it is what makes React a very efficient way to work with a browser’s DOM tree. We’ll see an example of it shortly.

# How virtual DOM works in the ReactJS

## How DOM is build

Every time the DOM changes, browser need to changes to the DOM, recalculate the CSS, do layout, and repaint the web page. This is what takes time.

Browser makers are continually working to shorten the time it takes to repaint the screen. The biggest thing that can be done is to minimize and batch the DOM changes that make redraws necessary.

This strategy of reducing and batching DOM changes, taken to another level of abstraction, is the idea behind React’s Virtual DOM.

## How virtual DOM works 

Each time the underlying data changes in a React app, a new Virtual DOM representation of the user interface is created

Updating the browser’s DOM is a three-step process in React.

1. Whenever anything may have changed, the entire UI will be re-rendered in a Virtual DOM representation.

   ![vdom](https://github.com/sudheerj/reactjs-interview-questions/raw/master/images/vdom1.png)

2. The difference between the previous Virtual DOM representation and the new one will be calculated.![vdom2](https://github.com/sudheerj/reactjs-interview-questions/raw/master/images/vdom2.png)

3. The real DOM will be updated with what has actually changed. This is very much like applying a patch.![vdom3](https://github.com/sudheerj/reactjs-interview-questions/raw/master/images/vdom3.png)

React is keeping two Virtual DOM trees in memory.

# Single-page Application (SPA)

![img](https://images.viblo.asia/2d35ba29-12ec-4f56-8fa7-a6c7aac13aba.png)

Single page Application là một ứng dụng web giúp nâng cao trải nghiệm người dùng bằng cách sử dụng HTML5 và AJAX. Đầu tiên khi tải một trang web bất kỳ, SPA sẽ tải một trang HTML đơn, sau đó dựa trên request của người dùng, SPA sẽ tiếp tục tải các HTML khác trong cùng một trang đó, SPA có thể sử dụng một vài thư viện JavaScript như AngularJS, Backbone.js, Durandal...

# JSX

JSX is a syntax extension to JavaScript. It is similar to a template language, but it has full power of JavaScript. JSX gets compiled to `React.createElement()` calls which return plain JavaScript objects called “React elements”.

# Comment

```jsx
{/* Single-line comments(In vanilla JavaScript, the single-line comments are represented by double slash(//)) */}
```

# Why React use className

class is a keyword in JavaScript, and JSX is an extension of JavaScript. That's the principal reason why React uses className instead of class

# Compilers (Babel)

A JavaScript compiler takes JavaScript code, transforms it and returns JavaScript code in a different format. The most common use case is to take ES6 syntax and transform it into syntax that older browsers are capable of interpreting

# Bundlers (Webpack and Browserify)

Bundlers take JavaScript and CSS code written as separate modules (often hundreds of them), and combine them together into a few files better optimized for the browsers.

# Elements

React elements are the building blocks of React applications.

An element describes what you want to see on the screen. React elements are **immutable**.

 An element is like a single frame in a movie: it represents the UI at a certain point in time.

```javascript
const element = <h1>Hello, world</h1>;
```

# props

`props` are inputs to a React component. They are data passed down from a parent component to a child component.

Remember that `props` are readonly.

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props)

    console.log(this.props) // prints { name: 'John', age: 42 }
  }
}

//Not pass props to super
class MyComponent extends React.Component {
  constructor(props) {
    super()

    console.log(this.props) // prints undefined

    // but props parameter is still available
    console.log(props) // prints { name: 'John', age: 42 }
  }

  render() {
    // no difference outside constructor
    console.log(this.props) // prints { name: 'John', age: 42 }
  }
}
```

## props.children

`props.children` is available on every component. That allow you to pass components as data to other components, just like any other prop you use. It contains the content between the opening and closing tags of a component. For example:

```jsx
<Welcome>Hello world!</Welcome>
```

The string `Hello world!` is available in `props.children` in the `Welcome` component:

## propTypes

1. `PropTypes.number`
2. `PropTypes.string`
3. `PropTypes.array`
4. `PropTypes.object`
5. `PropTypes.func`
6. `PropTypes.node`
7. `PropTypes.element`
8. `PropTypes.bool`
9. `PropTypes.symbol`
10. `PropTypes.any`

```jsx
import React from 'react'
import PropTypes from 'prop-types'

class User extends React.Component {
  static propTypes = {
    name: PropTypes.string.isRequired,
    age: PropTypes.number.isRequired
  }

  render() {
    return (
      <>
        <h1>{`Welcome, ${this.props.name}`}</h1>
        <h2>{`Age, ${this.props.age}`}</h2>
      </>
    )
  }
}
```

# state![state](https://github.com/sudheerj/reactjs-interview-questions/raw/master/images/state.jpg)

A component needs `state` when some data associated with it changes over time.

The most important difference between `state` and `props` is that `props` are passed from a parent component, but `state` is managed by the component itself. A component cannot change its `props`, but it can change its `state`.

```jsx
class CounterButton extends React.Component {
  state = {
    clickCounter: 0,
    currentTimestamp: new Date(),
  };
  
  handleClick = () => {
    this.setState((prevState) => {
     return { clickCounter: prevState.clickCounter + 1 };
    });
  };
  
  componentDidMount() {
   setInterval(() => {
     this.setState({ currentTimestamp: new Date() })
    }, 1000);
  }
  
  render() {
    return (
      <div>
        <button onClick={this.handleClick}>Click</button>
        <p>Clicked: {this.state.clickCounter}</p>
        <p>Time: {this.state.currentTimestamp.toLocaleString()}</p>
      </div>
    );
  }
}
```

## setState

The first thing React will do when setState is called is merge the object you passed into setState into the current state of the component **asynchronously**.  It tells React that this component and its children need to be re-rendered

## Dynamic key name

```
handleInputChange(event) {
  this.setState({ [event.target.id]: event.target.value })
}
```

## Using setState Correctly

1. Do Not Modify State Directly

   If you try to update state directly then it won't re-render the component.

2. State Updates May Be Asynchronous

   ```javascript
   // may fail
   this.setState({
     counter: this.state.counter + this.props.increment,
   });
   ```

   To fix it, use a second form of `setState()` that accepts a function rather than an object. That function will receive the previous state as the first argument, and the props at the time the update is applied as the second argument:

   ```javascript
   // Correct
   this.setState((state, props) => ({
     counter: state.counter + props.increment
   }));
   ```

3. State Updates are Merged (Only class this.setState)

   The merging is shallow, so `this.setState({comments})` leaves `this.state.posts` intact, but completely replaces `this.state.comments`.

   ```javascript
     componentDidMount() {
       fetchPosts().then(response => {
         this.setState({
           posts: response.posts
         });
       });
   
       fetchComments().then(response => {
         this.setState({
           comments: response.comments
         });
       });
     }
   ```

## We updated the state using two different ways

1. By passing a function that returned an object. We did that inside the `handleClick` function.

   (read and write to the state at the same time)

2. By passing a regular object. We did that inside the interval callback.

   (Only writing)

## useState Hook

```jsx
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```



You **don’t have to** use many state variables. State variables can hold objects and arrays just fine, so you can still group related data together. However, unlike `this.setState` in a class, updating a state variable always *replaces* it instead of merging it.

# Components

Conceptually, components are like JavaScript functions. They accept arbitrary inputs (called “props”) and return React elements describing what should appear on the screen.

React components are small, reusable pieces of code that return a React element to be rendered to the page

Components can be broken down into distinct pieces of functionality and used within other components. Components can return other components, arrays, strings and numbers.

A good rule of thumb is that if a part of your UI is used several times (Button, Panel, Avatar), or is complex enough on its own (App, FeedStory, Comment), it is a good candidate to be a reusable component

## Controlled Components

A component that controls the input elements within the forms on subsequent user input is called **Controlled Component**, i.e, every state mutation will have an associated handler function.

## Uncontrolled Components

An *uncontrolled component* works like form elements do outside of React. When a user inputs data into a form field (an input box, dropdown, etc) the updated information is reflected without React needing to do anything. And you query the DOM using a ref to find its current value when you need it. This is a bit more like traditional HTML.

In React, an `<input type="file" /> ` is always an uncontrolled component

initial value = `defaultValue` attribute 

```jsx
class UserProfile extends React.Component {
  constructor(props) {
    super(props)
    this.handleSubmit = this.handleSubmit.bind(this)
    this.input = React.createRef()
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.input.current.value)
    event.preventDefault()
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          {'Name:'}
          <input type="text" ref={this.input} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

| feature                                                      | uncontrolled | controlled |
| ------------------------------------------------------------ | ------------ | ---------- |
| one-time value retrieval (e.g. on submit)                    | ✅            | ✅          |
| [validating on submit](https://goshakkk.name/submit-time-validation-react/) | ✅            | ✅          |
| [instant field validation](https://goshakkk.name/instant-form-fields-validation-react/) | ❌            | ✅          |
| [conditionally disabling submit button](https://goshakkk.name/form-recipe-disable-submit-button-react/) | ❌            | ✅          |
| enforcing input format                                       | ❌            | ✅          |
| several inputs for one piece of data                         | ❌            | ✅          |
| [dynamic inputs](https://goshakkk.name/array-form-inputs/)   | ❌            | ✅          |

# Lifecycle

The component lifecycle has three distinct lifecycle phases:

1. **Mounting:** The component is ready to mount in the browser DOM. This phase covers initialization from `constructor()`, `getDerivedStateFromProps()`, `render()`, and `componentDidMount()` lifecycle methods.
2. **Updating:** In this phase, the component get updated in two ways, sending the new props and updating the state either from `setState()` or `forceUpdate()`. This phase covers `getDerivedStateFromProps()`, `shouldComponentUpdate()`, `render()`, `getSnapshotBeforeUpdate()` and `componentDidUpdate()` lifecycle methods.
3. **Unmounting:** In this last phase, the component is not needed and get unmounted from the browser DOM. This phase includes `componentWillUnmount()` lifecycle method.

React internally has a concept of phases when applying changes to the DOM. They are separated as follows

1. **Render** The component will render without any side-effects. This applies for Pure components and in this phase, React can pause, abort, or restart the render.
2. **Pre-commit** Before the component actually applies the changes to the DOM, there is a moment that allows React to read from the DOM through the `getSnapshotBeforeUpdate()`.
3. **Commit** React works with the DOM and executes the final lifecycles respectively `componentDidMount()` for mounting, `componentDidUpdate()` for updating, and `componentWillUnmount()` for unmounting.

![image-20200208132318599](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200208132318599.png)

1. First, we define a template for React to create elements from the component.
2. Then, we instruct React to use it somewhere. For example, inside a `render` call of another component, or with `ReactDOM.render`.
3. Then, React instantiates an element and gives it a set of **props** that we can access with `this.props`. Those props are exactly what we passed in step 2 above.
4. Since it’s all JavaScript, the `constructor` method will be called (if defined). This is the first of what we call: **component lifecycle methods***.*
5. React then computes the output of the render method (the virtual DOM node).
6. Since this is the first time React is rendering the element, React will communicate with the browser (on our behalf, using the DOM API) to display the element there. This process is commonly known as **mounting**.
7. React then invokes another lifecycle method, called `componentDidMount`. We can use this method to, for example, do something on the DOM that we now know exists in the browser. Prior to this lifecycle method, the DOM we work with was all virtual.
8. Some components stories end here. Other components get unmounted from the browser DOM for various reasons. Right before the latter happens, React invokes another lifecycle method, `componentWillUnmount`.
9. The **state** of any mounted element might change. The parent of that element might re-render. In either case, the mounted element might receive a different set of props. React magic happens here and we actually start **needing** React at this point! Prior to this point, we did not need React at all, honestly.
10. A component might need to re-render when its state gets updated or when its parent decides to change the props that it passed to the component
11. If the latter happens, React invokes another lifecycle method, `componentWillReceiveProps`.
12. If either the state object or the passed-in props are changed, React has an important decision to do. Should the component be updated in the DOM? This is why it invokes another important lifecycle method here, `shouldComponentUpdate`. This method is an actual question, so if you need to customize or optimize the render process on your own, you have to answer that question by returning **either** true or false.
13. If there is no custom `shouldComponentUpdate` specified, React defaults to a very smart thing that’s actually good enough in most situations.
14. First, React invokes another lifecycle method at this point, `componentWillUpdate`. React will then compute the new rendered output and compare it with the last rendered output.
15. If the rendered output is exactly the same, React does nothing (no need to talk to Mr. Browser).
16. If there is a difference, React takes that difference to the browser, as we’ve seen before.
17. In any case, since an update process happened anyway (even if the output was exactly the same), React invokes the final lifecycle method, `componentDidUpdate`.

## Methods

Lifecycle methods are custom functionality that gets executed during the different phases of a component. There are methods available when the component gets created and inserted into the DOM ([mounting](https://reactjs.org/docs/react-component.html#mounting)), when the component updates, and when the component gets unmounted or removed from the DOM.

### `componentDidMount` is where an AJAX request should be made in a React component.

This method will be executed when the component “mounts” (is added to the DOM) for the first time. This method is only executed once during the component’s life. Importantly, you can’t guarantee the AJAX request will have resolved before the component mounts. If it doesn't, that would mean that you’d be trying to setState on an unmounted component, which would not work. Making your AJAX request in `componentDidMount` will guarantee that there’s a component to update.

## useEffect Hook

you can think of `useEffect` Hook as `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` combined.

# Handling Events

Handling events with React elements is very similar to handling events on DOM elements. There are some syntactic differences:

- React events are named using camelCase, rather than lowercase.

- With JSX you pass a function as the event handler, rather than a string.

- in HTML, you can return `false` to prevent default behavior. Whereas in React you must call `preventDefault()` explicitly:

  ```JSX
  function handleClick(event) {
    event.preventDefault()
    console.log('The link was clicked.')
  }
  ```

```jsx
//Function
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
//Class
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};
  }

  handleClick() {
    console.log('The link was clicked.');
  }

  render() {
    return (
        <a href="#" onClick={handleClick}>
          Click me
        </a>
    );
  }
}

  // Wrong: handleClick is called instead of passed as a reference!
  return <button onClick={this.handleClick()}>{'Click Me'}</button>
   // Correct: handleClick is passed as a reference!
  return <button onClick={this.handleClick}>{'Click Me'}</button>
```

### Bind methods or event handlers in JSX

1. **Binding in Constructor:** In JavaScript classes, the methods are not bound by default. The same thing applies for React event handlers defined as class methods. Normally we bind them in constructor.

```jsx
class Component extends React.Componenet {
  constructor(props) {
    super(props)
    this.handleClick = this.handleClick.bind(this,event)
  }

  handleClick(event) {
    console.log('this is:', this)
  }
}
```

1. **Public class fields syntax:** If you don't like to use bind approach then *public class fields syntax* can be used to correctly bind callbacks.

```jsx
handleClick = () => {
  console.log('this is:', this)
}
<button onClick={this.handleClick}>
  {'Click me'}
</button>
```

1. **Arrow functions in callbacks:** You can use *arrow functions* directly in the callbacks.

```jsx
<button onClick={(event) => this.handleClick(event)}>
  {'Click me'}
</button>
```

# Fragments

It's common pattern in React which is used for a component to return multiple elements. *Fragments* let you group a list of children without adding extra nodes to the DOM.

```jsx
render() {
  return (
    <React.Fragment>
      <ChildA />
      <ChildB />
      <ChildC />
    </React.Fragment>
  )
}
render() {
  return (
    <>
      <ChildA />
      <ChildB />
      <ChildC />
    </>
  )
}
```

1. Fragments are a bit faster and use less memory by not creating an extra DOM node. This only has a real benefit on very large and deep trees.
2. Some CSS mechanisms like *Flexbox* and *CSS Grid* have a special parent-child relationships, and adding divs in the middle makes it hard to keep the desired layout.
3. The DOM Inspector is less cluttered.

# Pure Components

Đối với react component nó sẽ re-render mỗi khi state hay props thay đổi kéo theo các component con cũng sẽ bị re-render kể cả props truyền xuống component con là không đổi. Giải pháp đặt ra là chúng có thể bổ sung code check props vào method shouldComponentUpdate ở component để đảm bảo nó sẽ chỉ re-render mỗi khi có sự thay đổi về props. 

React.PureComponent is similar to React.Component. The difference between them is that React.Component doesn’t implement shouldComponentUpdate(), but React.PureComponent implements it with a shallow prop and state comparison.

Only extend `PureComponent` when you expect to have simple props and state, or use [`forceUpdate()`](https://reactjs.org/docs/react-component.html#forceupdate) when you know deep data structures have changed. Or, consider using [immutable objects](https://facebook.github.io/immutable-js/) to facilitate fast comparisons of nested data.

Furthermore, `React.PureComponent`’s `shouldComponentUpdate()` skips prop updates for the whole component subtree. Make sure all the children components are also “pure”.

PureComponent dùng shallow equality để kiểm tra props và state có thay đổi hay không, vậy nên trong nhiều trường hợp props và state là dạng complex và có sự thay đổi nhưng shallow equality không phát hiện ra thay đổi đó vì vậy sẽ kông render khi cần thiết.

# Higher-Order Components

A higher-order component (HOC) is an advanced technique in React for reusing component logic. HOCs are not part of the React API, per se. They are a pattern that emerges from React’s compositional nature.

Concretely, **a higher-order component is a function that takes a component and returns a new component.**

We call them **pure components** because they can accept any dynamically provided child component but they won't modify or copy any behavior from their input components.

```javascript
const EnhancedComponent = higherOrderComponent(WrappedComponent);

import {connect} from 'react-redux';
// code
export default connect(mapStateToProps, mapDispatchToProps)(EgComponent);
```

.![img](https://images.viblo.asia/d08aa39a-285d-483d-baf7-087b621aaf24.jpg)

1. Props Proxy:

   ```javascript
   function propsProxyHOC(WrappedComponent) {
       return class PropsProxy extends React.Component {
           render() {
               return <WrappedComponent {...this.props}/>
           }
       }
   }
   ```

   - CRUD `props`
   - 

2. Inheritance Inversion:

   ```javascript
   function inheritanceInversionHOC(WrappedComponent) {
       return class InheritanceInversion extends WrappedComponent {
           render() {
           return super.render()
         }
       }
   }
   ```

   

# Keys

A “key” is a special string attribute you need to include when creating arrays of elements. Keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside an array to give the elements a stable identity.

# Context

*Context* provides a way to pass data through the component tree without having to pass props down manually at every level. For example, authenticated user, locale preference, UI theme need to be accessed in the application by many components.

```jsx
const {Provider, Consumer} = React.createContext(defaultValue)

//Lets create a context with a default theme value "luna"
const ThemeContext = React.createContext('luna');
// Create App component where it uses provider to pass theme value in the tree
class App extends React.Component {
  render() {
    return (
      <ThemeContext.Provider value="nova">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}
// A middle component where you don't need to pass theme prop anymore
function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}
// Lets read theme value in the button component to use
class ThemedButton extends React.Component {
  static contextType = ThemeContext;
  render() {
    return <Button theme={this.context} />;
  }
}
```

# Portal

*Portal* is a recommended way to render children into a DOM node that exists outside the DOM hierarchy of the parent component.

```jsx
ReactDOM.createPortal(child, container) // modal tooltip
```

The first argument is any render-able React child, such as an element, string, or fragment. The second argument is a DOM element

# Refs and the DOM

The **ref** is used to return a reference to the element. They can be useful when we need direct access to DOM element or an instance of a component.

Refs provide a way to access DOM nodes or React elements created in the render method.

In the typical React dataflow, **props are the only way** that parent components interact with their children. To modify a child, you re-render it with new props. However, there are a few cases where you need to imperatively modify a child outside of the typical dataflow.

The child to be modified could be an instance of a React component, or it could be a DOM element. For both of these cases, React provides an escape hatch.

There are a few good use cases for refs:

- Managing focus, text selection, or media playback.
- Triggering imperative animations.
- Integrating with third-party DOM libraries.

## Accessing Refs

The value of the ref differs depending on the type of the node:

- When the `ref` attribute is used on an HTML element, the `ref` created in the constructor with `React.createRef()` receives the underlying DOM element as its `current` property.
- When the `ref` attribute is used on a custom class component, the `ref` object receives the mounted instance of the component as its `current`.

### Adding a Ref to a DOM Element

This code uses a `ref` to store a reference to a DOM node:

```javascript
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    // create a ref to store the textInput DOM element
    this.textInput = React.createRef();
    this.focusTextInput = this.focusTextInput.bind(this);
  }

  focusTextInput() {
    // Explicitly focus the text input using the raw DOM API
    // Note: we're accessing "current" to get the DOM node
    this.textInput.current.focus();
  }

  render() {
    // tell React that we want to associate the <input> ref
    // with the `textInput` that we created in the constructor
    return (
      <div>
        <input
          type="text"
          ref={this.textInput} />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
        />
      </div>
    );
  }
}
```

React will assign the `current` property with the DOM element when the component mounts, and assign it back to `null` when it unmounts. `ref` updates happen before `componentDidMount` or `componentDidUpdate` lifecycle methods.

### Adding a Ref to a Class Component

If we wanted to wrap the `CustomTextInput` above to simulate it being clicked immediately after mounting, we could use a ref to get access to the custom input and call its `focusTextInput` method manually:

```javascript
class AutoFocusTextInput extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = React.createRef();
  }

  componentDidMount() {
    this.textInput.current.focusTextInput();
  }

  render() {
    return (
      <CustomTextInput ref={this.textInput} />
    );
  }
}
```

Note that this only works if `CustomTextInput` is declared as a class

### Refs and Function Components

If you want to allow people to take a `ref` to your function component, you can use [`forwardRef`](https://reactjs.org/docs/forwarding-refs.html) (possibly in conjunction with [`useImperativeHandle`](https://reactjs.org/docs/hooks-reference.html#useimperativehandle)), or you can convert the component to a class.

You can, however, **use the `ref` attribute inside a function component** as long as you refer to a DOM element or a class component:

```javascript
function CustomTextInput(props) {
  // textInput must be declared here so the ref can refer to it
  let textInput = React.createRef();

  function handleClick() {
    textInput.current.focus();
  }

  return (
    <div>
      <input
        type="text"
        ref={textInput} />
      <input
        type="button"
        value="Focus the text input"
        onClick={handleClick}
      />
    </div>
  );
}
```

## Forwarding Refs

Ref forwarding is a technique for automatically passing a [ref](https://reactjs.org/docs/refs-and-the-dom.html) through a component to one of its children. 

```jsx
const ButtonElement = React.forwardRef((props, ref) => (
  <button ref={ref} className="CustomButton">
    {props.children}
  </button>
));

// Create ref to the DOM button:
const ref = React.createRef();
<ButtonElement ref={ref}>{'Forward Ref'}</ButtonElement>
```

## Callback Refs

Instead of passing a `ref` attribute created by `createRef()`, you pass a function. The function receives the React component instance or HTML DOM element as its argument, which can be stored and accessed elsewhere.

```jsx
class MyComponent extends Component {
  constructor(props){
    super(props);
    this.node = createRef();
  }
  componentDidMount() {
    this.node.current.scrollIntoView();
  }

  render() {
    return <div ref={this.node} />
  }
}
```

```jsx
class MyComponent extends Component {
  renderRow = (index) => {
    // This won't work. Ref will get attached to DataTable rather than MyComponent:
    return <input ref={'input-' + index} />;

    // This would work though! Callback refs are awesome.
    return <input ref={input => this['input-' + index] = input} />;
  }

  render() {
    return <DataTable data={this.props.data} renderRow={this.renderRow} />
  }
}
```

# Fiber

Fiber is the new *reconciliation* engine or reimplementation of core algorithm in React v16. 

# CRA

The `create-react-app` CLI tool allows you to quickly create & run React applications with no configuration step.

1. React, JSX, ES6, and Flow syntax support.
2. Language extras beyond ES6 like the object spread operator.
3. Autoprefixed CSS, so you don’t need -webkit- or other prefixes.
4. A fast interactive unit test runner with built-in support for coverage reporting.
5. A live development server that warns about common mistakes.
6. A build script to bundle JS, CSS, and images for production, with hashes and sourcemaps.

# How to use innerHTML in React?

The `dangerouslySetInnerHTML` attribute is React's replacement for using `innerHTML` in the browser DOM. Just like `innerHTML`, it is risky to use this attribute considering cross-site scripting (XSS) attacks. You just need to pass a `__html` object as key and HTML text as value.

In this example MyComponent uses `dangerouslySetInnerHTML` attribute for setting HTML markup:

```jsx
function createMarkup() {
  return { __html: 'First &middot; Second' }
}

function MyComponent() {
  return <div dangerouslySetInnerHTML={createMarkup()} />
}
```

# Best practice

## What is the recommended ordering of methods in component class?
Recommended ordering of methods from mounting to render stage:

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
click handlers or event handlers like onClickSubmit() or onChangeDescription()
getter methods for render like getSelectReason() or getFooterContent()
optional render methods like renderNavigation() or renderProfilePicture()
render()

## What is a switching component?

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

## How to re-render the view when the browser is resized?

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

```jsx
componentWillUpdate(object nextProps, object nextState)
componentDidUpdate(object prevProps, object prevState)
```

## What is the recommended approach of removing an array element in React state?

The better approach is to use `Array.prototype.filter()` method.

For example, let's create a `removeItem()` method for updating the state.

```jsx
removeItem(index) {
  this.setState({
    data: this.state.data.filter((item, i) => i !== index)
  })
}
```

## use React without rendering HTML?

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

## How to focus an input element on page load?

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

## What are the possible ways of updating objects in state?

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

or just run `set HTTPS=true && npm start`

## How to update a component every second?

You need to use `setInterval()` to trigger the change, but you also need to clear the timer when the component unmounts to prevent errors and memory leaks.

```jsx
componentDidMount() {
  this.interval = setInterval(() => this.setState({ time: Date.now() }), 1000)
}

componentWillUnmount() {
  clearInterval(this.interval)
}
```

## How to define constants in React?

You can use ES7 `static` field to define constant.

```jsx
class MyComponent extends React.Component {
  static DEFAULT_PAGINATION = 10
}
```

## How to programmatically trigger click event in React?

You could use the ref prop to acquire a reference to the underlying `HTMLInputElement` object through a callback, store the reference as a class property, then use that reference to later trigger a click from your event handlers using the `HTMLElement.click` method. This can be done in two steps:

1. Create ref in render method:

   ```jsx
   <input ref={input => this.inputElement = input} />
   ```

2. Apply click event in your event handler:

   ```jsx
   this.inputElement.click()
   ```

## What are the common folder structures for React?

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

## How to make AJAX call and in which component lifecycle methods should I make an AJAX call?

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

# React Router

## What is React Router?

React Router is a powerful routing library built on top of React that helps you add new screens and flows to your application incredibly quickly, all while keeping the URL in sync with what's being displayed on the page.

## How React Router is different from history library?

React Router is a wrapper around the `history` library which handles interaction with the browser's `window.history` with its browser and hash histories. It also provides memory history which is useful for environments that don't have global history, such as mobile app development (React Native) and unit testing with Node.

## What are the <Router> components of React Router v4?
React Router v4 provides below 3 <Router> components:

1. <BrowserRouter>
2. <HashRouter>
3. <MemoryRouter>

The above components will create browser, hash, and memory history instances. React Router v4 makes the properties and methods of the history instance associated with your router available through the context in the router object.

## What is the purpose of `push()` and `replace()` methods of `history`?

A history instance has two methods for navigation purpose.

1. `push()`
2. `replace()`

If you think of the history as an array of visited locations, `push()` will add a new location to the array and `replace()` will replace the current location in the array with the new one.

## How do you programmatically navigate using React Router v4?

There are three different ways to achieve programmatic routing/navigation within components.

1. **Using the `withRouter()` higher-order function:**

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

2. **Using `` component and render props pattern:**

   The `` component passes the same props as `withRouter()`, so you will be able to access the history methods through the history prop.

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

3. **Using context:**

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

## How to get query parameters in React Router v4?

The ability to parse query strings was taken out of React Router v4 because there have been user requests over the years to support different implementation. So the decision has been given to users to choose the implementation they like. The recommended approach is to use query strings library.

```jsx
const queryString = require('query-string');
const parsed = queryString.parse(props.location.search);
```

You can also use `URLSearchParams` if you want something native:

```jsx
const params = new URLSearchParams(props.location.search)
const foo = params.get('name')
```

You should use a *polyfill* for IE11.

## Why you get "Router may have only one child element" warning?

You have to wrap your Route's in a `` block because `` is unique in that it renders a route exclusively.

At first you need to add `Switch` to your imports:

```jsx
import { Switch, Router, Route } from 'react-router'
```

Then define the routes within `` block:

```jsx
<Router>
  <Switch>
    <Route {/* ... */} />
    <Route {/* ... */} />
  </Switch>
</Router>
```

## How to pass params to `history.push` method in React Router v4?

While navigating you can pass props to the `history` object:

```jsx
this.props.history.push({
  pathname: '/template',
  search: '?name=sudheer',
  state: { detail: response.data }
})
```

The `search` property is used to pass query params in `push()` method.

## How to implement *default* or *NotFound* page?

A `` renders the first child `` that matches. A `` with no path always matches. So you just need to simply drop path attribute as below

```jsx
<Switch>
  <Route exact path="/" component={Home}/>
  <Route path="/user" component={User}/>
  <Route component={NotFound} />
</Switch>
```

## How to get history on React Router v4?

1. Create a module that exports a `history` object and import this module across the project.

   For example, create `history.js` file:

   ```jsx
   import { createBrowserHistory } from 'history'
   
   export default createBrowserHistory({
     /* pass a configuration object here if needed */
   })
   ```

2. You should use the `` component instead of built-in routers. Imported the above `history.js` inside `index.js` file:

   ```jsx
   import { Router } from 'react-router-dom'
   import history from './history'
   import App from './App'
   
   ReactDOM.render((
     <Router history={history}>
       <App />
     </Router>
   ), holder)
   ```

3. You can also use push method of `history` object similar to built-in history object:

   ```jsx
   // some-other-file.js
   import history from './history'
   
   history.push('/go-here')
   ```

## How to perform automatic redirect after login?

The `react-router` package provides `` component in React Router. Rendering a `` will navigate to a new location. Like server-side redirects, the new location will override the current location in the history stack.

```
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
![image-20200611175535965](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611175535965.png)

https://github.com/huynonstop/react-redux-links

https://goshakkk.name/on-react/

https://academind.com/learn/react/

# React is a JavaScript “library”

https://stackoverflow.com/questions/51506440/mvvm-architectural-pattern-for-a-reactjs-application

[https://www.quora.com/Which-architectural-pattern-does-React-Native-use-MVC-or-MVVM#:~:text=to%20use%20...-,Actually%20it%27s%20up%20to%20you.,Flux%20or%20any%20unidirectional%20architecture.](https://www.quora.com/Which-architectural-pattern-does-React-Native-use-MVC-or-MVVM#:~:text=to use ...-,Actually it's up to you.,Flux or any unidirectional architecture.)

React is an open-source JavaScript library created by Facebook for building complex, interactive UIs in web and mobile applications. React’s core purpose is to build UI components; it is often referred to as just the “V” (View) in an “MVC” architecture.

# Advantages of ReactJS

1. Increases the application’s performance with Virtual DOM handled by React
2. JSX makes code is easy to read and write. It is also really easy to see the layout, or how components are plugged/combined with each other.
3. It renders both on client and server side. This enables improves SEO.
4. Easy to integrate with other frameworks (Angular, BackboneJS) since it is only a view library
5. Easy to write UI Test cases and integration with tools such as JEST

# One-way data binding

![img](https://viblo.asia/uploads/29fea837-c6e8-416f-a11f-d708b33c2a78.png)

# JSX

JSX is a syntax extension to JavaScript. It is similar to a template language, but it has full power of JavaScript. JSX gets compiled to `React.createElement()` calls which return plain JavaScript objects called “React elements”.

## element `className`

class is a keyword in JavaScript, and JSX is an extension of JavaScript. That's the principal reason why React uses `className` instead of class

# Components

Components are the **core building block of React apps**. Actually, React really is just a library for creating components in its core.

Each component needs to return/ render some JSX code (React element) - it defines HTML that React should render to the real DOM in the end. Components can return other components, arrays, strings and numbers.

Conceptually, components are JavaScript functions**. They accept arbitrary inputs (called “props”) and return React elements describing what should appear on the screen.

1. **Functional components** (also referred to as "presentational", "dumb" or "stateless" components)

   ````jsx
   const cmp = () => { return <div>some JSX</div> }
   ````

2. **class-based components** (also referred to as "containers", "smart" or "stateful" components)  

   ```jsx
   class Cmp extends Component { 
       render () { return <div>some JSX</div> } 
   }
   ```

A good rule of thumb is that if a part of your UI is used several times (Button, Panel, Avatar), or is complex enough on its own (App, FeedStory, Comment), it is a good candidate to be a reusable component

![image-20200623234919972](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200623234919972.png)

# Render Elements

https://reactjs.org/docs/rendering-elements.html

React elements are the building blocks of React applications.

An element describes what you want to see on the screen.

```jsx
const element = <h1>Hello, world</h1>;
```

Unlike browser DOM elements, React elements are plain objects, and are cheap to create. 

React DOM takes care of updating the DOM to match the React elements.

React elements are **immutable**. An element is like a single frame in a movie: it represents the UI at a certain point in time.

Elements are what components are “made of”

React DOM compares the element and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state.

![DOM inspector showing granular updates](https://reactjs.org/c158617ed7cc0eac8f58330e49e48224/granular-dom-updates.gif)

## Adjacent JSX rendering

```jsx
return [
    <ComponentA></ComponentA>,
    <ComponentB></ComponentB>,
]
//or 
return <>
         <ComponentA></ComponentA>
    	 <ComponentB></ComponentB>
	   </>
//or
return <React.Fragment>
	<ComponentA></ComponentA>
	<ComponentB></ComponentB>
</React.Fragment>
```



## conditional rendering

https://reactjs.org/docs/conditional-rendering.html

```jsx
const Component = (props) => {
    return (
    	props.name ? //can be a state
        	<h1>No name available</h1> : 
        	<Hello name={props.name}>
            	{props.male ? "guys" : "girl"}
        	</Hello>
    )
}
```

## list rendering

https://reactjs.org/docs/lists-and-keys.html

```jsx
/* 
	persons = [
		{name:"max",age: 28,id: 1},
		{name:"huy",age: 27,id: 1},
	]
*/
const Persons = (props) => {
    return (
    	props.persons.map(p => <Person key={p.id} age={p.age}>Hi! {p.name}</Person>) //can be a state
    )
}
```

`Key`s help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside an array to give the elements a stable identity.

React need our help to keep tracking (find out) which element in the list changing and which didn't by using `key` property. So React can re-renders the element which did change and not the whole list

# props

`props` are inputs to a React component (**data passed down** the component tree)

`props` are read-only.

can trigger an UI update

```jsx
class MyComponent extends React.Component {
  constructor(props) { // use props inside constructor
    super(props)
	this.state = {
        title: this.props.title
    }
  }
 // or simple like this
 state = {
 	title: this.props.title
  }
 render() {
     return <h1>{this.props.title}</h1>
 }
}
const MyComponent = props => {
    return <h1>{props.title}</h1>
}
```

```jsx
<MyComponent title="huy abc"/>
// <h1>huy abc</h1>
```

## props.children

`props.children` is available on every component. That allow you to pass components as data to other components, just like any other prop you use. It contains the content between the opening and closing tags of a component. For example:

```jsx
const Welcome = props => {
    return <h1>{props.children}</h1>
}
<Welcome>Hello world!</Welcome>
// <h1>Hello world!</h1>
```

## propTypes

https://reactjs.org/docs/typechecking-with-proptypes.html

```jsx
import React from 'react'
import PropTypes from 'prop-types'
// {number,string,array,object,func,node,element,bool,symbol,any}
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

# state

![state](https://github.com/sudheerj/reactjs-interview-questions/raw/master/images/state.jpg)

A component needs `state` when some data associated with it changes over time.

`state` is **managed by the component itself.** 

A component cannot change its `props`, but it can change its `state`.

Changes to state also trigger an UI update.

## setState

When `setState` is called, it **merge the object you passed** with the current state of the component **asynchronously**.  It tells React that this component and its children need to be re-rendered.

`setState` allow you have a callback and pass a function instead of an object

```jsx
class CounterButton extends React.Component {
  state = {
    clickCounter: 0,
    currentTimestamp: new Date(),
  };
  
  handleClick = () => {
    this.setState(
        (prevState,prevProps) => ({
            clickCounter: prevState.clickCounter + 1 //merged 
        }), 
        () => console.log("Cliked to",this.state.clickCounter)
	);
  };
  
  componentDidMount() {
   setInterval(() => {
     this.setState({ currentTimestamp: new Date() }) // merged
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

![image-20200615215913665](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200615215913665.png)

### Dynamic key name

```
handleInputChange(event) {
  this.setState({ [event.target.id]: event.target.value })
}
```

## Using setState Correctly

1. Do Not Modify State Directly

   If you try to update state directly then it won't re-render the component.

2. Updating State **Immutably**

3. State Updates May Be Asynchronous

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

4. `setState` Updates are Merged

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

# Handling Events

https://reactjs.org/docs/events.html#supported-events

Handling events with React elements is very similar to handling events on DOM elements. There are some syntactic differences:

React events are named using camelCase, rather than lowercase.

With JSX you pass a function as the event handler, rather than a string.

```jsx
    <a href="#" onClick={handleClick}>
      Click me
    </a>
```

in HTML, you can return `false` to prevent default behavior. Whereas in React you must call `preventDefault()` explicitly:

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
```

Can pass method reference as props to handle event in child component

```jsx
<ChildComponent click={handleClick}></ChildComponent>
```

## Bind methods or event handlers in JSX

### Binding in Constructor

In JavaScript classes, the methods are not bound by default. The same thing applies for React event handlers defined as class methods. Normally we bind them in constructor.

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

### Public class fields syntax

If you don't like to use bind approach then *public class fields syntax* can be used to correctly bind callbacks.

```jsx
handleClick = () => {
  console.log('this is:', this)
}
<button onClick={this.handleClick}>
  {'Click me'}
</button>
```

### Arrow functions in callbacks

You can use *arrow functions* directly in the callbacks.

```jsx
<button onClick={(event) => this.handleClick(event)}>
  {'Click me'}
</button>
```

## 2 ways binding

```jsx
class MyName extends React.Componenet {
    state = {
        name: "abc"
    };
	handleChange = (e) => {
        setState({
            name: e.target.value
        })
    }
    render() {
        return (
        	<div>
            	<h1>{this.state.name}</h1>
                <input onChange={handleChange} value={this.state.name}></input>
            </div>
        )
    }
}
```

# Forms

https://reactjs.org/docs/forms.html

https://goshakkk.name/controlled-vs-uncontrolled-inputs-react/

https://www.codebeast.dev/react-forms-then-and-now/

https://goshakkk.name/array-form-inputs/

https://goshakkk.name/form-recipe-disable-submit-button-react/

https://goshakkk.name/instant-form-fields-validation-react/

https://goshakkk.name/submit-time-validation-react/

There are two conventional ways to manage form states in React. The first one takes state management control from the form fields and lifts the control to React state using `setState`. The second leaves state management to the form fields but extracts the values using React’ s `ref`.

## Controlled Components

Controlled components are the opposites of uncontrolled. They are the components that use states for data preservation.

A component that controls the input elements within the forms on subsequent user input is called **Controlled Component**, i.e, every state mutation will have an associated handler function.

form data is handled by a React component

We can instead, use one handler that deals with all the fields’ needs

```js
handleChange = event => {
  const target = event.target;
  const value = target.type === 'checkbox' ? target.checked : target.value;
  const name = target.name;
  this.setState({
    [name]: value,
  });
};
```

## Uncontrolled Components

Uncontrolled components avoid preserving data in state but instead, preserve data in what React calls refs

An *uncontrolled component* works like form elements do outside of React.

form data is handled by the DOM itself

In other words, **you have to ‘pull’ the value from the field when you need it**. This can happen when the form is submitted.

When a user inputs data into a form field (an input box, dropdown, ...) the updated information is reflected without React needing to do anything. And you query the DOM using a ref to find its current value when you need it. This is a bit more like traditional HTML.

In React, an `<input type="file" /> ` is always an uncontrolled component

initial value by `defaultValue` attribute 

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

| feature                                   | uncontrolled | controlled |
| ----------------------------------------- | ------------ | ---------- |
| one-time value retrieval (e.g. on submit) | ✅            | ✅          |
| validating on submit                      | ✅            | ✅          |
| instant field validation                  | ❌            | ✅          |
| conditionally disabling submit button     | ❌            | ✅          |
| enforcing input format                    | ❌            | ✅          |
| several inputs for one piece of data      | ❌            | ✅          |
| dynamic inputs                            | ❌            | ✅          |

## Validation

- Validate.js (you may import its functionality into your React projects): https://validatejs.org/
- Get more ideas about potential validation approaches: https://react.rocks/tag/Validation
- input html pattern

# Lifecycle

https://reactjs.org/docs/state-and-lifecycle.html#adding-lifecycle-methods-to-a-class

The component lifecycle has three distinct lifecycle phases:

1. **Mounting:** The component is ready to mount in the browser DOM. This phase covers initialization from `constructor()`, `getDerivedStateFromProps()`, `render()`, and `componentDidMount()` lifecycle methods.
2. **Updating:** In this phase, the component get updated in two ways, sending the new props and updating the state either from `setState()` or `forceUpdate()`. This phase covers `getDerivedStateFromProps()`, `shouldComponentUpdate()`, `render()`, `getSnapshotBeforeUpdate()` and `componentDidUpdate()` lifecycle methods.
3. **Unmounting:** In this last phase, the component is not needed and get unmounted from the browser DOM. This phase includes `componentWillUnmount()` lifecycle method.

[![image-20200208132318599](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200208132318599.png)](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

React internally has a concept of phases when applying changes to the DOM. They are separated as follows

1. **Render** The component will render without any side-effects. This applies for Pure components and in this phase, React can pause, abort, or restart the render.
2. **Pre-commit** Before the component actually applies the changes to the DOM, there is a moment that allows React to read from the DOM through the `getSnapshotBeforeUpdate()`.
3. **Commit** React works with the DOM and executes the final lifecycles respectively `componentDidMount()` for mounting, `componentDidUpdate()` for updating, and `componentWillUnmount()` for unmounting.

## Mounting

![image-20200623235236575](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200623235236575.png)

![image-20200624001528878](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200624001528878.png)

![image-20200624001758575](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200624001758575.png)

If  you try to setState on an unmounted component, which would not work.

Don't setState immediately inside componentDidMount (trigger a re-rendering cycle), setState when side-effect finishing callback (then in a promise,...)

## Updating

![image-20200624000344927](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200624000344927.png)

Don't setState inside componentDidUpdate without control (setState => update => setState => Loop...) 

![image-20200624001411868](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200624001411868.png)

### optimize 

#### shouldCompoentUpdate

remove useless re-render, when props change

![image-20200624003938434](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200624003938434.png)

#### pure component

remove useless re-render, when props change and you want to check all props 

![image-20200624004138678](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200624004138678.png)

#### React.memo

react which memorize result (store a snapshot of the function => store element of component also) and only input change, it will re calculate (re-render for component)

```js
React.memo(FunctionName)
```

#### when

optimize when a components could change because something not relative to it when parent component update.

don't optimize  when in almost case parent change the components change also

## useEffect Hook

https://reactjs.org/docs/hooks-effect.html

you can think of `useEffect` Hook as `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` combined.

### ComponentDidMount

```js
//Class
componentDidMount() {    
    console.log('I just mounted!');
} 
//Hooks
useEffect(() => {    
    console.log('I just mounted!');
}, [])
```

### ComponentWillUnmount

```js
//Class
componentWillUnmount() {    
    console.log('I am unmounting');
} 
//Hooks
useEffect(() => {    
    return () => console.log('I am unmounting');
}, [])
```

### ComponentWillReceiveProps

```js
//Class
componentWillReceiveProps(nextProps) {    
    if (nextProps.count !== this.props.count) {        
        console.log('count changed', nextProps.count);    
    }
} 
//Hooks
useEffect(() => {    
    console.log('count changed', props.count);
}, [props.count])
```

# Pure Components

`React.PureComponent` is similar to `React.Component`. The difference between them is that `React.Component` doesn’t implement `shouldComponentUpdate()`, but `React.PureComponent` implements it with a shallow prop and state comparison.

Only extend `PureComponent` when you expect to have simple props and state

when you know deep data structures have changed. Or, consider using [immutable objects](https://facebook.github.io/immutable-js/) to facilitate fast comparisons of nested data.

Furthermore, `React.PureComponent`’s `shouldComponentUpdate()` skips prop updates for the whole component subtree. Make sure all the children components are also “pure”.

# Higher-Order Components

https://reactjs.org/docs/higher-order-components.html

A higher-order component (HOC) is an advanced technique in React for reusing component logic. HOCs are not part of the React API, per se. They are a pattern that emerges from React’s compositional nature.

Concretely, **a higher-order component is a function that takes a component and returns a new component.**

We call them **pure components** because they can accept any dynamically provided child component but they won't modify or copy any behavior from their input components.

.![img](https://images.viblo.asia/d08aa39a-285d-483d-baf7-087b621aaf24.jpg)

![image-20200624010831877](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200624010831877.png)

![image-20200624010927236](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200624010927236.png)

# render Props

https://reactjs.org/docs/render-props.html

https://formik.org/docs/overview

https://www.youtube.com/watch?v=oiNtnehlaTo

# ![image-20200831130138878](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200831130138878.png)

```jsx
<Formik onSubmit={this.handleSubmit}>
  {({ values, handleChange, handleSubmit }) => (
    <form className={styles.form} onSubmit={handleSubmit}>
      <div className={styles.formGroup}>
        <label htmlFor="name">Full Name</label>
        <input
          type="text"
          name="name"
          value={values.name}
          onChange={handleChange}
        />
      </div>
      <div className={styles.formGroup}>
        <label htmlFor="email">Email</label>
        <input
          type="email"
          name="email"
          value={values.email}
          onChange={handleChange}
        />
      </div>
      <div className={styles.inlineGroup}>
        <div className={styles.formGroup}>
          <label htmlFor="meal">Meal Preference</label>
          <select name="meal" value={values.meal} onChange={handleChange}>
            <option value="1">Jollof Rice</option>
            <option value="2">Fried Rice</option>
          </select>
        </div>
        <div className={styles.formGroup}>
          <label htmlFor="meal">Is Going?</label>
          <input
            name="isGoing"
            type="checkbox"
            value={values.isGoing}
            onChange={handleChange}
          />
        </div>
      </div>
      <div className={styles.formGroup}>
        <button type="submit">Submit</button>
      </div>
    </form>
  )}
</Formik>
```

What should we take away from refactoring with render props? Notice how we are longer relying on the state. In fact, we can delete the state object now. Here are clear reasons why render props are preferred:

1. Form states are ephemeral — they last for a very short time. Why bother with redux or even component state with such a temporary state. Formik is now handling the short-lived temporary state.
2. We can reuse state and form logic without bothering about the details — just focus on creating and styling fields; leave the rest to Formik.

# Portal

*Portal* is a recommended way to render children into a DOM node that exists outside the DOM hierarchy of the parent component.

```jsx
ReactDOM.createPortal(child, container) // modal tooltip
```

The first argument is any render-able React child, such as an element, string, or fragment. The second argument is a DOM element

# Error

https://reactjs.org/docs/error-boundaries.html

# Redux & Context API

Context API: lightweight store management, couple with a context, complex provider in big apps

Redux: handle asynchronous actions inside of redux middleware and compose ability of components

# CRA

The `create-react-app` CLI tool allows you to quickly create & run React applications with no configuration step.

# Compilers (Babel)

A JavaScript compiler takes JavaScript code, transforms it and returns JavaScript code in a different format. The most common use case is to take ES6 syntax and transform it into syntax that older browsers are capable of interpreting

# Bundlers (Webpack and Browserify)

Bundlers take JavaScript and CSS code written as separate modules (often hundreds of them), and combine them together into a few files better optimized for the browsers.

# Composition vs Inheritance

React has a powerful composition model, and we recommend using composition instead of inheritance to reuse code between components.

https://reactjs.org/docs/composition-vs-inheritance.html



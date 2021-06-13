# How DOM is build

https://developer.mozilla.org/en-US/docs/Mozilla/Introduction_to_Layout_in_Mozilla

<img src="assets/react-render/Gecko_Overview_9.png" alt="img" style="zoom:150%;background: white" />

Each time something in the DOM changes. Since DOM is represented as a tree structure, changes to the DOM is pretty quick but the changed element, and it’s children’s has to go through **Reflow/Layout** stage and then the changes has to be **Re-painted** which are slow. Therefore more the items to reflow/repaint, more slow your app becomes.

Browser makers are continually working to shorten the time it takes to repaint the screen. The biggest thing that can be done is to minimize and batch the DOM changes that make redraws necessary.

This strategy of reducing and batching DOM changes, taken to another level of abstraction, is the idea behind React Virtual DOM.

# Virtual DOM and ReactJS

The virtual DOM (VDOM) is a programming concept where an ideal, or “virtual”, representation of a UI is kept in memory and synced with the “real” DOM

https://github.com/reactjs/react-basic

## How React works with virtual DOM 

When we tell React to render a tree of elements in the browser, it first generates a virtual representation of that tree and keeps it around in memory for later. Then it’ll proceed to perform the DOM operations that will make the tree show up in the browser.

When **state changes** in a component , which identifies what has **changed in the virtual DOM**. 

React **update** the tree of elements it previously rendered, it does not discard what has already been rendered. Instead it **generates** **a new virtual representation** of the updated tree. Now React has **2 versions of the DOM** in memory!

React **compare the 2 virtual versions** of the tree that it has in memory, **compute the differences** between them, figure out what sub-trees in the main tree **need to be updated **(diffing algorithm), and only **update these changed sub-trees** in the DOM . (this process is "reconciliation")

In summary, here’s what happens when you try to update the DOM in React:

1. The entire virtual DOM gets updated.
2. The virtual DOM gets compared to what it looked like before you updated it. React figures out which objects have changed.
3. The changed objects, and the changed objects only, get updated on the *real* DOM.
4. Changes on the real DOM cause the screen to change.

## How React update real DOM

![image-20200624005326042](assets/react-render/image-20200624005326042.png)

1. Whenever anything may have changed, the components will be **re-rendered in a Virtual DOM representation**. (render)
2. The **difference** between the previous Virtual DOM representation and the new one will be **calculated**. (reconciliation)
3. The **real DOM will be updated with what has actually changed**. This is very much like applying a patch. (commit, *painting*)

> \* The “render” phase: create React elements `React.createElement`
> \* The “reconciliation” phase: compare previous elements with the new ones 
> \* The “commit” phase: update the DOM (if needed).

# Why React "fast"

React only really re-render the changed part of the DOM, not all the DOM 

=> not re-calculate, re-building, re-painting all element in the real DOM of browser

=> make it an efficiency way to works with Browser UI.

# React Work Phase

https://reactjs.org/docs/react-component.html

https://techdoma.in/react-16-tutorial/what-are-render-phase-and-commit-phase-in-react/

https://dev.to/spukas/react-work-phases-4eaj

When it's time to render the page (usually caused by calling `this.setState` somewhere), Conceptually, React does work in two phases:

- The **render** phase determines what changes need to be made to e.g. the DOM. During this phase, React calls `render` and then compares the result to the previous render.
- The **commit** phase is when React applies any changes. (In the case of React DOM, this is when React inserts, updates, and removes DOM nodes.) React also calls lifecycles like `componentDidMount` and `componentDidUpdate` during this phase.

Both phases run synchronously and make sure, that user sees already calculated styles, layout and updated UI.

In render phase, React DOM **calculates the changes** that needs to be committed to DOM, while in commit phase, React DOM **actually commits those changes**. 

One of the major difference between render phase and commit phase is that **render phase can be called multiple times**, but **commit phase is called only once for a change**.



![image-20200830171544982](assets/react-render/image-20200830171544982.png)

![image-20200830171607515](assets/react-render/image-20200830171607515.png)

![image-20200830225417685](assets/react-render/image-20200830225417685.png)

## Render Phase

When the time comes to render a page, usually caused by a change of component's state or props, React does a process, called **reconciliation**.

First, it creates a virtual DOM by recursively calling components render functions down the tree until all React elements are returned.

Second, it compares that virtual DOM with the last render looking for differences.

> The commit phase is usually very fast, but rendering can be slow.
>
> For this reason, the upcoming concurrent mode (which is not enabled by default yet) breaks the rendering work into pieces, pausing and resuming the work to avoid blocking the browser. This means that React may **invoke render phase lifecycles more than once** before committing, or it may invoke them without committing at all (because of an error or a higher priority interruption).
>
> => not contain side-effects
>
> *The render method just creates the fiber node but doesn’t paint anything yet*



## Commit (*painting*) Phase

This is the phase where React directly applies the changes to the DOM (inserts new, updates existing or removes unnecessary DOM nodes.)

> During this process it's possible that some components got created for the first time (ie, "mounted"), or they got their props changed (ie, "updated"), or they were removed entirely ("unmounted"). Components that had this happen to them will have their `componentDidMount`, `componentDidUpdate` and `componentWillUnmount` functions called as appropriate.
>
> If the state or view changes as a side-effect, the render phase will be triggered safely again.

And now the rendering is done and the page has been updated. 

# Reconciliation

https://reactjs.org/docs/implementation-notes.html

https://reactjs.org/docs/codebase-overview.html

https://reactjs.org/docs/reconciliation.html

**reconciliation**

The algorithm React uses to diff one tree with another to determine which parts need to be changed.

**update**

A change in the data used to render a React app. Usually the result of `setState`. Eventually results in a re-render.

## Motivation

When you use React, at a single point in time you can think of the `render()` function as creating a tree of React elements. On the next state or props update, that `render()` function will return a different tree of React elements. React then needs to figure out how to efficiently update the UI to match the most recent tree. So that component updates are predictable while being fast enough for high-performance apps.



## The Diffing Algorithm of React

https://medium.com/@abdellani/how-does-the-diff-algorithm-work-in-react-js-c4296548f84b

React implements a heuristic O(n) algorithm based on two assumptions:

1. Different component types are assumed to generate substantially different trees. React will not attempt to diff them, but rather replace the old tree completely.
2. Diffing of lists is performed using keys. Keys should be "stable, predictable, and unique."

https://www.cronj.com/blog/diff-algorithm-implemented-reactjs/

When **diffing two trees**, React first compares the two root elements. The behavior is different depending on the types of the root elements.

- If the root element is different , it completely tears down the old tree and begins from the root of the new tree
- If the root element is the same, then the algorithm compares each and every difference in attributes keeps the same valued attributes and changes only the new/differed attributes.
- When a component updates, the instance stays the same, so that state is maintained across renders. React updates the props of the underlying component instance to match the new element
  Next, the `render()` method is called and the diff algorithm recurses on the previous result and the new result.
- By default, when recursing on the children of a DOM node, React just iterates over both lists of children at the same time and generates a mutation whenever there’s a difference.
  When children have **keys**, React uses the key to match children in the original tree with children in the subsequent tree.

### Elements Of Different Types

Whenever the root elements have different types, React will tear down the old tree and build the new tree from scratch. Going from `<a>` to `<img>`, or from `<Article>` to `<Comment>`, or from `<Button>` to `<div>` - any of those will lead to a full rebuild. Any state associated with the old tree is lost.

```jsx
<div>
  <Counter /> => Destroyed componentWillUnmount()
</div>

<span>
  <Counter /> => Rebuild componentWillMount() and then componentDidMount()
</span>
```

### **DOM** Elements Of The Same Type

When comparing two React DOM elements of the same type, React looks at the attributes of both, keeps the same underlying DOM node, and only updates the **changed** attributes.

```jsx
<div className="before" title="stuff" />

<div className="after" title="stuff" />
```

After handling the DOM node, React then recurses on the children.

### **Component** Elements Of The Same Type

When a component updates, the instance stays the same, so that state is maintained across renders.

React updates the props of the underlying component instance to match the new element, and calls `componentWillReceiveProps()` and `componentWillUpdate()` on the underlying instance.

Next, the `render()` method is called and the diff algorithm recurses on the previous result and the new result.

### Recursing On Children

By default, when recursing on the children of a DOM node, React just iterates over both lists of children at the same time and generates a mutation whenever there’s a difference.

```jsx
<ul>
  <li>Duke</li>
  <li>Villanova</li>
</ul>

<ul>
  <li>Connecticut</li>
  <li>Duke</li>
  <li>Villanova</li>
</ul>
```

React will mutate every child instead of realizing it can keep the `<li>Duke</li>` and `<li>Villanova</li>` subtrees intact. This inefficiency can be a problem.

### Keys

https://reactjs.org/docs/lists-and-keys.html

Keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity

Keys used within arrays should be unique among their siblings

https://stackoverflow.com/questions/42801343/what-is-the-significance-of-keys-in-reactjs/42801409

https://dev.to/francodalessio/understanding-the-importance-of-the-key-prop-in-react-3ag7

https://coderwall.com/p/jdybeq/the-importance-of-component-keys-in-react-js

https://stackoverflow.com/questions/38903698/the-importance-of-component-keys-in-react-js

 When children have keys, React uses the key to match children in the original tree with children in the subsequent tree.

 adding a `key`can make the tree conversion efficient

```jsx
<ul>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>

<ul>
  <li key="2014">Connecticut</li>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>
```

Now React knows that the element with key `'2014'` is the new one, and the elements with the keys `'2015'` and `'2016'` have just moved.

Reorders can also cause issues with component state when indexes are used as keys.

Component instances are updated and reused based on their key. If the key is an index, moving an item changes it. As a result, component state for things like uncontrolled inputs can get mixed up and updated in unexpected ways.

## Tradeoffs

It is important to remember that the reconciliation algorithm is an implementation detail. React could rerender the whole app on every action; the end result would be the same. Just to be clear, rerender in this context means calling `render` for all components, it doesn’t mean React will unmount and remount them. It will only apply the differences following the rules stated in the previous sections.

React are regularly refining the heuristics in order to make common use cases faster. In the current implementation, you can express the fact that a subtree has been moved amongst its siblings, but you cannot tell that it has moved somewhere else. The algorithm will rerender that full subtree.

Because React relies on heuristics, if the assumptions behind them are not met, performance will suffer.

1. The algorithm will not try to match subtrees of different component types. If you see yourself alternating between two component types with very similar output, you may want to make it the same type. In practice, we haven’t found this to be an issue.
2. Keys should be stable, predictable, and unique. Unstable keys (like those produced by `Math.random()`) will cause many component instances and DOM nodes to be unnecessarily recreated, which can cause performance degradation and lost state in child components.

## Stack reconciler

https://reactjs.org/docs/implementation-notes.html

**The “stack” reconciler is the implementation powering React 15 and earlier.**

Let’s start with our familiar `ReactDOM.render(<App />, document.getElementById('root'))`.

The `ReactDOM` module will pass the `<App/ >` along to the reconciler

1. What does `<App />` refer to?
2. What is the reconciler?

`<App />` is a React element, and “elements describe the tree.”

> “An element is a plain object describing a component instance or DOM node and its desired properties.” – [React Blog](https://reactjs.org/blog/2015/12/18/react-components-elements-and-instances.html#elements-describe-the-tree)

In other words, elements are *not* actual DOM nodes or component instances; they are a way to *describe* to React what kind of elements they are, what properties they hold, and who their children are.

React abstracts away all the complex pieces of how to build, render, and manage the lifecycle of the actual DOM tree by itself, effectively making the life of the developer easier.

Let’s look at a traditional approach using object-oriented concepts.

In the typical object-oriented programming world, the developer needs to instantiate and manage the lifecycle of every DOM element. For instance, if you want to create a simple form and a submit button, the state management even for something as simple as this requires some effort from the developer.

 In React, there are two kinds of elements:

- **DOM element:** When the element’s type is a string, e.g., `<button class="okButton"> OK </button>`
- **Component element:** When the type is a class or a function, e.g., `<Button className="okButton"> OK </Button>`, where `<Button>` is a either a class or a functional component.

**Both types are simple objects**. They are mere **descriptions** of what needs to be rendered on the screen and **don’t actually cause any rendering** to happen when you **create and instantiate** them. This makes it easier for React to parse and traverse them to build the DOM tree. The actual rendering happens later when the traversing is finished.

```jsx
<App>
	<Form>
      <Button>
        Submit
      </Button>
    </Form>
</App>

const Form = (props) => {
  return(
    <div className="form">
      {props.form}
    </div>
  )
}
```

When React encounters a class or a function component, it will ask that element what element it renders to based on its props. For instance, if the `<App>` component rendered, then React will ask the `<Form>` and `<Button>` components what they render to based on their corresponding props.

React will call `render()` to know what elements it renders and will eventually see that it renders a `<div>` with a child. React will repeat this process until it knows the underlying DOM tag elements for every component on the page.

This exact process of **recursively traversing** a tree to know the underlying DOM tag elements of a React app’s component tree is known as **reconciliation**. 

By the end of the reconciliation, React knows the result of the DOM tree, and a renderer like `react-dom` or `react-native` applies the minimal set of changes necessary to update the DOM nodes

when you call `ReactDOM.render()` or `setState()`, React performs a **reconciliation**. In the case of `setState`, it performs a traversal and figures out what **changed** in the tree by **diffing** the new tree with the rendered tree. Then it applies those changes to the current tree, thereby updating the state corresponding to the `setState()` call.

### Problem

“stack” reconciler is derived from the “stack” data structure, which is a last-in, first-out mechanism.  Since we are effectively doing a recursion, it has everything to do with a stack.

The reconciliation algorithm we just saw is a purely recursive algorithm. An update results in the entire subtree being re-rendered immediately. But this has some limitations

- In a UI, it’s not necessary for every update to be applied immediately; in fact, doing so can be wasteful, causing frames to drop and degrading the user experience
- Different types of updates have different priorities — an animation update needs to complete more quickly than, say, an update from a data store

> Frame rate is the frequency at which consecutive images appear on a display. Everything we see on our computer screens are composed of images or frames played on the screen at a rate that appears instantaneous to the eye.
>
> Having said that, most devices these days refresh their screens at 60 FPS — or, in other words, 1/60 = 16.67ms, which means a new frame is displayed every 16ms.
>
> This number is very important because if React renderer takes more than 16ms to render something on the screen, the browser will drop that frame.
>
> In reality, however, the browser has housekeeping work to do, so all of your work needs to be completed inside 10ms. When you fail to meet this budget, the frame rate drop, and the content judders on screen. This negatively impacts the user’s experience.

So if the React reconciliation algorithm traverses the entire `App` tree each time there is an update and re-renders it, and if that traversal takes more than 16ms, it will cause dropped frames, and dropped frames are bad.

This is a big reason why it would be nice to have updates categorized by priority and not blindly apply every update passed down to the reconciler. Also, another nice feature to have is the ability to pause and resume work in the next frame. This way, React will have better control over working with the 16ms budget it has for rendering.

This led the React team to rewrite the reconciliation algorithm, and the new algorithm is called **Fiber**.

## Fiber Reconciler

The “fiber” reconciler is a new effort aiming to resolve the problems inherent in the stack reconciler and fix a few long-standing issues. It has been the default reconciler since React 16.

Its main goals are:

- Ability to split interruptible work in chunks.
- Ability to prioritize, rebase and reuse work in progress.
- Ability to yield back and forth between parents and children to support layout in React.
- Ability to return multiple elements from `render()`.
- Better support for error boundaries.

https://github.com/acdlite/react-fiber-architecture

https://indepth.dev/inside-fiber-in-depth-overview-of-the-new-reconciliation-algorithm-in-react/

https://medium.com/the-guild/under-the-hood-of-reacts-hooks-system-eb59638c9dba

https://github.com/facebook/react/issues/7942?source=post_page---------------------------

https://www.youtube.com/watch?v=ZCuYPiUIONs

https://www.youtube.com/watch?v=nLF0n9SACd4&feature=emb_logo

https://reactjs.org/docs/strict-mode.html

https://www.freecodecamp.org/news/lets-fall-in-love-with-react-fiber-90f2e1f68ded/

https://blog.logrocket.com/deep-dive-into-react-fiber-internals/

https://blog.ag-grid.com/inside-fiber-an-in-depth-overview-of-the-new-reconciliation-algorithm-in-react/

**TLDR,** React Fiber is an internal engine change that allows React to break the limits of the call stack. It’s creation enables React to pause/start rendering work at will. Eventually, React users will be able to hint at the “priority” of work.

> React before Fiber was like working at a fast paced company without git.

With Fiber, React can:

- Assign priority to different types of work
- Pause work and come back to it later
- Abort work if it’s no longer needed
- Reuse previously completed work

That's the purpose of React Fiber. Fiber is reimplementation of the stack, specialized for React components. You can think of a single fiber as a **virtual stack frame**.

![image-20200830231646104](assets/react-render/image-20200830231646104.png)

# Lifecycle of Components

https://www.w3schools.com/REACT/react_lifecycle.asp

![image-20200829221520546](assets/react-render/image-20200829221520546.png)

![image-20200829221029941](assets/react-render/image-20200829221029941.png)

# What prompts React to trigger the render phase?

> \* Component receives new props.
> \* state is updated.
> \* Context value is updated (if the component listens to context change using useContext).
> \* Parent component re-renders due to any of the above reasons.

=> state change is the actual trigger re-rendering.



# When a component re-renders

https://www.codebeast.dev/usestate-vs-useref-re-render-or-not/

Reconciliation is React ’s attempt to re-render **ONLY** the changed diff of the DOM tree

There are two reasons why we need to know when re-rendering happens:

1. Re-rendering **updates** the UI based on the latest changes
2. Re-rendering triggers DOM tree reconciliation which is a **performance** factor

https://stackoverflow.com/questions/55106951/react-with-hooks-when-do-re-renders-happen

https://reactjs.org/blog/2015/12/18/react-components-elements-and-instances.html

https://lucybain.com/blog/2017/react-js-when-to-rerender/

https://stackoverflow.com/questions/40819992/react-parent-component-re-renders-all-children-even-those-that-havent-changed

https://kentcdodds.com/blog/fix-the-slow-render-before-you-fix-the-re-render

```jsx
function App() {
  const [value, setValue] = React.useState("");
  const handleInputChange = e => {
    setValue(e.target.value);
  };
  return (
    <div className="App">
      <Input value={value} onChange={handleInputChange} />
      <Button>Button</Button>
    </div>
  );
}
const Input = ({ value, onChange }) => (
  <input value={value} onChange={onChange} />
);
```

`App`, `Input` and `Button` are re-rendering. And this re-render is triggered by just a change made to one component, `Input`

These updates are not as a result of the `input` element changes. They are triggered by the `value` state in `App` which the `input` element updated.

What this means is that if the `App`’s state changes, then **every child** of `App` will **re-render**. 

Fix: (**do not blindly optimize**.)

1. explicitly tell `Input` or `Button` not to re-render using `PureComponent` or `shouldComponentUpdate` or `React.memo`

2. move state logic closer to the presentation component that triggers these updates

   ```jsx
   const Input = () => {
     const [value, setValue] = React.useState("");
     const handleInputChange = e => {
       setValue(e.target.value);
     };
     return <input value={value} onChange={handleInputChange} />;
   };
   ```

3. use memorization hooks

## With hooks

https://medium.com/the-guild/under-the-hood-of-reacts-hooks-system-eb59638c9dba

https://reactjs.org/docs/hooks-faq.html

https://medium.com/@guptagaruda/react-hooks-understanding-component-re-renders-9708ddee9928

https://medium.com/swlh/how-does-react-hooks-re-renders-a-function-component-cc9b531ae7f0

![image-20200829221338257](assets/react-render/image-20200829221338257.png)

> 1. `useState` causes re-render; `useRef` does not.
> 2. Both `useState` and `useRef` remembers their data after a re-render

# PureComponent for Functional Components

`React.memo()` is a HOC that prevents a component from rendering on props change if the props are the same. It basically runs a shallow equal on the props in the `shouldComponentUpdate()` lifecycle, but for functional components that don’t have access to it (without switching to a class).

```js
const MyComponent = React.memo(function MyComponent(props) {
  /* render using props */
});
```

And if the props contain complex objects, we can add a function inside the component to check:

```js
function MyComponent(props) {
  /* render using props */
}
function areEqual(prevProps, nextProps) {
  /*
  return true if passing nextProps to render would return
  the same result as passing prevProps to render,
  otherwise return false
  */
}
export default React.memo(MyComponent, areEqual);
```

This is a great performance gain for component and design systems that rely on functional components for rendering lower-level UI elements.

# Virtual DOM works in the ReactJS

The virtual DOM (VDOM) is a programming concept where an ideal, or “virtual”, representation of a UI is kept in memory and synced with the “real” DOM

## How DOM is build

Every time the DOM changes, browser need to changes to the DOM, recalculate the CSS, do layout, and repaint the web page. This is what takes time.

Browser makers are continually working to shorten the time it takes to repaint the screen. The biggest thing that can be done is to minimize and batch the DOM changes that make redraws necessary.

This strategy of reducing and batching DOM changes, taken to another level of abstraction, is the idea behind React Virtual DOM.

## How virtual DOM works in React

React is keeping two Virtual DOM trees in memory. 

When state changes in a component it **firstly** runs a "diffing" algorithm, which **identifies what has changed in the virtual DOM**. 

The **second** step is "reconciliation", where it **updates the DOM exactly where the nodes are changed.**

Each time the underlying data changes in a React app, a new Virtual DOM representation of the user interface is created

![image-20200624005326042](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200624005326042.png)

Updating the browser’s DOM is a three-step process in React.

1. Whenever anything may have changed, the components will be re-rendered in a Virtual DOM representation.

2. The difference between the previous Virtual DOM representation and the new one will be calculated.

3. The real DOM will be updated with what has actually changed. This is very much like applying a patch.


## Reconciliation

When a component’s props or state change, React decides whether an actual DOM update is necessary by comparing the newly returned element with the previously rendered one. When they are not equal, React will update the DOM. This process is called “reconciliation”.

### The Diffing Algorithm of React

React implements a heuristic O(n) algorithm based on two assumptions:

1. Two elements of different types will produce different trees.
2. The developer can hint at which child elements may be stable across different renders with a `key` prop.

When we tell React to render a tree of elements in the browser, it first generates a virtual representation of that tree and keeps it around in memory for later. Then it’ll proceed to perform the DOM operations that will make the tree show up in the browser.

When we tell React to **update** the tree of elements it previously rendered, it **generates** **a new virtual representation** of the updated tree. Now React has **2 versions of the DOM** in memory!

To render the **updated** tree in the browser, React does not discard what has already been rendered. Instead, it will **compare the 2 virtual versions** of the tree that it has in memory, **compute the differences** between them, figure out what sub-trees in the main tree **need to be updated**, and only **update these sub-trees** in the browser.

## Why React "fast"

React only really re-render the changed part of the DOM, not all the DOM 

=> not re-calculate, re-building, re-painting all element in the real DOM of browser

=> make it an efficiency way to works with Browser UI.

# Fiber

Fiber is the new *reconciliation* engine or reimplementation of core algorithm in React v16. 
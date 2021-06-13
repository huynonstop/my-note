# Rendering Mechanisms and Optimizations

https://v3.vuejs.org/guide/optimizations.html

![image-20201019184044486](assets/VueJS-Rendering/image-20201019184044486.png)

Now that we know how watchers are updating the components, you might ask how those changes eventually make it to the DOM! Perhaps you’ve heard of the Virtual DOM before, many frameworks including Vue use this paradigm to make sure our interfaces reflect the changes we’re updating in JavaScript effectively

We make a copy of the DOM in JavaScript called the Virtual DOM, we do this because touching the DOM with JavaScript is computationally expensive. While performing updates in JavaScript is cheap, finding the required DOM nodes and updating them with JS is expensive. So we batch calls, and change the DOM all at once.

The Virtual DOM in is a lightweight JavaScript object, created by a render function. It takes three arguments: the element, an object with data, props, attrs and more, and an array. The array is where we pass in the children, which have all these arguments too, and then they can have children and so on, until we build a full tree of elements.

If we need to update the list items, we do so in JavaScript, using the reactivity we mentioned earlier. We then make all the changes to the JavaScript copy, the virtual DOM, and perform a diff between this and the actual DOM. Only then do we make our updates to just what has changed. The Virtual DOM allows us to make performant updates to our UIs!

# Concurrent mode

https://reactjs.org/docs/concurrent-mode-intro.html

https://www.youtube.com/watch?v=ByBPyMBTzM0

Concurrent Mode is a set of new features that help React apps stay responsive and gracefully adjust to the user’s device capabilities and network speed.

## Blocking vs Interruptible Rendering

UI libraries, including React, typically work today. Once they start rendering an update, including creating new DOM nodes and running the code inside components, they can’t interrupt this work. We’ll call this approach “blocking rendering”.

In Concurrent Mode, rendering is not blocking. It is interruptible. This improves the user experience. It also unlocks new features that weren’t possible before. 

### Interruptible Rendering

Consider a filterable product list. Have you ever typed into a list filter and felt that it stutters on every key press? Some of the work to update the product list might be unavoidable, such as creating new DOM nodes or the browser performing layout.

The reason for the stutter is simple: once rendering begins, it can’t be interrupted. So the browser can’t update the text input right after the key press.

if a UI library uses blocking rendering, a certain amount of work in your components will always cause stutter.

**Concurrent Mode fixes this fundamental limitation by making rendering interruptible.**

> This means when the user presses another key, React **doesn’t need to block the browser from updating** the text input. Instead, it can **let the browser paint an update** to the input, and then **continue rendering the updated list *in memory***. When the rendering is finished, React updates the DOM, and changes are reflected on the screen.
>
> Conceptually, you can think of this as React preparing every update “on a branch”. Just like you can abandon work in branches or switch between them, React in Concurrent Mode can interrupt an ongoing update to do something more important, and then come back to what it was doing earlier. This technique might also remind you of double buffering in video games.

Concurrent Mode techniques reduce the need for debouncing and throttling in UI. Because rendering is interruptible, React doesn’t need to artificially *delay* work to avoid stutter. It can start rendering right away, but interrupt this work when needed to keep the app responsive.

### Intentional Loading Sequences

### Concurrency

Let’s recap the two examples above and see how Concurrent Mode unifies them. **In Concurrent Mode, React can work on several state updates \*concurrently\*** — just like branches let different team members work independently:

- For CPU-bound updates (such as creating DOM nodes and running component code), concurrency means that a more urgent update can “interrupt” rendering that has already started.
- For IO-bound updates (such as fetching code or data from the network), concurrency means that React can start rendering in memory even before all the data arrives, and skip showing jarring empty loading states.

## React lazy + Suspense

```jsx
const ProfilePage = React.lazy(() => import('./ProfilePage')); // Lazy-loaded

// Show a spinner while the profile is loading
<Suspense fallback={<Spinner />}>
  <ProfilePage />
</Suspense>
```


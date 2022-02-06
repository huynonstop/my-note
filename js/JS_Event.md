# Event

![image-20200608132055539](assets/JS_Event/image-20200608132055539.png)

## Browser Event Objects

![image-20200608132201928](assets/JS_Event/image-20200608132201928.png)

Whatever causes an event provides you some data along with such an event to describe it to help you control it

![image-20200608142509047](assets/JS_Event/image-20200608142509047.png)

![image-20200608142527917](assets/JS_Event/image-20200608142527917.png)

## Adding event

HTML `on` attributes => mix HTML and JS code

element `on` property => only 1 event handler

element `addEventListener()` => most flexibles ways (can add and remove event listener later)

## Remove event

![image-20200608134906555](assets/JS_Event/image-20200608134906555.png)

can only remove exact the same function (not work direct with anonymous function or `bind()`)

## `preventDefault()`

form submit to server or link navigation

## Bubbling & Capturing, Propagation

![image-20200608144128162](assets/JS_Event/image-20200608144128162.png)

Capturing => Bubbling

By default all event listeners are registered in the bubbling phase

```html
<div>
    <button>
    </button>
</div>
```

![image-20200608144923627](assets/JS_Event/image-20200608144923627.png)

![image-20200608145005127](assets/JS_Event/image-20200608145005127.png)

![image-20200608145139650](assets/JS_Event/image-20200608145139650.png)

![image-20200608145211518](assets/JS_Event/image-20200608145211518.png)

The way event occurred on this button but it's listenable on all ancestors called **Event Propagation** (some event are not propagated)

![image-20200608145736200](assets/JS_Event/image-20200608145736200.png)

![image-20200608150018545](assets/JS_Event/image-20200608150018545.png)

## Event delegation pattern

```html
<ul id="parent-list">
 <li id="post-1">Item 1</li>
 <li id="post-2">Item 2</li>
 <li id="post-3">Item 3</li>
 <li id="post-4">Item 4</li>
 <li id="post-5">Item 5</li>
</ul>
```

Event delegation is a technique involving adding event listeners to a **parent** element instead of adding them to the child elements.

![image-20200608150822633](assets/JS_Event/image-20200608150822633.png)

The listener will fire whenever the event is triggered on the descendant elements due to **event bubbling up** the DOM. The benefits of this technique are:

- Only have 1 listener
- There is no need to unbind the handler from elements that are removed and to bind the event for new elements.

```js
document.getElementById("parent-list").addEventListener("click",    function(e) {
  // e.target is the clicked element!
     event.target.closest('li').classList.toggle('highlight');
    }
 );
```

## Trigger event programmatically

use element method (submit(),click(),...)

## Observer Pattern

![image-20200623095423563](assets/JS_Event/image-20200623095423563.png)

- **subject** – This is the object that will send out a notification to all of the ‘observers’ who want/need to know that the subject was updated.
- **observers** – These are the objects that want to know when the subject has changed

```js
// Observable or Subject
class Observable {
  constructor() {
    this.observers = [];
  }
  subscribe(f) {
    this.observers.push(f);
  }
  unsubscribe(f) {
    this.observers = this.observers.filter(subscriber => subscriber !== f);
  }
  notify(data) {
    this.observers.forEach(observer => observer.update(data));
  }
}
// Observer
class Observer {
   constructor(element,update) {
    this.element = element;
    this.update = update;
  }
}
//--------------------------------------------------------------------
const headingsObserver = new Observable();
// DOM 
const p1 = document.querySelector('.p1')
const p2 = document.querySelector('.p2')
document.querySelector('.button').addEventListener('click', e => {
  // notify all observers about new data on event
  headingsObserver.notify("clicked");
});

const o1 = new Observer(p1,text => p1.textContent = text)
const o2 = new Observer(p2,text => p2.textContent = text)

headingsObserver.subscribe(o1);
headingsObserver.subscribe(o2);

headingsObserver.unsubscribe(o2)
```

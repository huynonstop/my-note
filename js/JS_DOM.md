# JS DOM  (browser)

![image-20200525233145601](assets/JS_DOM/image-20200525233145601.png)

browser creates a **tree** of DOM elements (nodes)  

![image-20200525235601043](assets/JS_DOM/image-20200525235601043.png)

![image-20200527003706893](assets/JS_DOM/image-20200527003706893.png)

![image-20200527003720277](assets/JS_DOM/image-20200527003720277.png)

![image-20200527005853080](assets/JS_DOM/image-20200527005853080.png)

![image-20200528131446628](assets/JS_DOM/image-20200528131446628.png)

![image-20200528174420170](assets/JS_DOM/image-20200528174420170.png)

![image-20200528174430981](assets/JS_DOM/image-20200528174430981.png)

![image-20200528180342871](assets/JS_DOM/image-20200528180342871.png)

can have many child elements or child nodes

only element nodes can have child nodes

can't have more than 1 parent element or parent node

```js
element.parentElement
element.parentNode
//only get access to the nearest direct parent
element.closet('selectore')
//select ancestor
```

![image-20200528182000907](assets/JS_DOM/image-20200528182000907.png)

![image-20200528184451058](assets/JS_DOM/image-20200528184451058.png)

![image-20200528185034010](assets/JS_DOM/image-20200528185034010.png)

![image-20200528191941422](assets/JS_DOM/image-20200528191941422.png)

innerHTML : re-render all html (change everything)

HTML string : cannot direct access to the newly created element

createElement : have the DOM object to work with

If an element is a part of the DOM (rendered), if you insert it somewhere else it will be detached from the place and moved to the new place 

clone method: cloneNode(?deep)

![image-20200528191813736](assets/JS_DOM/image-20200528191813736.png)

## dataset

`data-whatever` attribute in HTML to attach data to DOM element and read from it

![image-20200606010826278](assets/JS_DOM/image-20200606010826278.png)

![image-20200606010949847](assets/JS_DOM/image-20200606010949847.png)

![image-20200606010911285](assets/JS_DOM/image-20200606010911285.png)

## Size and position

![image-20200608110146679](assets/JS_DOM/image-20200608110146679.png)

![image-20200608110019415](assets/JS_DOM/image-20200608110019415.png)

![image-20200608110440482](assets/JS_DOM/image-20200608110440482.png)

![image-20200608110509828](assets/JS_DOM/image-20200608110509828.png)

![image-20200608110632181](assets/JS_DOM/image-20200608110632181.png)

![image-20200608110724344](assets/JS_DOM/image-20200608110724344.png)

![image-20200608110903513](assets/JS_DOM/image-20200608110903513.png)

## DOM Prototype

![image-20200608112114172](assets/JS_DOM/image-20200608112114172.png)

## Template

![image-20200608120150082](assets/JS_DOM/image-20200608120150082.png)

![image-20200608120249290](assets/JS_DOM/image-20200608120249290.png)

## Loading Script dynamically

![image-20200608120504634](assets/JS_DOM/image-20200608120504634.png)

![image-20200608120659103](assets/JS_DOM/image-20200608120659103.png)
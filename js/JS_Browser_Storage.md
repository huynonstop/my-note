# Browser Storage

<https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies>

<https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API>

<https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API>

<https://medium.com/analytics-and-data/cookies-tracking-and-pixels-where-does-your-web-data-comes-from-ff5d9b8bc8f7>

![image-20200611154542654](assets/JS_Browser_Storage/image-20200611154542654.png)

![image-20200611154906597](assets/JS_Browser_Storage/image-20200611154906597.png)

## Difference `cookie`, `sessionStorage` and `localStorage`

![Hình ảnh có thể có: văn bản](assets/JS_Browser_Storage/131986214_3556451341101909_5452667036261089041_o.jpg)

All the above-mentioned technologies are key-value storage mechanisms on the client side. They are only able to store values as strings. Each key-name pair is unique for a protocol and domain, regardless of the paths.

|                                        | `cookie`                                                     | `localStorage`                                             | `sessionStorage`                                             |
| -------------------------------------- | ------------------------------------------------------------ | :--------------------------------------------------------- | ------------------------------------------------------------ |
| Initiator                              | Client or server                                             | Client                                                     | Client                                                       |
| Expiry                                 | Manually set (`expires` or `max-age` or `Set-Cookie` on server) or being deleted | Data persist window and browser lifetimes or being deleted | persist only as long as the window or tab in which they stored or being deleted |
| Persistent across browser sessions     | Depends on whether expiration is set (default is when session expires) | Persisting until we clear it.                              | Accessible as long as our tab is open, even we refresh the page.   Lose when close tab |
| Sent to server with every HTTP request | Cookies are attached to `Cookie` header of the Http requests and automatically being sent to the server | No                                                         | No                                                           |
| Capacity (per domain)                  | 4kb                                                          | 5MB                                                        | 5MB                                                          |
| Accessibility                          | Any tab/page/session open in the browser or at the server side | Any same origin tab/page/session open in the browser       | Same tab/page/session open in the browser                    |

```js
const storeBtn = document.getElementById('store-btn');
const retrBtn = document.getElementById('retrieve-btn');

storeBtn.addEventListener('click', () => {
  const userId = 'u123';
  const user = {name: 'Max', age: 30};
  sessionStorage.setItem('uid', userId);
  localStorage.setItem('user', JSON.stringify(user));
});

retrBtn.addEventListener('click', () => {
  const extractedId = sessionStorage.getItem('uid');
  const extractedUser = JSON.parse(localStorage.getItem('user'));
  console.log(extractedUser);
  if (extractedId) {
    console.log('Got the id - ' + extractedId);
  } else {
    console.log('Could not find id.');
  }
});
```

```js
const storeBtn = document.getElementById('store-btn');
const retrBtn = document.getElementById('retrieve-btn');

storeBtn.addEventListener('click', () => {
  const userId = 'u123';
  const user = {name: 'Max', age: 30};
  document.cookie = `uid=${userId}; max-age=360`;
  document.cookie = `user=${JSON.stringify(user)}`;
});

retrBtn.addEventListener('click', () => {
  console.log(document.cookie);
  const cookieData = document.cookie.split(';');
  const data = cookieData.map(i => {
    return i.trim();
  });
  console.log(data[1].split('=')[1]); // user value
});

```

`localStorage`, `sessionStorage`, and cookies are all **client storage** solutions. Session data is held on the server where it remains under your direct control. (It's unique per `protocol://host:port`(**Origin**) combination)

 They are only able to store values as strings.

- Cookies

  An HTTP cookie (web cookie, browser cookie) is a small piece of data that a server sends to the user's web browser. The browser may store it and send it back with the next request to the same server.

  - Stores data that has to be sent back to the server with subsequent requests. Its expiration varies based on the type and the **expiration duration** can be set from either server-side or client-side (normally from server-side).
  - Cookies can be read on server-side and client-side.
  - Size must be less than 4KB.
  - Cookies can be made secure by setting the `httpOnly` flag as true for that cookie. This prevents client-side access to that cookie

  Cookies are mainly used for three purposes:

  - Session management: Logins, shopping carts, game scores, or anything else the server should remember

  - Personalization: User preferences, themes, and other settings

  - Tracking: Recording and analyzing user behavior

    **Cross-Site Request Forgery** (XSRF).

    **Third-party cookies**

- `sessionStorage` (1 Tab)

  - The `sessionStorage` object stores data only for a session, meaning that the data is stored until the browser (or tab) is closed.
  - Data is never transferred to the server.
  - Storage limit is larger than a cookie (at least 5MB).
  - `sessionStorage` can only be read on client-side

- `localStorage`(Browser)

  - Stores data with no expiration date, and gets cleared only through JavaScript, or clearing the Browser cache / Locally Stored Data
  - Storage limit is the maximum amongst the three
  - Data is never transferred to the server.
  - `localStorage`can only be read on client-side

## IndexDB

<https://github.com/jakearchibald/idb>

```js
const storeBtn = document.getElementById('store-btn');
const retrBtn = document.getElementById('retrieve-btn');

let db;

const dbRequest = indexedDB.open('StorageDummy', 1);

dbRequest.onsuccess = function(event) {
  db = event.target.result;
};

dbRequest.onupgradeneeded = function(event) {
  db = event.target.result;

  const objStore = db.createObjectStore('products', { keyPath: 'id' });

  objStore.transaction.oncomplete = function(event) {
    const productsStore = db
      .transaction('products', 'readwrite')
      .objectStore('products');
    productsStore.add({
      id: 'p1',
      title: 'A First Product',
      price: 12.99,
      tags: ['Expensive', 'Luxury']
    });
  };
};

dbRequest.onerror = function(event) {
  console.log('ERROR!');
};

storeBtn.addEventListener('click', () => {
  if (!db) {
    return;
  }
  const productsStore = db
    .transaction('products', 'readwrite')
    .objectStore('products');
  productsStore.add({
    id: 'p2',
    title: 'A Second Product',
    price: 122.99,
    tags: ['Expensive', 'Luxury']
  });
});

retrBtn.addEventListener('click', () => {
  const productsStore = db
    .transaction('products', 'readwrite')
    .objectStore('products');
  const request = productsStore.get('p2');

  request.onsuccess = function() {
    console.log(request.result);
  }
});

```

## More

More on localStorage / sessionStorage: <https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage>

More on Cookies (in JS): <https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie>

More on IndexedDB: <https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API/Using_IndexedDB>

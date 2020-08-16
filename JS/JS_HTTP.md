# HTTP

![image-20200610234056906](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200610234056906.png)

![image-20200610234900270](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200610234900270.png)

# XMLHttpRequest

## create

![image-20200611102645496](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611102645496.png)

## open (config)

![image-20200611102728958](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611102728958.png)

## send

![image-20200611102816532](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611102816532.png)

## onload property

![image-20200611104019142](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611104019142.png)

![image-20200611104044218](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611104044218.png)

## responeType

![image-20200611104302030](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611104302030.png)

## Use with promise

```js
function sendHttpRequest(method, url, data) {
  const promise = new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        xhr.open(method, url);
        xhr.responseType = 'json';
        xhr.onload = function() {
            resolve(xhr.response);
        };
        xhr.send(JSON.stringify(data));
    });
  return promise;
}
```

```js
async function fetchPosts() {
  const responseData = await sendHttpRequest(
    'GET',
    'https://jsonplaceholder.typicode.com/posts'
  );
  const listOfPosts = responseData;
  for (const post of listOfPosts) {
    const postEl = document.importNode(postTemplate.content, true);
    postEl.querySelector('h2').textContent = post.title.toUpperCase();
    postEl.querySelector('p').textContent = post.body;
    postEl.querySelector('li').textContent = post.id;
    listElement.append(postEl);
  }
}
```
```js
async function createPost(title, content) {
  const userId = Math.random();
  const post = {
    title: title,
    body: content,
    userId: userId
  };

  sendHttpRequest('POST', 'https://jsonplaceholder.typicode.com/posts', post);
}
```

```js
fetchPosts();
createPost('DUMMY', 'A dummy post!');
```

![image-20200611110251836](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611110251836.png)

![image-20200611110723734](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611110723734.png)

## Error handle

### onerror (client)

![image-20200611111518201](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611111518201.png)

### response code (server)

![image-20200611111541613](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611111541613.png)

## More

https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest

# fetch API

```js
function sendHttpRequest(method, url, data) {
  return fetch(url,{
      //config object
      method: method,  //default is 'GET'
      body: JSON.stringify(data)
  }).then(res => {
      return res.json(); //=> convert to data
  }).catch(err => {
      throw new Error(err.message)
  });
}
```

![image-20200611115051143](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611112459987.png)

![image-20200611115152534](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611115152534.png)

# Header request

![image-20200611114101723](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611114101723.png)

![image-20200611114158894](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611114158894.png)

# Form data

![image-20200611124024123](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611124024123.png)

```js
function sendHttpRequest(method, url, data) {
  return fetch(url,{
      //config object
      method: method,  //default is 'GET'
      body: data
  }).then(res => {
      return res.json(); //=> convert to data
  }).catch(err => {
      throw new Error(err.message)
  });
}
```

![image-20200611124132522](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611124132522.png)

![image-20200611124325513](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611124325513.png)
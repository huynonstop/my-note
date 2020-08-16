# Redux

It consists of four main building blocks:

1. A **single, centralized state** (i.e. a global JS object you could say) which is **not** directly accessible or mutable
2. **Reducer functions** that contain the logic to change and update the global state (by returning a new copy of the old state with all the required changes)
3. **Actions** that can be dispatched to trigger a reducer function to run
4. **Subscriptions** to get data out of the global state (e.g. to use it in React components)

## Why redux

State managing and share data between components easier

Do not pass un-using props for between components (prop drilling)

![image-20200618172322498](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200618172322498.png)

## Flow

![image-20200623140532415](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200623140532415.png)

## Reducer

return a new version of state when dispatch an action

![image-20200618181653587](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200618181653587.png)

## Action

![image-20200618181734946](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200618181734946.png)

## Middleware

Something that receive action object before they reach the root reducer

## Async redux

Dispatch action after the async task done

```js
//Action.js
export const updateShop = (collections) => ({
	type: UPDATE_SHOP,
	payload: collections,
});

export const fetchStart = () => ({
	type: FETCH_SHOP_START,
});
export const fetchSuccess = () => ({
	type: FETCH_SHOP_SUCCESS,
});
export const fetchFail = (error) => ({
	type: FETCH_SHOP_FAILURE,
	payload: error,
});
```



```js
function fetchData() {
    dispatch(fetchStart());
    const collectionRef = firestore.collection("collections");
    collectionRef
      .get() //promise
      .then((snapshot) => {
         const collectionData = transformCollectionsSnapshot(snapshot);
         dispatch(updateShop(collectionData));
         dispatch(fetchSuccess());
        })
      .then((error) => dispatch(fetchFail(error)));
}
```

### redux-thunk

![image-20200623112850576](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200623112850576.png)

```js
export const fetchShop = () => (dispatch) => {
	dispatch(fetchStart());
	const collectionRef = firestore.collection("collections");
	collectionRef
		.get()
		.then((snapshot) => {
			const collectionData = transformCollectionsSnapshot(snapshot);
			dispatch(updateShop(collectionData));
			dispatch(fetchSuccess());
		})
		.then((error) => dispatch(fetchFail(error)));
};
```

### redux-saga

![image-20200623131340647](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200623131340647.png)

# 
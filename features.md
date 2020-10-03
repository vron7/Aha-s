---

The **Promise** *(ES6)* object represents the eventual completion (or failure) of an asynchronous operation and its resulting value.  
Simply - container for a future value.  

```js
const p = new Promise((resolve, reject) => {
  setTimeout(resolve, 200, 'success!');
});
```


---

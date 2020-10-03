---
## Promise
The **Promise** *(ES6)* object represents the eventual completion (or failure) of an asynchronous operation and its resulting value.  
Simply - container for a future value.

```js
const p = new Promise((resolve, reject) => {
  setTimeout(resolve, 200, 'success!');
});
```
All Promise instances have a **.then()** method, which accepts two callbacks. Then mehtod always returns a promise.   
```js
const resolved = (val) => console.log(val);
const rejected = (err) => console.log(err);

p.then(resolved, rejected);
```

Chaining  
```js
const resolved = (val) => val;
p.then(resolved);
p.then(console.log); // returns val
```

* Promise can only succeed or fail **once**
* If a callback (.then) is added to already resolved promise, a correct callback will be called even though the event took palce earlier.
* Promise executes as soon as you declear and bind it to a variable.


---

## Async Await
**async** before a function means that the function will **return a promise**.
```js
async function f() {
  return 'hello!' // could also explicitly return Promise.resolve(1), which would be the same
}

f().then(alert); // hello!
```


---

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
  return 'hello' // could also explicitly return Promise.resolve('hello'), which would be the same
}

f().then(alert); // hello
```
**await** only works inside function with *async* keyword  
* makes javascript wait until promise settles and returns itself,
* is a syntax sugar for .then()
* is non blocking, makes async code look and behave a bit like syncronous
```js
async function f() {

  let p = new Promise((resolve, reject) => {
    setTimeout(resolve, 1000, 'hello')
  });

  let result = await p; // execution is paused until the promise resolves (*)
  alert(result); // "hello"
}
```
Promise vs Await
```js
const makeRequest = () =>
  getJSON()
    .then(data => {
      console.log(data)
      return "done"
    })

const makeRequest = async () => {
  console.log(await getJSON())
  return "done"
}
```

---

## {...} spread syntax

** for object literals** *(ES9)*
```js
const archetypes = {
  king: 1,
  warrior: 2,
  lover: 3,
  magician: 4
}
const { lover, king , ...others} = archetypes;
console.log(lover); // 3
console.log(others); // { warrior:2, magician:4 }
```


---

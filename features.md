---
### Promise
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

**.finally** - do some processing after promise is settles, regardless of outcome
```js
const p = Promise.resolve('done')
p.then(fn);
p.catch(fn);
p.finally(fn); // fn is called without arguments
```


* Promise can only succeed or fail **once**
* If a callback (.then) is added to already resolved promise, a correct callback will be called even though the event took palce earlier.
* Promise executes as soon as you declear and bind it to a variable.


---

### Async Await
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
### await of
*(ES9)*
```js
const getData = async function(urls) {
  const promises = urls.map(url => fetch(url));
  for await (let request of promises) {
    const data = await request.json();
    console.log('Wohoo, data! ', data);
  }
}
```


---

### {...} spread syntax

**in arrays literals**
```js
let rest = ['warrior', 'lover']; 
let archetypes = ['king', ...rest, 'and', 'magician']; // [ 'king', 'warrior', 'lover', 'and', 'magician']
```

**in object literals** *(ES9)*
```js
const archetypes = {
  king: 1,
  warrior: 2,
  lover: 3,
  magician: 4
}
const { lover, king , ...others } = archetypes;
console.log(lover); // 3
console.log(others); // { warrior:2, magician:4 }
```
---

### shorthand object notations
*(ES6)*  
**shorthand property names**
```js
let a = 1, b = 'foo', c = {}:
let o = {a, b, c}
```  

**shorthand method names**
```js
let o = { 
  draw(params){ } 
}
```  

---

### globalThis
*(ES2020)*

**globalThis** proprety contains the global **this** value accross all environments (window and non-window)
```js
window === globalThis; // true
global === globalThis; // true in Node.js
``` 

---

### bigInt
*(ES2020)*

**bigInt** - is a new javascript type, we can use to make calculations which are bigger than Number.MAX_SAFE_INTEGER
```js
typeof 1n // returns bigin

r = Number.MAX_SAFE_INTEGER
r + 10 // returns incorrect calculation, since r is set to maximum safe integer value
BigInt(r) + 10n // return correct calculation
``` 

---

### optional chaining operator ( ? )
*(ES2020)*

```js
wizard.gandalf?.hasStaff
wizard.gandalf?.weapons?[0]
wizard.gandalf?.fireball?(1000)
``` 

---

### nullish coalescing operator (??)
*(ES2020)*

```js
wizard = {
  name: 'Gandalf',
  power: 0
}
power = wizard.power || 'no power'; // returns 'no power' - checks if value is 'falsey'
power = wizard.power ?? 'no power'; // returns 0 - checks if value is null/undefined
``` 

---

### Promise.allSetteled
*(ES2020)*

```js
const p1 = new Promise((resolve, reject) => {resolve()})
const p2 = new Promise((resolve, reject) => {reject()})

Promise.all([p1,p2]).then(data => console.log(data)) // throws error since it expects all promises to be resolved

Promise.allSettled([p1,p2]).then(data => console.log(data)) // all good
``` 

---
Javascript **Native Modules** (ES6)   
- Adding **type="module** propery on a script defines it as a module.
- Modules have a **module scope** - anything defined in a module cannot be accessed from outside unless you **export** it.
- Module code runs only when module is **imported**. When imported multiple times, it runs on the first import.
- File which imports a module must itself also be a module
- Modules use strict mode
- **this** inside a module is **undefined**
   
An **export** keyword must be used to make a variable/function inside the module public.     
An **import** keyword must be used to import something from the module.   
    
```js
// utils.js
const something = "smth"; // will not be in the global scope
const saySomething = function() {
	return something;
}
console.log("Helloo Utils!"); // runs only once and only when this module is imported
export { saySomething }
```
```js
// main.js
import { saySomething } from "./utils.js"
saySomething(); // "smth"
```
```js
<html>
<head></head>
<body>
	<script src="utils.js" type="module"></script>
	<script src="main.js" type="module"></script>
	<script>
		console.log(something); // undefined
	</script>
</body>
</html>
```
---



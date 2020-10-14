### Factory function  

```js
function createCircle(radius) {
  return {
    radius,
    draw() {
      console.log('drawing...');
    }
  }
}
```

```js
const circle = createCircle(5);
```
---

### Constructor function

```js
function Circle(radius) { //Pascal notation!
  this.radius = radius;
  this.draw = function() {
    console.log();
  }
}
```

```js
const circle = new Circle(5);
```

What happens when you call **new** operator ?
- a new empty object is created
- **this** is set to the newly created object
- returns **this** if function does not return an object

```js
const o = new Object();
```
---
### Singelton

```js
const Singelton = (function(){
    function ProcessManager(){ // constructor funtion
        this.numProcess = 0;
    }
    let pm;
    function createProcessManager(){
        pm = new ProcessManager();
        return pm;
    }
    return {
        getProcessManager: () => {
            if(!pm)
                pm = createProcessManager();
            return pm;
        }
    }
})()
```
```js
pm = Singelton.getProcessManager();
pm2 = Singelton.getProcessManager();
pm === pm2 // return true, they point to the same reference on memory
```

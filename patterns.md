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

What happens when you call **new** ?
- a new empty object is created
- **this** is set to the newly created object
- returns **this** if function does not return an object

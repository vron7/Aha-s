### Factory Function  

```js
function createCircle(radius) {
  return {
    radius, // radius: radius
    draw: function() {
      console.log('drawing...');
    }
  }
}
```

---

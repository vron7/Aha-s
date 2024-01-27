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
*note : Arrow function cannot be used as constructor*

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
const pm = Singelton.getProcessManager();
const pm2 = Singelton.getProcessManager();
pm === pm2 // return true, they point to the same reference on memory
```
---
### Single Responsibility Priniciple
```js
class DearDiary {
    count = 0;

    constructor() {
        this.entries = {};
    }

    addEntry(text) {
        let c = ++this.count;
        this.entries[c] = `${c}: ${text}`;
        return c;
    }

    removeEntry(index) {
        delete this.entries[index];
    }

    toString() {
        return Object.values(this.entries).join('\n');
    }

    save(filename) {
        // save a diary to filesystem
        // fs.writeFileSync(filename, this.toString());
    }

    load(filename) {
        // load a diary from the filesystem
    }

    loadFromUrl(url) {
        // load a diary from the url
    }
}
```
Currently the Diary class has two responsibilities:   
* managing the entries
* managing the loading and saving of the diary   
 
Whats the **problem**? Let's say we want to have some common processing when we load/save the diary.      
Or we might have 10 different classes in out project, which we want to load/save.   
It makes sense to take all the presistance functionality and add them to separate component,
which we can generalize to handle different types of objects.  
This way if we have some problem with loading/saving, we have one place to look for, instead of 10 separate classes.
```js
class DearDiary {
    count = 0;

    constructor() {
        this.entries = {};
    }

    addEntry(text) {
        let c = ++this.count;
        this.entries[c] = `${c}: ${text}`;
        return c;
    }

    removeEntry(index) {
        delete this.entries[index];
    }

    toString() {
        return Object.values(this.entries).join('\n');
    }
}

class PersistenceManager {
    preprocess(j) {
        //
    }

    saveToFile(diary, filename) {
        fs.writeFileSync(filename, diary.toString());
    }

    load(filename) {
        // load from the filesystem
    }
}
```

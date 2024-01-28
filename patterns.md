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

const d = new DeadDiary();
d.addEntry('I cried today.');

const p = new PersistenceManager();
p.saveToFile(d, 'c:/temp/journal.txt');
```
---
### Open-Closed Principle
A class is open for extension but closed for modification
```js
// enums
const Color = Object.freeze({
    red: 'red',
    green: 'green',
    yellow: 'yellow'
});

const Size = Object.freeze({
    small: 'small',
    medium: 'medium',
    large: 'large',
});

class Fruit {
    constructor(name, color, size) {
        this.name = name;
        this.color = color;
        this.size = size;
    }
}

class FruitFilter {
    filterByColor(products, color) {
        return products.filter(p => p.color === color);
    }

    filterBySize(products, size) {
        return products.filter(p => p.size === size);
    }

    filterBySizeAndColor(products, size, color) {
        return products.filter(p =>
            p.size === size && p.color === color);
    }

    // the problem comes when we have to introduce new filters
    // 1 - let say we have to introduce a new criteria eg. Taste, Origin
    // 2 - and we also have to implement OR filter eg. filter fruits which are large OR green
    // by introducing new filters, we have to modify this class
    // it will create something called "state space explosion"
    // adding new criterias = 3 criteria (+weight) = 7 new methods (NO THANKS)
    // OCP = open for extension, closed for modification
}

// but why is this a problem?
// let say this class is already tested, deployed and used by others
// it means others will have to take the changes for this class, redo the testing and redeploy
// also the class will have unneccesary amount of code which might not even be used by others
// so instead of modifing, prefer extending(inherticance)

const fruits = [
    new Fruit('Apple', Color.green, Size.medium),
    new Fruit('Kiwi', Color.green, Size.medium),
    new Fruit('Cherry', Color.red, Size.small),
    new Fruit('Melon', Color.yellow, Size.large)
];

const fruitFilter = new FruitFilter();
for (const fruit of fruitFilter.filterByColor(fruits, Color.green)) {
    console.log(` * ${fruit.name} is green`);
}
```
Let's try out **extension** instead of **modification**
```js
class ColorSpecification {
    constructor(color) {
        this.color = color;
    }

    isSatisfied(item) {
        return item.color === this.color;
    }
}

class SizeSpecification {
    constructor(size) {
        this.size = size;
    }

    isSatisfied(item) {
        return item.size === this.size;
    }
}

class BetterFilter {
    filter(items, spec) {
        return items.filter(x => spec.isSatisfied(x));
    }
}

// specification combinator
class AndSpecification {
    constructor(...specs) {
        this.specs = specs;
    }

    isSatisfied(item) {
        return this.specs.every(x => x.isSatisfied(item));
    }
}

// this approach is more modular and flexible

const betterFilter = new BetterFilter();
for (const fruit of betterFilter.filter(fruits,
    new ColorSpecification(Color.green))) {
    console.log(` * ${fruit.name} is green`);
}

console.log(`Large and green products:`);
const spec = new AndSpecification(
    new ColorSpecification(Color.green),
    new SizeSpecification(Size.large)
);
for (let fruit of betterFilter.filter(fruits, spec))
    console.log(` * ${fruit.name} is large and green`);
```
---
### Dependency Inversion Principle
DIP states that high level modules should not directly depend on low level modules
```
Let's try out **extension** instead of **modification**
```js

class Person {
    constructor(name) {
        this.name = name;
    }
}

// LOW-LEVEL MODULE (STORAGE)
class Relationships {
    constructor() {
        this.data = [];
    }

    addParentAndChild(parent, child) {
        this.data.push({
            from: parent,
            to: child
        });
    }


    findAllChildrenOf(name) {
        return this.data.filter(r =>
            r.from.name === name
        ).map(r => r.to);
    }
}

// HIGH-LEVEL MODULE (RESEARCH)
class Research {
    // Here we directly depend on the implementaion details of the low level module(relationships)
    // What if relationships.data changes from an array to a map or some other structure
    // We would have to refactor our Research class    
    constructor(relationships)
    {
      // problem: direct dependence ↓↓↓↓ on storage mechanic (relationships.data)
      let relations = relationships.data;
      for (let rel of relations.filter(r =>
        r.from.name === 'John'
      ))
      {
        console.log(`John has a child named ${rel.to.name}`);
      }
    }
}

let parent = new Person('John');

// low-level module
let rels = new Relationships();
rels.addParentAndChild(parent, new Person('Chris'));
rels.addParentAndChild(parent, new Person('Matt'));

new Research(rels);
```
```js

class Person {
    constructor(name) {
        this.name = name;
    }
}

// LOW-LEVEL (STORAGE)
class RelationshipBrowser {
    constructor() {
        // ABSTRACT CLASS IN JS
        if (this.constructor.name === 'RelationshipBrowser')
            throw new Error('RelationshipBrowser is abstract!');
    }

    findAllChildrenOf(name) { }
}

class Relationships extends RelationshipBrowser {
    constructor() {
        super();
        this.data = [];
    }

    addParentAndChild(parent, child) {
        this.data.push({
            from: parent,
            to: child
        });
    }


    findAllChildrenOf(name) {
        return this.data.filter(r =>
            r.from.name === name
        ).map(r => r.to);
    }
}

// HIGH-LEVEL MODULE (RESEARCH)
// DIP states that high level modules should not directly depend on low level modules
// They should rather depend on abstractions(abstract classes/interfaces)
class Research {
    // Here we have an abstraction, we do not depend on storage mechanic
    constructor(browser) {
        for (let p of browser.findAllChildrenOf('John')) {
            console.log(`John has a child named ${p.name}`);
        }
    }
}

let parent = new Person('John');
let child1 = new Person('Chris');
let child2 = new Person('Matt');

// low-level module
let rels = new Relationships();
rels.addParentAndChild(parent, child1);
rels.addParentAndChild(parent, child2);

new Research(rels);
```

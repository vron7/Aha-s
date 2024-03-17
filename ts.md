# TS aha-s

**Type assignments**
```js
// no need to explicitly assign a type here since TS will 
// implicitly determine it by itself based on the value (inference)
const num = 5;

// let's assign a type here, because we do not initiate number2 
// with a value but we want it to be a number
let num1: number; 

// any type since there is no value nor explicit type assigment 
let input;

// union type, can be either
let input1: string | number

// literal type
let pasta1: 'spaghetti'

// union type with literal types
let pasta2: 'fussili' | 'penne'

const eatPasta = (pasta: 'spaghetti' | 'penne') => {console.log('om nom nom')}
eatPasta(pasta1);
eatPasta(pasta2); // error type 'fussili' not assignable to 'spaghetti' | 'penne'

// more about literal types
const win1 = 777; // also a literal type since its a constant
let win2 = 5;     // type number
const jackpot = (num: 777) => {alert('JACKPOT!')}
jackpot(win)
jackpot(notWin) // error type 'number' not assignable to type '777'

// literal + union
let isFoodGood: true | false; // same as isFoodGood:boolean

// type alias
type GoodPastas = 'fussili' | 'penne' | 'spaghetti'
let goodPasta: GoodPastas;
const eatGoodPasta = function(pasta: GoodPastas) {console.log('om nom nom')}
eatGoodPasta("fussili");

type Pasta = {name: GoodPastas, cookingTime: number}
const makePasta = function(pasta: Pasta) {console.log('made pasta:', pasta.name)}

// if function does not have return statement, the return type is void by default
// added below explicitly on purpuse
const log = function(): void {
  console.log('hola!');
}
const logOther = function(): undefined {
  console.log('hola!');
  return; // when you want to use undefined type, you must use empty return statement
}

// function type
let fn1: (a: number) => boolean; // fn1 variable accepts a function which has a
                                 // number parameter and returns a boolean
const fn2 = (num: number) => num > 0
fn1 = fn2 // no error, type matching

// callbacks 
// (note: by returning void we say that we don't care whats returned)
function orderPasta(name: GoodPastas, cb: (name: GoodPastas) => void) {
    cb(name);
}
orderPasta('penne', (name) => console.log('ordered', name));

// unknown type
// a bit better than any, use when you don't know 
// the type, for example when fetching some data
let input1: any;
let input2: unknown;
input1.toUpperCase() // no error, TS ignores type checking for any
input2.toUpperCase() // TS error, indexOf does not exist on type unknown
if (typeof input2 === 'string') {
  input2.toUpperCase() // works, unknown requires some sort of type check
}
```

------

**private vs protected**
```js
class Animal {
    // shorthand initialization of class properties
    // this.id = id;
    constructor(private id: number, protected species: string, public name: string) { }
  }

class Lion extends Animal {
    constructor(name: string) {
        super(0, "cats", name);
        this.id = 1; // compilation error, private is only accessible in class where its defined
        this.species = "lizards"; // all good here, protected is accessible by extended classes
    }
}
const lion = new Lion("Leo");
lion.name = "John"; // all good here, public properties can be accessed outside the class
```
------

**abstract**
```js
abstract class Animal {
    // abstract property, cannot be implemented in base class
    // must be implemented in inherited classes
    abstract sound: string; 
    constructor() { }

    // abstract method, cannot be implemented in base class
    // must be implemented in inherited classes
    abstract makeSomeNoise(): void;
}

class Lion extends Animal {
    sound = "Roaaarr!"
    constructor() {
        super(); 
    }
    makeSomeNoise() {
        console.log(this.sound);
    }
}

const animal = new Animal(); // error, cannot create instance of abstract class
const lion = new Lion();
lion.makeSomeNoise() // Roaaarr!
```
------

**getters, setters**
```js
class Animal {
    constructor(private species: string) { }
    
    // getter, writtern like method, accessable like a property
    get group() {
        return this.species;
    }
    
    // setter, writtern like method, accessable like a property
    set group(species: string) {
        this.species = species;
    }
}


const animal = new Animal("cats");
console.log(animal.group); // cats
animal.group = "dogs"; // setter can be used like a property
```
------

**Singelton and private constructor**
```js
class Animal {
    static instance: Animal;

    // private constructor prevents creating new Animal outside the class
    private constructor() { }

    static getInstance() {
        return this.instance ? this.instance : new Animal()
    }
  }

const animal = Animal.getInstance();
```
------

**Interface** allows us to define the structure of an object.       
Prefer interface to custom types when defining the object structure.   
```js
interface Levelable {
  level: number;
}

interface Magical extends Levelable {
  shootFireball(): void;
  pickUpPotion(n: string): number;
}

// implements - will obligate us to implement all of the properties and methods defined in the interface.
class Hero implements Magical {
  level = 1; // Typescript throws error if level property is missing on Hero class
  constructor(private readonly name: string) {
    this.greet();
  }

  greet() {
    console.log(`Greetings, I'm level ${this.level} hero, and my name is ${this. name}`);
  }

  // Typescript throws error if shootFireball method is missing on Hero class
  shootFireball(): void {
    console.log("Woooooosh!");
  }

  // Typescript throws error if pickUpPotion method is missing on Hero class
  pickUpPotion(n: string): number {
    return n === "+10health" ? 10 : 0;
  }
}




```

------

**How to use javascript libraries with TS ??**

1) Install type definitions for this specific library (if avaiable), for example:   
```js
  // let say we want to use lodash library in our TS project
  // so we need to provide our project with translations 
  // otherwise there will be compilation errors 
  npm install --save @types/lodash
```
2) Using **declare**   
```js
  // GLOBAL in a global variable which we know is there
  // but our file/project does not know about it
  declare var GLOBAL: string; 
```

------

# SOLID DESIGN PRINCIPLES IN TS

------

**SINGLE RESPONSIBILITY PRINCIPLE (SRP)**   
A class should only have **one** reason to change. (It should have only **one** responsibility / job)
```js
class BlogPost {
  title: string;
  content: string;

  constructor(title: string, content: string) {
    this.title = title;
    this.content = content;
  }

  createPost() {} // not implemented

  deletePost() {} // not implemented

  displayHTML() {
    return `<h1>${this.title}</h1><p>${this.content}</p>`;
  }
}
```
Currently the he blog post class has two responsibilites:   
1. to create/update/delete the properties of the blog post.
2. to display the html
       
Let's apply **SRP** and implement the display of the blog post in a separate class
```js
class BlogPost {
  title: string;
  content: string;

  constructor(title: string, content: string) {
    this.title = title;
    this.content = content;
  }

  createPost() {} // not implemented

  deletePost() {} // not implemented
}

class BlogPostDisplay {
  constructor(public blogPost: BlogPost) {}

  displayHTML() {
    return `<h1>${this.blogPost.title}</h1><p>${this.blogPost.content}</p>`;
  }
}
```

------

**OPEN CLOSED PRINCIPLE (OCP)**   
A software entity(class/module/function) should be **open** for extension but **closed** for modification
```js
class Discount {
  giveDiscount(customerType: "premium" | "regular"): number {
    if (customerType === "regular") {
      return 10;
    } else if (customerType === "premium") {
      return 20;
    } else {
      return 10;
    }
  }
}
```
Let's say we need to implement a new customer type - we would need to modify the current funtionality of the class          
Let's fix this by implementing Discount class by applying **OCP**
```js
interface Customer {
  giveDiscount(): number;
}

class RegularCustomer implements Customer {
  giveDiscount(): number {
    return 10;
  }
}

class PremiumCustomer implements Customer {
  giveDiscount(): number {
    return 20;
  }
}

class Discount {
  giveDiscount(customer: Customer): number {
    return customer.giveDiscount();
  }
}

const premiumCustomer: PremiumCustomer = new PremiumCustomer();
const discount: Discount = new Discount();
console.log(discount.giveDiscount(premiumCustomer))
```

------

**SINGELTON (a creational pattern)**

The **Singleton** pattern is used to ensure that a class has only one instance and provides a global point of access to that instance.

**Usecases**  
- Managing shared resources (like database connections, filesystem, cache).     
- Maintaining a single configuration object throughout an application, or creating a central point for managing application state.

**Problems**  
- It introduces a global state, which might lead to tight coupling and make testing more challenging.  
  (tests need to reset singeltons state for each test)  
```js
class Singleton {
  private static intance: Singleton;
  private static _value: number;

  private constructor() {}

  public static getInstance(): Singleton {
    if (!Singleton.intance) {
      Singleton.intance = new Singleton();
    }
    return Singleton.intance;
  }

  set value(value: number) {
    Singleton._value = value;
  }

  get value() {
    return Singleton._value;
  }

  public someMethod() {
      console.log("Executing some method...");
  }
}

let instance1 = Singleton.getInstance();
let instance2 = Singleton.getInstance();
console.log(instance1 === instance2); // true
```

------

**PROTOTYPE (a creational pattern)**

The **Prototype** pattern is used when the construction of a new object is more efficient by copying an    
existing instance rather than creating a new one from scratch.

**Usecases**  
- Cloning existing objects is less resource-intensive or complex compared to creating a new objects.    
- When you need multiple instances of similar objects with minor variations.

**Problems**  
- If your objects contain nested or complex structures, cloning them shallowly might lead to unintended sharing of references. 
```js
class Prototype {
    property1: string;
    property2: number;

    constructor(property1: string, property2: number) {
        this.property1 = property1;
        this.property2 = property2;
    }

    clone(): this {
        const clone = Object.create(this);
        return clone;
    }
}

// Usage
const prototypeInstance = new Prototype("value1", 123);
const clonedInstance = prototypeInstance.clone();

console.log(clonedInstance.property1); // Output: value1
console.log(clonedInstance.property2); // Output: 123
```
------

**BUILDER (a creational pattern)**

The **builder** pattern is used to create complex objects step by step.

**When to use**  
- Simplify the construction process of complex object.    
- Can be used to create different representations of objects.  

**Problems**  
- Increased code complexity.
- Potential for misuse by other developers.
- Learning curve.

```js
interface Builder {
  setSeats(seats: number): this;
  setEngine(engine: string): this;
}

class Car {
  private seats: number;
  private engine: string;

  public setSeats(seats: number): void {
    this.seats = seats;
  }

  public setEngine(engine: string): void {
    this.engine = engine;
  }

  public show(): void {
    console.log(`Seats: ${this.seats}`);
    console.log(`Engine: ${this.engine}`);
  }
}

class Motorcycle {
  private seats: number;
  private engine: string;

  public setSeats(seats: number): void {
    if (seats > 2) {
      throw new Error("Motorcycle can not have more than 2 seats");
    }
    this.seats = seats;
  }

  public setEngine(engine: string): void {
    this.engine = engine;
  }

  public show(): void {
    console.log(`Seats: ${this.seats}`);
    console.log(`Engine: ${this.engine}`);
  }
}

class CarBuilder implements Builder {
  private car: Car;

  constructor() {
    this.car = new Car();
  }

  public setSeats(seats: number): this {
    this.car.setSeats(seats);
    return this;
  }

  public setEngine(engine: string): this {
    this.car.setEngine(engine);
    return this;
  }

  public getResult(): Car {
    return this.car;
  }
}

class MotorcycleBuilder implements Builder {
  private motorcycle: Motorcycle;

  constructor() {
    this.motorcycle = new Motorcycle();
  }

  public setSeats(seats: number): this {
    this.motorcycle.setSeats(seats);
    return this;
  }

  public setEngine(engine: string): this {
    this.motorcycle.setEngine(engine);
    return this;
  }

  public getResult(): Motorcycle {
    return this.motorcycle;
  }
}

class Director {
  public buildToyota(): Car {
    return new CarBuilder().setSeats(7).setEngine("V-6").getResult();
  }

  public buildYamaha(): Motorcycle {
    return new MotorcycleBuilder().setSeats(1).setEngine("V-2").getResult();
  }
}

// Example code using directly the builders
const car = new CarBuilder().setSeats(2).setEngine("V-12").getResult();
const motorcycle = new MotorcycleBuilder()
  .setSeats(2)
  .setEngine("V-4")
  .getResult();

// Example code using the director
const director = new Director();
director.buildToyota();
director.buildYamaha();
```
Another example
```js
 interface User {
  name: string;
  age: number;
  gender: "male" | "female" | "unspecified";
}

class UserBuilder {
  private readonly _user: User;

  constructor() {
    this._user = {
      name: "",
      age: 0,
      gender: "unspecified"
    };
  }

  name(name: string): UserBuilder {
    this._user.name = name;
    return this;
  }
  
  age(age: number): UserBuilder {
    this._user.age = age;
    return this;
  }

  build(): User {
    return this._user;
  }
}

// Example of usage
const userWithName: User = new UserBuilder()
  .name("John")
  .build();

// Versus without the builder
// Here we must specify all the parameters
// When we want to apply test cases, we would always need to add all the values
const user: User = { name: "John", age: 0, gender: "male" }
```
------

**FACTORY (a creational pattern)**

The **Factory** pattern is used to centralize object creation logic, providing a flexible way to create   
instances of different classes without exposing the instantiation logic to the client code

**When to use**  
- Use the Factory Pattern when you want to create objects without specifying the exact class of object that will be created.
- If the process of object creation involves complex logic or conditions, a factory method can encapsulate this logic in one place.
- If you supposed to create different types of objects, and you don't know what these objects will be until runtime.
- If you're dealing with a large number of similar classes that share a common superclass and you often need to instantiate one of these classes,
  but you don't know ahead of time which one you'll need to instantiate.
- If your code involves conditional creation of objects based on certain parameters or environmental conditions,  
  a Factory Method can encapsulate this conditional logic and make your code easier to read and maintain.

**Problems**  
- Factory methods usually provide a static interface, which can make it difficult to subclass or modify the behavior of the factory without modifying its code directly.
- Introducing a factory can add complexity to the codebase, especially if it's not necessary for the scale of the project.

**Usecases**  
- **Database access**   
  Factories can be employed to create database connection instances, allowing for flexibility in choosing the type of database or connection parameters at runtime.
- **Object Pooling**   
  Factories can be used to manage object pools, where a fixed set of objects is created and reused to improve performance in scenarios with high object creation costs.
- **Dependency Injection**  
  Factories are often used in Dependency Injection frameworks to instantiate objects dynamically based on configuration or runtime conditions.
- **UI Component Libraries**  
  In frontend development, UI component libraries often utilize the Factory Pattern to create various types of UI components (e.g., buttons, forms, modals) based on configuration options provided by developers.
- **API Clients**  
  A factory can be used to create instances of API clients tailored to different endpoints, versions, or authentication methods, while abstracting away the creation details from the client code.
- **Game Development**  
  Factories can be used to create various types of game objects (e.g., characters, weapons, enemies) based on game states, levels, or player actions.

```js
interface GameCharacter {
    attack(): void;
}

class Warrior implements GameCharacter {
    attack(): void {
        console.log('Warrior attacks with sword!');
    }
}

class Mage implements GameCharacter {
    attack(): void {
        console.log('Mage casts a fireball spell!');
    }
}

class Archer implements GameCharacter {
    attack(): void {
        console.log('Archer shoots an arrow!');
    }
}

class CharacterFactory {
    // Factory method to create characters based on type
    static createCharacter(type: string): GameCharacter {
        switch (type) {
            case 'warrior':
                return new Warrior();
            case 'mage':
                return new Mage();
            case 'archer':
                return new Archer();
            default:
                throw new Error('Invalid character type');
        }
    }
}

// Client code
const warrior = CharacterFactory.createCharacter('warrior');
warrior.attack(); // Output: Warrior attacks with sword!

const mage = CharacterFactory.createCharacter('mage');
mage.attack(); // Output: Mage casts a fireball spell!

const archer = CharacterFactory.createCharacter('archer');
archer.attack(); // Output: Archer shoots an arrow!
```
------

**ABSTRACT FACTORY (a creational pattern)**

The **Abstract Factory** involves multiple Factory mehtods, one for each type of object to be created.

**When to use**    
- When you need to create families of related objects, where each family consists of objects that are designed to work together.  
- When different families of objects need to be swapped interchangeably, like when supporting multiple database engines in an application.  
- When you want to ensure that groups of related objects are created together and are compatible, such as various UI elements in a graphical interface.    
- When you need to test or mock complex systems by abstracting object creation behind interfaces, facilitating unit testing and code maintenance.  

**Pros**  
- By separating the responsibility of object creation from the client code, Abstract Factory promotes a clean separation of concerns.      
 This enhances code readability, maintainability, and scalability, especially in large or complex systems.   
 
**Problems**    
- Increased complexity.  
- Increased abstraction.  
- More rigid codebase.  

**Usecases**  
- Similar to Factory pattern

```js
interface Button {
  render(): void;
  onClick(f: Function): void;
}

interface GUIFactory {
  createButton(): Button;
}

class WindowsButton implements Button {
  public render(): void {
    console.log("Render a button in Windows Style");
  }
  public onClick(f: Function): void {
    console.log("Windows button was clicked");
    f();
  }
}

class MacOSButton implements Button {
  public render(): void {
    console.log("Render a button in MacOS Style");
  }
  public onClick(f: Function): void {
    console.log("MacOS button was clicked");
    f();
  }
}

class WindowsFactory implements GUIFactory {
  public createButton(): Button {
    return new WindowsButton();
  }
}

class MacOsFactory implements GUIFactory {
  public createButton(): Button {
    return new MacOSButton();
  }
}

// Client Code
function renderUI(factory: GUIFactory) {
  const button = factory.createButton();

  button.render();

  button.onClick(() => {
    console.log("Button Clicked");
  });
}

renderUI(new WindowsFactory());
renderUI(new MacOsFactory());
```
------

**FACADE PATTERN (a structural pattern)**

The **Facade** provides a simplified interface to a complex system, subsystem, or set of interfaces.   
It aims to hide the complexities of the underlying system and present a unified interface to the client code.

**When to use**    
- When you need to hide the complexities of the subsystem and provide a single, straightforward interface for interacting with it.
- When you need to encapsulate the interactions with the subsystem within a single class.

**Pros**  
- Reduces coupling between client code and the subsystem, as clients interact with the facade rather than directly with subsystem components.
- Improves the readability of client code by abstracting away the details of the subsystem.
- Encapsulates the complexities of the subsystem, promoting encapsulation and information hiding.
-  
**Problems**    
- May limit flexibility in certain scenarios, as it provides a fixed interface to the subsystem.
- Adds an additional layer of abstraction, which may introduce some overhead, especially in performance-critical systems.
- While it simplifies the interface for client code, it might add complexity to the facade itself, especially if the subsystem evolves over time.

**Usecases**  
- **Game Engines**  
  Provides simplified API for game developers, hiding complexities of underlying engine subsystems

```js
// Subsystem components
class InputSystem {
    constructor() {
        console.log("Input system initialized");
    }

    handleInput(): void {
        console.log("Handling user input");
    }
}

class GraphicsSystem {
    constructor() {
        console.log("Graphics system initialized");
    }

    render(): void {
        console.log("Rendering graphics");
    }
}

class AudioSystem {
    constructor() {
        console.log("Audio system initialized");
    }

    playSound(): void {
        console.log("Playing game sounds");
    }
}

// Facade
class GameEngineFacade {
    private inputSystem: InputSystem;
    private graphicsSystem: GraphicsSystem;
    private audioSystem: AudioSystem;

    constructor() {
        this.inputSystem = new InputSystem();
        this.graphicsSystem = new GraphicsSystem();
        this.audioSystem = new AudioSystem();
    }

    startGame(): void {
        console.log("Game started");
    }

    updateGame(): void {
        this.inputSystem.handleInput();
        this.graphicsSystem.render();
        this.audioSystem.playSound();
    }

    stopGame(): void {
        console.log("Game stopped");
    }
}

// Client code
const gameEngine = new GameEngineFacade();

// Using the facade
gameEngine.startGame();
gameEngine.updateGame();
gameEngine.stopGame();

```
------




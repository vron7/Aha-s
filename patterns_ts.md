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
**BRIDGE PATTERN (a structural pattern)**

The **Bridge** pattern lets you split a large class or a set of closely related classes into two separate hierarchies -      
abstraction and implementation, which can be developed independently of each other. It allows you to decouple the    
abstraction (what you want to achieve) from the implementation (how you achieve it), making it easier to change   
or extend either part independently 

**When to use**    
- When you want to hide implementation details from clients.
- When you want to switch implementations at runtime.
- When you need to support multiple platforms or systems.

```js
// Abstraction
interface GameObject {
    draw(): void;
}

// Implementor
interface RenderingEngine {
    render(x: number, y: number): void;
}

// Concrete Implementor 1
class WebGLRenderer implements RenderingEngine {
    render(x: number, y: number): void {
        console.log(`Drawing object at (${x}, ${y}) using WebGL`);
    }
}

// Concrete Implementor 2
class Canvas2DRenderer implements RenderingEngine {
    render(x: number, y: number): void {
        console.log(`Drawing object at (${x}, ${y}) using 2D Canvas`);
    }
}

// Refined Abstraction
class GameObject2D implements GameObject {
    private x: number;
    private y: number;
    private renderingEngine: RenderingEngine;

    constructor(x: number, y: number, renderingEngine: RenderingEngine) {
        this.x = x;
        this.y = y;
        this.renderingEngine = renderingEngine;
    }

    draw(): void {
        this.renderingEngine.render(this.x, this.y);
    }
}

// Usage
const object1 = new GameObject2D(100, 200, new WebGLRenderer());
const object2 = new GameObject2D(300, 400, new Canvas2DRenderer());

object1.draw();
object2.draw();

```
------
**COMPOSITE PATTERN (a structural pattern)**

The **Composite** pattern lets you compose objects into tree-like structures and then work    
with these structures as if they were individual objects.

The main benefit of this design pattern is that it allows you to run operations over both simple and complex elements.         
The client code can treat both types of elements in the same way (treat a file same way as a folder).     

**When to use**    
- You want to perform operations on a collection of objects the same way you’d perform them on individual objects.
- The structure of objects forms a tree-like pattern.

**Usecases**  
- **GUI Components**   
  The Composite Pattern helps simplify working with complex GUI elements like panels and simple ones like buttons by
  treating them uniformly in a tree-like structure. For example they all have common render method.

```js
// Component interface - an interface for all objects in the composition
interface GameObject {
    name: string;
    render(): void;
}

// Leaf -  defines the behavior for the elements in the composition. It has no children.
class Sprite implements GameObject {
    constructor(public name: string) {}

    render(): void {
        console.log(`Rendering Sprite: ${this.name}`);
    }
}

// Composite - stores child components and implements child-related operations in the component interface.
class Scene implements GameObject {
    private children: GameObject[] = [];

    constructor(public name: string) {}

    add(object: GameObject): void {
        this.children.push(object);
    }

    remove(object: GameObject): void {
        const index = this.children.indexOf(object);
        if (index !== -1) {
            this.children.splice(index, 1);
        }
    }

    render(): void {
        console.log(`Rendering Scene: ${this.name}`);
        this.children.forEach(child => {
            child.render();
        });
    }
}

// Client code
const sprite1 = new Sprite("Character");
const sprite2 = new Sprite("Enemy");

const mainScene = new Scene("MainScene");
mainScene.add(sprite1);
mainScene.add(sprite2);

const world = new Scene("World");
world.add(mainScene);

// Rendering the game world
world.render();

```
------

**DECORATOR PATTERN (a structural pattern)**

The **Decorator** pattern allows you to dynamically add or override behaviour in an existing object without changing its implementation.    

This pattern is particularly useful when you want to modify the behavior of an object without affecting other objects of the same class.     

**When to use**    
- Is a good alternative to subclassing when you need to add functionality at runtime.
- You want to add a few additional properties to some objects, but not to all.
- When extending class functionality is not a viable option.
- You want to ensure that the system can be easily extended in the future.
- You need to add responsibilities to an object that can be withdrawn later. 

**Usecases**  
- Middleware in web dev
- **GUI toolkits**   
  Instead of having many sublasses (WindowWithScrollbar, WindowWithMenu, WindowWithScrollbarAndMenu),
  you can use decorators to add each feature individually to a Window object. 

```js
// Base Button interface
interface Button {
    render(): void;
}

// Concrete implementation of the Base Button
class BasicButton implements Button {
    render(): void {
        console.log("Rendering Basic Button");
    }
}

// Decorator for adding hover effect
class HoverButton implements Button {
    constructor(private button: Button) {}

    render(): void {
        this.button.render();
        console.log("Adding Hover Effect");
    }
}

// Decorator for adding special effects
class FXButton implements Button {
    constructor(private button: Button) {}

    render(): void {
        this.button.render();
        console.log("Adding Special FX");
    }
}

// Usage
const basicButton: Button = new BasicButton();
const hoverButton: Button = new HoverButton(basicButton);
const hoverFXButton: Button = new FXButton(hoverButton);
```
Same example but with having an abstract class for decorators
```js
// Base Button interface
interface Button {
    render(): void;
}

// Concrete implementation of the Base Button
class BasicButton implements Button {
    render(): void {
        console.log("Rendering Basic Button");
    }
}

// Abstract class for button decorators
abstract class ButtonDecorator implements Button {
    constructor(protected button: Button) {}

    abstract render(): void;
}

// Decorator for adding hover effect
class HoverButton extends ButtonDecorator {
    render(): void {
        this.button.render();
        console.log("Adding Hover Effect");
    }
}

// Decorator for adding special effects
class FXButton extends ButtonDecorator {
    render(): void {
        this.button.render();
        console.log("Adding Special FX");
    }
}

// Usage
const basicButton: Button = new BasicButton();
const hoverButton: Button = new HoverButton(basicButton);
const hoverFXButton: Button = new FXButton(hoverButton);
```
One more example
```js
// Base Character interface
interface Character {
    attack(): void;
}

// Concrete implementation of the base Character
class BasicCharacter implements Character {
    attack(): void {
        console.log("Basic Attack");
    }
}

// Character decorator representing equipped items
class EquipmentDecorator implements Character {
    constructor(private character: Character, private equipments: string[]) {}

    attack(): void {
        this.character.attack();
        for (const equipment of this.equipments) {
            console.log(`Attacking with ${equipment}`);
        }
    }
}

// Usage
const basicCharacter: Character = new BasicCharacter();
const characterWithSwordAndMagic: Character = new EquipmentDecorator(basicCharacter, ["Sword", "Magic Wand"]);

basicCharacter.attack();
console.log("------");
characterWithSwordAndMagic.attack();

```
------

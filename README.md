# Javascript Aha-s

**Array is an Object** (key = index)
```js
var arr = {
	0: 'hola'
	1: 'hi'
	2: 'privet'
}
typeof([]); // returns "object"
```

------

**Method** is a function inside an Object
```js
var wizard = {
	name: 'Gandalf'
	excuse: function(){
		return 'A wizard is never late, nor is he early!'
	}
}
```

--------------------------------------------------------------------------------------------------------------

**Null** is special Javascript type, it’s an empty Object
```js
var wizard = {};
wizard.name = 'Gandalf';

wizard = undefined; 	 // wizard is NOT an object anymore
wizard = null; 		 // wizard is an object but completely empty
typeof(wizard); 	 // returns "object"
wizard.name = 'Gandalf'; // returns error
```

---------------------------------------------------------------------------------------------------------

**Arguments** vs **Parameters**
```js
function foo ( username ){}  	// username is a parameter
foo('vova'); 			// vova is an argument
```
Function accepts **parameters** but is run using **arguments** to match parameters.

-------------------------------------------------------------------------------------------------------

**Expressions end with semicolons, not declarations**
```js
function foo() {} 	// declaration
var foo = function(){}; // expression (somethign that produces a value)
if (n == 0){} 		// declaration
```

------------------------------------------------------------------------------------------------------------------------------

**++a** or **a++**
```js
var a = 0;
a++;	// returns 0 (original value)
a 	// returns 1
var a = 0;
++a;  	// returns 1
a 	// returns 1 
```

------------------------------------------------------------------------------------------------------------------------------
```js
btn.addEventListener("click",handler) - handler is treated as callback

<button onclick=”handler()”></button> - handler is a string, treated as JS expression
```

For example can be: *onclick="handler();alert(77);"*

---------------------------------------------------------------------------------------------------------------------------------

*jQuery* - was good when it came, now deprecated, is imperative    
*React* - is declarative

----------------------------------------------------------------------------------------------------------------------------------

**Ternary operator** (use when 1 check and 2 possible outcomes)
```js
condition ? expr1 : expr2; //(Is this condition true/false?, if true then expr1 else expr2)
var mood = isTodayFriday() ? 'super' : 'not bad either'
```

---------------------------------------------------------------------------------------------------------------------------------------------------------
**let** vs **var**
```js
var notChange = true;
let notReallyChange = true; 
if (true){
	var notChange = false;
	let notReallyChange = false;
}
console.log(notChange); // false
console.log(notReallyChange); // true
```

Anytime let is wrapped with curly brackets (block), it creates a new scope(block scope), just like var inside a function
```js
let player = yes;
let player = false;  // Returns an error! Already decleared!
```

-------------------------------------------------------------------------------------------------------------------------------

**const** (wannabe constant)
```js
const player = { name: 'Tim' };
player = {}; 		// returns error, cannot be reassigned
player.name = 'Bob'; 	// no error, allowed
```

---

**Destructing**
```js
const player = {
	name = 'Tim',
	level = 5
}
const {name, level} = player; // Select specific properties from an object
```

Same as:
```js
const name = player.name;
const level = player.level;
```
---

**Dynamic object properties**
```js
const prop = 'firstName';
const player = { [prop]: 'Timmy'};
const silly = { [1+2]: 3 };
```
Also we can do:
```js
const name = 'Timmy';
const level = 1;
const bag= {};
const player = {name, level, bag};
```
---

**Template strings** (uses character  **`** )
```js
const name = 'Timmy';
const age = 33;
const greetingOld = "Hello " + name + " your age is " + age;
const greetingNew = `Hello ${name} your age is  ${age}`
```
---

**Arrow functions**
```js
function add(a, b) {
	return a + b;
} 
```

Can be written as:
```js
const add = (a, b) => a + b;
```

Or
```js
const add = (a, b) => {
	return a + b;
}
```


Oneliner assumes there is a single return:
```js
const add = (a, b) => a + b;
```

Extra: when 1 parameter, brackets can be ignored:
```js
const sqrt = b => b * b;
```

An arrow function expression is a compact alternative to a regular function expression, although without its own bindings to the this, arguments, super, or new.target keywords. Arrow function expressions are ill suited as methods, and they cannot be used as constructors.
```js
var r = { n: function( ) { console.log(this) } }  // this returns r
var r = { n: ( ) => { console.log(this) } } 	  // this returns Window, arrow function not suited as method!
```
An arrow function **cannot** be used as a **constructor**
```js
const Car = (model) => {this.model = model}
const car = new Car('Ford'); // throws error 'Car is not a constructor'
```

---

**map, reduce, filter** vs **forEach**  
Map, reduce and filter are pure functions, they all expect a return, forEach does not.
```js
const arr = [1, 4, 6];
const mapArr = arr.map(num => num * 2); //Each callback has to return something
```
	vs
```js
const double = []
const newArr = arr.forEach(num => double.push(num*2));  //Returns undefined 
```
Also - map, reduce, filter all return a new array!

---

**Currying** is a process of turning a function with multiple arity into a function with less arity.
Or other words - transform a function of arity n to n functions of arity 1.
f(a, b, c) ---------------> f(a)(b)(c)
```js
const mult = (a, b) => a * b;		// normal
const cMult = (a) => (b) => a * b;  	// curried

mult(2, 3) is same as cMult(2)(3) 

//Benefits? I can build and configure functions like so:

const mult2 = cMult(2);
mult2(4); //returns 8
mult2(6); //returns 12
```
---

**Closure** is a feature in Javascript when the inner function has access to outer function variables(scope/context).  
Closure remembers variables from the place where it is defined, no matter where it is executed.
```js
const hello = ( ) => {
	const greet = 'Hello';
	const sayHello = ( name ) => {
		alert(`${greet} ${name} !`);
	}
	return sayHello
}
```
Closure = function + functions lexical scope

---

**More about Currie-ing**
Curried function is a function which takes multiple arguments, one at a time.
```js
const add = a => b => a + b;
const result = add(2)(4); // 6
```

In this example, the add function takes one argument and then returns a partial application of itself with a fixed in the closure scope.

A Partial application is a function which has been applied to some, but not yet all of its arguments. In other words a function which has some arguments fixed inside its closure scope.

Partial, Curry what's the difference??
Partial applications can take as many or as few arguments a time as desired. 
Curried functions always return a unary function.(function which takes one argument).

So all curried functions return partial applications but not all partial applications are the result of curried functions.

---

**Compose** is a way to combine multiple functions to create a new function
```js
const compose = ( f, g ) => a => f(g(a));
const f1 = n => n + ‘!!!’
const f2 = n => n + ‘???’
const create = compose(f1, f2);
create(‘Whats up’); // Whats up!!!???
```

---
**Reference type**

**Primitive types** (Known by javascript, immutable) - string, number, boolean, null, undefined
```js
var a = 1; //value gets stored in variable (memory)
var b = a; //value gets copied to a new variable (new slot in memory)
a === b;   // returns true 
a = 2;     // here we do not mutate number 1 into 2, we just replace the 1 with 2
a === b    // returns false, because a holds 2 and b holds 1
```

Primitive types are immutable, meaning their value(state/structure) cannot be changed after creation.
```js
var name = 'Tim'; //name now points to 'Tim' in memory
name = 'Tom'; // new block of memory is allocated for 'Tom', name now points to 'Tom' in memory,  'Tim' is still in memory and will be garbage collected.
```

Using const is just protection, not immutability!

**Reference types** (created by developer) - Object (Array, Function - also technically objects)
```js
var n = { id: 1} //value gets stored in memory, address of the stored object gets stored in variable
var m = n; //address gets copied, a reference to the value is set
m === n; //returns true, they both point to a same address
m.id = 2 //here we mutate the object
m === n //returns true, they both point to the same mutated object in memory
m = n = { id: 3 } // new object is created in memory, both variables receive the new reference, reference to the old object is lost, old object can be garbage collected!
```
```js
[] === []; // returns false, they do not share the same reference
```

For immutable data, the equality is more reliable!
```js
1 === 1 //true
{id: 1} === {id: 1} //false
```

---------------------------------------------------------------------------------------------------------------------------------------------------------

**Cloning objects**
```js
var w = { name: {fn: Harry, sn: Potter}, house: Gryffindor};

//Shallow clone
var w2 = {...w}; //w2 no longer references w but w2.name still references w.name object in memory! Only the first layer is cloned!

//Deep clone
var w3 = JSON.parse(JSON.stringify(w)); //Object is converted to string and then back to object - no more references (Can be slow!).
```

---

**Context**

*this* - inside what object am I right now? What’s the context?
```js
console.log(this); //returns Window (in browser)
function a(){ console.log(this) }; //returns Window 
var obj = { 
	a: function(){
		var self = this;
		console.log(this) // returns obj
		var b = function(){
			console.log(this) // returns Window :o - just how JS works
			console.log(self) // returns obj - wohoo workaround
			// b goes out to its lexical environment to find self
		}
		// arrow function preserve the context of this
		var c = () => {
			console.log(this) // returns obj
		}
	} 
}; 
const a = obj.a;
a(); // returns window (undefined in strict mode), since this gets lost when function in detached from object;

```
```js
class Robot {
    id = "BB-MT1";
    f1 = () => console.log(this.id) // field
    f2 = function() {console.log(this.id)}  //field  
    f3() {console.log(this.id)} //method
}
const robot = new Robot();
robot.f1(); // returns id
robot.f2(); // returns id
robot.f3(); // returns id
const { f1, f2, f3 } = robot; // destructing, this gets lost
f1(); // returns id since arrow funtion preserves its context and has a closure to this
f2(); // returns error sice this is undefined (Classes use strict mode, otherwise would return Window)
f3(); // same as f2
```
---

**Classes**
Javascript classes are special functions (syntactical sugar to prototype inheritance).    
Classes user strict mode by default.

*super* - refers to parent class, it is used to call the constructor of the parent’s class and access its properties and functions.
```js
class Hooman {
    // class field
    id = "homo sapeins";
    
    // also class field, added to instance not prototype    
    walk = () => console.log("walk walk")
    
    constructor(name) {
		this.name = name;
    }
    
    // class mehtod, added to prototype  
    talk() {
        console.log("bla bla");
    }
}
class Worker extends Hooman{
	constructor(name, profession) {
		//console.log(this.name); // returns error, call super first!
		// since we extend Hooman, we must call super in order to invoke the constructor function of the Hooman
		super(name) // access parents properties and functions
		console.log(this.name); // returns name
		this.profession = profession;
	}
}
```
---

**===** or **==** ?
```js
1 == "1"   //returns true, type coercion is applied
1 === "1"  //returns false, explicit check
true == 1  //true, 1 is coerced to true by JS
true === 1 //false
```

---

**Array.prototype.join()**
```js
const arr =[['My', 'name'],['is', 'Constantine']];
arr.map(n => n.join(" ")).join(' '); //My name is Constantine

const obj = {my:'name', is: 'Constantine'};
Object.entries(obj).map(n=>n.join(' ')).join(' '); //My name is Constantine
```
---

**Enumeration** vs **Iteration**

```js
for(item of arr) {} // iterate, throws error when using on object, objects are not iterable

for (item in obj) {} //enumerate - loop through all properties which are enumerable, also works for arrays, because array is an object
```
Example:
```js
var cars = ['Toyota', 'Ford']
cars.vw = 'Volkswagen'
Object.defineProperty(cars, 'bmw', {value:'BMW', enumerable: false })

for(car in cars){console.log(car)} //Prints out all the properties
//0
//1
//vw

//bmw is ignored, because its property .enumerable is set to false, for others its set to true by default!
```
---

**An object** is a collection of properties!
*Property* - is an association between a name(key) and a value
Property's value can be a function in which case the property is known as a method.

```js
var wizard = { name: 'Gandalf' } //name is a property with a value Gandalf
var wizard = ['Gandalf'] // 0 is a property with a value Gandalf
```
---

How **async** works in **synchronous** Javascript:  
![alt text](https://github.com/vron7/Aha-s/blob/master/jsre.png "How JS engine works")  
*Image credit:* https://medium.com/@olinations/the-javascript-runtime-environment-d58fa2e60dd0

---
More **let**
```js
var cb = []

for(var i = 0; i < 3; i++){
	cb[i] = function(){ return i }
}
cb[0](); //returns 3 :(

for(let i = 0; i < 3; i++){
	cb[i] = function(){ return i }
}
cb[0](); //returns 0
```

Let’s try to fix the the first example using IIFE (Immediately Invoked Function Expression):
```js
for(var i = 0; i < 3; i++){
	(function(i){
		cb[i] = function(){ return i }
})(i);
}
cb[0](); //returns 0
```
---
Let's **exploit** following code
```js
const bag = () => {
  let arr = [];
  return {
    append: (n) => arr.push(n),
    get: (i) => arr[i],
    store: (i, n) => arr[i] = n
  };
}
```

Is it possible to access internal variable **arr** outside the bag? Let's try!

```js
var arrSteal;
var b = bag();
b.store('push', function(x){arrSteal = this}); // arr['push'] = function(){this}, ARRAY is an OBJECT!
b.append(2);
console.log(arrSteal); // arr
```
How to fix?
```js
arr.splice(index, 0, n) // instead of arr[i] = n
```

---

**new** operator

When a function is executed with **new** keyword, following occurs:  
* a new empty object is created and assigned to **this**
* the value of **this** is **returned**.

```js
function User(name) {
  // this = {};  (implicitly by js engine)
  this.name = name;
  // return this;  (implicitly by js engine), unless function itself returns a value
}

var john = new User('John'); // {name: 'John'}
var mary = User('Mary');     // mary is undefined

// when using new operator, could think of it as following:
var john = {};
User.call(john, 'John');

```

---

Why **var** global but **let/const** not??

```js
var a = 'Marco!'
let b = 'Polo!'
window.a // returns 'Marco!'
window.b // returns undefined :( whyyy??
```
Both are still **global** - var is stored in window object, let/const is stored in declarative environment,
they do not create properties of the window object when declared globally.

---

**Hoisiting**

```js
b() // returns bar!
a   // returns undefined :(
c   // returns Error - c is not defined
d() // return Error - d is not a function
 
var a = 'foo!'
function b(){ return 'b'} // function declatarion is hoisted
var d = function(){ return 'd'} // function expression is not hoisted (variables are assigned undefined)
```
**Why??** - execution context is created in two phases:    

**1) Creation phase:**  
	* global object, this and outer environment is created 
	* parser sets up memory space for variables and functions
	* variables are assigned **undefined** as their value
	* functions are placed into memory in their **entirety**   
	
**2) Execution phase:**   
	* code starts to execute line by line, we can now access these variables and functions
	* variables are assigned a value as the code executes

What I write is not what's directly being executed!

---

Every time a function is **invoked** (invocation):   
* a new execution context is created for that function (in two phases - creation phase and execution phase)
* execution context is put at the top of the execution stack
* when funtion is finished the execution context is popped off the stack
```js
function b(){
	// b is invoked, a new execution context is created and placed to the top of execution stack
	return // execution context for b is popped off the stack
}
function a(){
	// a is invoked, a new execution context is created and placed to the top execution stack
	b(); // invoke function b
	return // execution context for a is popped off the stack
}
 
a(); // global -> a -> b -> a -> global
```

---

**scope chain**

Along with execution context, each function gets a reference to its **outer lexical environment**.   
Lexical meaning - where the funtion sits inside the code?

```js
function a(){
	console.log(n); // 1
}
function b(){
	var n = 2;
	a();
}
var n = 1;
b();
```

Why?  

Because function **a** sits lecically in the global environment, it gets reference to the global environment.  
It does not find variable n from its own execution contexts, so it tries to find it from its outer lexical environment.  
The scope chain runs all the way up to the global environment.

```js
function b(){
	function a(){
		// a sits lecically inside b, it goes to b to look for n
		// it finds n from b, n is undefined because a is invoked before n gets a value
		console.log(n); // undefined
	}
	function c(){
		// c sits lexically inside b
		// c finds n from b
		console.log(n); // 2

		// it does not find m from b 
		// it goes fruter in scope chain and finds m from global
		console.log(m); // 3
	}
	a();
	var n = 2;
	c();
	
}
var n = 1;
var m = 3;
b();
```

---

Use **null** when you want to set variables to nothing, leave **undefined** for Javascript engine.  

```js
var n = 1;
n = null;
n = undefined; // DON'T
```

---

**operator** is just a special function which is syntatically different
```js
sum(1, 2); // function notation

var n = 1 + 2; // infix notation - function name (the operator) sits between the parameters

// conceptually var n = 1 + 2; can be written as:
function +(a, b) { return a + b}
function =(a, b){ return a = b}
=(n, sum(a,b))

+5    // unary operator (single operand)
1 + 5 // binary operator (two operands)
```

---

operator **precedence** and **associativity**
```js
var n = 2 + 3 * 4 // precedence determines which operator is run first
var m = 2 + 3 + 4 // associatity determines in which order operators with the same precedence are run
```

---

**cohersion** - converting a value from one type to another
```js
var n = '1' + 2; // '12'
var m = 3 < 2 < 1; // true - why?? 3 < 2 returns false, false < 1 returns true because false is cohersed to 0
Number(null); // 0 - this is blasphemy!!!
Boolean(null); // false - this is acctaully useful

```
Since operator is a **function**, cohersion is part of the process of calling the function.

---

**||** - (or) operator takes two values and returns a first one which coherses to **true**
```js
false || true; // true
"carrots" || null // carrots
"marco" || "polo" // marco
var name = params.name || 'john' // setting default value
```

---

All these scrips operate in the same global execution context.   
Imagine them being inside the single file.
```js
<html>
<head></head>
<body>
	<script>
		var n = 0;
	</script>
	<script src='start.js'></script> // var n = 1;
	<script src='lib1.js'></script> // var n = 2;
	<script src='lib2.js'></script>	// var n = 3; THE FINAL VALUE!
	<script>
		console.log(n); // 3
	</script>
</body>
</html>
```

---

**Dot** is just an operator :o  
**.** is an operator which accepts two parameters:
1. object
2. name of property   

object **.** property
```js
// operator takes object and propery name, and looks for the name on this object
person.name // member access operator, parser will convert name to "name"
person["name"] // computed member access operator
person   ["name"]  // there is space? no problem, also works, sicne it's an operator
person  .  name    // same
window  .  alert() // jep, same

person.address.street = 'wall-street' // 3 operators here, .(person, address) .(adress, street) =(street, 'wall-street')

var computer = {}
computer.software.os = 'Linux' // Uncaught TypeError: Cannot set property 'os' of undefined
// two operators, left to right associativity
// first operator (computer . software) returns undefined
// second operator (software . os ) returns error, since software is undefined

```

---

Every valid JSON is also a valid Javascript Object literal.   
Not every valid Javascript Object literal is a valid JSON.
```js
{ "name": "John" } // valid JSON and also a valid Object literal
{ name: "John" } // valid Object literal but invalid JSON because of quotes missing from property name

```

---

**Function** is an **Object**

```js
function greet(){ return 'helloo' } ;
greet.foo = 'bar';  // we can add primitives/objects/funtions
greet.name 	    // returns "greet" - functions can have special propery called name
greet 		    // returns the function code as text
greet() 	    // invoke the function code
greet(function hi(){}); // function is an object, we can create it on the fly and pass it to other functions

```
function **statement**
```js
function helloo(){}	     
// defines an object in current execution context
// created during creation phase - can be hoisted
```
function **expression** (always results in a value)
```js
var helloo = function(){};     
// returns an object
// created during execution phase - cannot be hoisted

helloo(function(){}); 
// also an expression, we create a function object on the fly and pass it to another function as argument
```

---

More **execution context**

```js
const a = 1; // is ignored, since function exec finds 'a' from its own variable space
const exec = () => {
    console.log(a); // undefined
    // 1) when execution context is created, all variables are set to undefined 
    // 2) now during execution phase, we set 'a' to its own value, which is undefined
    var a = a;     
    console.log(a); // undefined
}
exec();
```

---

automatic **;**
```js
function getSomething() {
	return  // js parser automatically adds  a semicolon here because it sees a cartridge return (enter press)
	{ 	// this part never reached because of return;
		name: 'Something'
	}
}
getObject(); // returns undefined :o

function getSomething() {
	return {
		name: 'Something'
	}
}
getObject(); // returns an object as expected
```

---
**IIFE**
```js
function(param){
	console.log('hello! ' + param)
} 
// returns an error, since parser sees word function and expects a function statement with function name

------------------------

(function(param){
	console.log('hello! ' + param)
}('World'))
// IIFE classical implementation
// no error, since it's inside parentheses and is exexuted as an expression
// it creates a function object on the fly and then invokes its code
```

---

Inside **closures**  
```js
function greet(whatToSay){
	return function(name){
		console.log(whatToSay + ' ' + name)
	}
}
sayHi = greet('Hola')
sayHi('John') // Hola John
```
So what happens here?
1. When greet function is invoked a new execution context is created and variable whatToSay is sitting in its variable environment
2. A new function object is created on the fly and returned
3. After a return, greet execution context is popped of the stack, its gone
4. But every execution context has its space in memory where variables and functions created inside of it live
5. On the normal circumstances, at some point this memory space is cleared up by GC when the execution context goes away
6. Function sayHi is invoked and a new execution contexts is created
7. Inside sayHi, when JS engine finds whatToSay variable, it goes up the scope chain to find it (from outer lexical environment reference)
8. Even though the greet function ended, every function inside it will still have a reference to that greet functions memory space
9. So we can say that the execution context has CLOSED in its outer variables
10. JS engine will always make sure that whatever function I'm running, it will have access to the variables it should have access to!

---

more **closures**

```js
function build(){
    var arr = [];
    for (var i = 0; i < 3; i++){
        arr.push(
            function(){ 
	    	// when this function get invoked it will start to look for variable i
	    	// it goes up the scope chain to find it from its outer environment
		// execution context for build function is gone, but memory space is still preserved thaks to the closure
		// i will have its final value as a result of the for loop
	    	console.log(i); // 3	    	
	    } 
        )
    }
    return arr;
}
var b = build();
b[0](); // 3
b[1](); // 3
b[2](); // 3
```

how to **"fix"** using IIFE? ( I want to print out the intermediate value of **i** in each loop cycle)

```js
function build(){
    var arr = [];
    for (var i = 0; i < 3; i++){
        arr.push(
            (function(j){  // here we get the value of the i 
	    	return function(){
			// when this function get invoked it will start to look for variable j
			// it will find j stored in a closure from its outer function (as a parameter)
			console.log(j); 
		}	    	   	
	    }(i)) // immideately invoke and store current value of i
        )
    }
    return arr;
}
var b = build();
b[0](); // 0
b[1](); // 1
b[2](); // 2
```

---

**call, bind, apply**    
These are methods on a function object and they all let you decide on what **this** should be
```js
var person = { 
	name: 'John', 
	getName: function() {
		return this.name;
	}
};
logName = function(num = 1) {
	console.log(this.name, num);
}

// bind creates and returns a copy of the function with this already binded to a passed object
logNameBinded = logName.bind(person);
logNameBinded(); // John 1
logNameBinded(2); // John 2

// call invokes the function
logName.call(person); // John 1
logName.call(person, 2); // John 2

// apply does same as call, but arguments need to be passed as an array
logName.apply(person, [2]); // John 2

//an example of a currying using bind - creating a copy of a function with some present parameters
function multiply(a, b) {
	return a * b;
}
var multiplyByTwo = multiply.bind(this, 2); // giving arguments sets the permanent parameter value when the copy is made
multiplyByTwo(5); // 10
```

---

**prototypal inheritance**  
*inheritance* - object gets access to another object properties and methods    
All objects in javascripts (including functions) have a **prototype** property - its a reference to another object, a proto.
```js
var person = {};
person.__proto__.name = 'Ivan';
person.name // returns Ivan, js engine first looks name property from person, if not found then from proto

var person2 = {};
person.name // returns Ivan, both objects reference the same proto

var arr = [1, 2];
arr.__proto__ = {}; // we remove the array proto
arr.pop(); // error : pop is not a function

// EVERYTHING IS AN OBJECT
var a = [];
a.__proto__.push(1)   // push method comes from arrays proto
a.__proto__.__proto__ // returns Object, we reached the bottom of prototype chain, the base object

var b = function() { };
b.__proto__.apply()   // apply method comes from functions proto
b.__proto__.__proto__ // returns Object, we reached the bottom of prototype chain, the base object

var c = { };
c.__proto__.toString() // toString method comes from objsets proto
c.__proto__.__proto__  // returns null, Objects proto is already the base object

a.__proto__.__proto__.toString = null
c.toString() // error, toString is not a function
// MEANING:
// everything is an Object
// everything points to the same base Object


```

---

**Javascript** name was created to attract **Java** developers :o

---

**Function constructor** - a function that is used to create objects.   
When invoked with the **new** operator, **this** is set to an empty object and that object is returned automagically. 
```js
function Person(name) {
	this.name = name;
}
var john = new Person('John');
```
---

**Function Constructor and prototype**   
Every function in javascript has a **prototype** property. It's **not** the prototype of the function, it's the prototype of the object which is created when
using the function as a function constructor ( **new** keyword )
```js
var n = {};
n.prototype // undefined, only funtions have the prototype property

var f = function() { }
f.prototype // an empty object
f.__proto__ // functions prototype
f._proto__ === f.prototype // false, not the same
f.prototype.__proto__ === f.__proto__.__proto__ // true, eventually point to the same base object

var Person = function(name) {
	this.name = name;
	// we could also define getName method here as this.getName = function (name) {...}
	// but funtions in js are objects and they take up memory space
	// every single Person object would get a copy of the getName function
}

// for efficiency its better to put methods on prototype, it's only created once
Person.prototype.getName = function() {
	return this.name;
}

// new keyword sets the prototype of the newly created object to the prototype property of the function which created the object
var john = new Person('John'); 
john.__proto__ === Person.prototype // true

// Wohoom, since protype is passed by reference, we can add new methods even after creating the object!
Person.prototype.shout = function(what) {
	console.log(what);
}
john.shout('Wohoooo!'); // Wohoooo!
john.call(); // error, it does not get the prototype of function, it gets the prototype property of the function

```

---

**Built-In Function Constructors** 

```js
typeof(String) 		  // function, a constructor function
String.indexOf 		  // undefined, indexOf is not in String prototype
String.prototype.indexOf  // function, indexOf belongs to String prototype property

var s = new String('John')  // a string object in created, not a primitive
var j = 'John' 	 	    // a string primitive
s == j 			    // true, coercion
s === j 		    // false, object vs primitive
s.__proto__ === j.__proto__ // true, js engine converts primitive to a string object
s.indexOf('h')		    // 2
j.indexOf('h')		    // 2, primitive converted to object
"John".indexOf('h')         // 2, primitive converted to object
"John".__proto__ === String.prototype // true, wohooooo

// Lets say we want to add a feature to all strings in js
// We modify the prototype property of built-in function constructor
String.prototype.isLengthGreaterThan = function(limit) {
	return this.length > limit;
}
"John".isLengthGreaterThan(3) // true, primitive converted to object
```
Let's try same on numbers
```js
Number.prototype.isPositive = function() {
	return this > 0;
}
var n = new Number(7)
var m = 7
m === n	           // false, primitive vs object
n.isPositive()     // true
m.isPositive()     // true
7.isPositive()     // error, js engine gets 7.0isPositive, i is unexpected token since there is no dot opetator
7..isPositive()    // true, js engine gets 7.0.isPositive()
7.0.isPositive()   // true

-7..isPositive()   // returns -1, why???? its a result of precedence plus coercion!
// 1) . operator has higher precedence than -
// 2) 7..isPositive() is executed first, which returns true
// 3) - is applied to the returned true, which js engine converts intro -1
(-7.).isPositive() // returns false, all good now

```

---

**Promise** is a standardized approach to dealign with asynchronous events and callbacks. now lets write one
```js
const PENDING = 0;
const FULFILLED = 1;
const REJECTED = 2;

function MyPromise(executor) {
    let state = PENDING;
    let value = null;
    let handlers = []

    // resolve function will be passed to the executor funtion
    // executor funtion calls resolve with a result
    function resolve(result) {
        if(state !== PENDING) return; // resolve only once

        state = FULFILLED;
        value = result;

        // pass result to the handlers
        handlers.forEach((h) => h(value));
    }

    // reject will be passed to the executor funtion
    function reject(err) {
        if (state !== PENDING) return; // reject only once
        state = REJECTED;
        value = err;
        // add logic here to handle reject
    }

    // accepts a callback and depending on state,
    // execute immideately or when promise is resolved 
    this.then = function(callback) {        
        if (state === FULFILLED) {
            callback(value);
        } else {
            handlers.push(callback);
        }
    }

    // finally run the executor funtion, which means
    // the creation of the Promise starts the actual work!
    executor(resolve, reject);
}

// lets use our promise
// define an executor function
const work = (res, rej) => {
    setTimeout(() => { res("Work Done!") }, 1000)
}
let promise = new MyPromise(work);
promise.then((result) => console.log(result)); // Work Done!
```

---
**Thenable** - any object with a **then** mehtod 
```js
var myCustomObject = { 
    then: (cb) => cb('hola!')  // do some work          
}
Promise.resolve()
.then(() => myCustomObject)
.then(console.log) // 'hola!'
```
---

Need to convert something into a  **number** fast? Use **unary plus** operator
```js
+"5"   // 5
+true  // 1
+false // 0
```

---

**destructing** object with new variable name and default value
```js
const person = { name: 'Paul' }; 
const { name:firstname = 'John', surname = 'Smith' } = person;
console.log(firstname, surname); // Paul Smith
console.log(name); // toyota is not defined
```

---
Let's write our own **javascript engine**
```js
function jsengine(code) {
	return code.split(/\s+/);
}
jsengine("const n = 10"); //  ['const', 'n', '=', '10']
```
This basic engine demonstrate how js parser reads code and breaks it into tokens.   
An abstract syntax tree is then formed out of the tokens, check it in action at https://astexplorer.net/

---

**positional** vs **named** arguments in js
```js
// positional arguments
function greet(firstName, lastName) {
	return "Hola " + firstName + " " + lastName + "!";
}

greet("Hozee", "Gonzales");

// named arguments do not exist in js, 
// but it can be achieved with passing an object as an argument
function greet({firstName, lastName}) {
	return "Hola " + firstName + " " + lastName + "!";
}

greet({firstName: "Hoze", lastName: "Gonzalez"});
```
benefits of using **named arguments** :   
- easier typings ( can use one block for types vs have to add types inline )
- we do not have to keep the order of arguments (less bugs)
- we can skip arguments we don't want to provdie   

But we can also use the **combination** of both, for example fetch method:
```js
// request with url as positional arg and options as named args.
fetch('https://google.com', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
});

// basic GET requests with just positional args
fetch('https://google.com');
```
---

Some fun:
```js
// How to get funtion name?
function helloWorld(){
	console.log(arguments.callee.name); // helloWorld
}
// How to log funtion properties?
dir(hellooWorld);
```
---

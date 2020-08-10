# Javascript Aha-s

**Array is an Object (key = index)**
```js
var arr = [
	0: 'hola'
	1: 'hello'
	2: 'privet'
]
typeof(arr) // returns "object"
```

------

**Method is a function inside an Object**
```js
var wizard = {
	name: 'Gandalf'
	excuse: function(){
		return 'A wizard is never late, nor is he early!'
	}
}
```

--------------------------------------------------------------------------------------------------------------

**Null is special Javascript type, it’s an empty Object**
```js
var wizard = {}
wizard.name = 'Gandalf';

wizard = undefined; // wizard is NOT an object anymore

wizard = null; // wizard is an object but completely empty
typeof(wizard); // returns "object"
wizard.name = 'Gandalf'; // returns error
```

-----------------------------------------------------------------------------------------------------------

**JS types:** Number, String, Object, Null, Undefined, Boolean, Symbol

---------------------------------------------------------------------------------------------------------

**Arguments VS Parameters**
```js
function foo ( username ){}  // username is a parameter
foo('vova'); // vova is an argument
```
Function accepts **parameters** but is run using **arguments** to match parameters.

-------------------------------------------------------------------------------------------------------

**Expressions end with semicolons, not declarations**
```js
function foo() {  } 	// declaration
var foo = function(){ }; 	// expression (somethign that produces a value)
if (n == 0){} 	// declaration
```

------------------------------------------------------------------------------------------------------------------------------

**++a or a++**
```js
var a = 0;
a++;  // returns 0 (original value)
a // returns 1
var a = 0;
++a;  // returns 1
a // returns 1 
```

-----------------------------------------------------------------------------------------------------------------------------

Prefer **querySelector** over getElementBy, it has css syntax and is more powerful

------------------------------------------------------------------------------------------------------------------------------
```js
btn.addEventListener("click",handler) - handler is treated as callback

<button onclick=”handler()”></button> - handler is string treated as JS expression
```

For example can be: onclick=”handler();alert(77);”

---------------------------------------------------------------------------------------------------------------------------------

jQuery - was good when it came, now deprecated, is imperative
React - is declarative

----------------------------------------------------------------------------------------------------------------------------------

**Ternary operator** (use when 1 check and 2 possible outcomes)
```js
condition ? expr1 : expr2; //(Is this condition true/false?, if true then expr1 else expr2)
var mood = isTodayFriday() ? 'super' : 'not bad either'
```

--------------------------------------------------------------------------------------------------------------------------------------------------

**Babel** - js compiler, which takes in newest ES code and compiles into regular javascript for all browsers.
Idea here is to write the latest js syntax so that developers have similar understanding of code.

---------------------------------------------------------------------------------------------------------------------------------------------------------
**let vs var**
```js
var level = false;
let level1 = false; 
If (true){
	var level = true;
	let level1 = true;
}
console.log(level); // true
console.log(level1); // false
```

Anytime let is wrapped with curly brackets (block), it creates a new scope(block scope), just like var inside a function
```js
let player = yes;
let player = false;  // Returns an error! Already decleared!
```

-------------------------------------------------------------------------------------------------------------------------------

**const** (wannabe constant)
const player = { name: 'Tim' };
player = {}; // returns error, cannot be reassigned
player.name = 'Bob'; // no error, allowed

-------------------------------------------------------------------------------------------------------------------------------

Destructing
const player = {
	name = ‘Tim’
level = 99
}
const {name, level} = player; //Select properties you want from an object

Same as:

const name = player.name;
const level = player.level;

-------------------------------------------------------------------------------------------------------------------------------

Dynamic object properties
const playerProp = ‘firstName’’
const player = { [playerProp ]: ‘Bob’}
const silly= { [1+2 ]: 3 }
Also we can do:
const name = ‘Bob’;
const level = 1;
const bag= {};
const player = {name, level, bag};
-------------------------------------------------------------------------------------------------------------------------------

Template strings (uses character  `)
const name = ‘Bob’;
const age = 33;
const greetingOld = “Hello ” + name + “ your age is ” + age;
const greetingNew = `Hello ${name} your age is  ${age}`

-------------------------------------------------------------------------------------------------------------------------------

Arrow functions
function add(a,b) {
	return a+b;
} 

Can be written as:

const add = (a, b) => a + b;

Or

const add = (a, b) =>{
	return a + b;
}


Oneliner assumes there is a single return (const add = (a, b) => a + b;)

Extra: when 1 parameter, brackets can be ignored:
const sqrt = b => b*b;

An arrow function expression is a compact alternative to a regular function expression, although without its own bindings to the this, arguments, super, or new.target keywords. Arrow function expressions are ill suited as methods, and they cannot be used as constructors.
var r = { n: function( ) { console.log(this) } } //this returns r
var r = { n: ( ) => { console.log(this) } } //this returns Window, arrow function not suited as method!

----------------------------------------------------------------------------------------------------------------------------------------------------------

map, reduce, filter vs forEach
Map, reduce and filter are pure functions, they all expect a return, forEach does not.
const arr = [1, 4, 6];
const mapArr = arr.map(num => num * 2); //Each callback has to return something
	vs
const double = []
const newArr = arr.forEach(num => double.push(num*2));  //Returns undefined 
Also - map, reduce, filter all return a new array!
------------------------------------------------------------------------------------------------------------------

Currying is a process of turning a function with multiple arity into a function with less arity.
Or other words - transform a function of arity n to n functions of arity 1.
f(a, b, c) ---------------> f(a)(b)(c)

const mult = (a, b) => a * b;	//normal
const cMult = (a) => (b) => a * b;  //curried

mult(2, 3) is same as cMult(2)(3) 

Benefits? I can build and configure functions like so:

const mult2 = cMult(2);
mult2(4); //returns 8
mult2(6); //returns 12

----------------------------------------------------------------------------------------------------------------------------

Closure is a feature in Javascript when the inner function has access to outer function variables(scope/context). Closure remembers variables from the place where it is defined, no matter where it is executed.

const hello = ( ) => {
	const greet = ‘Hello!’;
	const sayHello = ( ) => {
		const name = ‘John’;
		alert(greet + name);
}
return sayHello
}

Closure = function + functions lexical scope
----------------------------------------------------------------------------------------------------------------------------------------------------------

More about Currie-ing
Curried function is a function which takes multiple arguments, one at a time.

const add = a => b => a + b;
const result = add(2)(4); //6

In this example, the add function takes one argument and then returns a partial application of itself with a fixed in the closure scope.

A Partial application is a function which has been applied to some, but not yet all of its arguments. In other words a function which has some arguments fixed inside its closure scope.

Partial, Curry what's the difference??
Partial applications can take as many or as few arguments a time as desired. 
Curried functions always return a unary function.(function which takes one argument).

So all curried functions return partial applications but not all partial applications are the result of curried functions.

----------------------------------------------------------------------------------------------------------------------------------------------------------

Compose is a way to combine multiple functions to create a new function

const compose = ( f, g ) => a => f(g(a));
const f1 = n => n + ‘!!!’
const f2 = n => n + ‘???’
const create = compose(f1, f2);
create(‘Whats up’); //Whats up!!!???


----------------------------------------------------------------------------------------------------------------------------------------------------------
Reference type

Primitive types (Known by javascript, immutable) - string, number, boolean, null, undefined
var a = 1; //value gets stored in variable (memory)
var b = a; //value gets copied to a new variable (new slot in memory)
a === b; // returns true 
a = 2; // here we do not mutate number 1 into 2, we just replace the 1 with 2
a === b // returns false, because a holds 2 and b holds 1

Primitive types are immutable, meaning their value(state/structure) cannot be changed after creation.
var name = “John”; //name now points to “John” in memory
name = “Fred”; // new block of memory is allocated for “Fred”, name now points to “Fred” in memory,  “John” is still in memory and will be garbage collected.

Using const is just protection, not immutability!

Reference types (created by developer) - Object (Array, Function - also technically objects)
var n = { id: 1} //value gets stored in memory, address of the stored object gets stored in variable
var m = n; //address gets copied, a reference to the value is set
m === n; //returns true, they both point to a same address
m.id = 2 //here we mutate the object
m === n //returns true, they both point to the same mutated object in memory
m = n = { id: 3 } // new object is created in memory, both variables receive the new reference, reference to the old object is lost, old object can be garbage collected!

[] === []; // returns false, they do not share the same reference

For immutable data, the equality is more reliable!
1 === 1 //true
{id: 1} === {id: 1} //false

---------------------------------------------------------------------------------------------------------------------------------------------------------

Cloning objects
var w = { name: {fn: Harry, sn: Potter}, house: Gryffindor};

Shallow clone
var w2 = {...w}; //w2 no longer references w but w2.name still references w.name object in memory! Only the first layer is cloned!

Deep clone
var w3 = JSON.parse(JSON.stringify(w)); //Object is converted to string and then back to object - no more references (Can be slow!).

---------------------------------------------------------------------------------------------------------------------------------------------------------

Context

this - inside what object am I right now? What’s the context?

console.log(this); //returns Window (in browser)
function a(){ console.log(this) }; //returns Window 
var obj = { a: function(){ console.log(this) } }; //returns obj
--------------------------------------------------------------------------------------------------------------------------------------------------------

Classes
Javascript classes are special functions (syntactical sugar to prototype inheritance). 

super - refers to parent class, it is used to call the constructor of the parent’s class and access its properties and functions.

class Hooman {
	constructor(name) {
		this.name = name;
}
}
class Worker extends Hooman{
	constructor(name, profession) {
		console.log(this.name); //returns error, call super first!
	super(name) //access parents properties and functions
console.log(this.name); //returns name
	this.profession = profession;
}
}

----------------------------------------------------------------------------------------------------------------------------------------------------------

=== or == ?

1 == "1" //returns true, type coercion is applied
1 === "1" //returns false, explicit check
true == 1 //true, 1 is coerced to true by JS
true === 1 //false

----------------------------------------------------------------------------------------------------------------------------------------------------------

Array.prototype.join()


const arr =[['My', 'name'],['is', 'Constantine']];
arr.map(n => n.join(" ")).join(' '); //My name is Constantine

const obj = {my:'name', is: 'Constantine'};
Object.entries(obj).map(n=>n.join(' ')).join(' '); //My name is Constantine

----------------------------------------------------------------------------------------------------------------------------------------------------------

Enumeration vs Iteration

for(item of arr) {} // iterate, throws error when using on object, objects are not iterable

for (item in obj) {} //enumerate - loop through all properties which are enumerable, also works for arrays, because array is an object

Example:
var cars = ['Toyota', 'Ford']
cars.vw = 'Volkswagen'
Object.defineProperty(cars, 'bmw', {value:'BMW', enumerable: false })


for(car in cars){console.log(car)} //Prints out all the properties
0
1
vw

//bmw is ignored, because its property .enumerable is set to false, for others its set to true by default!

----------------------------------------------------------------------------------------------------------------------------------------------------------

An object is a collection of properties!
Property - is an association between a name(key) and a value
Property's value can be a function in which case the property is known as a method.


var wizard = { name: 'Gandalf' } //name is a property with a value Gandalf
var wizard = ['Gandalf'] // 0 is a property with a value Gandalf

----------------------------------------------------------------------------------------------------------------------------------------------------------

How async works in synchronous Javascript:

----------------------------------------------------------------------------------------------------------------------------------------------------------

More let

var cb = []

for(var i = 0; i < 3; i++){
	cb[i] = function(){ return i }
}
cb[0](); //returns 3 :(

for(let i = 0; i < 3; i++){
	cb[i] = function(){ return i }
}
cb[0](); //returns 0


Let’s try to fix the the first example using IIFE (Immediately Invoked Function Expression):

for(var i = 0; i < 3; i++){
	(function(i){
		cb[i] = function(){ return i }
})(i);
}
cb[0](); //returns 0

----------------------------------------------------------------------------------------------------------------------------------------------------------
read

https://hacks.mozilla.org/2018/03/es-modules-a-cartoon-deep-dive/
https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/get-started/ch1.md
https://javascript.info/
http://dmitrysoshnikov.com/ecmascript/javascript-the-core-2nd-edition/
https://www.freecodecamp.org/news/how-to-think-like-a-programmer-lessons-in-problem-solving-d1d8bf1de7d2/


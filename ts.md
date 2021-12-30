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
let input1: any;
let input2: unknown;
input1.toUpperCase() // no error, TS ignores type checking for any
input2.toUpperCase() // TS error, indexOf does not exist on type unknown
if (typeof input2 === 'string') {
  input2.toUpperCase() // works, unknown requires some sort of type check
}
```

------

**Browserify** - enables to use modules ( **require** from Node.js ) in a browser

---

**Babel** - js compiler, which takes in newest ES code and compiles into regular javascript for all browsers.
Idea here is to write the latest js syntax so that developers have similar understanding of code.

---

**React** - scalable and declarative Javascript library for building interactive UI applications.    
Solves the problem of imperative actions that mutate the state over time (it is very difficult to understand what   
the state is at the end of these mutations) by taking these mutations and presenting them as a snapshot of program   
in a single point of time.

---

**Redux** is a state manager, mainly used to manage state in React, has 3 principles:
1) Single source of thruth
2) Is read only (immutable)
3) Changes using pure functions

**ACTION** (user action) --> **REDUCER** (pure function) --> **STORE** (application state) --> **CHANGE VIEW** (react)

**ACTION** --> **CHANGE VIEW** *(the old way, for example jQuery - gets complex on modern apps)*

---

**Flux** - is an application archiceture utilizing an undirectional data flow  
Action --> Dispatcher --> Store --> View

---


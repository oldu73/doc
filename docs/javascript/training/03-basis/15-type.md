# 15-type

***

## Introduction

Javascript is a dynamically typed language.

Statically typed languages types are checked during the compilation phase:

```text
bool test = '123' // error
```

Dynamic types, in Javascript, may change during the execution and is inferred from the type of the value assigned to the variable:

```js
let test = '123' // ok

test = true // ok

test = 55 // ok
```

In Javascript there's two main family of types:

- primitive  
- object

Everything that's ain't an object is a primitive.

***

## Primitive

- boolean  
- number // may contain any type of number, integer, float, etc.  
- string of characters  
- Undefined // not yet assigned, automatic, implicit  
- Null // like 'Undefined' but set manually, explicit  
- Symbol // since ES6  
- BigInt // since ES2020

Primitives are immutable which mean on a reassignment the variable name point to a new value, but the previous value is still held in memory. Hence the need for garbage collection.

***

## Object

- Literal object  
- Array  
- Function  
- Date  
- .. all the rest // all what is not a primitive, is an object

Objects and arrays are mutable.

A mutable object is an object whose state can be modified after it is created.

***

## Review

src/index.js:

```js
// string, console -> string
const test01 = "jean";
console.log(typeof test01);

// number, console -> number
const test02 = 123.4;
console.log(typeof test02);

// null, console -> object = bug!
const test03 = null;
console.log(typeof test03);

// undefined, console -> undefined
// could not be a const like above
// because const should be initialized
// on the same line where declared
let test04;
console.log(typeof test04);

// Symbol, console -> symbol
let test05 = Symbol();
console.log(typeof test05);

// Boolean, console -> boolean
let test06 = true;
console.log(typeof test06);

// Literal object, console -> object
const test07 = {
  test71: "Hello",
  test72: "World"
}
console.log(typeof test07);

// Function object, console -> function, although it's an object
const test08 = function() {
  console.log("Hello");
};
console.log(typeof test08);

// Date object, console -> object
const test09 = new Date();
console.log(typeof test09);

// Array object, console -> object
const test10 = [1, 2, 3];
console.log(typeof test10);
```

***

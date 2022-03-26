# 12-variable

***

## Introduction

Chapter (03) objectives summary:

- Variables (var, let and const)  
- Hoisting  
- Types  
- Operators  
- Coercion  
- Scope, Execution context  
- Reference and value

Environment is inherited from previous chapter

***

## var

src/index.js

```js
// declaration
var firstName;

// undefined
console.log(firstName);

// initialisation
firstName = 'Olivier';

console.log(firstName);

// reference error, does not exist
// and stop execution if not commmented
// console.log(lastName);

// declare many variables at the same time
var myvar1, myvar2;

myvar1 = 123;
myvar2 = 55;

console.log(myvar1, myvar2);
```

***

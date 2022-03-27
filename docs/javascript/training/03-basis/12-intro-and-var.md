# 12-intro-and-var

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

Environment is inherited from previous chapter and without particular mention, thereafter we will assume that the webpack environment has been launched with following command through a terminal at the root level of the project:

```console
npm start
```

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

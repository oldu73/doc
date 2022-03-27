# 13-let-and-const

***

## var problematic

let has been introduced to fix some var's problem.

var allow to redeclare a variable that has already been declared with the same name, which could lead in very problematic issues in huge js file when you don't remember that a variable with a particular name has already been declared and you redeclare it with the same name for a different usage:

```js
var test = 123;
var test = 456;
```

Curly brace delimitate a code block but does not limit variable scope:

```js
{
  var test = 123;
}

console.log(test);
```

***

## let

Reuse a name will raise a "Identifier 'test' has already been declared." error in browser and webpack consoles:

```js
let test = 123;
let test = 456;
```

Scope of let is limited to code block, below example will raise a "test not defined" error:

```js
{
  let test = 123;
}

console.log(test);
```

Use let instead of var is mostly advised.

***

## const

const should be initialized on line where she's declared:

```js
const test = 123;
```

Like expected const cannot be reassigned and trying to will raise a read only error.

For an object reference it's possible to reassign a key inside a const object but not the reference himself:

```js
const simpleObject = {
  test: 123
};

console.log(simpleObject.test);

// OK
simpleObject.test = 456;

console.log(simpleObject.test);

// KO
simpleObject = 456
```

***

## Priority order of usage

1. const  
2. let  
3. var

***

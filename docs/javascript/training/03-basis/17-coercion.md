# 17-coercion

***

## Introduction

Due to the dynamic nature of JavaScript's typing.

[Type coercion](https://developer.mozilla.org/en-US/docs/Glossary/Type_coercion)

***

## Implicit coercion

Implicit or automatic conversion.

```js
let a = 1;
let b = 'hello';
console.log(a + b); // 1hello
```

Unable to convert hello to a number but 1 is easily converted to a string and then the result of the addition is the concatenated string.

Another example when adding a number to a true boolean value (converted to 1 (false would be converted to zero)):

```js
let a = 1;
let b = true;
console.log(a + b); // 2
```

Another example with string and boolean value, here converted to a string:

```js
let a = 'test';
let b = false;
console.log(a + b); // testfalse
```

***

## Constructor conversion

Types's constructor call with a value trying to convert, e.g. Number('hello').

```js
console.log(Number('hello, world')); // NaN = not a number

let a = 1;
let b = undefined;

console.log(a + b); // NaN
console.log(Number(undefined)); // NaN

console.log(String(undefined)); // undefined
console.log(String(1)); // 1
console.log(String(null)); // null
```

NaN, not a number, coercion not possible!

***

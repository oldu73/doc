# 21-ternary-operator

***

## Introduction

Ternary operator: - '?'

Syntax ([SRC](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)):

```js
condition ? exprIfTrue : exprIfFalse
```

Simplify below syntax with ternary operator:

```js
const hour = 21;

let isBeforeNoon;

if (hour <= 12) {
  isBeforeNoon = true;
} else {
  isBeforeNoon = false;
}

console.log(isBeforeNoon); // false
```

With ternary operator:

```js
const hour = 10;

const isBeforeNoon = hour <= 12 ? true : false;

console.log(isBeforeNoon); // true
```

Nested ternary:

```js
const hour = 19;

const test = hour >= 6 && hour < 12 ? 'morning' : hour >= 12 && hour < 18 ? 'afternoon' : 'night';

console.log(test); // night
```

***

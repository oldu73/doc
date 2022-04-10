# 16-operator-1

***

## Introduction

Operators and the notions of precedence and associativity.

***

## Assignation

```js
let a = 1;
console.log(a);
```

***

## Addition

```js
let a = 1 + 2;
console.log(a);
let b = 'hello, ' + 'world';
console.log(b);
```

***

## Precedence

[Operator precedence](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

Precedence determine in which order operators are executed.

For, let's say:

```js
let a = 1 + 2; // 3
```

We have two operators, '=' and '+'.

By clicking one link above you may find Table precedence section with "weight" of each operators that will determine execution order:

- for '+' it's 12  
- for '=' it's 2

Means that '+' will be executed before '='.

For:

```js
let a = 2 + 2 * 5; // 12
```

- for '*' it's 13  
- for '+' it's 12  
- for '=' it's 2

Means that first '*' then '+' and finally '='.

***

## Associativity

Associativity rules apply when there's many operator with same precedence weight.

Even though the calculation below gives the same result no matter which way you apply the operators, we still use it as an example.

Multiplication and division operator have same precedence weight of 13.

```js
let a = 2 + 2 * 5 / 3; // 5.333333333333334
```

But then what matter here is what 'Associativity' column contain (in above mentioned table) which is, in our case, 'left-to-right'.

Means, calculation start by multiplication and then division.

Note that parenthesis is also an operator with maximal precedence weight.

***

## Conditional (ternary) operator (?)

[Source](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)

```js
let a = 1;

// a += 2;

let b = a == 3 ? 5 : 6;

console.log(b); // 6
```

***

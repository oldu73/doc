# 26-decimal

***

## Introduction

Decimals, exponent and scientific notation

```js
const dec = 15.45;

console.log(dec); // 15.45
```

## Common case

Due to how decimal number are stored as fraction, sometimes there's a little approximate:

```js
console.log(0.1 + 0.2); // 0.30000000000000004
console.log(0.1 + 0.1); // 0.2
console.log(5 / 3); // 1.6666666666666667
console.log(1 / 3); // 0.3333333333333333
```

A strategy used by some libraries (e.g. [stripe](https://stripe.com/fr-ch) for financial transaction) and to get correct result is to multiply each operand by 10 before main arithmetical operation and the to divide the final result by 10:

```js
console.log(0.1 + 0.2); // 0.30000000000000004
console.log((0.1 * 10  + 0.2 * 10) / 10); // 0.3
```

***

## Scientific notation

Exponent (10^n), useful to manipulate big number:

```js
const price = 1e6;

console.log(price); // 1000000
```

***

## Exponent operator

Exponentiation: **

```js
console.log(2 ** 3); // 8
console.log(3 ** 2); // 9
```

***

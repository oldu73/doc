# 27-global-method

***

## Introduction

We already have seen that we can convert a string to a number when possible:

```js
console.log(Number("38")); // 38
console.log(Number("38test")); // NaN
```

***

## parseInt()

The parseInt() function is a little bit more power full than Number():

```js
console.log(parseInt("38test")); // 38
console.log(parseInt("101", 2)); // 5, 101 = 5 in binary
console.log(parseInt("1a", 16)); // 1a = 26 in hexadecimal
console.log(parseInt("11.5")); // 11, because integer
```

It convert input in specified base (default = 10, 2, 8, 16) output is in base 10. It convert all convertible character and stop when reach an none convertible one.

***

## parseFloat()

```js
console.log(parseFloat("11.5")); // 11.5
console.log(parseFloat("11")); // 11
```

***

## +

Another way to convert string to number is to add '+' sign before string:

```js
console.log(+"44"); // 44
console.log(1 + "43"); // 143
console.log(1 + +"43"); // 44
```

***

## Test

isFinite():

```js
console.log(isFinite( 1 / 0 )); // false
console.log(isFinite( 1 / 3 )); // true
```

isNaN():

```js
console.log(isNaN("test")); // true
console.log(isNaN("44")); // false
```

***

## Undefined

```js
console.log(Number.isNaN(undefined)); // false
console.log(isNaN(undefined)); // true, not advised, always use Number.isNaN()
```

***

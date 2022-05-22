# 28-number-method

***

## Introduction

Number object give access to methods for manipulating them.

```js
const a = 1;
const b = new Number(1);

console.log(a); // 1, primitive
console.log(b); // Number {1}, object
```

***

## Primitive

When invoking dot on a primitive the JS engine convert it first to a Number object with constructor, then apply called function and convert it back to primitive:

```js
const a = 1.555555555555555555;

console.log(a.toFixed(2)); // 1.56, primitive string (black in console) (not a Number object)

// to get a float back

console.log(parseFloat(a.toFixed(2))); // 1.56, primitive number (blue in console) (not a Number object)
```

Each primitive has his object equivalent.

***

## Number.isNaN()

Without instantiating a number object we may invoke [static](https://www.w3schools.com/js/js_class_static.asp) method of JS native object Number, e.g. Number.isNanN():

```js
const a = Number("123");
const b = Number("123test");

console.log(Number.isNaN(a)); // false, a string has been converted to number
console.log(Number.isNaN(b)); // true, b string has not been converted to number
```

***

## Number.isInteger()

```js
console.log(Number.isInteger(55)); // true
console.log(Number.isInteger(55.0)); // true, 55.0 equivalent to 55 so integer
console.log(Number.isInteger(55.1)); // false
```

***

## ().toExponential()

Convert a number to scientific notation (exponential):

```js
console.log((1577).toExponential()); // 1.577e+3
console.log((1577).toExponential(2)); // 1.58e+3
const b = 400000000000;
console.log(b.toExponential()); // 4e+11
```

***

## ().toFixed()

Round to specified number of decimal:

```js
console.log((1.45e2).toFixed(2)); // "145.00" (string)
console.log((57.654981).toFixed(1)); // "57.7" (string)
console.log((53).toFixed(1)); // "53.0" (string)
```

***

## ().toPrecision()

Keep given precision without changing notation expect for integer of precision is lower than the needed numbers of digit to write it:

```js
console.log((1.79e2).toPrecision(1)); // "2e+2" (string)
console.log((795).toPrecision(2)); // "8.0e+2" (string)
console.log((795).toPrecision(3)); // "795" (string)
console.log((1.65498416).toPrecision(3)); // "1.65" (string)
```

***

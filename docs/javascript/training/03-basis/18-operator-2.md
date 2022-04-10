# 18-operator-2

***

## Introduction

= VS == VS ===

and others logical operators

***

## Assignment (=)

= [assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Assignment)

Set a variable value

***

## Equality (==)

Not recommended!

== [equality](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Equality)

Automatic (implicit) coercion, enabled!

If compared values are of different type, coercion happen before comparison.

A bit tricky to use/understand, usage of '===' is mostly advised.

```js
let a = 1;
let b = 1;
let c = 2;

console.log(a == b); // true
console.log(a == c); // false
console.log(a == true); // true
console.log(1 == '1'); // true
console.log(1 == '2'); // false
console.log(true == "true"); // false

console.log(undefined == false); // false
```

***

## Strict equality (===)

Recommended!

=== [strict equality](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_equality)

Automatic (implicit) coercion, disabled!

Compared values should be same type and value to return true.

```js
let a = 1;
let b = 1;
let c = 2;

console.log(a === true); // false
console.log(1 === '1'); // false

console.log(undefined === false); // false
```

***

## Less than (<)

```js
console.log(1 < 2 < 3); // true = OK!
console.log(3 < 2 < 1); // true = WHAT??
```

Explanation is associativity from left to right, so first 3 < 2 is executed which return false and then false < 1 equivalent to 0 < 1, which is true!

***

## Logical NOT (!)

Invert logical value.

```js
let a = 0;
console.log(!a); // true
console.log(!true); // false
console.log(!!true); // true
```

***

## Logical AND (&&)

```js
console.log(true && true); // true
console.log(true && false); // false
console.log(false && true); // false
console.log(false && false); // false
```

But when the two elements do not evaluate to boolean (true or false) the following rule must be used: - If the left element can be converted to false then it will be returned. In all other cases the second element will be returned:

```js
console.log('1' && '2'); // 2
```

Other usage sample:

```js
let a = 1, b = 1;

if (a && b) {
  console.log('ok'); // ok
}
```

***

## Logical OR (||)

```js
console.log(true || true); // true
console.log(true || false); // true
console.log(false || true); // true
console.log(false || false); // false
```

But when the two elements do not evaluate to boolean (true or false) the following rule must be used: - If the left element can be converted to true then it will be returned. In all other cases the second element will be returned:

```js
console.log(false || undefined); // undefined
```

Other usage sample:

```js
let a = 1, b = 0;

if (a || b) {
  console.log('ok'); // ok
}
```

***

## Summary

```js
console.log(!2 || 'test' && 42); // 42
```

***

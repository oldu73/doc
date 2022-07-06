# 45-copy

***

## Introduction

Copy object.

***

## Shallow copy

The result of Object.assign() is a shallow copied object due to the fact that if the object contains a nested object, he still refer to the same original object after assignment. For a more modern approach use the spread operator:

```js
const a = {
  name: 'John',
  foo: {
    bar: 'zoo'
  }
}

console.log(a); // {name: "John", foo: { bar: 123 }}
// !! be aware, here we may expect to have "zoo" as bar's value (due to hoisting?)

var b = Object.assign({}, a);
b.name = 'Jane';
b.foo.bar = 123

const c = { ...a } // spread operator

console.log(a); // {name: "John", foo: { bar: 123 }}
console.log(b); // {name: "Jane", foo: { bar: 123 }}
console.log(c); // {name: "John", foo: { bar: 123 }}
```

***

## Deep copy

With JSON native object:

```js
const a = {
  name: 'John',
  foo: {
    bar: 'zoo'
  }
}

const b = JSON.parse(JSON.stringify(a));

b.name = 'Jane';
b.foo.bar = 123;

console.log(a); // {name: "John", foo: { bar: "zoo" }}
console.log(b); // {name: "Jane", foo: { bar: 123 }}
```

***

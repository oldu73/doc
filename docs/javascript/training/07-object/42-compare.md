# 42-compare

***

## Introduction

Compare object:

```js
const a = {};
const b = {};

console.log(a == b); // false
console.log(a === b); // false

const c = b;

console.log(c === b); // true
```

Even they've same value, 'a' and 'b', their references are different, so not equal.

For 'b' and 'c', they reference same object (same stack address), so, equal.

It doesn't matter that the two objects have the same properties because it is the references that are compared, never the contents of the objects.

***

# 43-iterate

***

## Introduction

Iterate over objects.

***

## for in

```js
const obj = {
  a: 'one',
  b: 'two',
  c: 'three'
}

for (prop in obj) {
  console.log(prop); // keys
  console.log(obj[prop]); // values
}
```

Ouput:

```txt
a
one
b
two
c
three
```

***

## Object.keys()

```js
console.log(Object.keys(obj)); // (3) ['a', 'b', 'c']
```

Output an array of object's keys.

***

## Object.values()

```js
console.log(Object.values(obj)); // (3) ['one', 'two', 'three']
```

Output an array of object's values.

***

## Object.entries()

```js
console.log(Object.entries(obj));
```

Output:

```txt
(3) [Array(2), Array(2), Array(2)]
0: (2) ['a', 'one']
1: (2) ['b', 'two']
2: (2) ['c', 'three']
```

Output an array of arrays of object's keys, values.

***

# 41-merge-object

***

## Introduction

The goal is to combine two objects:

```js
const hearth1 = {
  population: 7e9,
  satellite: "Moon",
};

const hearth2 = {
  temperature: {
    min: -70,
    max: 40
  },
  isOld: false
};
```

***

## Assign

The 'assign()' native method of 'Object':

Start with an empty object (the one to be assigned, a new reference), then merged from left to rigth:

```js
console.log(Object.assign({}, hearth1, hearth2)); // {population: 7000000000, satellite: 'Moon', temperature: {…}, isOld: false}
```

***

## Spread

With '...' spread operator:

```js
const hearth = { ...hearth1, ...hearth2 };

console.log(hearth); // {population: 7000000000, satellite: 'Moon', temperature: {…}, isOld: false}
```

***

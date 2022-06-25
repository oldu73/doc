# 40-discard-property

***

## Introduction

Delete or discard properties.

First, as a recap, we add three new properties in three different ways:

```js
const hearth = {
  population: 7e9,
  satellite: "Moon",
  temperature: {
    min: -70,
    max: 40
  },
  isOld: false
};

// add properties, 3 ways
hearth.liveable1 = true;
hearth["liveable2"] = true;
const liveable3 = "liveable3";
hearth[liveable3] = true;

console.log(hearth); // {..., liveable1: true, liveable2: true, liveable3: true, ...}
```

***

## Remove property

Operator delete:

```js
delete hearth.liveable1;

console.log(hearth); // {..., liveable2: true, liveable3: true, ...}
```

***

## Set property to no value

Set to 'null', not 'undefined' as it was never assigned:

```js
hearth.liveable2 = null;

console.log(hearth); // {..., liveable2: null, liveable3: true, ...}
```

***

## Copy without property

Decomposition by specifiying which key(s) to **not** copy, then Rest ... operator:

```js
const { population, satellite, ...copyHearth } = hearth;

console.log(copyHearth); // {temperature: {…}, isOld: false, liveable2: null, liveable3: true}
```

***

# 39-test-existence

***

## Introduction

Test the existence and value of a property

***

## Test for value

If property contain a value, then it's converted to true:

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

// test for value

if (hearth.population) {
  console.log('OK'); // OK
} else {
  console.log('KO');
}
```

***

## Test for key

in operator:

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

// test for key

if ('population' in hearth) {
  console.log('OK'); // OK
} else {
  console.log('KO');
}
```

### hasOwnProperty

Native method available on all object:

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

// hasOwnProperty

if (hearth.hasOwnProperty('population')) {
  console.log('OK'); // OK
} else {
  console.log('KO');
}
```

***

## falsy values

As a reminder, falsy values in JavaScript are false, null, undefined, 0, NaN, '', "" and ``.

Also as a reminder, a value is said to be false because it is automatically converted to false, especially in if … else.

***

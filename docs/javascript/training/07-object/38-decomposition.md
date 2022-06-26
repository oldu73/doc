# 38-decomposition

***

## Introduction

Extract key value from an object:

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

const { population, satellite, temperature } = hearth;

console.log(population, satellite, temperature); // 7000000000 'Moon' {min: -70, max: 40}
```

***

## Alias

If identifier has already been declared:

```js
const population = 50; // first declaration of population

const hearth = {
  population: 7e9,
  satellite: "Moon",
  temperature: {
    min: -70,
    max: 40
  },
  isOld: false
};

const { population: population2, satellite, temperature } = hearth; // alias

console.log(population2, satellite, temperature); // 7000000000 'Moon' {min: -70, max: 40}
```

***

## Rest operator

3 dots '...' (Rest operator) to get the rest of decomposed object:

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

const { temperature, ...restArgs } = hearth; // spread operator to get the rest

console.log(temperature, restArgs); // {min: -70, max: 40}
                                    // {population: 7000000000, satellite: 'Moon', isOld: false}
```

Rest operator should always be at the end of the decomposition.

***

## Default value

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

const { temperature, isOld, isYoung = true } = hearth; // default value for isYoung which is not defined in hearth object

console.log(temperature, isOld, isYoung); // {min: -70, max: 40} false true
```

***

## Nested

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

const { temperature: { min } } = hearth;

console.log(min); // -70

console.log(temperature); // Error: temperature is not defined
```

***

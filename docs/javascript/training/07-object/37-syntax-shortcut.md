# 37-syntax-shortcut

***

## Introduction

Since ES6, in an object, if a key is not initialized with a value, it presupposes that a key of the same name has already been initialized before:

```js
const population = 7e9;
const satellite = "Moon";
const temperature = {
  min: -70,
  max: 60
};

// Before ES6

const hearth1 = {
  population: population,
  satellite: satellite,
  temperature: temperature,
  isOld: false
}

console.log(hearth1); // {population: 7000000000, satellite: 'Moon', temperature: {…}, isOld: false}

// Since ES6

const hearth2 = {
  population,
  satellite,
  temperature,
  isOld: false
}

console.log(hearth2); // {population: 7000000000, satellite: 'Moon', temperature: {…}, isOld: false}

// Object's key from a const, at declaration

const pop = "population";

const hearth3 = {
  [pop]: population,
  satellite,
  temperature,
  isOld: false
}

console.log(hearth3); // {population: 7000000000, satellite: 'Moon', temperature: {…}, isOld: false}

// Object's key from a const, after declaration

const hearth4 = {
  satellite,
  temperature,
  isOld: false
}

hearth4[pop] = population;

console.log(hearth4); // {satellite: 'Moon', temperature: {…}, isOld: false, population: 7000000000}

```

***

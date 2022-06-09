# 36-property

***

## Introduction

Declaration of different properties and different manner to access them:

```js
const hearth = {
  population: 7e9,
  satellite: 'Moon',
  temperature: {
    min: -70,
    max: 60
  },
  isOld: false,
  // function in an object = method
  getTemperature: function () {
    console.log("14.5");
  },
  // Since ES6 function isn't needed anymore to declare a function
  getHumidity() {
    console.log('98');
  }
}

console.log(hearth); // {population: 70000000, satellite: 'Moon', temperature: {…}, isOld: false}

console.log(hearth.population); 7000000000

console.log(hearth["population"]); 7000000000

const populationProperty = 'population';

console.log(hearth[populationProperty]); 7000000000

// copy object reference
const copy = hearth;

// modifying copy modify original because both reference same object
copy.population = 7.5e9
console.log(hearth[populationProperty]); 7500000000

// Invoke method = function that belong to an object
copy.getTemperature(); // 14.5

hearth.getHumidity(); // 98
```

***

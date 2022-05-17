# 22-switch

***

## Introduction

```js
const wheel = 4;
let vehicleType;

switch (wheel) {
  case 2: {
    vehicleType = 'motorbike';
    break;
  }
  case 4: {
    let test = 123;
    console.log(test); // 123
    vehicleType = 'car';
    break;
  }
  default: {
    console.log(test);  // e.g. with wheel = 6 -> error test is not defined
    vehicleType = 'truck';
    break;
  }
}

console.log(vehicleType); // car
```

break is needed otherwise all code below matching condition is also executed.

Advantage of using curly brace for each case block is that make possible to declare let variable in it with limited scope.

***

## String

It's not limited to number, it works with string as well:

```js
const wheel = "4";
let vehicleType;

switch (wheel) {
  case 2: {
    vehicleType = 'motorbike';
    break;
  }
  case 4: {
    let test = 123;
    console.log(test);
    vehicleType = 'car';
    break;
  }
  case "4": {
    vehicleType = 'car2'; 
    break;
  }
  default: {
    console.log(test);
    vehicleType = 'truck';
    break;
  }
}

console.log(vehicleType); // car2
```

***

## Many cases

Many cases with same treatment:

```js
const wheel = "4";
let vehicleType;

switch (wheel) {
  case 2: {
    vehicleType = 'motorbike';
    break;
  }
  case 4:
  case "4": {
    let test = 123;
    console.log(test); // 123
    vehicleType = 'car';
    break;
  }
  default: {
    console.log(test);
    vehicleType = 'truck';
    break;
  }
}

console.log(vehicleType); // car
```

***

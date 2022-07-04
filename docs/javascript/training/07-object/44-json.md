# 44-json

***

## Introduction

JavaScript Object Notation, just for data (no method):

```json
{
  "firstname": "John",
  "lastname": "Doe",
  "age": 37
}
```

***

## JSON

Native object to convert a JavaScript object to JSON and vice versa.

### stringify

```js
const obj = {
  firstname: "John",
  lastname: "Doe",
  age: 37
}

console.log(JSON.stringify(obj)); // {"firstname":"John","lastname":"Doe","age":37}
```

### parse

```js
console.log(JSON.parse('{"firstname": "John", "lastname": "Doe", "age": 37}')); // {firstname: 'John', lastname: 'Doe', age: 37}
```

***

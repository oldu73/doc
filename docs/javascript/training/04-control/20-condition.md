# 20-condition

***

## Introduction

if, else, else if

***

## If

```js
const condition = true;

if (condition === true) {
  console.log('here!'); // here!
}
```

By omitting strict comparison in if condition, implicit coercion to boolean happen:

```js
const condition = true;

if (condition) {
  console.log('here!'); // here!
}
```

Even if 'condition' here in example is not a boolean, implicit coercion happen as well:

With a number

```js
const condition = 0;

if (!condition) {
  console.log('here!'); // here!
}
```

With a string

```js
const condition1 = "john"; // true
const condition2 = ""; // false

if (condition1) {
  console.log('condition1'); // condition1
}

if (!condition2) {
  console.log('not condition2'); // not condition2
}
```

***

## else

if the condition is not met, an "else" branch is provided to allow this case to be processed:

```js
//const test = "John";
const test = "";

if (test) {
  console.log("hello " + test); // hello John
} else {
  console.log("hello !"); // hello !
}
```

***

## else if

Treat more than two cases (if/else) with "else if":

```js
//const test = "John";
const test = "Paul";
//const test = "";

if (test === "John") {
  console.log("hello John"); // hello John
} else if (test === "Paul") {
  console.log("hello Paul"); // hello Paul
} else {
  console.log("hello !"); // hello !
}
```

Not recommended! But if only one instruction, not forced to delimit instruction block with curly braces:

```js
//const test = "John";
const test = "Paul";
//const test = "";

if (test === "John") console.log("hello John"); // hello John
else if (test === "Paul") console.log("hello Paul"); // hello Paul
else console.log("hello !"); // hello !
```

***

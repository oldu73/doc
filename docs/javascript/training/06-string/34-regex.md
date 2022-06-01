# 34-regex

***

## Introduction

Regular expression are available with string methods.

Two ways for declaring regex:

- Literal  
- With constructor, new RegExp()

***

## test

Test for matching pattern:

```js
const a = 'I am 123 the sun';

const evaluate1 = /am/.test(a);
console.log(evaluate1); // true

const evaluate2 = /\d{1,3}/.test(a);
console.log(evaluate2); // true
```

***

## exec

Extract part of sting:

```js
// e.g. extract a file name without extension from a string

const a = "picture_12345.jpg"

const evaluate = /(.*)\./.exec(a);
console.log(evaluate); // ['picture_12345.', 'picture_12345', ..]
```

***

## match

Return an array that contain result of search:

```js
const a = 'I am 123 the sun';

console.log(a.match(/am/)); // ['am', index: 2, input: 'I am 123 the sun', ..]
```

## split

Split between matching pattern:

```js
const a = 'I am 123 the sun';

console.log(a.split(/am/)); // ['I ', ' 123 the sun']
```

***

## replace

Replace matching pattern:

```js
const a = 'I am 123 the sun';

console.log(a.replace(/am/, 'hello')); // I hello 123 the sun
```

***

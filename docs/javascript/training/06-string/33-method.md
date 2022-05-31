# 33-method

***

## Introduction

Different methods available for string treatment.
All those methods are case sensitive and return a new string because primitive are immutable.

Calling a method on a character string results in the creation of a temporary String object by the JavaScript interpreter on which the method is located because a primitive has, by definition, no method!

Not all string methods are available on the 2015 version of JavaScript. It is by using Babel and Webpack that we can access all the features!

***

## charAt

Return char at given position:

```js
console.log('I am the sun'.charAt(0)); // I
```

***

## endsWith

Return true if string end with given parameter:

```js
console.log('I am the sun'.endsWith('sun')); // true
```

***

## startWith

Return true if string start with given parameter:

```js
console.log('I am the sun'.startsWith('You')); // false
```

***

## indexOf

Return position of given parameter:

```js
console.log('I am the sun'.indexOf('the')); // 5
console.log('I am the sun'.indexOf('the', 6)); // start at 6, not found = -1
```

If not found return -1.

***

## replace

Replace portion of string with given parameter:

```js
console.log('I am the sun'.replace('I am', 'You are')); // You are the sun

a = 'I am the sun';
a.replace('I am', 'You are');
console.log(a); // I am the sun, because string is a primitive immutable
```

***

## search

Returns the first iteration of the given parameter found:

```js
console.log('I am the sun'.search('am')); // 2
```

***

## slice

Returns cut portion of string between two given indexes as parameters:

```js
const a = 'I am the sun';
const b = a.slice(1, a.length);
const c = a.slice(1, -1);

console.log(b); // am the sun
console.log(c); // am the su
```

***

## trim

Remove all spaces before/after a string:

```js
console.log('   I am the sun   '.trim()); // I am the sun
```

***

## split

Returns an array of values separated by the separator given as parameter:

```js
console.log('sun,earth,moon'.split(',')); // ['sun', 'earth', 'moon']
```

***

## charCodeAt

Get UTF-16 character code at given position:

```js
console.log('Hello 👍'.charCodeAt(0)); // 72
console.log('Hello 👍'.charCodeAt(6)); // 55357
console.log('Hello 👍'.charCodeAt(7)); // 56397
```

***

## codePointAt

Get Unicode code of character at given position:

```js
console.log('Hello 👍'.codePointAt(6)); // 128077
```

***

## fromCharCode

Create a string from UTF-16 code:

```js
console.log(String.fromCharCode(72)); // H
```

***

## fromCodePoint

Create a string from Unicode code:

```js
console.log(String.fromCodePoint(128077)); // 👍
```

***

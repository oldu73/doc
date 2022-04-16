# 19-value-and-reference

***

## Introduction

Variable (let, const, var)

Address + value

In a browser:

- Web API (e.g. [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest))  
- [Event loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop) in charge to intercept event triggered by user interaction  
- V8 JavaScript engine, execute but do not store anything  
- [Stack](https://felixgerschau.com/javascript-memory-management/#:~:text=for%20different%20purposes.-,Stack%3A%20Static%20memory%20allocation,-You%20might%20know): Static memory allocation for storing primitives, small and fast.  
- [Heap](https://felixgerschau.com/javascript-memory-management/#:~:text=on%20the%20browser.-,Heap%3A%20Dynamic%20memory%20allocation,-The%20heap%20is): Dynamic memory allocation for storing objects, huge and slow.

***

## Value VS Reference

```js

const a = 'hello' // string -> stack(a => address1 | 'hello')

const b = { c: 1 } // literal object -> stack(b => address2 | address3), heap(address3 | { c: 1 })

const c = a; // assignment by values copied in new memory address stack(c => address4 | 'hello')

const d = b; // assignment by reference copied in new memory address stack that point on same value in heap -> stack(d => address5 | address3), heap(address3 | { c: 1 })

```

***

## By Value

```js
let a = 1;
let b = a;

console.log({a, b}); // { "a": 1, "b": 1 }

b += 3;

console.log({a, b}); // { "a": 1, "b": 4 }
```

## By Reference

```js
let a = { b: 1 };
let c = a;

c.b = 2;

console.log(a); // { "b": 2 }
console.log(c); // { "b": 2 }
```

***

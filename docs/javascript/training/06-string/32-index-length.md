# 32-index-length

***

## Introduction

Index and length properties on string.

```js
const a = 'test';

console.log(a.length); // 4
console.log(a[1]); // e
```

***

## Index

[#] retrieve character at given index like for an array:

```js
const a = 'test';

console.log(a[0]); // t
console.log(a[1]); // e
console.log(a[2]); // s
console.log(a[3]); // t
```

Another sample:

```js
const a = 'Hello ! 😀';

console.log(a[0]); // H
console.log(a[6]); // !
console.log(a[8]); // �
```

For emoji at index 8 output = '�' due to 32 bits encoded char and a[8] represent the 16 first bits of the character which do not have an Unicode significant value.

***

## Length

Return string length:

```js
const a = 'test';

console.log(a.length); // 4
```

***

## Last char of a sring

```js
const a = 'test';

console.log(a[a.length - 1]); // t
```

## String object

With a string object we may retrieve each characters with their respective index and also the length property.

```js
const b = new String('test');

console.log(b);
```

Output:

```txt
String {'test'}
0: "t"
1: "e"
2: "s"
3: "t"
length: 4
```

***

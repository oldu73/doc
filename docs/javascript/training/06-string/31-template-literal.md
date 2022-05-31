# 31-template-literal

***

## Introduction

Use of Backtick (grave accent) ` to declare a template literal:

```js
const a =  `I 

am 

    a string`;

console.log(a);
```

Output:

```txt
I 

am 

    a string
```

***

## Interpolation

Interpolation inside a string:

```js
const a = 'BIG';

const b =  `I 

am ${a}

    a string`;

console.log(b);
```

Output:

```txt
I 

am BIG

    a string
```

Anything in between ${} will be interpolated as a string:

```js
const a =  `I 

am ${ 1 + 1 }

    a string`;

console.log(a);
```

Output:

```txt
I 

am 2

    a string
```

***

## Double interpolation

```js
const tired = true;
const action = 'sleep';

const stateOfTiredness =   `Paul is ${tired ? `tired and would like to ${action}!` : `in fine fettle!`}`;

console.log(stateOfTiredness); // Paul is tired and would like to sleep!
```

***

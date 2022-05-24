# 30-introduction

***

## Introduction

Introduction to strings, JS primitive type.

[Course on Unicode text encoding (french)](https://buzut.net/cours/computer-science/encodage-du-texte-en-unicode)

```js
const a = 'hello'; // Literal
console.log(a); // hello

const b = String('hello');  // Through special constructor function String()
                            // Convert anything to a string
console.log(b); // hello

const c = new String('hello');  // Constructor function with new operator
                                // Create an object that contain a string primitive
console.log(c); // String {'hello'}

const d = "hello 'world'";
console.log(d); // hello 'world'

const e = 'hello \'world\'';
console.log(d); // hello 'world'

// \n = new line, \t = tab

const f = 'hello\n' + '\tworld';
console.log(f); // hello
                //     world
```

***

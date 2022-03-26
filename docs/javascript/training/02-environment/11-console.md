# 11-console

***

## Introduction

console is an object which is part of javascript API.

console.log is used to display information in browser console.

console.error display output in red as an error.

***

## Error

In a terminal from project's root folder start webpack:

```console
npm start
```

Edit src/index.js as follow:

```javascript
let test = "123";
console.log(tost);
```

tost variable does not exist and it will through an error in browser's console.

Open a browser and DevTools window (F12 in Google Chrome), click on console tab.

You can click on link to browse in file that trigger the error, index.js:2:13

You are now in original source code even he supposed to have been optimized by webpack.

It's thanks to dist/main.bundle.js.map file that ease debugging stage with showing us original source code.

***

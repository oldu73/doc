# 08-install-babel

***

Babel is a free and open-source JavaScript transcompiler that is mainly used to convert code into a backwards compatible version of JavaScript.

[Wiki](https://en.wikipedia.org/wiki/Babel_(transcompiler))  
[Website](https://babeljs.io/)

***

## Install

In a terminal:

```console
mkdir test
cd test
npm init -y
touch index.html
touch babel.config.js
touch es6.js
```

es6.js:

```javascript
let test = "123";
```

let keyword only available since es6 and here we want to show that babel do his job by converting to an earlier version understandable by most of the browsers.

In a terminal:

```console
npm i @babel/core @babel/cli @babel/preset-env
```

Then all dependencies are in created in node_modules folder.
If node_modules does not exist or removed, type command below will read package.json file and reinstall everything:

```console
npm i
```

First, test babel without preset by adding script below in package.json:

```javascript
"scripts": {
    ...,
    "babel": "babel es6.js"
  }
```

Then, in a terminal:

```console
npm run babel

> test@1.0.0 babel
> babel es6.js

let test = "123";
```

No difference in output but at least we now that babel core is running well.

To have output in a new file, package.json:

```javascript
"scripts": {
    ...,
    "babel": "babel es6.js -o es6after.js"
  }
```

Then, again, in a terminal:

```console
npm run babel

> test@1.0.0 babel
> babel es6.js -o es6after.js
```

Now, as expected, we have a new file es6after.js

Now we configure babel in babel.config.js to use preset-env:

```javascript
module.exports = {
  presets: [["@babel/preset-env"]]
}
```

Then, again, in a terminal:

```console
npm run babel

> test@1.0.0 babel
> babel es6.js -o es6after.js
```

And now, in es6after.js has been converted in an earlier version:

```javascript
"use strict";

var test = "123";

```

***

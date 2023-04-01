# misc

---

## Hello world

Check that `NodeJS` is installed:

```console
node -v
```

Create a `JS` file with following content, test.js:

```js
console.log("Hello, world!");
```

To execute, from terminal, type:

```console
node test.js
```

---

## Spread Vs Rest

Same synthax '…' for the Spread operator and the Rest operator.

The difference between the two is the context of use, they do not have the same effect.

### Spread

The spread syntax expands iterables into individual elements.

In other words, the spread operator unpacks the elements of an iterable object.

[Spread demo](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Operators/Spread_syntax):

```js
function sum(x, y, z) {
  return x + y + z;
}

const numbers = [1, 2, 3];

console.log(sum(...numbers)); // expected output: 6

console.log(sum.apply(null, numbers)); // expected output: 6
```

### Rest

The rest operator puts the rest of some specific user-supplied values into a JavaScript array.

In other words, the rest parameter packs the elements into an array.

[Rest demo](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Functions/rest_parameters):

```js
function sum(...theArgs) {
  let total = 0;
  for (const arg of theArgs) {
    total += arg;
  }
  return total;
}

console.log(sum(1, 2, 3)); // expected output: 6

console.log(sum(1, 2, 3, 4)); // expected output: 10
```

---

## Create simple simple js application

From [source](https://create-project.js.org/).

Install `create-js-project` via npm:

```console
npm i -g create-js-project
```

Create project, optionally providing a `project-name`:

```console
create-js-project <project-name>
```

---

## Create simple simple webpack application

From [source](https://www.npmjs.com/package/create-webpack-application).

Install `create-webpack-application` via npm:

```console
npm install -g create-webpack-application
```

Create project:

```console
create-webpack-application
```

---

# 116-multipages-webpack

Setup Webpack project for multi page application.

[Source code on GitHub](https://github.com/oldu73/blog-001)

---

## Goals

It's not a "Single Page Application" as we may do like with a framework (Angular, Vue.js or React).

The objective is not to recode a framework but to master Webpack and a project in vanilla JavaScript (without framework).

After initial setup from [previous chapter](115-introduction.md) we want to create a multi page application.

To do this, you must configure Webpack to have exactly one bundle per page.

---

## Second page

Create a second page.

First create a new folder 'form' in 'src' folder:

```console
../blog-001/src$ mkdir form
```

In this 'form' folder create the three base needed files:

```console
../blog-001/src$ touch form.html
../blog-001/src$ touch form.scss
../blog-001/src$ touch form.js
```

---

## Edit html

### form.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Form</title>
</head>
<body>
  <a href="./index.html">Accueil</a>
</body>
</html>
```

### index.html

```html
<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Blog</title>
  </head>
  <body>
    <div class="container">
      <a href="./form.html">Form</a>
    </div>
  </body>
</html>
```

---

## Edit scss

### form.scss

```scss
a {
  color: red;
}
```

### index.scss

Rename 'style.scss' to 'index.scss'

```console
mv style.scss index.scss
```

index.scss:

```scss
a {
  color: orange;
}
```

---

## Edit js

### form.js

```js
import "./form.scss";

console.log("form");
```

### index.js

```js
import "./index.scss";

console.log("index");
```

---

## Webpack

Edit Webpack configuration file to add new entry point and then a new bundle, webpack.config.js:

```js
...
module.exports = {
  entry: {
    main: path.join(__dirname, "src/index.js"),
    form: path.join(__dirname, "src/form/form.js"),
  },
...
  plugins: [
    new HtmlWebpackPlugin({
      filename: "index.html",
      template: path.join(__dirname, "./src/index.html"),
      chunks: ["main"]
    }),
    new HtmlWebpackPlugin({
      filename: "form.html",
      template: path.join(__dirname, "./src/form/form.html"),
      chunks: ["form"]
    }),
  ],
...
```

### Properties

- **filename** to specify html output  
- **template** to specify html input  
- **chunks** specify the bundle to insert

---

## Generate

To observe generated structure by Webpack:

```console
npm run webpack
```

Then browse the 'dist' folder.

---

## Test

Test it:

```console
npm start
```

Then browse to [http://localhost:4000/](http://localhost:4000/)

---

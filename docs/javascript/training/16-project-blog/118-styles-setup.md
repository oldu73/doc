# 118-styles-setup

Setting up styles for our application.

---

## Setup

Create a `styles` folder inside `assets` folder:

```console
../blog-001$ mkdir src/assets/styles
```

We start by creating the `scss` files we will need in folder  `src/assets/styles`:

[Sass @import and Partials](https://www.w3schools.com/sass/sass_import.php)

```console
../blog-001$ cd src/assets/styles
touch styles.scss
touch _reset.scss
touch _media-queries.scss
touch _classes.scss
touch _base.scss
touch _variables.scss
touch _utils.scss
```

Edit `styles.scss` to import partials, no need to put underscore as file name prefix due to the fact those are partials:

```scss
@import "base";
@import "classes";
@import "media-queries";
@import "reset";
@import "utils";
@import "variables";
```

### reset

_reset.scss:

```scss
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
```

### variables

To choose colors, [FLAT UI COLOR](https://flatuicolors.com/)

For `--font-family` c.f. [fonts section](#fonts)

For `--box-shadow` it comes from inspected element, "card" class on [firebase](https://firebase.google.com/) web site.

_variables.scss:

```scss
:root {
  --primary: #2ecc71;
  --dark: #27ae60;
  --accent: #2c3e50;
  --text: #333;
  --divider: #ecf0f1;
  --font-family: "Mulish", sans-serif;
  --box-shadow: 0 1px 2px 0 rgba(60,64,67,.3),0 1px 3px 1px rgba(60,64,67,.15)
}
```

### fonts

We use `Mulish` from [google fonts](https://fonts.google.com/)

light 300, regular 400, bold 700

index.html:

```html
<head>
...
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link
    href="https://fonts.googleapis.com/css2?family=Mulish:wght@300;400;700&display=swap"
    rel="stylesheet"
  />
...
</head>
```

### base

_base.scss:

```scss
:root {
  font-size: 62.5%;
}

body {
  font-size: 1.6rem;
  color: var(--text);
  font-family: var(--font-family);
}

h1,
h2,
h3,
h4 {
  margin-bottom: 2rem;
}

h1 {
  font-size: 3.5rem;
}

h2 {
  font-size: 3rem;
}

h3 {
  font-size: 2.5rem;
}

h4 {
  font-size: 2rem;
}

ul {
  list-style: 0;
}

img {
  max-width: 100%;
}

a {
  color: var(--text);
  text-decoration: none;
}
```

### media-queries

[From CV project of HTML & CSS course](https://github.com/oldu73/formation-dyma-htmlcss-projet4-cv-001/blob/main/scss/_media-queries.scss)

_media-queries.scss:

```scss
/* Landscape phones and down */
@mixin xs {
  @media (max-width: 480px) {
    @content;
  }
}

/* Landscape phone to portrait tablet */
@mixin sm {
  @media (max-width: 767px) {
    @content;
  }
}

/* Portrait tablet to landscape and desktop */
@mixin md {
  @media (min-width: 768px) and (max-width: 979px) {
    @content;
  }
}

/* Large desktop */
@mixin xl {
  @media (min-width: 1200px) {
    @content;
  }
}
```

### index scss and js

If `index.scss` file exist at root `src` folder from previous chapter, empty it otherwise create it.

index.js:

```js
import "./assets/styles/styles.scss";
import "./index.scss";

console.log("index");
```

### Have a try

```console
npm start
```

browse to [local](http://localhost:4000/)

---

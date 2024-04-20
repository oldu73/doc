# Setup

Setting up VS Code environment to work with Tailwind.

---

## Create first folder

Create a `tailwind` folder for your project and open it with `VS Code`.

Create a `src` folder in it.

Create `style.css` and `index.html` files in `src` folder.

index.html:

```html
<!doctype html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body>
  <h1 class="text-3xl font-bold underline">
    Hello Tailwind !
  </h1>
</body>
</html>
```

---

## Initialize with npm

Open a terminal from VS Code in `tailwind` folder and initialize `package.json` file by typing following command:

```console
npm init -y
```

---

## Install Tailwind CSS CLI

Install `Tailwind CSS` with npm (-D option = development dependency):

```console
npm install -D tailwindcss
```

---

## Initialize Tailwind CSS

Always in terminal, initialize `Tailwind` configuration file with following command:

```console
npx tailwindcss init
```

This create a file `tailwind.config.js`.

---

## Setup templates path

In tailwind configuration file, tailwind.config.js:

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{html,js}"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

---

## Add Tailwind directives in CSS file

src\style.css:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

---

## Start build process of Tailwind CLI

```console
npx tailwindcss -i ./src/style.css -o ./src/output.css --watch
```

This build CSS and scan for modifications.

Create a `npm start` script in `package.json` with this command:

```json
{
  "name": "tailwind",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "npx tailwindcss -i ./src/style.css -o ./src/output.css --watch"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "tailwindcss": "^3.4.1"
  }
}
```

---

Use Tailwind in HTML

Include generated `CSS` in `index.html` file by adding `<link href="./output.css" rel="stylesheet" />`:

```html
<!doctype html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link href="./output.css" rel="stylesheet">
</head>
<body>
  <h1 class="text-3xl font-bold underline">
    Hello Tailwind !
  </h1>
</body>
</html>
```

---

Open `index.html` file by double clicking on it from file explorer to open it in a browser and seeing the result.

..

[Configuration des extensions VS Code](https://dyma.fr/developer/list/v2/chapters/core/659d7db381e610279ed48dc6/659ea3e881e610279ed4a387/lesson)

---

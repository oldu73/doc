# Setup

Setting up VS Code environment to work with Tailwind CSS.

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

## Use Tailwind in HTML

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

Open `index.html` file by double clicking on it from file explorer to open it in a browser and seeing the result.

---

## Tailwind CSS IntelliSense

In `VS Code` browse to `Extension` tab and search/install for extension `Tailwind CSS IntelliSense`.

Create a `.vscode` folder and a file `settings.json` in it, with following content:

```json
{
  "files.associations": {
    "*.css": "tailwindcss",
    "*.scss": "tailwindcss"
  },
  "editor.quickSuggestions": {
    "strings": "on"
  }
}
```

---

## Prettier

In `VS Code` browse to `Extension` tab and search/install for extension `Prettier`.

In `VS Code` browse to `File` menu then `Preferences` then `Settings`. Search `format on save` and tick the box.

Always in `VS Code` and always in `Settings` search for `Default Formatter` and in dropdown list select `Prettier - Code Formatter esbenp.prettier-vscode`.

---

## Prettier plugin tailwindcss

Install official `Tailwind` plugin for `Prettier`:

```console
npm install -D prettier prettier-plugin-tailwindcss
```

At same level than `tailwind.config.js` file (project's root level), create a `prettier.config.js` file with following content:

```js
module.exports = {
  plugins: ['prettier-plugin-tailwindcss'],
}
```

It automatically put Tailwind's utility class in recommended order, at save.

---

## Material Icon Theme

In extension tab search for `Material Icon Theme` plugin and install it.

It display icons in relation with file extensions, in VS code.

---

## Error Lens

In extension tab search for `Error Lens` plugin and install it.

It indicates (highlighted text) error, directly in line.

---

## Live Server

In extension tab search for `Live Server` plugin and install it.

It's a minimal local development server that alow hot reloading page, at save.

Ideal, when without any framework (React, Vue, Angular).

Then right click in html file, e.g. `index.html` click on `Open with Live Server` to render it in a browser.

When saving changes, You may observe live hot reload in the browser.

---

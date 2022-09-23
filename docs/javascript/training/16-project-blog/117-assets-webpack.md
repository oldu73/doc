# 117-assets-webpack

Assets are common files used throughout the application, such as images, javascripts, styles, etc..

---

## Setup

Webpack setup for assets.

We will have to use two specific plugins to copy assets and clean 'dist' folder.

`copy-webpack-plugin`

This plugin will copy the contents of the 'assets' directory from 'src' to the 'dist' directory.

`clean-webpack-plugin`

This plugin will empty the 'dist' directory before each build (npm run webpack)

Install:

```console
npm i -D copy-webpack-plugin clean-webpack-plugin
```

webpack.config.js:

```js
...
const CopyWebpackPlugin = require("copy-webpack-plugin");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
...
...
  plugins: [
    new CleanWebpackPlugin(),
    new CopyWebpackPlugin({
      patterns: [
        {
          from: "./src/assets/images/*",
          to:path.resolve(__dirname, 'dist','assets/images', '[name][ext]'),
        },
      ],
    }),
...
```

For testing, add an image in 'src/assets/images' folder and edit html files accordingly.

index.html:

```html
...
<body>
    <div class="container">
      <h1>Blog</h1>
      <a href="./form.html">Form</a><br /><br />
      <img src="./assets/images/proto.jpg" alt="protocoles" width="300" />
    </div>
</body>
...
```

form.html:

```html
...
<body>
    <h1>Form</h1>
    <a href="./index.html">Accueil</a><br /><br />
    <img src="./assets/images/proto.jpg" alt="protocoles" width="600" />
</body>
...
```

Build application:

```console
npm run webpack
```

Observe 'dist' folder that now contain an 'assets' folder with copied image.

---

## Code sharing

Sharing `javascript` code among `bundles`.

Create a 'javascripts' folder inside 'assets' folder with a 'topbar.js' in it.

topbar.js:

```js
console.log("topbar");
```

We want to share this code with the two pages, 'index.html' and 'form.html'.

To do so, we should add a new entry point in the webpack configuration and share the 'chunk' among our two html files.

webpack.config.js:

```js
...
module.exports = {
  entry: {
    main: path.join(__dirname, "src/index.js"),
    form: path.join(__dirname, "src/form/form.js"),
    topbar: path.join(__dirname, "src/assets/javascripts/topbar.js"),
  },
...
new HtmlWebpackPlugin({
      filename: "index.html",
      template: path.join(__dirname, "./src/index.html"),
      chunks: ["main", "topbar"],
    }),
    new HtmlWebpackPlugin({
      filename: "form.html",
      template: path.join(__dirname, "./src/form/form.html"),
      chunks: ["form", "topbar"],
    }),
...
```

Build application:

```console
npm run webpack
```

Observe in 'dist' folder that now html files include 'topbar.js'.

---

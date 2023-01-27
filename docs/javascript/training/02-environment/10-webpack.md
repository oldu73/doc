# 10-webpack

---

## Basic principles

Webpack is installed through npm.

What is installed:

- webpack
- webpack-cli
- webpack-dev-server
- babel-loader

### Webpack

Bundle creation.

Webpack is configured through webpack.config.js configuration file.

Webpack use babel-loader and take html + js content to create a dist bundle that contain optimized bundle.js and html content.

### Webpack-cli

Use webpack in a terminal.

### Webpack-dev-server

Local development web server the list at localhost address on dedicated port and return index.html file from dist folder.

---

## Installation

### Prerequisite

First, rename used file in previous chapter src/es6.js to src/index.js and remove dist folder if it exist and move index.html in src/:

```console
mv src/es6.js src/index.js
rm -rf dist
mv index.html src/
```

In src/index.html remove script section.

src/index.html:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h1>My app</h1>
  </body>
</html>
```

Edit src/index.js as follow.

src/index.js:

```javascript
let test = "123";
console.log(test);
```

In package.json remove line in script section that concern babel because we'll let webpack to take it in charge.

package.json:

```json
{
  "name": "test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@babel/cli": "^7.17.6",
    "@babel/core": "^7.17.5",
    "@babel/preset-env": "^7.16.11"
  }
}
```

### Launch

```console
npm i webpack webpack-cli webpack-dev-server babel-loader
```

Add new file at the project root level called webpack.config.js:

```console
touch webpack.config.js
```

webpack.config.js:

```javascript
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: path.resolve(__dirname, "src/index.js"),
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "[name].bundle.js",
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
        },
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "src/index.html"),
    }),
  ],
  devtool: "source-map",
  mode: "development",
  devServer: {
    static: path.resolve(__dirname, "./dist"),
    open: true,
    port: 4000,
  },
};
```

Html webpack plugin automatically inject output js bundle to html file.

Install:

```console
npm i html-webpack-plugin
```

Edit package.json to use webpack and webpack server in script section.

package.json

```json
...
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "webpack": "webpack",
    "start": "webpack serve"
  },
...
```

### Run

```console
npm run webpack
```

Now a dist folder appear with an index.html file that does include call to output result of bundled js, main.bundle.js.

dist/index.html:

```html
...
<head>
  ...
  <script defer src="main.bundle.js"></script>
  ...
</head>
...
```

In dist/main.bundle.js we may observe that output code went through babel because let has been replaced by var.

dist/main.bundle.js:

```javascript
...
var test = "123";
...
```

### Start

```console
npm start
```

Browse to [http://localhost:4000/](http://localhost:4000/) and hit F12 to open browser development tools and observe console.

Retrieve code for this chapter on [github](https://github.com/dymafr/javascript-chapitre2)

---

## Debug

Common issues

### Hot update

Browser does not auto refresh on change (fixed from [source](https://stackoverflow.com/questions/65640449/how-to-solve-chunkloaderror-loading-hot-update-chunk-second-app-failed-in-webpa)).

webpack.dev.js:

```js
module.exports = {
    ...,
    optimization: {
        runtimeChunk: 'single'
    },
    ...
}
```

Works for Windows and WSL native folder as well.

---

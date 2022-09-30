# 115-introduction

Project presentation and setup.

[Dyma.fr Source code on GitHub](https://github.com/dymafr/javascript-chapitre16-projet_blog-partie1)  
[OLDU Source code on GitHub](https://github.com/oldu73/blog-001)

---

## Goals

Our goal is to create a real app to blog.

Project setup use Polyfill and Sass in addtion of what was used in previous project (todo).

Goals:

- Create many pages (many Webpack bundles)  
- Sass setup  
- Use of images  
- Creation of articles  
- Deletion of articles  
- Responsive Topbar with menu for mobile

We gonna use Webpack and initialize project's root folder accordingly.

---

## Create project

(Create a new project's folder)

[Refer to environment chapter to get more details](../02-environment/06-install-environnement.md)

### Git

[Git setup](../../../git/git.md#initialize)

### Npm

Initialize package.json file at the project's root folder level by typing:

```console
npm init
```

### Dependencies

Install dependencies:

```console
npm i -D @babel/cli @babel/core @babel/preset-env babel-loader css-loader html-webpack-plugin style-loader webpack webpack-cli webpack-dev-server core-js@3 regenerator-runtime
```

### Webpack

Create a Webpack configuration file, webpack.config.js:

```js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: {
    main: path.join(__dirname, "src/index.js")
  },
  output: {
    path: path.join(__dirname, "dist"),
    filename: "[name].bundle.js"
  },
  module: {
    rules: [
      {
        test: /\.js/,
        exclude: /(node_modules)/,
        use: ["babel-loader"]
      },
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"]
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.join(__dirname, "./src/index.html")
    })
  ],
  stats: "minimal",
  devtool: "source-map",
  mode: "development",
  devServer: {
    open: false,
    static: path.resolve(__dirname, './dist'),
    port: 4000
  }
};
```

### Babel

Then for Babel, a configuration file as well, babel.config.js:

```js
const presets = [
    [
      "@babel/preset-env",
      {
        useBuiltIns: "usage",
        debug: true,
        corejs: 3,
        targets: "> 0.25%, not dead"
      }
    ]
  ];

  module.exports = { presets };
```

### Gitignore

Edit .gitignore file by adding following entries (.history folder is for VS Code):

```txt
.history
node_modules
dist
```

### Source folder

Create a source folder and add the minimum files needed for the application:

```console
mkdir src
cd src
touch index.html
touch style.css
touch index.js
```

### Git push initial setup

```console
git status
git add .
git commit -m "initial setup"
git push
```

### VS Code

Launch VS Code from terminal in project's root folder:

```console
code .
```

Ready!

### Sass with Webpack

Edit webpack.config.js:

```js
...
module: {
    rules: [
      {
        test: /\.js/,
        exclude: /(node_modules)/,
        use: ["babel-loader"]
      },
      {
        test: /\.scss$/i,
        use: ["style-loader", "css-loader", "sass-loader"]
      }
    ]
  },
...
```

Then install loader and compiler that allow to transform Sass to CSS:

```console
npm install sass-loader sass --save-dev
```

---

## Setup

### index.html

Edit src/index.html with following content:

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
      <h1>Accueil</h1>
    </div>
  </body>
</html>
```

### index.js

Import styles in index.js to be considered as a dependency by Webpack and then, bundled.

src/index.js:

```js
import "./style.scss";
```

### style.scss

Rename style.css file created above with Sass file extension:

```console
mv src/style.css src/style.scss
```

Edit src/style.scss with following content:

```scss
.container {
  h1 {
    color:red;
  }
}
```

### package.json

Edit script part of the package.json file for Webpack:

package.json:

```json
..
"scripts": {
  "test": "echo 'Error: no test specified' && exit 1",
  "webpack": "webpack",
  "start": "webpack serve"
},
..
```

### Git push minimal files

```console
git status
git add .
git commit -m "minimal files"
git push
```

---

## Start

Start local Webpack development server:

```console
npm start
```

Browse to [http://localhost:4000/](http://localhost:4000/)

---

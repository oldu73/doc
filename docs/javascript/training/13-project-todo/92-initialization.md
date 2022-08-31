# 92-initialization

Project presentation and setup.

[Source code on GitHub](https://github.com/oldu73/todo-list-001)

---

## Goals

Our goal is to create a to-do list.

We want to be able to create, edit, validate and delete todos, entirely in JavaScript.

To achieve this we gonna use Webpack and initialize project's root folder accordingly.

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
npm i -D @babel/cli @babel/core @babel/preset-env babel-loader css-loader html-webpack-plugin style-loader webpack webpack-cli webpack-dev-server
```

### Webpack

Create a Webpack configuration file, webpack.config.js:

```js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: {
    main: path.join(__dirname, "src/index.js"),
  },
  output: {
    path: path.join(__dirname, "dist"),
    filename: "[name].bundle.js",
  },
  module: {
    rules: [
      {
        test: /\.js/,
        exclude: /(node_modules)/,
        use: ["babel-loader"],
      },
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.join(__dirname, "./src/index.html"),
    }),
  ],
  stats: "minimal",
  devtool: "source-map",
  mode: "development",
  devServer: {
    open: false,
    static: path.resolve(__dirname, "./dist"),
    port: 4000,
  },
};
```

### Babel

Then for Babel, a configuration file as well, babel.config.js:

```js
module.exports = {
  presets: [["@babel/preset-env"]],
};
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
    <title>To do</title>
  </head>
  <body>
    <div class="container">
      <form>
        <input type="text" />
        <button>Add</button>
      </form>
      <ul>
        <li>
          <span class="todo done"></span>
          <p>text</p>
          <button>Edit</button>
          <button>Delete</button>
        </li>
      </ul>
    </div>
  </body>
</html>
```

### index.js

Import styles in index.js to be considered as a dependency by Webpack and then, bundled.

src/index.js:

```js
import "./style.css";
```

### style.css

Edit src/style.css with following content:

```css
* body {
  box-sizing: border-box;
}

body {
  margin: 0;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

p {
  margin: 0;
}

.container {
  width: 500px;
  border: 1px solid #eee;
  border-radius: 3px;
  padding: 20px;
  display: flex;
  flex-direction: column;
}

form {
  display: flex;
  margin-bottom: 20px;
}

button {
  padding: 5px 15px;
  border: 0px;
  border-radius: 5px;
  cursor: pointer;
  margin: 0 3px;
}

input {
  padding: 8px 15px;
  outline: 0;
  border: 1px solid #ddd;
  border-radius: 3px;
}

form input {
  flex: 1;
  margin-right: 15px;
}

ul {
  padding: 0;
  list-style: none;
}

li {
  display: flex;
  align-items: center;
}

li p {
  flex: 1;
}

li .todo {
  flex: 0 0 20px;
  height: 20px;
  border-radius: 30px;
  margin-right: 15px;
  border: 2px solid #333;
}

li .todo.done {
  background: #333;
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

# 81-css-with-webpack

***

## How to

### css

Create a style.css file in src folder.

syle.css:

```css
body {
  height: 300px;
  background-color: blue;
  border: 1px solid grey;
}
```

### js

In index.js file, import css file.

index.js:

```js
import "./style.css";
```

Now css file is a dependency managed by Webpack.

### Webpack setup

For Webpack to process the CSS file correctly, add two new loaders and configuration to the webpack.config.js file.

Install necessary loaders:

```console
npm install css-loader style-loader -D
```

webpack.config.js:

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
        test: /\.css$/,
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

For CSS files, we first apply the css-loader and then the style-loader.

Loaders are applied from right to left of the array passed to the use property.

***

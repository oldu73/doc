# 102-polyfill-webpack

A [polyfill](https://developer.mozilla.org/en-US/docs/Glossary/Polyfill) is a piece of code (usually JavaScript on the Web) used to provide modern functionality on older browsers that do not natively support it.

---

## Webpack setup

Webpack alone does not take care of adding polyfills to your application, you must use Babel and Browserlist which is a dependency of Babel.

Webpack setup to use polyfills, in a terminal, at the root folder level of the project, type in the command below:

```console
npm i -D core-js@3 regenerator-runtime
```

---

## Babel setup

Edit file babel.config.js as follow:

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

### useBuiltIns

'useBuiltIns' option, allow Babel's polyfills management.  
Default value is 'false' and Babel therefor will not add polyfills automatically.

'usage' value allow to import **only needed polyfills** for targeted browsers.

'entry' value will import **all polyfills** for targeted browsers (even those not used by your application).

### debug

'debug' option, will log browser release targets and all your code needs in term of polyfills.

### corejs

'corejs' option, specify release of core-js library to use.

### targets

'target' option, allow to specifiy targeted browser.  
This option is extremely powerful.

```txt
Support the browsers used in France by at least 0.2% of people:

"> 0.2% in FR"

Support all versions of browsers representing at least 1% of users or which are not dead:

"> 1%, not dead"

Latest three versions of browsers:

"last 3 versions"

Recommended:

"defaults"
```

"default" is equivalent to "> 0.5%, last 2 versions, Firefox ESR, not dead"

---

## Conclusion

The more polyfills there are, the heavier your application will be, but the more it will be supported by all browsers.

There is no recommendation, it all depends on your target!

If you don't have a particular target use 'defaults' which is the official Browserslist recommendation.

---

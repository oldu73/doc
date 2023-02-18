# 07-use-create-react-app

---

## Project

Setup a new `React` project:

```console
npx create-react-app myapp

cd myapp

npm start
```

To install in current folder:

```console
npx create-react-app .
```

On install fail try to ([src](https://stackoverflow.com/questions/71733043/npm-install-no-audit-save-save-exact-loglevel-error-react-react-dom-reac)):

```console
npm cache clean --force
and then, again
npx create-react-app myapp
```

To reinstall node_modules folder:

```console
npm i
```

---

## Build

Build project for production:

```console
npm run build
```

---

## Webpack

Expand project to fine tune `Webpack` configuration, warning, this operation is irreversible:

```console
npm run eject
```

---

## Sass

To install [Sass](https://sass-lang.com/) for current project (inside project folder):

```console
npm i sass
```

---

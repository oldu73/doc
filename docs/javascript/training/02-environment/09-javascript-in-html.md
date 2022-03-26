# 09-javascript-in-html

***

## Initial setup

In root folder project create a src folder that will contain all javascript source code:

```console
mkdir src
```

Move file (just a test file) used from previous chapter in src folder:

```console
mv es6.js src
```

Edit babel script in package.json to specify input = src folder, output = dist folder:

```javascript
"scripts": {
   "babel": "babel src --out-dir dist"
  }
```

Test, type below command in a terminal and have a look in dist folder to see the result of babel transpiled output:

```console
npm run babel
```

***

## Html

In root project folder:

```console
touch index.html
```

Open index.html in VS Code, and type "!" + tab to get a minimal html file set.

Add a h1 section in body and from file explorer drag and drop it in a browser to see the result:

```html
<body>
  <h1>My app</h1>
</body>
```

***

## Source

### In html

Now to incorporate javascript in our html page, add a script section in head:

```html
<head>
  ...
  <script type="text/javascript"></script>
</head>
```

Not really common expect for some dedicated usage like google analytic, but you may insert javascript directly inside script section of an html page (be aware to put only retro compatible code in it as long as it's directly interpreted by the browser, doesn't went through babel transpiler):

```html
<head>
  ...
  <script type="text/javascript">
    console.log('Hi');
  </script>
</head>
```

Drag and drop index.html file in a browser, right click inspect and then observe message in console.

You may incorporate anywhere inside a head or body section of an html document.

***

### Out html

Separation of concern.

Edit head, script section of index.html as follow:

```html
<script type="text/javascript" src="dist/es6.js"></script>
```

Never edit files in dist folder.

Edit src/es6.js by adding a line as follow:

```javascript
console.log("test");
```

Recompile application with babel:

```console
npm run babel
```

See the result in dist folder and refresh the page in the browser to observe message in log.

***

### From web

E.g. [jquery](https://jquery.com/)

CDN = Content Delivery Network

Browse for [jquery cdn](https://releases.jquery.com/) and copy script snippet of last minified version and add it to head section of html file:

```html
<head>
  
  <script src="https://code.jquery.com/jquery-3.6.0.min.js" integrity="sha256-/xUj+3OJU5yExlq6GSYGSHk7tPXikynS7ogEvDej/m4=" crossorigin="anonymous"></script>

  <script type="text/javascript" src="dist/es6.js"></script>
  
</head>
```

Test that jquery is working by adding following line in src/es6.js file:

```javascript
console.log($);
```

npm run babel and refresh page in browser, in console you may observe a message that correspond to a function like:

```text
ƒ (e,t){return new S.fn.init(e,t)}
```

What happen here is that we bring back javascript from internet.

***

# 163-debugger-vscode

VS Code debugger

---

## Extension

Install VS Code [JavaScript Debugger Companion Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-companion).

### Set up

Click on `Run and Debug` icon in the left navigation pan.

Click on `create a launch.json file` and select `Web App (Chrome)`.

Edit `launch.json` file for `url` port number (4000) and `webRoot` folder, as follow:

```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "type": "chrome",
      "request": "launch",
      "name": "Launch Chrome against localhost",
      "url": "http://localhost:4000",
      "webRoot": "${workspaceFolder}/src"
    }
  ]
}
```

Start `Webpack dev server`:

```console
npm start
```

---

## debugger statement

The [debugger](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/debugger) statement invoke debugging functionality.

Just add `debugger;` statement directly into your `.js` file.

In VS Code, it stop script execution when reached, allowing to inspect execution context, e.g.:

```js
...
li.addEventListener("click", () => {
      debugger;
      ...
    });
...
```

---

## Launch debugger

When `debugger;` statement VS Code will automatically open source currently being debugged.

Otherwise you may launch debugger in `Debug` tab by clicking on green play button at top left corner, or by clicking on down blue bar shortcut, play `Launch Chrome against localhost..`.

---

## Run to next line

```txt
Step Over (F10)
```

---

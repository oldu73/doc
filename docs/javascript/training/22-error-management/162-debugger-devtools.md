# 162-debugger-devtools

Chrome DevTools debugger

---

## debugger statement

The [debugger](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/debugger) statement invoke debugging functionality.

Just add `debugger;` statement directly into your `.js` file and inspect page in browser to debug.

In Chrome DevTools debugger it stop script execution when reached, allowing to inspect execution context, e.g.:

```js
...
li.addEventListener("click", () => {
      debugger;
      ...
    });
...
```

---

## Synchronize file

To synchronize changes made directly in Chrome DevTools, with local project source files.

In `Sources` tab (right click inspect page in Chrome), click on `+ Add folder to workspace` select your root project folder and allow full access to it by clicking on `Authorize` to let Chrome synchronize your changes.

---

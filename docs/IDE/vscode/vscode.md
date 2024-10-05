# VS Code

---

## Switch windows

To set shortcut to switch from editing area (above) and terminal area (below):

Hit `ctrl+shift+p`and search for `key` and select `Preferences: Open Keyboard Shortcuts`.

Search for `focus terminal` then edit `Focus on Terminal View`, command = terminal.focus, shortcut = `ctrl+1`.

Search for `focus active` then edit `Focus Active Editor Group`, command = workbench.action.focusActiveEditorGroup, shortcut = `ctrl+2`.

---

## Code snippet shortcut

Ctrl+space

Console code block in markdown file:  
Ctrl+space, then select fenced codeblock, then select console

```console
...
```

---

## Move line

Alt + Up/down keys

---

## Duplicate line

If you want to copy the line to the line above itself, press Shift + Alt + Up Arrow Key.

If you want to copy the line to the line below itself, press Shift + Alt + Down Arrow Key.

Hit Ctrl + c (without anything selected on a line) and then Ctrl + v will directly copy/paste the entire line even nothing is selected.

---

## Column select

Mouse  
**Shift + Alt** then click and drag

Keyboard  
**Ctrl + Shift + Alt** then use arrow keys

---

## Comment code

**Slash** from the numeric keypad!

Or..

Comment code block

```text
Ctrl + K + C
```

or line

```text
Ctrl + §
```

Uncomment code block

```text
Ctrl + K + U
```

or line

```text
Ctrl + §
```

---

## Multiple cursors

[Multiple selections (multi-cursor)](https://code.visualstudio.com/docs/editor/codebasics#_multiple-selections-multicursor)

**Alt+Click**. Each cursor operates independently based on the context it sits in.

A common way to add more cursors is with **Ctrl+Alt+Down** or **Ctrl+Alt+Up** that insert cursors below or above.

---

## Open from terminal

To open a file in VS Code editor from integrated terminal:

```console
code -r fileName
```

---

## Open an other instance

To open an other instance (from VS Code integrated terminal) or a VS Code instance from another terminal:

```console
code .
```

---

## Keyboard shorcut

### Set

Press Ctrl + Shift + P keyboard shortcut and this will open Command Palette.  
There type **Open Keyboard Shortcuts** and select it from the list.

In search bar, type e.g.: - 'ctrl + §' to find default toggle line comment shortcut.

### Reset

Press Ctrl + Shift + P keyboard shortcut and this will open Command Palette.  
There type **Open Keyboard Shortcuts (JSON)** and select it from the list.

You may remove one or more undesired entries or simply remove everything from keybindings.json and type empty [] into it, then save.

---

## IntelliSense

[IntelliSense is a general term for various code editing features](https://code.visualstudio.com/docs/editor/intellisense)

### Suggestion widget

Hit **Ctrl + Space** to trigger IntelliSense suggestion widget.

---

## Hide terminal

To toggle terminal view:

```txt
Ctrl + J
```

---

## Hide file explorer

To toggle file explorer view:

```txt
Ctrl + B
```

---

## Run code

Quickly run a piece of code (JS) to debug, create a new `test.js` file and your code sample in it:

```txt
F1 -> Run ..
```

---

## New line

New line without need to go end of line:

```txt
ctrl + enter
```

---

## console.log shortcut

```txt
log + tab
```

---

## Todo Tree

[Extension to manage todo](https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.todo-tree), should be capital case `TODO` in code to be considered.

(Works also with `FIXME` but it's maybe better to stay simple).

---

## Prettier

Set up `Prettier` - Code formatter, to format code on save:

```txt
VS Code File -> Preferences -> Settings -> type "format" -> check "Editor: Format On Save"
```

---

## Reformat code

Optimized output build file are not easily readable (one liner), e.g.:

```css
body{-webkit-font-smoothing:antialiased;-moz-osx-font-smoothing:grayscale;font-family:-apple-system,BlinkMacSystemFont,Segoe UI,Roboto,Oxygen,Ubuntu,Cantarell,Fira Sans,Droid Sans,Helvetica Neue,sans-serif;margin:0}code{font-family:source-code-pro,Menlo,Monaco,Consolas,Courier New,monospace}.title{background-color:#d8e0da;color:green}
/*# sourceMappingURL=main.5fed45fe.css.map*/
```

If code formatter plugin is installed hit `ctrl + s` to save the opened file and format it in a readable manner:

```css
body {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, Oxygen,
    Ubuntu, Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
  margin: 0;
}
code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, Courier New, monospace;
}
.title {
  background-color: #d8e0da;
  color: green;
}
/*# sourceMappingURL=main.5fed45fe.css.map*/
```

---

## Delete line

Just type `Ctrl + X`. Now the whole line is deleted!

---

## Search every where

With text selected in current file, hit:

```txt
ctrl + shift + f
```

---

## Editor Font Size

Click Settings at the bottom left.

Click Settings.

Search Font Size.

Type in the Font size of your choice.

---

## Run Task

To run `npm` scripts from `package.json` file, click in main central search field of `VS Code` and select `Run Task`.

---

## Word Wrap

Preferences - Settings - search for 'word wrap' - set it to 'on' and column to '80'.

---

## Select all current

Select all what's currently selected:

```txt
ctrl + shift + l
```

---

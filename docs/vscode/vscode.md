# VS Code

***

## Switch windows

To switch from editing area (above) and terminal area (below):

Ctrl + arrow up/down

To switch from same area type windows (edit/terminal):

Alt + arrow left/right

***

## Code snippet shortcut

Ctrl+space

Console code block in markdown file:  
Ctrl+space, then select fenced codeblock, then select console

```console
...
```

***

## Move line

Alt + Up/down keys

***

## Duplicate line

If you want to copy the line to the line above itself, press Shift + Alt + Up Arrow Key.

If you want to copy the line to the line below itself, press Shift + Alt + Down Arrow Key.

Hit Ctrl + c (without anything selected on a line) and then Ctrl + v will directly copy/paste the entire line even nothing is selected.

***

## Column select

Mouse  
**Shift + Alt** then click and drag

Keyboard  
**Ctrl + Shift + Alt** then use arrow keys

***

## Comment code

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

***

## Multiple cursors

[Multiple selections (multi-cursor)](https://code.visualstudio.com/docs/editor/codebasics#_multiple-selections-multicursor)

**Alt+Click**. Each cursor operates independently based on the context it sits in.

A common way to add more cursors is with **Ctrl+Alt+Down** or **Ctrl+Alt+Up** that insert cursors below or above.

***

## Open from terminal

To open a file in VS Code editor from integrated terminal:

```console
code -r fileName
```

***

## Open an other instance

To open an other instance (from VS Code integrated terminal) or a VS Code instance from another terminal:

```console
code .
```

***

## Keyboard shorcut

### Set

Press Ctrl + Shift + P keyboard shortcut and this will open Command Palette.  
There type **Open Keyboard Shortcuts** and select it from the list.

In search bar, type e.g.: - 'ctrl + §' to find default toggle line comment shortcut.

### Reset

Press Ctrl + Shift + P keyboard shortcut and this will open Command Palette.  
There type **Open Keyboard Shortcuts (JSON)** and select it from the list.

You may remove one or more undesired entries or simply remove everything from keybindings.json and type empty [] into it, then save.

***

## IntelliSense

[IntelliSense is a general term for various code editing features](https://code.visualstudio.com/docs/editor/intellisense)

### Suggestion widget

Hit **Ctrl + Space** to trigger IntelliSense suggestion widget.

***

## Hide terminal

To toggle terminal view:

```txt
Ctrl + J
```

***

## Hide file explorer

To toggle terminal view:

```txt
Ctrl + B
```

***

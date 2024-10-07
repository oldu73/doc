# WSL - 01 - Misc

Windows Subsystem for Linux

***

## Explorer

[SRC](https://www.howtogeek.com/426749/how-to-access-your-linux-wsl-files-in-windows-10/)

Windows explorer to retrieve files from WSL (terminal), type in:

```console
explorer.exe .
```

From Windows explorer address bar (path), type in:

```text
\\wsl$
```

***

## Copy result of command in clipboard

To get result of a console command copied into the clipboard, add pipe + clip.exe to command (be aware that there might be a "return" character in the result):

```console
echo Hello, world! | clip.exe
```

***

# IntelliJ - 01 - Misc

***

## Setup project in WSL folder

Fix EOL (LF vs CRLF) issue:

- git clone  
- select root project folder  
- menu File - File Properties - Line Separators - LF Unix  
- git rollback entire project to reset CRLF issue

[SRC](https://www.jetbrains.com/help/idea/configuring-line-endings-and-line-separators.html)

OR

Terminal in a new "native" WSL folder, clone and [fix CRLF issue](https://stackoverflow.com/questions/9976986/force-lf-eol-in-git-repo-and-working-copy):

- git clone project_url  
- git config core.eol lf  
- git config core.autocrlf input

***

## Service window

For Docker connection, maybe not opened if comes from external project.

To open it manually:

```text
View | Tool Windows | Services or Alt+8
```

[SRC](https://www.jetbrains.com/help/idea/services-tool-window.html)

***

## Search for ; and replace with ;\r\n

For a file where each values are separated with ';' but need each values on a new
line to compare files.

replace ; enable regex by ;\r\n

To revert, do it in Notepad++ with a record sequence

***

## Caret Cloning

SRC: - [IntelliJ IDEA Tips & Tricks: Multiple Cursors](https://www.vojtechruzicka.com/intellij-idea-tips-tricks-multiple-cursors/#:~:text=This%20feature%20can%20be%20toggled,%2B%20%E2%8C%98%20%2B%208%20on%20Mac.)

```text
Alt + Shift + Insert to switch to column mode

Then Shift + Up/Down Arrow(s)
```

***

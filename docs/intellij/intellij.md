# Intellij

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

- git clone ptoject_url  
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

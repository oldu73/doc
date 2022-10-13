# misc

***

## Installation

```console
pip install mkdocs
```

check

```console
mkdocs --version
```

***

## Initialize current folder

```console
mkdocs new .
```

***

## Build documentation

```console
mkdocs build
```

***

## Deploy to github

First synchronize current folder with corresponding github repository.

```console
mkdocs gh-deploy
```

***

## Material for MkDocs (theme)

### Set up

```console
pip install mkdocs-material
```

### Configuration

Simply add the following lines to mkdocs.yml to enable the theme.

Also feature for code block annotation and extension setup.

```yaml
theme:
  name: material
  features:
    - content.code.annotate

markdown_extensions:
  - pymdownx.highlight:
      anchor_linenums: true
  - pymdownx.inlinehilite
  - pymdownx.snippets
  - pymdownx.superfences
```

***

## Link

A double dash section title like below

\## Node server project

Should be referenced like below to be used in a link:  

\#node-server-project

To link this section from another markdown file:  

\[Node server project\](otherFile.md#node-server-project)

***

## VS Code Extension

Usefull markdown editing VS Code extensions:  

- [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one)  
- [markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint)

Markdown All in One allow code autocompletion for inserting code block by hitting 'Ctrl+space' shortcut keys.

***

## Fix warning

[When published on github, fix warning issue](https://patchstack.com/articles/website-flagged-malware-google/)

***

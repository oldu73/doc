# MkDocs

***

## Installation

```
$ pip install mkdocs
```

check

```
$ mkdocs --version
```

***

## Initialize current folder

```
$ mkdocs new .
```

***

## Build documentation

```
$ mkdocs build
```

***

## Deploy to github

First synchronize current folder with corresponding github repository.

```
$ mkdocs gh-deploy
```

***

## Material for MkDocs (theme)

### Installation

```
$ pip install mkdocs-material
```

### Configuration

Simply add the following lines to mkdocs.yml to enable the theme.

```
theme:
  name: material
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

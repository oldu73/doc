# debug

---

## Article width

Article content width is greater than display.

To help finding issue, right click on page and inspect.

In `Styles` window click on `+` then click on `inspector-stylesheet` to add style with:

```txt
* {
  outline: 1px solid #f00 !important;
}
```

Source: - [Find element that is causing the showing of horizontal scrollbar in Google Chrome](https://stackoverflow.com/questions/31458477/find-element-that-is-causing-the-showing-of-horizontal-scrollbar-in-google-chrom)

Edit `index.scss` as follow:

```scss
...
.content {
  ...
  width: 100vw;
  .articles-container {
    ...
    .article {
      ...
      .article-content {
        max-width: 100%;
        white-space: pre-line;
        overflow: scroll;
      }
      ...
    }
  }
}

```

Source: - [How to limit max width and height to screen size in CSS](https://stackoverflow.com/questions/36253760/how-to-limit-max-width-and-height-to-screen-size-in-css)

---

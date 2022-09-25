# 119-layout-setup

Setting up layout for the main page, locations for the topbar, footer and main content of the page

---

## html

index.html:

```html
...
<body>
  <div class="container">
    <header>header</header>
    <div class="content">content</div>
    <footer>footer</footer>
  </div>
</body>
...
```

---

## sass

styles.scss:

```scss
...
.container {
  min-height: 100vh;
  display: grid;
  grid:
    "header" auto
    "content" 1fr
    "footer" auto /
    auto;
}

header {
  grid-area: header;
  background: green;
  padding: 20px;
}

.content {
  grid-area: content;
  background: red;
  padding: 20px;
}

footer {
  grid-area: footer;
  background: #333;
  padding: 20px;
}
```

---

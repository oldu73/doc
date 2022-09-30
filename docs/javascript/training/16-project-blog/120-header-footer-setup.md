# 120-header-footer-setup

Three elements, logo, link to welcome page and link to article creation page in header and only copyright mention in footer.

---

## html

index.html:

```html
...
<body>
  <div class="container">
    <header>
      <a class="header-brand" href="./index.html">Blog</a>
      <ul>
        <li>
          <a href="./index.html" class="header-nav active">Welcome</a>
        </li>
        <li>
          <a href="./form.html" class="header-nav">Article creation</a>
        </li>
      </ul>
    </header>
    <div class="content">content</div>
    <footer>© blog 2022</footer>
  </div>
</body>
...
```

---

## sass

_variables.scss:

```scss
:root {
  --primary: #2ecc71;
  --dark: #27ae60;
  --accent: #2c3e50;
  --text: #333;
  --divider: #ecf0f1;
  --dark-gey: #324355;
  --font-family: "Muli", sans-serif;
  --box-shadow: 0 1px 2px 0 rgba(60,64,67,.3),0 1px 3px 1px rgba(60,64,67,.15)
}
```

styles.scss:

```scss
...
header {
  grid-area: header;
  background: var(--dark);
  padding: 20px;
  display: flex;
  flex-flow: row nowrap;
  justify-content: space-between;
  align-items: center;
  a {
    color: white;
  }
  .header-brand {
    font-size: 4rem;
    font-weight: 700;
  }
  ul {
    display: flex;
    li {
      .header-nav {
        font-size: 1.8rem;
        padding: 0 10px;
      }
      .active {
        font-weight: 700;
        text-decoration: underline;
      }
    }
  }
}
...
footer {
  grid-area: footer;
  padding: 2rem;
  background: var(--dark-gey);
  font-size: 1.8rem;
  text-align: center;
  color: white;
}
...
```

---

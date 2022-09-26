# 121-form-create-article

Set up of form to create article.

---

## form.js

Import global styles in `form.js`:

```js
import "../assets/styles/styles.scss";
import "./form.scss";

console.log("form");
```

---

## form.html

Html setup for form page, `form.html`:

```html
<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Mulish:wght@300;400;700&display=swap"
      rel="stylesheet"
    />
    <title>Form</title>
  </head>
  <body>
    <div class="container">
      <header>
        <a class="header-brand" href="./index.html">Blog</a>
        <ul>
          <li>
            <a href="./index.html" class="header-nav">Welcome</a>
          </li>
          <li>
            <a href="./form.html" class="header-nav active">Article creation</a>
          </li>
        </ul>
      </header>
      <div class="content">
        <form>
          <h2 class="title-underline">Add new article</h2>
          <div class="form-group">
            <label>Author</label>
            <input type="text" name="author" />
          </div>
          <div class="form-group">
            <label>Category</label>
            <input type="text" name="category" />
          </div>
          <div class="form-group">
            <label>Article</label>
            <textarea name="content"></textarea>
          </div>
          <div class="form-btn-container">
            <button class="btn btn-secondary">Cancel</button>
            <button class="btn btn-primary">Save</button>
          </div>
        </form>
      </div>
      <footer>© blog 2022</footer>
    </div>
  </body>
</html>
```

`active` class is set on `Article creation` link, this way we know where we are.

---

## form.scss

Own style for the form defined in the file `form.scss`:

```scss
.content {
  display: flex;
  justify-content: center;
  align-items: center;
  form {
    width: 700px;
    padding: 4rem;
    box-shadow: var(--box-shadow);
    border-radius: 3px;
    display: flex;
    flex-direction: column;
    .form-group {
      margin-bottom: 2rem;
      display: flex;
      flex-direction: column;
      label {
        font-size: 1.8rem;
        color: var(--accent);
        font-weight: 700;
        margin-bottom: 1rem;
      }
      input,
      textarea {
        border: 1px solid var(--divider);
        padding: 1rem;
        outline: 0;
        border-radius: 3px;
        &:focus {
          border-color: var(--accent);
        }
      }
      textarea {
        min-height: 20rem;
      }
    }
    .form-btn-container {
      display: flex;
      justify-content: flex-end;
      align-content: center;
      .btn {
        margin-left: 0.5rem;
      }
    }
  }
}
```

---

## _classes.scss

We add some generique classes commonly used in partial, file _classes.scss:

```scss
.title {
  &-underline {
    padding-bottom: 2rem;
    border-bottom: 1px solid var(--divider);
  }
}

.btn {
  border: 0;
  border-radius: 3px;
  padding: 1rem 2rem;
  font-weight: 700;
  cursor: pointer;
  transition: box-shadow 0.2s;
  &:hover {
    box-shadow: var(--box-shadow);
    transition: box-shadow 0.2s;
  }
  &-primary {
    background: var(--primary);
    color: white;
  }
  &-secondary {
    border: 1px solid var(--primary);
    background: white;
    color: var(--primary);
  }
}
```

---

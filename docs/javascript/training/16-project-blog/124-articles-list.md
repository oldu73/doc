# 124-articles-list

Design setup on how to render article list to home page.

---

## index.html

```html
...
<div class="content">
  <div class="articles-container">
    <div class="article">
      <img src="./assets/images/73.jpg" alt="profile" />
      <h2>Article title</h2>
      <p class="article-author">John Doe</p>
      <p class="article-content">
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Magni autem,
        porro dicta dolores beatae dolor officia accusantium, esse dolore
        pariatur ad tempore, aperiam odit? Placeat doloremque, voluptatum
        provident praesentium minus reiciendis fuga magni. Incidunt reiciendis
        corrupti consectetur nihil aperiam! Repudiandae qui sint rem inventore
        modi impedit, provident eaque consequatur deserunt?
      </p>
      <div class="article-actions">
        <button class="btn btn-delete">Delete</button>
        <button class="btn btn-primary">Edit</button>
      </div>
    </div>
  </div>
</div>
...
```

---

## index.scss

```scss
.content {
  display: flex;
  justify-content: center;
  align-items: center;
  .articles-container {
    max-width: 800px;
    width: 100%;
    margin: 5rem 0 10rem 0;
    .article {
      background: white;
      padding: 0 5rem;
      border-radius: 0.3rem;
      display: flex;
      margin-bottom: 5rem;
      flex-direction: column;
      align-items: center;
      box-shadow: var(--box-shadow);
      img {
        height: 6rem;
        width: 6rem;
        border-radius: 50%;
        margin-top: -3rem;
      }
      h2 {
        margin-top: 2rem;
        margin-bottom: 0;
      }
      .article-author {
        color: var(--hint);
        margin-bottom: 3rem;
      }
      .article-content {
        max-width: 550px;
      }
      .article-actions {
        width: 100%;
        display: flex;
        justify-content: flex-end;
        padding: 2rem 0;
        margin-top: 2rem;
        border-top: 1px solid var(--divider);
        .btn {
          margin-left: 1rem;
        }
      }
    }
  }
}
```

---

## _variables.scss

```scss
...
--hint: #95a5a6;
...
```

---

## styles.scss

```scss
...
.content {
  ...
  background: var(--divider);
}
...
```

---

## form.scss

```scss
.content {
  ...
  form {
    background: white;
    ...
  }
  ...
}
...
```

---

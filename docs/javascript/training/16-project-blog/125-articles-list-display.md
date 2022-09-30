# 125-articles-list-display

---

## form.html

We add two new properties to article, `title` and `img` for author profil image.

```html
...
<div class="content">
  <form>
  ...
    <div class="form-group">
      <label>Profile Picture</label>
      <input type="text" name="img" />
    </div>
    ...
    <div class="form-group">
      <label>Title</label>
      <input type="text" name="title" />
    </div>
  ...
  </div>
</form>
...
```

---

## form.js

Validation of those two new fields.

```js
...
const formIsValid = (article) => {
  errors = [];
  if (
    !article.author ||
    !article.category ||
    !article.content ||
    !article.img ||
    !article.title
  )
  ...
};
```

---

## index.html

Remove hard coded article because the list will be then created from JavaScript

```html
...
<body>
  <div class="container">
    ...
    <div class="content">
      <div class="articles-container"></div>
    </div>
    ...
  </div>
</body>
...
```

---

## _classes.scss

New class for delete button.

```scss
...
.btn {
  ...
  &-delete {
    border: 1px solid var(--error);
    background: white;
    color: var(--error);
  }
}
...
```

---

## index.scss

Add a `CSS` property to allow new lines to be correctly interpreted in `textarea` of article content.

```scss
.content {
  ...
  .articles-container {
    ...
    .article {
      ...
      .article-content {
        ...
        white-space: pre-line;
      }
      ...
    }
  }
}
```

---

## index.js

To handle display of articles list.

```js
import "./assets/styles/styles.scss";
import "./index.scss";

console.log("index.js");

const articleContainerElement = document.querySelector(".articles-container");

const createArticles = (articles) => {
  const articlesDOM = articles.map((article) => {
    const articleDOM = document.createElement("div");
    articleDOM.classList.add("article");
    articleDOM.innerHTML = `
<img
  src="${article.img}"
  alt="profile"
/>
<h2>${article.title}</h2>
<p class="article-author">${article.author} - ${article.category}</p>
<p class="article-content">
  ${article.content}
</p>
<div class="article-actions">
  <button class="btn btn-delete" data-id=${article._id} >Delete</button>
</div>
`;
    return articleDOM;
  });
  articleContainerElement.innerHTML = "";
  articleContainerElement.append(...articlesDOM);
};

const fetchArticle = async () => {
  try {
    const response = await fetch("https://restapi.fr/api/article");
    const articles = await response.json();
    createArticles(articles);
  } catch (e) {
    console.log("e : ", e);
  }
};

fetchArticle();
```

---

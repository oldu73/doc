# 142-sort-by-date

Menu to sort articles by date.

Trigger a dedicated call to server API.

---

## index.html

Add select list in welcome page to choose a sort order in file index.html:

```html
...
<!DOCTYPE html>
<html lang="fr">
  <head>
    ...
  </head>
  <body>
    <div class="container">
      <header>...</header>
      <div class="content">
        <div class="sidebar">
          <select>
            <option value="desc">most recent</option>
            <option value="asc">most old</option>
          </select>
          <ul class="categories"></ul>
        </div>
        <div class="articles-container"></div>
      </div>
      <footer>© blog - 2022</footer>
    </div>
  </body>
</html>
```

---

## index.scss

Edit style sheet for select list in index.scss:

```scss
@import "./assets/styles/media-queries";

.content {
  ...
  .sidebar {
    flex: 0 0 25rem;
    padding: 0 2rem 2rem 2rem;
    select {
      width: 100%;
      margin-bottom: 2rem;
      border-radius: 0.3rem;
      background: white;
      border: 0;
      outline: 0;
      padding: 1rem;
      box-shadow: var(--box-shadow);
    }
    ...
  }
  ...
}

```

---

## index.js

Editing the index.js file to handle sort feature.

First add a sort variable:

```js
let sortBy = "desc";
```

By default, articles are sorted from newer to older.

Get reference to select list:

```js
const selectElement = document.querySelector("select");
```

Then listen `change` event triggered when an element from the list is selected to change sort order:

```js
selectElement.addEventListener("change", () => {
  sortBy = selectElement.value;
  fetchArticle();
});
```

In `fetchArticle()` function we gonna use value of `sortBy`:

```js
const fetchArticle = async () => {
  try {
    const response = await fetch(
      `https://<URL>/.../articles/${sortBy}/`,
      {
        method: "GET",
      }
    );
    let content = await response.json();
    // articles = content.body.Items;
    articles = content.body;
    // Standard api behavior return not an array if only one element
    // so below code convert it (one element) into an array
    // otherwise it cause an error when 'map' method (to create articles) will be called on it.
    if (!Array.isArray(articles)) {
      articles = [articles];
    }
    createArticles();
    createMenuCategories();
  } catch (e) {
    console.log("e : ", e);
  }
};
```

Server do the sort through two new resources, `https://.../articles/<asc || desc>/`

Notice also answer is now in `content.body` not anymore in `content.body.Items`.

Rest to add `active` class on selected category if a filter exist, when creating and the display of categories menu:

```js
const displayMenuCategories = (categoriesArr) => {
  const liElements = categoriesArr.map((categoryElem) => {
    const li = document.createElement("li");
    li.innerHTML = `${categoryElem[0]} ( <strong>${categoryElem[1]}</strong> )`;
    if (categoryElem[0] === filter) {
      li.classList.add("active");
    }
    ...
  });
  ...
};
```

---

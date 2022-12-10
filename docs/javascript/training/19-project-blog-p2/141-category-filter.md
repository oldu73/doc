# 141-category-filter

Several changes to make the category filter work when clicking on a category.

---

## index.js

Create an `articles` global (accessible from everywhere in index.js) variable to store all articles and to not http request each time a filter is selected:

```js
...
let articles;
...
```

Then `articles` argument when calling `createArticles()` and `createMenuCategories()` functions isn't needed any more:

```js
...
const createArticles = () => {
}
...
const createMenuCategories = () => {
}
...
```

For the filter, create a `filter` variable that we simply gonna use with `filter()` method on the articles array at articles elements creation stage on the DOM:

```js
...
let filter;
...
const createArticles = () => {
  const articlesDOM = articles
    .filter((article) => {
      if (filter) {
        return article.category === filter;
      } else {
        return true;
      }
    })
...
};
...
```

If filter is set we return the filtered array by selected category, otherwise we return the entire array with `true` for each elements.

Then we add a listener on each `li` list element of the categories menu:

```js
...
const displayMenuCategories = (categoriesArr) => {
  const liElements = categoriesArr.map((categoryElem) => {
    const li = document.createElement("li");
    li.innerHTML = `<li>${categoryElem[0]} ( <strong>${categoryElem[1]}</strong> )</li>`;
    li.addEventListener("click", () => {
      if (filter === categoryElem[0]) {
        filter = null;
        li.classList.remove("active");
      } else {
        filter = categoryElem[0];
        liElements.forEach((li) => {
          li.classList.remove("active");
        });
        li.classList.add("active");
      }
      createArticles();
    });
    return li;
  });
  ...
};
...
```

If a category is selected we check that there's no filter. If this is the case, we check that the filter is the same as the one the user clicked on, and in this case we remove the filter and `active` class.

Else, filter si defined as the clicked category and we remove `active` class to all categories, and afterwards we apply `active` class to selected category.

Finally, we sort categories alphabetically when creating the menu:

```js
...
const createMenuCategories = () => {
...
  const categoriesArr = Object.keys(categories)
    .map((category) => {
      return [category, categories[category]];
    })
    .sort();
...
};
...
```

---

## index.scss

We simply add `active` class for selected filter:

```scss
...
.content {
  ...
  .sidebar {
    ...
    .categories {
      ...
      li {
        ...
      }
      .active {
        color: var(--primary);
        font-weight: 700;
        strong {
          color: var(--primary);
        }
      }
    }
  }
...
}
...
```

---

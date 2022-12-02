# 140-menu-categories-dynamic

Dynamic categories creation for articles categories sidebar menu.

---

## index.html

We start by removing html elements that will be dynamically created in file index.html:

```html
...
<html lang="fr">
  ...
  <div class="content">
    <div class="sidebar">
      <ul class="categories"></ul>
    </div>
    ...
  </div>
  ...
</html>
```

---

## index.js

After fetching articles from server, generate dynamically categories menu content in file index.js:

```js
...
const categoriesContainerElement = document.querySelector(".categories");
...
const displayMenuCategories = (categoriesArr) => {
  const liElements = categoriesArr.map((categoryElem) => {
    const li = document.createElement("li");
    li.innerHTML = `<li>${categoryElem[0]} ( <strong>${categoryElem[1]}</strong> )</li>`;
    return li;
  });
  categoriesContainerElement.innerHTML = "";
  categoriesContainerElement.append(...liElements);
};

const createMenuCategories = (articles) => {
  const categories = articles.reduce((acc, article) => {
    if (acc[article.category]) {
      acc[article.category]++;
    } else {
      acc[article.category] = 1;
    }
    return acc;
  }, {});

  const categoriesArr = Object.keys(categories).map((category) => {
    return [category, categories[category]];
  });
  displayMenuCategories(categoriesArr);
};

const fetchArticle = async () => {
  try {
    ...
    createMenuCategories(articles);
  } catch (e) {
    ...
  }
};
...
```

Edit `fetchArticle()` function to call `createMenuCategories()` function.

`createMenuCategories()` function get fetched articles and start by transforming them in object with `reduce()` method.

This object keys are categories and values are number of articles by category.

Then this object is transformed as an array with with first index, the name of the category and second, the number of articles by category.

This array is passed to `displayMenuCategories()` function which will create a `<li>` html element for each categories contained in the array.

Each `<li>` html element contain category name and articles number that belong to this category.

Finally we `append()` elements on container with `categories` class for which we have the reference `const categoriesContainerElement`.

---

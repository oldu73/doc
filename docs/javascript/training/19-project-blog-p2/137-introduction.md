# 137-introduction

Project blog, part 2, presentation.

[Dyma.fr Source code on GitHub](https://github.com/dymafr/javascript-chapitre19-projet_blog-partie2)  
[OLDU Source code on GitHub](https://github.com/oldu73/blog-001)

---

## Goals

Goals:

- Improve article creation (redirection, date, cancel)
- Article edit
- Creation of a category menu
- Filter by category
- Sort by date with a select list
- Popup for deletion

---

## Redirection

In create article form, after click on save or cancel.

### After save

If response code < 299 (means it's ok) redirect to index.

form.js:

```js
...
form.addEventListener("submit", async (event) => {
  ...
  try {
    if (formIsValid(article)) {
      ...
      const response = await fetch(
        ...
      );
      if (response.status < 299) {
        location.assign("/index.html");
      }
    }
  }
  ...
});
...
```

### After cancel

Add types to buttons in form.html:

```html
...
<form>
  ...
  <div class="form-btn-container">
    <button type="button" class="btn btn-secondary">Cancel</button>
    <button type="submit" class="btn btn-primary">Save</button>
  </div>
</form>
...
```

By default, button in a form has submit action.

By defining types, only submit button will have submit action, on click.

...

---

## Date

In article list, display date instead of category.

Edit `index.js` as follow:

```js
const createArticles = (articles) => {
  const articlesDOM = articles.map((article) => {
    ...
    articleDOM.innerHTML = `
...
<p class="article-author">${article.author} - ${(new Date(article.createdAt)).toLocaleDateString("fr-CH", {
  weekday: 'long',
  day: '2-digit',
  month: 'long',
  year: 'numeric'
})}</p>
...
`;
    return articleDOM;
  });
  ...
};
```

---

## data attribute

[data-*](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/data-*#:~:text=Voir%20aussi-,data-*,peut%20manipuler%20avec%20des%20scripts.) are global attributes to exchange information between `HTML` and its `DOM`.

---

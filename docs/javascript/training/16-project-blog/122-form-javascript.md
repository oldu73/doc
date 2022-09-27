# 122-form-javascript

Form management by JavaScript.

---

## _classes.scss

Text style for error message in file _classes.scss:

```scss
...
.text {
  &-error {
    color: var(--error);
  }
}
...
```

---

## _variables.scss

Variable color for error message in file _variables.scss:

```scss
:root {
  ...
  --error: #e74c3c;
  ...
}
```

---

## form.html

Html to handle errors in file form.html:

```html
<div class="content">
  <form>
    ...
    <ul class="text-error" id="errors"></ul>
    ...
  </form>
</div>
```

---

## form.js

JavaScript to handle form in file form.js:

```js
import "../assets/styles/styles.scss";
import "./form.scss";

console.log("form.js");

const form = document.querySelector("form");
const errorElement = document.querySelector("#errors");
let errors = [];

form.addEventListener("submit", (event) => {
  event.preventDefault();
  const formData = new FormData(form);
  const article = Object.fromEntries(formData.entries());
  if (formIsValid(article)) {
    const json = JSON.stringify(article);
    // fetch
  }
});

const formIsValid = (article) => {
  errors = [];
  if (!article.author || !article.category || !article.content) {
    errors.push("All fields must be filled!");
  }
  if (article.content.length < 20) {
    errors.push("Article too short!");
  }
  if (errors.length) {
    let errorHtml = "";
    errors.forEach((e) => {
      errorHtml += `<li>${e}<\li>`;
    });
    errorElement.innerHTML = errorHtml;
    return false;
  } else {
    errorElement.innerHTML = "";
    return true;
  }
};
```

---

# 126-delete-article

---

## index.js

We only have to take care of JavaScript for welcome page.

On every article we have added `data-id=${article._id}` that allow to get access on `DOM` to `id` of each article set by database that is unique.

Create a reference to all delete buttons with `querySelectorAll`.

Go through every buttons to add a listener on `click` event.

If a click occure target button will execute his event handler function and we know which article is concerned with `event.target.dataset.id`.

All that remain to do is to send a `DELETE` request to the server to delete article.

Once server answerd and everything OK we request updated articles list.

```js
import { async } from "regenerator-runtime";
...
const createArticles = (articles) => {
  ...
  const deleteButtons = articleContainerElement.querySelectorAll(".btn-delete");
  console.log(deleteButtons);
  deleteButtons.forEach((button) => {
    button.addEventListener("click", async (event) => {
      try {
        const target = event.target;
        const articleId = target.dataset.id;
        console.log(articleId);
        const response = await fetch(
          `https://restapi.fr/api/article/${articleId}`,
          {
            method: "DELETE",
          }
        );
        const body = await response.json();
        console.log(body);
        fetchArticle();
      } catch (e) {
        console.log("e: ", e);
      }
    });
  });
};
...
```

---

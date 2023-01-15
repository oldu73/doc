# 144-modal-logic

Add logic to handle modal process.

---

## index.js

Make `createModal()` function return a promise so we will use `await`.

Obviously it's necessary to transform the event handler for the click to an asynchronous function to allow using `await`.

File index.js:

```js
...
import { openModal } from "./assets/javascripts/modal";
...
const createArticles = () => {
  ...
  deleteButtons.forEach((button) => {
    button.addEventListener("click", async (event) => {
      const result = await openModal(
        "Are you sure you want to delete your article?"
      );
      if (result === true) {
        try {
          const target = event.target;
          const articleId = target.dataset.id;
          const articleToDelete = new Object();
          articleToDelete.id = articleId;
          const json = JSON.stringify(articleToDelete);
          const response = await fetch(
            `https://<api url>`,
            {
              method: "DELETE",
              body: json,
              headers: {
                "Content-Type": "application/json",
              },
            }
          );
          const body = await response.json();
          fetchArticle();
        } catch (e) {
          console.log("e: ", e);
        }
      }
    });
  });
};
...
```

---

## modal.js

We return a new promise for our modal resolved when either user:

- click on the layer or cancel the promise is resolved to `false`.
- click on confirm button and then the promise is resolved to `true`.

We also add an event listener on modal to avoid closing when click on it by stopping event propagation (from modal to layer).

File modal.js:

```js
const body = document.querySelector("body");
let calc;
let modal;
let cancel;
let confirm;

const ctreateCalc = () => {
  calc = document.createElement("div");
  calc.classList.add("calc");
};

const createModal = (question) => {
  modal = document.createElement("div");
  modal.classList.add("modal");
  modal.innerHTML = `
  <p>${question}</p>
  `;
  cancel = document.createElement("button");
  cancel.innerText = "Cancel";
  cancel.classList.add("btn", "btn-secondary");
  confirm = document.createElement("button");
  confirm.innerText = "Confirm";
  confirm.classList.add("btn", "btn-primary");
  modal.addEventListener("click", (event) => {
    event.stopPropagation();
  });
  modal.append(cancel, confirm);
};

export function openModal(question) {
  ctreateCalc();
  createModal(question);
  calc.append(modal);
  body.append(calc);

  return new Promise((resolve, reject) => {
    calc.addEventListener("click", () => {
      resolve(false);
      calc.remove();
    });

    cancel.addEventListener("click", () => {
      resolve(false);
      calc.remove();
    });

    confirm.addEventListener("click", () => {
      resolve(true);
      calc.remove();
    });
  });
}
```

---

## form.js

We also use the modal in article creation form to avoid losing changes by mistake.

File form.js

```js
...
import { openModal } from "../assets/javascripts/modal";
...
btnCancel.addEventListener("click", async () => {
  const result = await openModal(
    "If you leave the page, you will lose your article"
  );

  if (result) {
    location.assign("/index.html");
  }
});
...
```

---

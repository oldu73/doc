# 143-modal-setup

Setup of the modal that we may reuse at many places.

---

## modal.js part 1

Create a new js file in folder `/src/assets`.

For now, we simply export the function:

```js
export function openModal(question) {}
```

---

## index.js

Import the function.

We could have created a `chunck` (to optimize sharing code caching by the browser) but this method is simply faster and amount of code isn't so huge.

We then use the function when user click on delete button, for now:

```js
...
import { openModal } from "./assets/javascripts/modal";
...
const createArticles = () => {
  ...
  deleteButtons.forEach((button) => {
    button.addEventListener("click", async (event) => {
      openModal("Are you sure you want to delete your article?");
      // if (result === true) {
      //   try {
      //     const target = event.target;
      //     const articleId = target.dataset.id;
      //     const articleToDelete = new Object();
      //     articleToDelete.id = articleId;
      //     const json = JSON.stringify(articleToDelete);
      //     const response = await fetch(
      //       `https://<api url>`,
      //       {
      //         method: "DELETE",
      //         body: json,
      //         headers: {
      //           "Content-Type": "application/json",
      //         },
      //       }
      //     );
      //     const body = await response.json();
      //     fetchArticle();
      //   } catch (e) {
      //     console.log("e: ", e);
      //   }
      // }
    });
  });
};
...
```

---

## modal.js part 2

The modal funstion start by creating a semi opaque layer.

Then the modal is created and append to the layer.

Finally the layer is append to the `body`.

In the `createCalc()`, we simply create a `div` to which we add then a specific class for the layer.  
We also add a listener to close it on click.

In the `createModal()` function we create a `div` with a `modal` class.  
We insert the question passed as argument and add two action buttons: - one to cancel and the other to confirm.

File modal.js:

```js
const body = document.querySelector("body");
let calc;
let modal;

const ctreateCalc = () => {
  calc = document.createElement("div");
  calc.classList.add("calc");
  calc.addEventListener("click", () => {
    calc.remove();
  });
};

const createModal = (question) => {
  modal = document.createElement("div");
  modal.classList.add("modal");
  modal.innerHTML = `
  <p>${question}</p>
  `;
  const cancel = document.createElement("button");
  cancel.innerText = "Cancel";
  cancel.classList.add("btn", "btn-secondary");
  const confirm = document.createElement("button");
  confirm.innerText = "Confirm";
  confirm.classList.add("btn", "btn-primary");
  modal.append(cancel, confirm);
};

export function openModal(question) {
  ctreateCalc();
  createModal(question);
  calc.append(modal);
  body.append(calc);
}
```

---

## Partial _classes.scss

We add style for the two new classes.

The layer should fill the entire screen, black with 50% opacity.

We make it a flex container centering its flex element (modal) vertically and horizontally.

File _classes.scss:

```scss
...
.calc {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  .modal {
    background: white;
    min-width: 30rem;
    border-radius: 3px;
    padding: 5rem;
    box-shadow: var(--box-shadow);
    text-align: center;

    p {
      margin: 1rem 0 3rem 0;
    }
    .btn {
      margin-left: 0.5rem;
    }
  }
}
```

---

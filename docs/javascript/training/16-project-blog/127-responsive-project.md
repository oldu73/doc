# 127-responsive-project

Adapt the project for mobile, by creating, among other things, a mobile menu in `JavaScript`.

---

## index.scss

We set a maximum width and a width of 100% so that the width of the content fits the screen.

We use an `@inclide xs` media query to decrease padding on mobile.

index.scss:

```scss
@import "./assets/styles/media-queries";

.content {
  ...
  .articles-container {
    max-width: 800px;
    width: 100%;
    ...
    .article {
      ...
      @include xs {
        padding: 0 2rem;
      }
      ...
}
```

---

## form.scss

For the width on `form.scss` we make the same modifications as for `index.scss`.

form.scss:

```scss
.content {
  ...
  form {
    ...
    max-width: 700px;
    width: 100%;
    ...
}

```

---

## index.html

We add `fonteawsome` in `index.html` to use hamburger icon (mobile menu).

We also add a container for the header menu and for the icon.

index.html:

```html
...
<html>
  <body>
    ...
      <header>
        ...
        <div class="header-menu">
          ...
          <div class="header-menu-icon">
            <i class="fas fa-bars"></i>
          </div>
        </div>
      </header>
      ...
</html>

```

## styles.scss

We make the list of links disappear with a media query on mobile.

The default header-menu-icon is not displayed and we only display it on mobile with a media query as well.

For the mobile menu, we do not display it by default. Only when we add a specific class in `JavaScript`.

styles.scss:

```scss
...
header {
  ...
  .header-menu {
    position: relative;
    ul {
      @include xs {
        display: none;
      }
      ...
    .header-menu-icon {
      display: none;
      font-size: 3rem;
      color: white;
      @include xs {
        display: block;
      }
    }
    .mobile-menu {
      display: none;
      position: absolute;
      box-shadow: var(--box-shadow);
      top: 9.5rem;
      right: 1rem;
      padding: 3rem 1.5rem;
      width: 20rem;
      background: white;
      ul {
        display: block;
        li {
          margin: 2rem 0;
          a {
            color: var(--text);
          }
        }
      }
    }
    .mobile-menu.open {
      display: block;
...
```

---

## topbar.js

Manage to display the top bar menun in `JavaScript`.

topbar.js:

```js
console.log("topbar.js");

const iconMobile = document.querySelector(".header-menu-icon");
const headerMenu = document.querySelector(".header-menu");
let isMenuOpen = false;
let mobileMenuDom;

const closeMenu = () => {
  mobileMenuDom.classList.remove("open");
};

const createMobileMenu = () => {
  mobileMenuDom = document.createElement("div");
  mobileMenuDom.classList.add("mobile-menu");
  mobileMenuDom.addEventListener("click", (event) => {
    event.stopPropagation();
  });
  mobileMenuDom.append(headerMenu.querySelector("ul").cloneNode(true));
  headerMenu.append(mobileMenuDom);
};

const openMenu = () => {
  if (mobileMenuDom) {
  } else {
    createMobileMenu();
  }
  mobileMenuDom.classList.add("open");
};

const toggleMobileMenu = (event) => {
  console.log(event);
  if (isMenuOpen) {
    closeMenu();
  } else {
    openMenu();
  }
  isMenuOpen = !isMenuOpen;
};

iconMobile.addEventListener("click", (event) => {
  event.stopPropagation();
  toggleMobileMenu();
});

window.addEventListener("click", () => {
  if (isMenuOpen) {
    toggleMobileMenu();
  }
});

window.addEventListener("resize", (event) => {
  console.log(window.innerWidth);
  if (window.innerWidth > 480 && isMenuOpen) {
    toggleMobileMenu();
  }
});

```

## form.html

Edit `form.html` to follow menu modifications.

form.html:

```html
<html>
  <head>
    ...
    <script
      src="https://kit.fontawesome.com/671ba50d24.js"
      crossorigin="anonymous"
    ></script>
    ...
  </head>
  <body>
    ...
      <header>
        ...
        <div class="header-menu">
          ...
          <div class="header-menu-icon">
            <i class="fas fa-bars"></i>
          </div>
        </div>
      </header>
      ...
  </body>
</html>

```

---

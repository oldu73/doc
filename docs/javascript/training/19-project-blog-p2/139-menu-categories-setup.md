# 139-menu-categories-setup

Setup of sidebar menu for articles categories.

---

## index.html

We start with html structure in file index.html:

```html
...
<html lang="fr">
  ...
  <div class="content">
    <div class="sidebar">
      <ul class="categories">
        <li>History ( <strong>3</strong> )</li>
        <li>Space ( <strong>4</strong> )</li>
        <li>Tech ( <strong>2</strong> )</li>
      </ul>
    </div>
    ...
  </div>
  ...
</html>
```

We add a side bar with which contain a none ordered list.

For now it's hard coded to work on `CSS` but then it will be dynamically displayed by `JavaScript`.

---

## index.scss

Adapt style to handle categories in index.scss:

```scss
...
.content {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding-top: 5rem;
  width: 100vw;
  .sidebar {
    flex: 0 0 25rem;
    padding: 0 2rem 2rem 2rem;
    .categories {
      padding: 2rem;
      background: white;
      border-radius: 0.3rem;
      box-shadow: var(--box-shadow);
      li {
        margin: 1rem;
        transition: color 0.2s;
        cursor: pointer;
        &:hover {
          color: var(--primary);
          strong {
            color: var(--primary);
          }
        }
      }
    }
  }
...
```

Alignement on cross vertical axis for class `content` is modified by using `align-items: flex-start`.

Define then `sidebar` class that to prevent `grow` or `shrink` with `flex: 0 0 25rem`.

Finally property `flex` of `articles-container` is set to allow `grow` and `shrink` depending on the space available.

---

## _base.scss

Modifying the partial `SCSS` for color of strong element in file _base.scss:

```scss
...
strong {
  color: var(--accent);
}
```

---

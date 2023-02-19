# 24-setup-header-footer

---

## Font Awesome

Setup [Font Awesome](https://fontawesome.com/) for a `React` project from [source](https://stackoverflow.com/questions/23116591/how-to-include-a-font-awesome-icon-in-reacts-render):

Npm instal font awesome:

```console
npm i --save @fortawesome/fontawesome
npm i --save @fortawesome/react-fontawesome
npm i --save @fortawesome/fontawesome-free-solid
npm i --save @fortawesome/fontawesome-free-regular
npm i --save @fortawesome/fontawesome-svg-core
```

Insert kit in `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    ...
    <script
      src="https://kit.fontawesome.com/<your kit>.js"
      crossorigin="anonymous"
    ></script>
    <title>...</title>
  </head>
  <body>
    ...
  </body>
</html>

```

Import icon in `JS` file:

```js
...
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faBars } from "@fortawesome/fontawesome-free-solid";

function Header() {
  return (
    <header className={`${...} ...`}>
      <i>
        <FontAwesomeIcon icon={faBars} />
      </i>
      ...
    </header>
  );
}

export default Header;

```

---

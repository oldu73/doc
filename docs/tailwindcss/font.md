# Font

---

## Inspect

How to see which font is rendered, with chrome DevTools open click on element, click on computed tab and scroll to bottom to see `Rendered Fonts` section.

Expand 'font-family' section to observe default (system or browser) font used.

---

## Setup project

Setup project to use custom fonts.

In [Google Fonts](https://fonts.google.com/), as an example select `Roboto`:

- Regular 400  
- Medium 500  
- Bold 700  
- Black 900

Copy code to Embed code in the `<head>` of your html:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700;900&display=swap" rel="stylesheet">
```

Edit `src\style.css` file by adding a base section as follow:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  html {
    font-family: "Roboto", sans-serif;
  }
}
```

Now with simple below content in your `index.html` file you may observe in DevTools window (as explained in `Inspect` section) that `Roboto` as been applied to paragraph:

```html
<!doctype html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700;900&display=swap"
      rel="stylesheet"
    />
    <link href="./output.css" rel="stylesheet" />
  </head>
  <body class="h-screen p-12">
    <p>
      Lorem ipsum, dolor sit amet consectetur adipisicing elit. Vitae harum
      nesciunt assumenda dolor laborum, architecto et ratione eligendi, ipsum
      voluptatum delectus laboriosam, voluptatem unde error! Molestias
      perferendis earum ipsam debitis.
    </p>
  </body>
</html>
```

---

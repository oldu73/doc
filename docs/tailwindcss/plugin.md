# plugin

---

## Official

Official Tailwind CSS plugins:

- **typography** stylized content from db, text mixed with not rendered html markup [tailwindcss-typography](https://github.com/tailwindlabs/tailwindcss-typography)  
- **form** minimal style for form (otherwise reset) [tailwindcss-forms](https://github.com/tailwindlabs/tailwindcss-forms)  
- **iframe** video ratio [tailwindcss-aspect-ratio](https://github.com/tailwindlabs/tailwindcss-aspect-ratio)  
- **container-queries** provides utilities for container queries [tailwindcss-container-queries](https://github.com/tailwindlabs/tailwindcss-container-queries)

---

## Install

```console
npm i -D @tailwindcss/typography @tailwindcss/forms @tailwindcss/aspect-ratio @tailwindcss/container-queries
```

tailwind.config.js:

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{html, js}"],
  theme: {
    extend: {},
  },
  plugins: [
    require("@tailwindcss/typography"),
    require("@tailwindcss/forms"),
    require("@tailwindcss/aspect-ratio"),
    require("@tailwindcss/container-queries"),
  ],
};
```

---

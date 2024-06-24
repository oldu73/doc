# Theme

---

## Colors

With chosen color hex code, e.g. for let's say "my" slate = '#708090'.

Browse to [Tailwind CSS Color Generator](https://uicolors.app/create) and paste above code in main field.

Click on 'Export' to get all of your slate variant to import in your tailwind config:

```json
'slate-gray': {
        '50': '#f5f8f8',
        '100': '#ecf2f3',
        '200': '#dde5e8',
        '300': '#c7d5da',
        '400': '#b0c0c9',
        '500': '#9bacb9',
        '600': '#8496a7',
        '700': '#708090',
        '800': '#5d6976',
        '900': '#4e5861',
        '950': '#2e3438',
    },
```

Now, in your project open file `tailwind.config.js` and paste above code as follow:

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{html,js}"],
  darkMode: "class",
  theme: {
    extend: {
      colors: {
        "slate-gray": {
          50: "#f5f8f8",
          100: "#ecf2f3",
          200: "#dde5e8",
          300: "#c7d5da",
          400: "#b0c0c9",
          500: "#9bacb9",
          600: "#8496a7",
          700: "#708090",
          800: "#5d6976",
          900: "#4e5861",
          950: "#2e3438",
        },
      },
    },
  },
  plugins: [],
};
```

Now this new color is available in your design class, e.g.:

```html
<ul class="text-slate-gray-700">
```

---

## Tip

In `extend` section of file `tailwind.config.js`, hit `ctrl + space` to get list of extendable properties.

---

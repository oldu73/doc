# Basic Layout

Sample Flex Layout:

- [Course](https://dyma.fr/developer/list/v2/chapters/core/65a65add44e8654ec711a4bb/65b54a314e18ba51f2be8206/lesson)
- [Layout](https://docs.google.com/presentation/d/18gwoc5qUWWNZISe4NSFnWoZKT575Dktg5EzbJlr-naU/edit?usp=sharing)

## Standard

code:

```html
<!doctype html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link href="./../output.css" rel="stylesheet" />
    <title>Basic Layout (source from course)</title>
  </head>
  <body class="flex min-h-screen flex-col bg-slate-100">
    <header class="flex basis-14 bg-red-500 p-2">
      <div class="basis-52 bg-gray-500"></div>
      <ul class="flex flex-auto justify-end gap-x-2 bg-teal-500">
        <li class="basis-36 bg-orange-500"></li>
        <li class="basis-36 bg-orange-500"></li>
        <li class="basis-36 bg-orange-500"></li>
      </ul>
    </header>
    <main class="flex flex-auto bg-blue-500">
      <aside class="flex basis-48 flex-col gap-y-2 bg-green-500 p-2">
        <div class="basis-12 bg-sky-500"></div>
        <div class="basis-12 bg-sky-500"></div>
        <div class="basis-12 bg-sky-500"></div>
      </aside>
      <section
        class="flex flex-auto flex-wrap content-start items-start gap-2 bg-amber-500 p-2"
      >
        <div class="h-48 basis-48 bg-fuchsia-500"></div>
        <div class="h-48 basis-48 bg-fuchsia-500"></div>
        <div class="h-48 basis-48 bg-fuchsia-500"></div>
        <div class="h-48 basis-48 bg-fuchsia-500"></div>
        <div class="h-48 basis-48 bg-fuchsia-500"></div>
        <div class="h-48 basis-48 bg-fuchsia-500"></div>
        <div class="h-48 basis-48 bg-fuchsia-500"></div>
        <div class="h-48 basis-48 bg-fuchsia-500"></div>
        <div class="h-48 basis-48 bg-fuchsia-500"></div>
        <div class="h-48 basis-48 bg-fuchsia-500"></div>
        <div class="h-48 basis-48 bg-fuchsia-500"></div>
        <div class="h-48 basis-48 bg-fuchsia-500"></div>
        <div class="h-48 basis-48 bg-fuchsia-500"></div>
        <div class="h-48 basis-48 bg-fuchsia-500"></div>
        <div class="h-48 basis-48 bg-fuchsia-500"></div>
      </section>
      <aside class="basis-48 bg-green-500"></aside>
    </main>
    <footer class="basis-14 bg-lime-500"></footer>
  </body>
</html>
```

---

## Improved

`Header` text centered on parent section = `<header>`, really centered on center of the view port, not on the remaining space between logo and header menus.

It also take into account the content width for the centering.

code:

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link href="./../output.css" rel="stylesheet" />
    <title>Improved Basic Layout</title>
  </head>
  <body class="flex min-h-screen flex-col bg-slate-100">
    <header class="relative flex basis-14 bg-red-500 p-2">
      <div class="basis-52 bg-gray-500 p-2">Logo</div>
      <h1
        class="absolute left-1/2 top-1/2 -translate-x-1/2 -translate-y-1/2 transform text-white"
      >
        Header
      </h1>
      <ul class="flex flex-auto justify-end gap-x-2 bg-teal-500">
        <li class="basis-36 bg-orange-500 p-2">header menu #1</li>
        <li class="basis-36 bg-orange-500 p-2">header menu #2</li>
        <li class="basis-36 bg-orange-500 p-2">header menu #3</li>
      </ul>
    </header>

    <main class="flex flex-auto bg-blue-500">
      <aside class="flex basis-48 flex-col gap-y-2 bg-green-500 p-2">
        <div class="basis-12 bg-sky-500 p-2">side menu #1</div>
        <div class="basis-12 bg-sky-500 p-2">side menu #2</div>
        <div class="basis-12 bg-sky-500 p-2">side menu #3</div>
      </aside>
      <section
        class="flex flex-auto flex-wrap content-start justify-center gap-2 bg-amber-500 p-2"
      >
        <div class="h-48 basis-48 bg-fuchsia-500 p-2 text-sm">card #01</div>
        <div class="h-48 basis-48 bg-fuchsia-500 p-2 text-sm">card #02</div>
        <div class="h-48 basis-48 bg-fuchsia-500 p-2 text-sm">card #03</div>
        <div class="h-48 basis-48 bg-fuchsia-500 p-2 text-sm">card #04</div>
        <div class="h-48 basis-48 bg-fuchsia-500 p-2 text-sm">card #05</div>
        <div class="h-48 basis-48 bg-fuchsia-500 p-2 text-sm">card #06</div>
        <div class="h-48 basis-48 bg-fuchsia-500 p-2 text-sm">card #07</div>
        <div class="h-48 basis-48 bg-fuchsia-500 p-2 text-sm">card #08</div>
        <div class="h-48 basis-48 bg-fuchsia-500 p-2 text-sm">card #09</div>
        <div class="h-48 basis-48 bg-fuchsia-500 p-2 text-sm">card #10</div>
        <div class="h-48 basis-48 bg-fuchsia-500 p-2 text-sm">card #11</div>
        <div class="h-48 basis-48 bg-fuchsia-500 p-2 text-sm">card #12</div>
        <div class="h-48 basis-48 bg-fuchsia-500 p-2 text-sm">card #13</div>
        <div class="h-48 basis-48 bg-fuchsia-500 p-2 text-sm">card #14</div>
      </section>
      <aside class="basis-48 bg-green-500"></aside>
    </main>
    <footer
      class="flex basis-14 items-center justify-center bg-lime-500 text-xs"
    >
      &copy; 2024 footer
    </footer>
  </body>
</html>
```

---

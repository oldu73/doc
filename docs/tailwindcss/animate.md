# animate

---

Below some samples to illustrate animate with either, SVG, Bootstrap or Heroicons:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta
      name="viewport"
      content="width=device-width,
    initial-scale=1.0"
    />
    <link href="./output.css" rel="stylesheet" />
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css"
    />
  </head>
  <body class="min-h-screen p-8">
    <div class="flex w-1/2 flex-col gap-y-10">
      <button
        class="rounded bg-sky-500 px-4 py-2 text-white hover:shadow-xl hover:shadow-green-500"
      >
        <div class="flex">
          <i class="bi bi-arrow-clockwise mr-2 animate-spin"></i>
          'animate-spin' bootstrap CDN icon in div flex
        </div>
      </button>

      <button
        type="button"
        class="flex rounded bg-sky-500 px-4 py-2 text-white hover:shadow-xl hover:shadow-green-500"
        disabled
      >
        <svg
          class="... mr-3 h-5 w-5 animate-spin"
          xmlns="http://www.w3.org/2000/svg"
          width="16"
          height="16"
          fill="currentColor"
          class="bi bi-arrow-clockwise"
          viewBox="0 0 16 16"
        >
          <path
            fill-rule="evenodd"
            d="M8 3a5 5 0 1 0 4.546 2.914.5.5 0 0 1 .908-.417A6 6 0 1 1 8 2z"
          />
          <path
            d="M8 4.466V.534a.25.25 0 0 1 .41-.192l2.36 1.966c.12.1.12.284 0 .384L8.41 4.658A.25.25 0 0 1 8 4.466"
          />
        </svg>
        'animate-spin' bootstrap SVG icon direct in index.html
      </button>

      <button
        class="rounded bg-sky-500 px-4 py-2 text-white hover:shadow-xl hover:shadow-green-500"
      >
        <div class="flex gap-2">
          <img src="./icon/arrow-path.svg" alt="" class="w-5 animate-spin" />
          <span>'animate-spin' heroicons SVG img icon from external file</span>
        </div>
      </button>

      <button
        class="relative rounded bg-sky-500 px-4 py-2 text-white hover:shadow-xl hover:shadow-green-500"
      >
        <i class="bi bi-circle-fill absolute -top-2 right-0 animate-ping"></i>
        'animate-ping' bootstrap CDN icon
      </button>

      <button
        class="animate-pulse rounded bg-sky-500 px-4 py-2 text-white hover:shadow-xl hover:shadow-green-500"
      >
        'animate-pulse'
      </button>

      <button
        class="rounded bg-sky-500 px-4 py-2 text-white hover:shadow-xl hover:shadow-green-500"
      >
        <div>'animate-bounce' bootstrap CDN icon</div>
      </button>
      <i
        class="bi bi-arrow-down -top-2 right-0 animate-bounce text-sky-500"
      ></i>
    </div>
  </body>
</html>
```

---

# flex

---

## Alignment

Main principle:

- justify = main axis content  
- content = secondary axis content  
- items = secondary axis items  
- self =  = secondary axis items

```html
<!doctype html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link href="./output.css" rel="stylesheet" />
  </head>
  <body class="min-h-screen">
    <div
      class="flex h-screen flex-row flex-wrap content-start items-end bg-slate-800"
    >
      <div class="m-1 h-28 basis-28 bg-blue-700">1</div>
      <div class="m-1 h-14 basis-28 self-start bg-blue-700">2</div>
      <div class="m-1 h-28 basis-28 bg-blue-700">3</div>
      <div class="m-1 h-32 basis-28 bg-blue-700">4</div>
    </div>
  </body>
</html>
```

---

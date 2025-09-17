# figma

---

## measure

select and hold alt key

---

## scale

to preserve proportionality (e.g. for stroke width) use scale tool instead of directly doing it with selection tool

---

## history

click on file name in top bar and select 'Show version history'

---

## spell

install desired language to Windows, then it becomes available in Figma icon -> Text -> Spell heck tool

---

## zoom

top right corner '##%' menu -> Zoom ...

Zoom to 100% `Ctrl+0`

Zoom to fit `Shift+1`

'Shift+1' only center layout but do not zoom to fit window

---

## goto selected frame with shortcut

Shift + 2

---

## auto layout

### padding

`Ctrl+click` on padding value to change left/right/top/down values at once

### alignment

double click on blue selected alignment mode to auto space (Gap)

### no auto layout

to have an "object" belonging to an auto layout frame but not following the auto layout behavior (alignment rules), hold `s` key while dropping the object in the frame

---

## open overlay

In Figma, when you create a prototype with the "Open overlay" interaction, you need to pay attention to the position of the overlay object.

If the overlay object is inside the same frame as the trigger element, Figma may treat it as part of the normal layout and it won’t appear properly as an overlay.

To make the overlay work as expected, the overlay object should be either:

- outside of the frame that contains the trigger element, or  
- not inside any frame (placed directly on the page).

In short: Do not put the overlay inside the same frame as the trigger element.

---

## reverse mocking

From `path` to `svg`.

On need of a vector design from existing web page:

1. Right click inspect existing web page  
2. Select element to inspect  
3. Double click in `path` field to select (only content in between double quotes) and copy  
4. Paste content to [svg-path-editor](https://yqnn.github.io/svg-path-editor/) `PATH` field  
5. Click on `Download as SVG`  
6. You may either download svg file to store it for further usage or directly `Copy to clipboard` and paste it in your figma design

E.g. of rocket icon path:

```path
M6 15c-.83 0-1.58.34-2.12.88C2.7 17.06 2 22 2 22s4.94-.7 6.12-1.88A2.996 2.996 0 0 0 6 15zm.71 3.71c-.28.28-2.17.76-2.17.76s.47-1.88.76-2.17c.17-.19.42-.3.7-.3a1.003 1.003 0 0 1 .71 1.71zm10.71-5.06c6.36-6.36 4.24-11.31 4.24-11.31S16.71.22 10.35 6.58l-2.49-.5a2.03 2.03 0 0 0-1.81.55L2 10.69l5 2.14L11.17 17l2.14 5 4.05-4.05c.47-.47.68-1.15.55-1.81l-.49-2.49zM7.41 10.83l-1.91-.82 1.97-1.97 1.44.29c-.57.83-1.08 1.7-1.5 2.5zm6.58 7.67-.82-1.91c.8-.42 1.67-.93 2.49-1.5l.29 1.44-1.96 1.97zM16 12.24c-1.32 1.32-3.38 2.4-4.04 2.73l-2.93-2.93c.32-.65 1.4-2.71 2.73-4.04 4.68-4.68 8.23-3.99 8.23-3.99s.69 3.55-3.99 8.23zM15 11c1.1 0 2-.9 2-2s-.9-2-2-2-2 .9-2 2 .9 2 2 2z
```

---

## horizontal scroll

To scroll horizontally `Shift + mouse scroll wheel`.

---

## direct select nested things

To avoid having to double-click a lot until you reach the level of the nested thing to select, press the `Ctrl` key and on hover, the selectable item becomes highlighted, then click on it.

---

## paste to replace

To paste to replace:

1. Select and copy an object using the keyboard shortcut: `Ctrl+c`.  
2. Select the objects you'd like to replace with the copied object.  
3. Right-click your selection and click `Paste to replace` from the menu. You can also use the keyboard shortcut: `Ctrl+Shift+r`.

---

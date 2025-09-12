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

## Open Overlay

In Figma, when you create a prototype with the "Open overlay" interaction, you need to pay attention to the position of the overlay object.

If the overlay object is inside the same frame as the trigger element, Figma may treat it as part of the normal layout and it won’t appear properly as an overlay.

To make the overlay work as expected, the overlay object should be either:

- outside of the frame that contains the trigger element, or  
- not inside any frame (placed directly on the page).

In short: Do not put the overlay inside the same frame as the trigger element.

---

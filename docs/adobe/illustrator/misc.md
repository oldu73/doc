# Illustrator Misc

---

## Split compound path

Choose Object > Compound Path > Release

---

## Align on key object

If you want to align two objects in Adobe Illustrator, but keep one of those objects in place, then you need a KEY OBJECT. Select both objects, and then simply single click the object you want to stay put. This'll add a thicker selection to that object and when you align, your original object will stay in place!

---

## Find object in a layer

Find in which layer an object is.

Window - Layers menu, select an object, click the magnifying glass (Research object) at the bottom of the layers window.

---

## Transformation with scaling

Transformation, scaling, maintaining proportion, rounding radius, outlines and effects.

Open the Transform panel (Window > Transform) and check the Rounding radius checkbox and the Scale strokes and effects checkbox as well.

---

## Arrow

Arrow symbol at the end of a line.

`Window` -> `Stroke`, `Show Options` or click `Stroke` in the `Properties` panel.

---

## Shadow

New: - Effect header menu - Special - Drop shadow.

Edit an existing shadow effect: - Click twice on its name in the Aspect panel.

---

## Screen shot to SVG

Maybe used for (to build then) icon (in react/nextjs project or other).

How to:

1. `Win + Shift + S` to capture screen region  
2. Open Illustrator, create new file (A4), paste in it.  
3. Menu `Fenêtre` - `Véctorisation de l'image` - `Vectoriser`  
4. Menu `Objet` - `Décomposer` - unselect `Fond` then click `OK`  
5. Maybe right click and then select `Simplifier`  
6. Menu `Objet` - `Tracé` - `Décalage`= 0  
7. In `Calques` tool palette, select the Group, and then at the bottom of the tool palette, `Créer masque d’écrêtage`  
8. In `Calques` tool palette, hide `Masque` and `Tracé transparent` calques and select calque `Ecrêter le groupe`  
8. `Fichier` - `Exporter` - `Exporter pour les écrans...`, choose `Plans de travail`, Format = `SVG` and then `Exporter le plan de travail`  
9. Et voilà, a ready to use clean svg file to build an icon

---

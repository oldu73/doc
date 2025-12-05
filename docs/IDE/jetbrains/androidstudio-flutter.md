# androidstudio flutter

---

## Formatting code

Install `Flutter` and `Dart` plugin.

Follow [format code on save](https://docs.flutter.dev/development/tools/formatting) instructions.

---

## Wrap

Select code hit `alt + enter` and wrap to what you want.

---

## Autocomplete

### state less widget

To create a state less widget template, in a dart file type:

```txt
stless + tab
```

It create basic code structure for a state less widget:

```dart
class  extends StatelessWidget {
  const ({super.key});

  @override
  Widget build(BuildContext context) {
    return const Placeholder();
  }
}
```

---

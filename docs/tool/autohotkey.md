# autohotkey

[Web site](https://www.autohotkey.com/).

---

## Remapping Keys

[Documentation](https://www.autohotkey.com/docs/v2/misc/Remap.htm).

To simplify `{}` and `[]`, for a developer mapped keyboard, in a file with `.ahk` extension placed in `C:\Users\<user name>\Documents\AutoHotkey` folder:

```txt
à::Send {{}
$::Send {}}
è::Send {[}
SC01B::]
F12::ExitApp
```

Double click on script to launch it.

Hit `F12` to quit the script.

---

## History of pressed keys

To get get key codes, in a file with `.ahk` extension placed in `C:\Users\<user name>\Documents\AutoHotkey` folder:

```txt
#InstallKeybdHook
KeyHistory
```

Double click on script to launch it.

---

# Windows - 01 - Misc

---

## Find process that uses a port

Open CMD prompt as admin

```console
C:\> netstat -ano -p tcp | find "50307"
```

Last column shows the PID that uses the port

```console
C:\> netstat --help

-a Displays all connections and listening ports.

-n Displays addresses and port numbers in numerical form.

-o Displays the owning process ID associated with each connection.
```

In PowerShell

```console
C:\> Get-Process -Id (Get-NetTCPConnection -LocalPort 50307).OwningProcess
```

---

## Grep equivalent

find "50307" or findstr 50307

```console
C:\> netstat -ano -p tcp | findstr 50307
```

You can find the application based on the PID on the Processes tab in Windows Task Manager.

[netstat](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/netstat)

---

## Kill process

```console
C:\> taskkill /F /PID pid_number
```

---

## Unable to bind port

[SRC](https://stackoverflow.com/questions/15619921/an-attempt-was-made-to-access-a-socket-in-a-way-forbidden-by-its-access-permissi)

Windows Power Shell in admin mode:

```txt
Restart-Service hns
```

---

## Path

PATH shortcut:

```txt
Windows key + type "path" in search field
```

---

## Voice input

To dictate text into a field, hit:

```txt
Windows + H
```

To change field focus:

```txt
Windows + Alt + H
```

---

## Terminal

### Split

Hit `ctrl + shift + p` and search for `split`.

---

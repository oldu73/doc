# Windows - 01 - Misc

***

## find process that uses a port

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

***

## grep equivalent

find "50307" or findstr 50307

```console
C:\> netstat -ano -p tcp | findstr 50307
```

***

## kill process

```console
C:\> taskkill /F /PID pid_number
```

***

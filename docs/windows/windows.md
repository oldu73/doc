# Windows

***

## find process that uses a port

Open CMD prompt as admin

```
C:\> netstat -ano -p tcp | find "50307"
```

Last column shows the PID that uses the port

```
C:\> netstat --help

-a Displays all connections and listening ports.

-n Displays addresses and port numbers in numerical form.

-o Displays the owning process ID associated with each connection.
```

In PowerShell

```
C:\> Get-Process -Id (Get-NetTCPConnection -LocalPort 50307).OwningProcess
```

***

## grep equivalent

find "50307" or findstr 50307

```
C:\> netstat -ano -p tcp | findstr 50307
```

***

## kill process

```
C:\> taskkill /F /PID pid_number
```

***

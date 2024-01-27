# Windows - 02 - Container

On Docker

Ref. [Get started: Run your first Windows container](https://learn.microsoft.com/en-us/virtualization/windowscontainers/quick-start/run-your-first-container)

---

## Setup

Install and run [Docker Desktop](https://www.docker.com/) for Windows (WSL2, requirements, etc.)

Right click Docker whale icon in the system tray and switch to 'Windows containers...'

Open a command prompt and type command:

```console
docker pull mcr.microsoft.com/windows/nanoserver:ltsc2022
```

Check that is image is there on local:

```console
docker images
```

Run a Windows container:

```console
docker run -it mcr.microsoft.com/windows/nanoserver:ltsc2022 cmd.exe
```

Inside container console:

```console
echo "Hello, world!" > Hello.txt
type Hello.txt
exit
```

There you go ;)

---

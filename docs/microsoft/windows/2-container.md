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

## Windows Server

Apparently it's a bit more complicated on Windows Server, but not tried yet.

Here are some article references that treat this particular topic:

- [Docker Containers on Windows Server 2022 101](https://www.dell.com/support/kbdoc/en-us/000201261/docker-containers-on-windows-server-2022-101)  
- [Get started: Prep Windows for containers](https://learn.microsoft.com/en-us/virtualization/windowscontainers/quick-start/set-up-environment?tabs=dockerce#windows-server-1)  
- [How to Configure and Run Docker on Windows Server](https://operavps.com/docs/configure-docker-on-windows-server/)

---

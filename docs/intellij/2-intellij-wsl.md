# IntelliJ - 02 - WSL

IntelliJ integration with a project hosted in a WSL2 native folder (not /mnt/c)

***

## Test Maven

To test that maven is correctly installed and running in Linux distribution.  
Setup a new maven project with a simple 'hello' class to test debug/run.

***

## Firewall rules

To allow IntelliJ to communicate with WSL.

SRC: - [set IntelliJ rules to communicate with WSL](https://www.jetbrains.com/help/idea/how-to-use-wsl-development-environment-in-product.html#debugging_system_settings)

PowerShell (admin mode):

```text
New-NetFirewallRule -DisplayName "WSL" -Direction Inbound  -InterfaceAlias "vEthernet (WSL)"  -Action Allow
Get-NetFirewallRule | where DisplayName -ILike "idea*.exe" | Remove-NetFirewallRule
```

***

## CRLF issue

After cloning a git repository maybe all files are marked as modified due to CRLF miss understanding between Windows and Linux file system.

SRC: - [fix CRLF issue](https://stackoverflow.com/questions/9976986/force-lf-eol-in-git-repo-and-working-copy)

From a terminal in a native (not /mnt/c) WSL folder:

```console
git clone <url>
git config core.eol lf
git config core.autocrlf input
```

***

## IntelliJ recipe

At least release "IntelliJ IDEA 2021.3.1 (Community Edition)"

Open existing project:

```text
- set project SDK with the one from WSL
- set Maven home (IDE settings, search for maven) to : "\\wsl$\<linux distribution>\usr\share\maven"
- invalidate cache and restart
```

Note: - original Maven setting for Windows = C:\Program Files\JetBrains\IntelliJ IDEA Community Edition 2021.3.1\plugins\maven\lib\maven3

***

## Fix IntelliJ

Fix IntelliJ to disable one wsl experimental feature.

SRC: - [fix IntelliJ](https://youtrack.jetbrains.com/issue/IDEA-286059)

```text
- press Shift-Shift to open "Search everywhere"
- enter "Experimental features" and select the action
- in the dialog that appears, turn off wsl.fsd.content.loader
- restart the IDE
```

***

## Fix Docker

For Docker Compose to prepare context with Docker Desktop for Windows.

Enable the **Docker Compose V2** option under the experimental settings.

SRC: - [fix Docker](https://stackoverflow.com/questions/68945027/wsl2-docker-compose-unable-to-prepare-context)

***

## Fix Logback

In case project fail to start due to Logback issue, "Failed to create parent directories" var log.

Manually create needed log folder for project and set rights accordingly:

```console
sudo mkdir /var/log/<needed project folder name for log>
sudo chmod a+rwx /var/log/<needed project folder name for log>
```

***

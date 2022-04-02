# Git

***

## Initialize

Local settings:

```console
git config user.name "Your Name"
git config user.email "youremail@domain.com"
```

### WSL Credential Manager setup

SRC: [microsoft](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-git)

```console
git config credential.helper "/mnt/c/Program\ Files/Git/mingw64/libexec/git-core/git-credential-manager-core.exe"
```

***

## Configuration

List configuration parameters:

```console
git config --list
```

Edit configuration parameters:

```console
git config --global --edit
or
git config --edit
```

***

## cache token

to cache token (use token instead of password) (for 15 minutes, by default).

SRC: [github](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token)

```console
git config credential.helper cache
```

or for 1 hour

```console
git config credential.helper 'cache --timeout=3600'
```

***

## SSH To HTTPS

Change remote URL from SSH To HTTPS.

[Source](https://docs.github.com/en/get-started/getting-started-with-git/managing-remote-repositories)

Check existing remote:

```console
git remote -v
```

Change your remote's URL from SSH to HTTPS with the git remote set-url command:

```console
git remote set-url origin https://github.com/USERNAME/REPOSITORY.git
```

***

## Many GitHub accounts

How to manage credentials for many different GitHub accounts.

[Source](https://stackoverflow.com/questions/25845963/git-credential-helper-update-password)

Configure credentials to use the full repository path:

```console
git config --global credential.useHttpPath true
```

***

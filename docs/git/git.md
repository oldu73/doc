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

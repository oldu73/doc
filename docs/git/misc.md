# misc

***

## Initialize

Set up a new project.

### Local, first step

```console
mkdir newproject-001
cd newproject-001
git init
touch .gitignore
git config user.email "john@doe.com"
git config user.name "John Doe"
git add .
git commit -m "initial commit"
```

### GitHub

Now go to your github account and create a new repository, e.g. "newproject-001".

### Local, second step

```console
git branch -M main
git remote add origin https://github.com/johndoe/newproject-001.git
git push -u origin main
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

### Global unset

Remove the line matching the key from config file, e.g. for `user.email`:

```console
git config --global --unset user.email
```

***

## Cache token

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

## Remove tracked folder

For files folder previously tracked and then added to .gitignore:

```console
git rm -r --cached <folder>
```

***

## Checkout remote branch

[Tutorial](https://www.freecodecamp.org/news/git-checkout-remote-branch-tutorial/)

### 1. Fetch all remote branches

`git fetch origin`

### 2. List the branches available for checkout

`git branch -a`

### 3. Pull changes from a remote branch

`git checkout -b local origin/remote`

***

## Cherry-pick without commit

[Tutorial](https://geekwebcast.com/git-cherry-picking-with-vs-code-part-1/)

`git cherry-pick <commit-hash> -n`

***

## Branch description

[Tutorial](https://stackoverflow.com/questions/11886132/can-i-add-a-message-note-comment-when-creating-a-new-branch-in-git)

### Set

`git branch --edit-description`

### Get

`git config branch.<branch>.description`

### Push description into merge commits

You can push this description into merge commits if you set:

`git config merge.branchdesc true`

This means when you issue `git merge --log <branch>`, it'll force the branch description into the stock merge commit message.

***

## Undo remote change(s)

Warning! To use only if you are working alone in repository!

```console
git reset --hard HEAD~<number of commit(s) before>
git push --force
```

Undo undo

```console
git pull
```

***

## Check Current Remotes

To see the list of remote repositories associated with your local repository, you can use the following command:

```console
git remote -v
```

***

## Find branch name

```console
git branch --list "*<searched text>*"
```

***

## Show diff

```console
git diff
```

***

## Show current branch

```console
git branch --show-current
```

***

## Git Fork Workflow

### 1. Fork a Repository

Forking a repository on GitHub creates a copy of the original repository in your own GitHub account, allowing you to work on it independently.

1. Go to the original repository on GitHub.
2. Click the **Fork** button (upper-right corner).
3. A copy of the repository will be created under your GitHub account.

### 2. Add `upstream` to Your Fork

After forking the repository, you can link it back to the original repository by adding an `upstream` remote. This will allow you to pull in new changes from the original repo.

```bash
# Add the original repository as a new remote called 'upstream'
git remote add upstream <original-repo-URL>
```

You can check your remotes with:

```bash
git remote -v
```

***

## Fetch Tags from the Original Repo

To get all tags from the original repository (upstream of forked repo), you need to fetch them. This will bring all tags from the original repo into your local copy.

```bash
# Fetch all tags from the upstream repository
git fetch upstream --tags
```

You can list all the tags with:

```bash
git tag
```

***

## Checkout a New Branch from a Tag

If you want to start working from a specific tag, you can create a new branch based on that tag.

```bash
# Create a new branch based on a specific tag
git checkout -b <new-branch-name> <tag-name>
```

For example:

```bash
git checkout -b feature-branch v1.0.0
```

This creates a new branch called `feature-branch` based on the `v1.0.0 tag`.

***

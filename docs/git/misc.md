# Git - Misc

### cache token

to cache token (use token instead of password) (for 15 minutes, by default).

src: [github](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token)

```
$ git config credential.helper cache
```

or for 1 hour

```
$ git config credential.helper 'cache --timeout=3600'
```

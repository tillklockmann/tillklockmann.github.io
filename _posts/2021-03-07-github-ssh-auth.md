---
layout: post
title: "github ssh keys authentication"
---

## Github Authentications with tokens

Github recently informed me after I had pushed into my repo that:

> Basic authentication using a password to Git is deprecated and will soon no longer work. Visit https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information around suggested workarounds and removal dates.

So, my solution was to use SSH keys. I managed by following [this](https://www.freecodecamp.org/news/git-ssh-how-to/) guide.

[To set up or change your git remote urls from https to ssh](https://docs.github.com/en/github/using-git/changing-a-remotes-url#switching-remote-urls-from-ssh-to-https):

```bash
git remote set-url origin <your-ssh-repo-url-name>
```
To check your remote urls: 

```bash
git remote -v
```

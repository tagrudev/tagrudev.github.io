---
title: "Git branches and fzf"
date: 2021-11-01 08:00
category: dev
tags: [git, fzf, web]
---

Recently, I had to clean up all my stale branches and since I am not really a big fan of a GUI tool for `git` I looked into an alternative ways of doing that.

In my ruby work I am mostly using VIM as a text editor and `fzf` is always there to help searching and selecting files - so I was wondering whether it could actually do the same for the git branches (since you can pipe anything to the `fzf` search).

Turns out a friend of mine had already figured it out (credit goes to @gmitrev):

```bash
git branch | fzf -m | xargs git branch -d
```

How does that work?

So, we pipe the output of `git branch` to `fzf` multi-select mode (you can mark lines with TAB or Shift-TAB for multiple lines), then we use `xargs` a great command that reads streams of data from standard input, then generates and executes command lines (meaning it can take output of a command and passes it as argument of another command). You can use `git branch -D` for branches that are not yet merged and of course you can add an alias for easy access of this command.

![](/assets/images/stalebranches.png)

Voilà! You can now manage your branches using `fzf`.

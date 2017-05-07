---
layout: episode
title: "Git rebase and commit squashing exercise"
teaching: 0
exercises: 15
questions:
  - How can we commit often and still arrive at an understandable history?
objectives:
  - Learn how to clean up branches.
---

## Git rebase and commit squashing exercise

### Objective

In this exercise we will practice how to squash incomplete commits into one
nice commit and replay it on top of the master branch.


### Motivation

This technique is useful in situations where you need to make changes to a pull
request before it can be integrated.


### Exercise

Start the exercise by forking and cloning [this repository](https://github.com/bast/git-rebase-squash-exercise).

The `haiku` branch represents a feature branch that is to be rebased (moved) and squashed.

On the `haiku` branch you find a script that prints a haiku:

```shell
$ git checkout haiku
$ python main.py

This is our haiku:

On a branch ...
                  by Kobayashi Issa

              On a branch
              floating downriver
              a cricket, singing.
```

The haiku is great but the
commit history on
the `haiku` branch is not (for the purpose of this exercise):

```shell
$ git log --oneline

65870f9 fix a copy-paste error
47a007d completed the haiku
a3278e3 another incomplete commit
54fba21 startign to work on it (commit with a typo)
3ff39a1 forgot to add a file
7e1f903 starting working on the haiku
c50a463 initial commit
```

Your task is to rebase the `haiku` branch on top
of `master` and squash the several small "incomplete" commits into one single
self-contained cherry-pickable commit.

In other words instead of this history:

![]({{ site.baseurl }}/img/rebase-exercise-1.svg)

We wish to first rebase the commits:

![]({{ site.baseurl }}/img/rebase-exercise-2.svg)

And in a second step squash the commits into one:

![]({{ site.baseurl }}/img/rebase-exercise-3.svg)

Verify the steps and the result with `git status` and `git log`.
Verify the history and also that the script still works after the operation.


### Hints

```shell
$ git rebase master         # rebases current branch on top of master
$ git reset --soft abc123   # move current branch back to commit abc123
                            # and stage all modifications after abc123
```

---
layout: episode
title: "Rebase vs. merge"
teaching: 10
exercises: 0
questions:
  - "What is the advantage of merging?"
  - "What is the advantage of rebasing?"
  - "How does rebasing affect testing?"
objectives:
  - "Obtain a mental representation of the rebase process."
---

## Rebase vs. merge

To illustrate rebasing we consider the following situation - we wish to merge
`master` into `devel`:

![]({{ site.baseurl }}/img/pre-rebase.svg)

Now you know how to do it:

```shell
$ git checkout devel
$ git merge master
```

This creates a merge commit:

![]({{ site.baseurl }}/img/git-branch-08.svg)


But there is an alternative:

```shell
$ git checkout devel
$ git rebase master
```

![]({{ site.baseurl }}/img/rebase.svg)

- `git rebase` replays the branch commits `b1` to `b3` on top of `master`.
- As if they were committed after `c5`.
- It changes history (notice that the commits `b1` to `b3` have been replaced by `b1*` to `b3*`).
- Discuss advantages and disadvantages.

### Advantages and disadvantages

- `git rebase` makes "merges" producing a linear history.
- `git rebase` may invalidate tests.
- When somebody asks you to rebase your work to the work of somebody else you know what this means.
- When working with others **do not rebase commits that you have shared**
  (history has changed).
- Reference: "Treehouse of Horror V: Time and Punishment", The Simpsons (1994).

![]({{ site.baseurl }}/img/simpsons.jpg)

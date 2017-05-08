---
layout: episode
title: "Avoiding conflicts"
teaching: 10
exercises: 0
questions:
  - How should we organize branches to avoid conflicts?
objectives:
  - Avoid conflicts, avoid double work, avoid losing patches.
---

## Avoiding conflicts

- Conflicts can be avoided if you think and talk with your colleagues before committing.
- Think and plan to which branch you will commit to.
- Fortran people: modifying common blocks often causes conflicts.
- Modifying global data often causes conflicts.
- Monolithic entangled spaghetti-code maximizes risk of conflicts.
- Modular programming minimizes risk of conflicts.
- Ball-of-mud branches for "everything" maximize risk of conflicts.
- One branch for one task only.
- Resolve conflicts early.
- If the branch affects code that is likely to be modified by others, the
  branch should:
  - be short-lived and/or merge often to the main development line
  - merge the main development line often to stay up-to-date

---

## Develop separate features on separate branches

Common situation:

- You create a branch for your new feature that you are working on.
- While working on your feature you discover a defect or bug that has nothing to do
  with your new feature.
- Or you see some ugly code and want to clean it up.
- You are a responsible developer and you do not want to leave this defect.
- You decide to fix this defect right on your new branch "while at it".
- This is probably a **bad idea** - why?

![]({{ site.baseurl }}/img/git-fix-1.svg)

- Reasoning
    - If you fix it on your branch other people might not see it.
    - You may want to merge it to `master` but you cannot since your new feature is not ready yet.
    - Perhaps somebody else will fix it on master in a different way and then it will conflict
      with your new feature.
    - Before you commit a change, think: "who needs this change?".
    - Based on the answer select the appropriate branch.
    - **In general apply changes as close to the trunk as possible.**
    - **Develop separate features on separate branches and be very strict and disciplined with this.**

- Better solution
    - Fix it on `master`, so other developers can see it.
    - Than merge it to your development/topic branch.

![]({{ site.baseurl }}/img/git-fix-2.svg)

- Developing separate features on separate branches minimizes conflicts.
- It makes branches shorter-lived.
- This again minimizes conflicts.

---

## Wrong branch - what now?

> *Help! I made a commit to the "wrong" branch and it is a public branch and I was told
> I should not edit public commits, what now?*

Solution: `git cherry-pick` the commit to the "right" branch:

![]({{ site.baseurl }}/img/git-fix-3.svg)

---

## Rewinding the local master branch

You made few commits to the *local* `master` branch.
You then realize that it broke some tests but you have no time now to fix them.
So you wish you had committed them to an experimental branch instead.

What now?

![]({{ site.baseurl }}/img/git-split-branch-1.svg)

You want to move last three commits to a separate branch.

First make sure that your working directory and index are empty.

Then create a new branch (e.g. `feature`):

![]({{ site.baseurl }}/img/git-split-branch-2.svg)

Now reset `master` back three commits:

```shell
$ git checkout master
$ git branch feature   # create feature branch but stay on master
$ git reset --hard c2  # on master
```

![]({{ site.baseurl }}/img/git-split-branch-3.svg)

Another job well done.
**However, this should not be done if the commits have already been shared with others.**

---

## How do we correct public commits?

- **Not** with `git reset` because we do not want to change the history for commits that others depend on.
- We use `git revert` which creates **new** commits that revert changes.
- `git revert` does not modify past commits.

---

## When is a good moment to merge?

- Feature branches merge to `master` typically once (at the end of their lifetime).
- But there may be good reasons for merging branches to `master` more often (release branch).
- Never merge a feature branch into another feature branch (branch pollution; discuss why).
- For modular projects with write-protected `master` and code review and very high discipline:
    - You typically should not merge `master` except the occasional `git cherry-pick`.
- For entangled projects where most of the development happens directly on `master`:
    - It is good to merge `master` to your topic branch often to stay in sync with main development line.
    - A Merge from `master` to your branch ideally should never conflict.
    - But it will sometimes, resolve conflicts early.

---

## Develop on feature branches

- Keeps bugs away from the main development line.
- Divide and conquer: do not create a branch for "everything".
- The more you do on one branch the longer it will take until you can reintegrate it.
- The more granular the branches, the shorter lived.
- Talk with your colleagues to avoid conflicts.

### How to test combinations of features?

- I develop `feature-a`.
- My colleague develops `feature-b`.
- Both are not ready yet to go into the main line.
- How can we test their combination?
- Do not cross-merge feature branches.
- Reason: if `feature-a` becomes ready, it cannot be integrated to the main line
  because it is then diluted with `feature-b`.
- It is easy to make soup out of vegetables, it is difficult to separate a vegetable out of a soup.
- Test combinations on integration branches.
- Integration branches only receive merges, we do not "work" on them.
- Same holds for testing combinations with the main line.
- The main line should ideally be an integration branch.

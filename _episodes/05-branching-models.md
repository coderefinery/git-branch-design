---
layout: episode
title: "Branching models"
teaching: 15
exercises: 0
questions:
  - "What is a good branch layout to simplify maintenance and release preparation?"
objectives:
  - "Discuss typical branch layouts."
---

## [Vincent Driessen model](http://nvie.com/posts/a-successful-git-branching-model/)


<img src="{{ site.baseurl }}/img/nvie-model.png" style="height: 600px;"/>

(c) Vincent Driessen, licensed under CC BY-SA.

- [Link to original post](http://nvie.com/posts/a-successful-git-branching-model/).
- Very popular and highly visible.
- Two long-lived branches: `develop` and `master`.
- New features are developed on feature branches.
- Feature branches branch off from `develop`.
- `master` is the latest stable release by definition.
- `master` holds all release versions.
- Everything merged to `develop` is ready to be released.


### Critique of this model

- Naming is unfortunate and often confuses coworkers (rename `develop` to `master` and `master` to `stable`).
- Model is not ideal if you need to support past versions and publish patches for past versions.
- Good if you do not distribute the stable release (e.g. if you run it on your servers).

---

## Alternative: separate branch for each major release

![]({{ site.baseurl }}/img/tree-model.svg)

- We recommend to name release branches e.g. `release-2.x` or `stable-2.x`.
- It is then crystal clear where the main development line is.
- Does not require to create new branches for patches of past versions.
- Good if you distribute code and support past versions.
- Patches need to be applied to the oldest supported release branch and cherry-picked or merged
  "up" to the main line and all supported release branches.
- We never merge `master` to release branches.
- The `master` branch ideally only receives merges and no direct commits.

---

## Alternative: separate branch for each minor release

- [https://github.com/robertodr/branching-model-discussion](https://github.com/robertodr/branching-model-discussion)
- API-preserving changes are submitted towards minor or major release branches
- API-breaking changes are submitted towards `master`

---

## Document and enforce your branch naming and strategies

- Document recommended branch naming.
- Document your branching layout/strategy.
- Use meaningful version numbers: e.g. [semantic versioning](http://semver.org).
- Require your developers to follow it (code review).
- Write-protect your main development line and release branch(es).

---

## Tag your releases

- Tags are also just pointers to commits.
- While branches are mutable, tags are (typically) immutable.
- Tags can carry extra annotation.
- Use annotated tags (then it is clear who created the tag).
- Always tag your releases:

```shell
$ git tag                            # list all tags
$ git tag -a v1.5 -m 'release v1.5'  # create annotated tag
$ git push origin v1.5               # share tag to upstream (origin)
$ git push origin --tags             # push all tags
```

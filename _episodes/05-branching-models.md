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


### Observations from real life use of this model

- If the project is typically cloned by users, default branch should be `master`
- If the project is typically cloned by developers, default branch should be `develop`
- If the project is typically cloned by developers and the default branch is `master`, then there is confusion and developers either complain about finding only old code or they commit to the wrong branch.
- Less confusing might be to rename `develop` to `master` and `master` to `stable`.
- Model is not ideal if you need to support past versions and publish patches for past versions.
- In the [Vincent Driessen model](http://nvie.com/posts/a-successful-git-branching-model/)
  every commit on the `master` branch is a new release by definition but publishing patches to
  past releases leads to release commits which are not on the `master` branch.
- The [Vincent Driessen model](http://nvie.com/posts/a-successful-git-branching-model/) model
  offers no protocol for discriminating feature pull requests (PRs) based on
  their target major or minor version. For maintainers it may therefore be
  difficult to accept an API-preserving feature PR after having accepted an
  API-breaking feature PR. For contributors it may be difficult to communicate
  the release visibility intent of a patch.
- Good if you do not distribute the stable release (e.g. if you run it on your servers).


---

## Semantic branching model

<img src="{{ site.baseurl }}/img/branching-model.svg" alt="Branching model" width="90%">

- Based on [semantic versioning](http://semver.org).
- [https://dev-cafe.github.io/branching-model/](https://dev-cafe.github.io/branching-model/)
- Good if you distribute code and support past versions.
- Separate development lines towards major, minor, or patch release.
- Communicate to contributors the meaning and effect of each branch.
- Communicate to maintainers the release visibility intent for each patch.
- Make it clear and simple to decide both for contributors and maintainers
  whether patches affect the next major, minor, or patch version.

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

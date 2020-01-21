---
title: 'Sanity checking your git commits'
author: 'Alexander Junge'
date: '2020-01-21'
slug: git-pre-commit-checks
categories:
  - tips
tags:
  - git
  - coding
---

Most projects I am working on enforce specific code styles, minimal requirements for documentation,
or other rules every code contribution needs to follow.
Git pre-commit hooks are very useful to automatically check that commits fulfill these
requirements.
This allows all project contributors to focus on the code instead of manually running different code formatters
or wasting time in unnecessary arguments over missing whitespace.

## Installing the pre-commit package manager

Pre-commit hooks are managed via the `pre-commit` package manager which can be
installed from PyPI:

```sh
pip install pre-commit
```

Alternative installation methods are described on the project's website: https://pre-commit.com/#install

### Setting up pre-commit hooks

I frequently use pre-commits for the [black](https://github.com/psf/black) code formatter and
[mypy](https://github.com/python/mypy) static type checker.
As an example, I will show how these tools can be installed as pre-commit hooks.

Create and open a file named `.pre-commit-config.yaml` in your local git repository
where you specify the black and mypy hooks:

```
repos:
  - repo: https://github.com/psf/black
    rev: stable
    hooks:
      - id: black
        language_version: python3.7 # adjust to your version

  - repo: https://github.com/pre-commit/mirrors-mypy
    rev: 'eef73e2'  # Use the sha / tag you want to point at
    hooks:
      - id: mypy
```

Finally install the hooks by executing:

```sh
pre-commit install
```

Everytime you run `git commit` both black and mypy will now be run against the files changed in the commit.
More hooks are available at: https://pre-commit.com/hooks.html

### Skipping git pre-commit hooks

Git allows you to skip all pre-commit hooks by adding the `-n`/`--no-verify` flag to
`git commit` (note: this will also skip any commit-msg hooks).
This is okay to do when you are in a hurry since any violations against the project's
rules should be called by downstream CI/CD checks anyway. But still: Don't overuse this.

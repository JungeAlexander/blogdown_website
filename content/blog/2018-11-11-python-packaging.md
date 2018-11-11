---
title: 'Packaging a Python library: first steps'
author: 'Alexander Junge'
date: '2018-11-11'
slug: python-packaging
categories:
  - python
tags:
  - packaging
  - software development
---

A central goal in the development of [CoCoScore](https://www.biorxiv.org/content/early/2018/10/16/444398) is to make it as easy as possible for new users to get start with context-aware co-occurrence text mining. So far, I recommended new users to set up a conda virtual environment to install CoCoScore and its dependencies. I now started working on releasing CoCoScore as a library on [PyPI](https://pypi.org/).

Releasing on PyPI will allow an installation via `pip` and make it even easier for new users to get started with CoCoScore because installing conda will not be needed anymore. You can follow my progress on wrapping up the PyPI release [in this pull request](https://github.com/JungeAlexander/cocoscore/pull/17).

Since I found packaging my first library for PyPI release far from trivial, I would like to share a handful of resources that helped me along the way:

- Mark Smith's [talk](https://2018.pycon-au.org/talks/44349-how-to-publish-a-package-on-pypi/) from pyconau 2018 on the topic
- [Cookiecutter template](https://github.com/ionelmc/cookiecutter-pylibrary) that saved me a lot of work
- [Introduction](https://blog.ionelmc.ro/2014/07/08/a-python-package-template/) to the above Cookiecutter template

Some of the main changes I made to the code base after going through these resources were:

- Switching from `cocoscore/` to a `src/cocoscore/` project layout (after reading [this](https://blog.ionelmc.ro/2014/05/25/python-packaging/))
- Switched from unittest-based to pytest-based code testing
- Using tox to run the test suite against multiple versions of Python and dependencies

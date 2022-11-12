---
title: 'Keeping sensitive information out of Jupyter notebooks stored in git version control'
author: 'Alexander Junge'
date: '2022-11-12'
slug: sensitive-info-out-of-jupyter-notebooks
categories:
  - tips
  - data
  - privacy
tags:
  - Python
  - Jupyter
  - pre-commit
draft: false
---

Jupyter notebooks are very useful to quickly prototype ideas in data science projects because they allow seeing code,
the output of that code, and a narrative explaining its logic right next to each other.
But when committing said Jupyter notebooks to version control systems like git, code output containing **sensitive information**
(such as passwords, access tokens, or sensitive data) can easily end up being stored in places where it should not be stored and
where it is accessible to others.

This post explains how to make sure that sensitive information is never committed to
version control by automatically stripping output from Jupyter notebooks using [nbstripout](https://github.com/kynan/nbstripout)
before every commit.

## The problem

What we are trying to avoid is having sensitive output like in the following example in version control where it can be read by others:

![](/posts/2022-11-12/bad.png)

Of course, if you manually strip all sensitive information before EVERY single commit: no problem.
But we all know what happens when you *just* want to finish and commit before having lunch, getting a coffee, or leaving work for the weekend: you forget to do exactly that and disaster strikes.

## The solution

Instead, we want to ensure that only the following is committed to version control without ever having to remember:

![](/posts/2022-11-12/good.png)

I recommend ensuring this behavior by:

1. Installing [pre-commit](https://pre-commit.com/#install) to manage [git pre-commit hooks](https://git-scm.com/docs/githooks)
2. Installing [nbstripout as a pre-commit hook](https://github.com/kynan/nbstripout#using-nbstripout-as-a-pre-commit-hook)
3. Running `pre-commit install` to set things up.
4. Done.

What happens is that `nbstripout` is now being executed to strip any output from `.ipynb` files before committing.
Since the pre-commit hook works directly in `git` the above automatically hooks into version control plugins in your favorite IDE, too.

## Caveat

`nbstripout` modifies the working copy of your notebook.
The main caveat is that an active development session could be disrupted due to having to re-run cells more frequently to
re-generate their output. I think this is a minor problem because:

a) only cell outputs are stripped while variables and the kernel working memory are not cleared, and
b) it's a good idea to run notebooks frequently top-to-bottom anyway after restarting the kernel - simply to make sure that things work as expected.

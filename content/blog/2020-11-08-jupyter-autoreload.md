---
title: '%autoreload: reload code before execution in Jupyter'
author: 'Alexander Junge'
date: '2020-11-08'
slug: jupyter-autoreload
categories:
  - Coding Tips
tags:
  - Python
  - Jupyter
draft: false
---

As I keep forgetting the following and keep finding outdated answers/documentation when searching online, I hope this will be useful to others and to future me: 

## Autoreload all imported packages in Jupyter

```
%load_ext autoreload
%autoreload 2
```

## Autoreload specific packages `foo` and `bar`

```
%load_ext autoreload
%autoreload 1
%aimport foo, bar
```

## Documentation

[Link](https://ipython.readthedocs.io/en/stable/config/extensions/autoreload.html)
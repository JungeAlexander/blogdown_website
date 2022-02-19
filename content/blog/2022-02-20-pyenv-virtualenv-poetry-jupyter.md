---
title: 'One JupyterLab, many packages'
author: 'Alexander Junge'
date: '2022-02-20'
slug: pyenv-virtualenv-poetry-jupyter
categories:
  - data science
  - tooling
tags:
  - Python
  - poetry
  - pyenv
  - Jupyter
  - pipx
draft: true
---

[Everything Gets a Package](https://www.ethanrosenthal.com/2022/02/01/everything-gets-a-package/?utm_campaign=Data_Elixir&utm_source=Data_Elixir_373/) post where Ethan Rosenthal describes his data science project setup.

![](/posts/2022-02-20/pyenv-virtualenv-poetry-jupyter.png)


Which problems this actually solves for me:
1 - jupyter plugins
2 - graduating from notebook -> .py -> package smooth

3 -pyenv, pyenv virtualenv, pipx, poetry, ipykernel

Possible issues: upgrading python/jupyter breaking jupyter - but maybe not since each pykernel is still its own venv


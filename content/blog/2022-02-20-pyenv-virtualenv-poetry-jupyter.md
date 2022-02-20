---
title: 'One JupyterLab, many projects'
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

[Jupyter notebooks](https://try.jupyter.org) are the tool of choice when working with and exploring data in Python.
I recently read the ["Everything Gets a Package""](https://www.ethanrosenthal.com/2022/02/01/everything-gets-a-package) post where Ethan Rosenthal describes his data science project setup.

![](/posts/2022-02-20/pyenv-virtualenv-poetry-jupyter.png)


Which problems this actually solves for me:
1 - jupyter plugins
2 - graduating from notebook -> .py -> package smooth

3 -pyenv, pyenv virtualenv, pipx, poetry, ipykernel

A short introduction to the tools used here. A detailed description is out-of-scope here, just follow the links to learn more: 

- `pyenv` [link](https://github.com/pyenv/pyenv) - simple and transparent management and installation of different Python versions.
- `pyenv virtualenv` [link](https://github.com/pyenv/pyenv-virtualenv) - management of virtual environements via `pyenv`.
- `pipx` [link](https://github.com/pypa/pipx) - system-wide installation of Python applications in their own environments.
- `poetry` [link](https://github.com/python-poetry/poetry) - dependency management for Python projects and applications.
- `JupyterLab` [link](https://github.com/jupyterlab/jupyterlab) - browser-based user interface to write Python code, interact with datasets, visualize them and much more.
- `ipykernel` [link](https://github.com/ipython/ipykernel) - Ipython as a kernel for Jupyter; I am using it to register project-level virtual environments as kernels in Jupyter.

Extensions [list 1](https://github.com/mauhai/awesome-jupyterlab) and [list 2](https://github.com/mauhai/awesome-jupyterlab)

Possible issues: upgrading python/jupyter breaking jupyter - but maybe not since each pykernel is still its own venv
Although Python story better under Win, no idea how well this works
this will change

I am not proving code examples in this post what would be happy to if there is interest - let me know.



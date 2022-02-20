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
draft: false
---

[Jupyter notebooks edited in JupyterLab](https://try.jupyter.org) are my tool of choice when working with and exploring data in Python.
I frequently mature code stored in notebooks to importable `.py` files and further to stand-alone Python packages.
I recently read the ["Everything Gets a Package"](https://www.ethanrosenthal.com/2022/02/01/everything-gets-a-package) post
where Ethan Rosenthal describes his data science project setup.
This led me to rethink how I manage virtual environments, dependencies and JupyterLab installations across projects.

Here, I will share my current process for setting up development environments on my local machine or a remote server I work on.
This approach solves three main problems for me:

1. Use and manage **one JupyterLab installation** across projects with useful JupyterLab extensions installed. Before adopting this process, each project I worked on came with its own JupyterLab which kept me from customizing and tweaking JupyterLab to maximize efficiency for every single project.
2. Graduating code from a notebook to a `.py` file to a **package should be easy** (this is the central point in Ethan's post which is essentially achieved using `poetry` from the start).
3. Of course, projects and tools used in my workflow should be **separated into virtual environments**.

If this sounds interesting, read on. But please also note that my approach will certainly change as months and years go by.
I will share updates on this website.

## Tools

Here is a short introduction to the tools used in my process.
A detailed description is out-of-scope here, just follow the links to learn more: 

- `pyenv` ([link](https://github.com/pyenv/pyenv)) - simple and transparent management and installation of different Python versions.
- `pyenv virtualenv` ([link](https://github.com/pyenv/pyenv-virtualenv)) - management of virtual environments as a plugin for `pyenv`.
- `pipx` ([link](https://github.com/pypa/pipx)) - system-wide installation of Python applications in their own environments.
- `poetry` ([link](https://github.com/python-poetry/poetry)) - dependency management for Python projects and applications; `poetry` is installed using `pipx`.
- `JupyterLab` ([link](https://github.com/jupyterlab/jupyterlab)) - browser-based user interface to write Python code, interact with datasets, visualize them and much more.
- `JupyterLab` extensions - I do not want to point out specific extensions here (that could be a later post) but there are many, see, e.g., [list 1](https://github.com/mauhai/awesome-jupyterlab) and [list 2](https://github.com/mauhai/awesome-jupyterlab)
- `ipykernel` ([link](https://github.com/ipython/ipykernel)) - IPython as a kernel for Jupyter; I am using it to register project-level virtual environments as kernels in Jupyter.

## Approach

My setup currently looks like this:

![](/posts/2022-02-20/pyenv-virtualenv-poetry-jupyter.png)

The most important ideas are:

1. I have a virtual environment called 'base' running JupyterLab and all extensions. The 'base' environment is created using `pyenv virtualenv` once and activated whenever I open a terminal (by editing my `.zshrc`).
2. Each project lives in in its own virtual environment manged by `pyenv virtualenv`.
3. Each project is a `poetry` project and `poetry` is used to install dependencies.
4. `ipykernel` is used to register each project's virtual environment as a kernel in the Jupyter instance living in the 'base' environment.

## Code 

When starting a new project, I go through the following steps.
Before getting started, run the following once to switch off poetry's built-in virtual environment management:
`poetry config virtualenvs.create false`.

```shell
mkdir my-project  # create a project directory
cd my-project

pyenv virtualenv my-project  # create the project virtual env
pyenv activate my-project  # activate the virtual env

poetry init  # initialize the poetry project
poetry add spacy  # install dependencies
poetry add --dev black ipykernel # install dev dependencies

python -m ipykernel install --user --name my-project  # register project kernel

git init  # initialize version control 
git add pyproject.toml poetry.lock  # add files produced by poetry
# commit, track more files, push to a remote repo
```

Besides setting up the project directory and virtual environment, it registers
the my-project kernel in the JupyterLab installation living in the 'base' environment.
It can be seen here:

![](/posts/2022-02-20/jupyter.png)

Note that the commands above do not set up the 'base' environment - let me know if that would be of interest.
For installing the tools used, refer to the links listed above.

## Caution

My approach has a few caveats:

1. Upgrading Python or JupterLab in the base environment could become messy when breaking changes occur. However, all project code and dependencies is and will always be self-contained and reproducible - that's what really matters.
2. There a quite a few tools at play here - these will all be updated or replaced by newer ones. There is no way around embracing change here.
3. Although running Python under Windows got much easier recently, I would be surprised if all this worked out of the box outside Linux or MacOS (let me know if it does!). So be careful/patient when trying to adapt this process on a Windows box!

Thanks for reading and do let me know if you have any feedback or suggestions for further improvements.
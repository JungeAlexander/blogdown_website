---
title: 'GitHub Actions: Setting up poetry and running CI'
author: 'Alexander Junge'
date: '2020-11-01'
slug: github-actions-poetry-ci
categories:
  - Testing
  - CI
  - Virtual environment
tags:
  - Python
  - Pytest
  - GitHub Actions
  - Poetry
draft: false
---

This weekend, I set up my first [GitHub Action](https://github.com/features/actions)
to run continuous integration using `Pytest` for one of my repositories.
Dependencies and virtual environment managment is done via `Poetry`.

For example, the following file, placed in `.github/workflows/python-app.yml` of a GitHub
repository, does the following:

1. checkout the repository
2. install Python
3. install Poetry
4. install dependencies using Poetry in a virtual environment
5. run pytest against a local test suite in `tests/`
6. Run the above for both Python 3.8 and 3.9.

```yaml
name: Python application

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  build:
    strategy:
      fail-fast: false
      matrix:
        python-version: [3.8, 3.9]
        poetry-version: [1.1.4]

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2

    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v2
      with:
        python-version: ${{ matrix.python-version }}

    - name: Install poetry ${{ matrix.poetry-version }}
      run: |
        python -m ensurepip
        python -m pip install --upgrade pip
        python -m pip install poetry==${{ matrix.poetry-version }}

    - name: View poetry --help
      run: poetry --help

    - name: Install dependencies
      shell: bash
      run: python -m poetry install

    - name: Test with pytest
      run: |
        python -m poetry run python -m pytest -v tests
```

For more information, see for example a GitHub repo [here](https://github.com/JungeAlexander/kbase_db_api)
using the mechanism.
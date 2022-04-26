---
title: 'Hosting ML apps easily and freely via Hugging Face Spaces'
author: 'Alexander Junge'
date: '2022-04-26'
slug: app-hf-spaces-intro
categories:
  - data science
  - app
  - hosting
tags:
  - Python
  - gradio
  - streamlit
draft: true
---

Data science and machine learning frequently involve prototyping models and solutions,
showing them to potential users and iterating to improve both model and problem-solution fit.

Tools like [Gradio](https://gradio.app/) or [Streamlit](https://streamlit.io/) allow fast prototyping of ML and data apps but making apps available
to users is not trivial and usually involve paying for cloud hosting and overcoming various technical hurdles.

[Hugging Face Spaces](https://huggingface.co/spaces) allows you to host and share ML apps easily and for free. Here is an example:

## Example app running of Hugging Face Spaces

Sometimes takes a bit for the app to load so not suited for production but only for testing and prototyping or, as in my case,
to host apps for pet projects or build a portfolio

![](/posts/2022-02-20/pyenv-virtualenv-poetry-jupyter.png)

I built a sample app using gradio that 
It is based on the *Google Patent Phrase Similarity Dataset* used in the
['U.S. Patent Phrase to Phrase Matching' Kaggle competition](https://www.kaggle.com/competitions/us-patent-phrase-to-phrase-matching/overview).

The app scores pairs of phrases from U.S. patents according to their similarity.
Similarity scores are between 0 and 1, higher scores mean higher similarrity.
The scores are computed as the cosine similarity of embeddings produced by the [AI-Growth-Lab/PatentSBERTa]() SentenceTransformer model.

Not made for best performance but rather as an example

App is available on [Hugging Face Space](https://huggingface.co/spaces/jungealexander/uspppm-demo).
The underlying code is on [GitHub](https://github.com/JungeAlexander/uspppm-demo) and I am using GitHub Actions to keep code and app in sync. 

## Workflow

- Create a new Space [https://huggingface.co/new-space](https://huggingface.co/new-space)
- This will create a git repository which I clone to my machine. Since I want to use GitHub as the primary repository location for my code, while setting up a GitHub Action to keep the deployed Space updated later, I set up my `.git/config` with two remote locations - ‘origin’ pointing to the GitHub repo and ‘space’ pointing to the Space repo.

```shell
[remote "origin"]
        url = git@github.com:JungeAlexander/uspppm-demo.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[remote "space"]
        url = https://huggingface.co/spaces/jungealexander/uspppm-demo
        fetch = +refs/heads/*:refs/remotes/space/*
```

- Create a write-access token for HugginFace account: [https://huggingface.co/settings/tokens](https://huggingface.co/settings/tokens)
- Setting up GitHub Action [https://huggingface.co/docs/hub/spaces#manage-app-with-github-actions](https://huggingface.co/docs/hub/spaces#manage-app-with-github-actions) Save as repository secret

## Summary

Easy and free hosting of protoypes and apps showcasing your work - give it a try.
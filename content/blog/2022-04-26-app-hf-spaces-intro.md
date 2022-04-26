---
title: 'Hosting Machine Learning apps easily and freely via Hugging Face Spaces'
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
draft: false
---

Data science and machine learning (ML) projects frequently involve prototyping models as part of ML apps,
showing them to potential users to get feedback, and iterating to improve both model as well as problem-solution fit.
Tools like [Gradio](https://gradio.app/) and [Streamlit](https://streamlit.io/) make it easy to develop visually appealing ML apps with a few lines of code.
Making these apps available to users is unfortunately not trivial and usually involves paying for cloud hosting and overcoming various technical hurdles.

[Hugging Face Spaces](https://huggingface.co/spaces) allows you to host and share ML apps easily and for free.
Here is an example from a recent hobby project:

## Example app computing patent phrase similarity running on Hugging Face Spaces

![](/posts/2022-04-26/app-hf-spaces-intro.png)

The app is available on this [Hugging Face Space](https://huggingface.co/spaces/jungealexander/uspppm-demo).
The underlying code is on [GitHub](https://github.com/JungeAlexander/uspppm-demo) and I am using GitHub Actions to keep code and app in sync.

I built this sample app using Gradio within a few hours. 
It scores the semantic similarity of pieces of text from U.S. patents.
Similarity scores are between 0 and 1; higher scores mean higher similarity.
A score is defined as the cosine similarity of the phrase-wise embeddings produced by the [AI-Growth-Lab/PatentSBERTa](https://huggingface.co/AI-Growth-Lab/PatentSBERTa) SentenceTransformer model and
could be used for down-stream tasks like clustering or search ranking.
The app and its examples are based on the *Google Patent Phrase Similarity Dataset* used in the
['U.S. Patent Phrase to Phrase Matching' Kaggle competition](https://www.kaggle.com/competitions/us-patent-phrase-to-phrase-matching/overview).

This simple app is not tweaked for performance - neither from an ML nor from a inference time perspective - but rather as an example of
what's with a few lines of code (around 45 currently).

## Workflow for deploying the app via GitHub Actions

Deploying the app was simple and involved the following steps:

- Create a new Space [https://huggingface.co/new-space](https://huggingface.co/new-space).
- The Space comes with a git repository which I cloned to my machine. Since I wanted to use GitHub as the primary repository location for my code, while setting up a GitHub Action to keep the deployed app in sync, I set up my `.git/config` with two remote locations - ‘origin’ pointing to the GitHub repository and ‘space’ pointing to the Space repo, e.g.:

```shell
[remote "origin"]
        url = git@github.com:JungeAlexander/uspppm-demo.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[remote "space"]
        url = https://huggingface.co/spaces/jungealexander/uspppm-demo
        fetch = +refs/heads/*:refs/remotes/space/*
```

- Create a write-access token for your Hugging Face account: [https://huggingface.co/settings/tokens](https://huggingface.co/settings/tokens)
- Set up the GitHub Action as describe in the [Hugging Face docs](https://huggingface.co/docs/hub/spaces#manage-app-with-github-actions) and save your API token as a GitHub repository secret.

## Summary

Spaces makes it possible to host ML app protoypes and to showcase your work to the world - give it a try, it's free and easy to get started.

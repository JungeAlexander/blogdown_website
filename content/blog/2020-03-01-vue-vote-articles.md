---
title: 'Building a Vue/Vuetify application to label articles'
author: 'Alexander Junge'
date: '2020-03-01'
slug: vue-vote-articles
categories:
  - kbase
tags:
  - kbase
  - arXiv
  - vue
  - vuetify
  - labeling
  - Docker
draft: true
---

[Vue](https://vuejs.org) JavaScript 
[Vuetify](https://vuetifyjs.com/en/) components

First time using a JavaScript web framework and first time, using JavaScript really and I am quite 
amazed how little time it took to develop a beautiful web application.

Using the application, it took me about an hour to label 500+ arxiv.org preprint articles
with an up- or down-vote, according to my interests.
As described, I used [Apache Airflow to query articles from arxiv.org](/blog/arxiv-airflow-fastapi-psql/).

The application looks like this:

![](/posts/2020-03-01/vue_app.png)

The code is fully dockerized and available on [GitHub](https://github.com/JungeAlexander/kbase/tree/user_ratings)
along with more detailed documentation.

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
draft: false
---

As described in my last post, I used [Apache Airflow to query articles from arxiv.org](/blog/arxiv-airflow-fastapi-psql/).
I now built a small web application to label articles that I find interesting (upvote)
or not interesting (downvote).

Using the application, it took me about an hour to label 500+ arxiv.org preprint articles
with an up- or downvote.
I plan to use these label to recommend new articles of interest in the future.

The application is build in the JavaScript web framework [Vue](https://vuejs.org) and
uses [Vuetify](https://vuetifyjs.com/en/) [Material Design](https://en.wikipedia.org/wiki/Material_Design) components.
This was my first time using Vue (my first time using JavaScript, really) and
I am quite amazed how little time it took to develop a beautiful web application.

The application looks like this:

![](/posts/2020-03-01/vue_app.png)

The code is fully dockerized and available on [GitHub](https://github.com/JungeAlexander/kbase/tree/user_ratings)
along with more detailed documentation.

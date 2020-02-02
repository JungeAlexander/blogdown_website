---
title: 'Querying arXiv preprints using Airflow'
author: 'Alexander Junge'
date: '2020-02-02'
slug: arxiv-airflow-fastapi-psql
categories:
  - kbase
tags:
  - kbase
  - arXiv
  - Airflow
  - FastAPI
  - PostgreSQL
---

# Querying arXiv preprints using Apache Airflow

I experimented with Apache Airflow to schedule hourly workflows
fetching recent preprint articles from different arXiv categories
via the public arXiv.org REST API.
These articles are then stored in a PostgreSQL database via
a custom-built fastAPI-based REST API.

The setup looks like this:

<img src = "/posts/2020-02-02/arxiv_airflow_fastapi_psql.png", width = "300", height = "350">

The code is fully dockerized and available on [GitHub](https://github.com/JungeAlexander/kbase/tree/arxiv_airflow_fastapi_psql)
along with more detailed documentation.

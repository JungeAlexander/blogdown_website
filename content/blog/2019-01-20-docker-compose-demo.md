---
title: 'Trying out Docker Compose'
author: 'Alexander Junge'
date: '2019-01-20'
slug: demo-docker-compose
categories:
  - docker
tags:
  - container
  - software deployment
---

[Docker](https://www.docker.com/) containers are well-suited to ship software because they allow a fast, flexible, and efficient deployment in any computing environment - be it on premise, in the cloud, etc.
[Docker Compose](https://docs.docker.com/compose/) allows users to orchestrate multiple Docker containers that can talk to one another.
Docker Compose applications are perfectly suited for providing data science and machine learning infrastructure where multiple tools need to interact to form a development environment.

I played around with Docker Compose over the last couple of days to produce an example of such an environment.
At the time of publishing this post, my demo environment consists of the following containers:

- `db` - a [PostgreSQL](https://www.postgresql.org/) database housing a toy dataset
- `api` -  a mock prediction API (built as a [responder](https://python-responder.org/en/latest/) app). In a real world setting this API could, for instance, run an input data point through a previously trained machine learning model and return a prediction.
- `dash` - a simple [Dash](https://dash.plot.ly/) dashboard that reads data from `db` and runs them trough the prediction `api`.
- `jnb`- a [Jupyter lab](https://jupyterlab.readthedocs.io/en/stable/) environment to run Jupyter notebooks being able to read/write data from `db`, run data through the `api` and so on.

The project is available on GitHub and its contents may change in the future as I continue working on it: https://github.com/JungeAlexander/demo-docker-compose

In the process of working on this little project, I found the following GitHub repository as well as the associated PyCon.DE 2018 talk very inspiring: https://github.com/jgoerner/beyond-jupyter


---
title: 'Book tip. Designing Machine Learning Systems by Chip Huyen'
author: 'Alexander Junge'
date: '2022-08-28'
slug: book-review-designing-ml-systems
categories:
  - books
tags:
  - machine learning
  - ml systems
  - mlops
draft: false
---

I recently finished (and enjoyed) reading Chip Huyen's book ["Designing Machine Learning Systems"](https://github.com/chiphuyen/dmls-book)
published by O'Reilly.

<img src="/posts/2022-08-28/book-designing-ml-systems.png" alt="cover" width="300"/>

I'd recommend this book to anyone looking for an introduction to what it takes to make machine
learning (ML) work in the real world, i.e., outside a pure research setting and in real products.
Here is what I think about the book:

# What's good

The three things I particularly liked about Chip's book are:

## A concise definition of ML systems design

As it should, the book starts out clearly defining what the book is about:

> ML systems design takes a system approach to MLOps, which means that it considers an ML system holistically to ensure that all the components and their stakeholders can work together to satisfy the specified objectives and requirements.

ML systems are complex, data-dependent systems that include: 

> ML algorithms, data, business logic, evaluation metrics, underlying infrastructure, etc., and involve many different stakeholders (data scientists, ML engineers, business leaders, users, even society at large).

In contrast, the popular MLOps concept is in the book defined as:

> Ops in MLOps comes from DevOps, short for Developments and Operations. MLOps is a set of tools and best practices for bringing ML into production.

## Lots of hands-on advice from practitioners and the ML systems community

It's clear that the author is well connected in the tech industry and large parts of the book
are focussed on collecting and formalizing these community insights. Two examples:

1. **Common ML metrics are often not good metrics to measure the success of ML systems.** Instead, rely on business metrics co-developed with domain experts. Simple example: Netflix uses a *take-rate* to asses recommender quality, a custom metric (number of quality plays/number of recommendations), rather than only looking at ML metrics such as mean average precision@K.
2. **The more one separates between teams developing and teams deploying ML models, the harder it becomes to fix problems with the ML models in production.** Instead, model developers need to be empowered to (with the right tooling) operate, monitor, and improve their own models.

## Establishes a clear vocabulary focused on ML systems

Too often people use different terms to refer to the same things. This is confusing.
The author tries to remedy some of this confusion by clearly defining technical terms and
she points out flaws in the existing informal vocabulary out there.
For example, Chip defines clear vocabularies and definitions for:

- data distribution shifts
- continual learning
- approaches testing ML in production
- batch vs streaming ML

# What could be better

Sometimes reading the book felt a bit lengthy and I found myself skipping pages here and there.
This often happened in sections overly relying on anecdotes the author had gathered in her network or
within the community, on company blogs, and so on.
I hope that these sections can be shortened and formalized better in future editions of the book,
especially as best-practices mature in the ML systems community and common vocabularies are adopted (see above).

# Verdict

👍 - recommended read for anyone looking for a production-focussed introduction to ML systems design
---
title: 'After evals, flywheels'
author: 'Alexander Junge'
date: '2024-04-08'
slug: after-evals-flywheels
categories:
  - short
tags:
  - ai
  - data
  - flywheels
  
draft: false
---

Let's assume you have a few [evals](/blog/starting-evals-hamel) for your AI product in place
that allow you to get a good idea of how well the underlying (set of) model(s) is doing and attract a few first users. And now what?

Start building data flywheels! Flywheels are mechanisms that allow you to leverage the data you have
to improve your model and attract more users. They are the key to scaling your AI product and the mechanism looks like this:

some users -> some data/feedback -> better model -> more users -> more data/feedback -> even better model -> lots of users... (you get the idea)

## Where to start?

A few ideas to get you started thinking about flywheels for your AI product:

1. focus on gathering simple, explicit (thumbs up/down) user feedback or implicit engagement data (e.g. clicks, reads, watch time).
2. make these mechanisms part of your core user experience.
3. use feedback to systematically improve your AI models. This can be tricky, potentially requiring a lot of MLOps infrastructure to continually retrain models.
4. ensure at all times that user data is handled responsibly and in compliance with privacy regulations. For example, you might not be able to train on data without explicit user consent (which you can ask for). The good thing is that not all flywheels require training on data for unique users, e.g. using feature-based approaches (averaging across users), [privacy-preserving technologies](/blog/short-diff-privacy-rag), synthetic data resembling actual usage, or even federated learning.


## Further reading

Two resources come to mind:

- a recent blog post [Data Flywheel Go Brrr: Using Your Users to Build Better Products](https://jxnl.co/writing/2024/03/28/data-flywheel/) by Jason Liu
- the book [Working Backwards: Insights, Stories, and Secrets from Inside Amazon](https://www.amazon.com/Working-Backwards-Insights-Stories-Secrets/dp/1250267595) also has a chapter on flywheels.
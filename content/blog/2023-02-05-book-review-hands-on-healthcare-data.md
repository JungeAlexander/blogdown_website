---
title: 'Book review. Hands-On Healthcare Data by Andrew Nguyen'
author: 'Alexander Junge'
date: '2023-02-05'
slug: book-review-hands-on-healthcare-data
categories:
  - books
  - healthcare
tags:
  - data engineering
  - analytics
  - machine learning
draft: true
---

I recently finished reading Andrew Nguyen's book ["Hands-On Healthcare Data"](https://www.oreilly.com/library/view/hands-on-healthcare-data/9781098112912/)
published by O'Reilly in September 2022. Here is what I thought

# What's good

The three things I particularly liked about the book are:

## Complexity of healthcare data especially when reused for different purpose

This is the situation for most data in healthcare -> used for another purpose than intended.

Layers of working with ICD complexity:

1. which ICD release e.g. ICD-9 vs ICD-10
2. which version updates applied to data
3. which local modifications applied by health agencies and government
4. which guidelines and version used for assigning codes  

## Concise definition of "real world data" and challenges

Pyramids/figures - people aspect to ensure that domain understanding is shared between technical and medical teams

## Points to useful resouces to avoid reinventing the wheel 

Especially for medical terminologies


# What could be better

graph DBs - yes potential but unclear why such focus, most chapters are like "hey graph DB could help here". There certainly is potential but would have preferred to describe potential in one place and be done with it instead of wake hints

Especially chapters on analytics, ML, etc. ie. how value can be extracted from data once modelled are pretty vague. I acknowledge that it's not easy to write this up consisely

# Verdict

👍 - recommended read for data engineer-types wanting to get an overview how to get started ingesting, harmonizing, and working with healthcare data

🤔 - maybe useful to anyone else wanting to learn about healthcare data and what to do with it. 
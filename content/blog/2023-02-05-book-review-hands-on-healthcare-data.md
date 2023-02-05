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
draft: false
---

I recently finished reading Andrew Nguyen's book ["Hands-On Healthcare Data"](https://www.oreilly.com/library/view/hands-on-healthcare-data/9781098112912/)
published by O'Reilly in September 2022. What I particularly liked about the book:

## Clearly illustrating the complexity of healthcare data

Performing scientific research, statistical analyses, data integration, or machine learning in a healthcare setting
often means reusing data originally collected with another goal in mind, namely delivering healthcare.
This makes working with and harmonizing healthcare data difficult.

For example, when working with International Classification of Diseases (ICD) codes, consider the following complexities:

1. ICD release used, e.g., ICD-9 vs ICD-10
2. ICD release version updates applied
3. local modifications by health agencies and governments applied 
4. coding guidelines and their version used by healthcare staff and adherence to these guidelines

In the end, we are talking about "real world data" here which:

- reflects routine clinical practice rather than highly controlled situations (such as a clinical trial).
- was collected to secure the health of patients, not to generate evidence.
- is used retrospectively rather than being collected over time for a specific cohort of patients.

## Pointing to useful resources to avoid reinventing the wheel 

When working with healthcare standard (e.g. FHIR or OMOP) and terminologies (e.g. ICD or SNOMED), don't reinvent the wheel
but check the plethora of services and code repositories listed in the book first. A partial list:

- GitLab repo for the book: https://gitlab.com/hands-on-healthcare-data
- Ontoserver FHIR terminology server https://ontoserver.csiro.au/site/
- Synthea synthetic health data simulator https://github.com/synthetichealth/synthea

## What could be better

It felt like every chapter of the book came with a message similar to

> "Hey, graph databases could help here and make working with healthcare data so much easier".

I agree with the author that modeling data as graphs adds flexibility and often makes querying data for complex analyses easier.
Nevertheless, I got a bit tired of this message after reading it time and time again and would have preferred a
message like this to be presented in one place, maybe a standalone chapter. 

In addition, book chapters describing analytics and machine learning for healthcare are pretty high-level and I would have preferred
more concrete examples of past successes and failures.

# Verdict

👍 - recommended read for data engineering personas wanting to get an overview of how to get started ingesting, harmonizing, and
working with healthcare data as well as the core challenges to be aware of. I think this is the first book trying to address this area.

🤔 - the book **could** be useful to anyone else wanting to learn about healthcare data and what to do with it.
But I think a more general audience will find themselves skipping many of the more technical sections of the book.

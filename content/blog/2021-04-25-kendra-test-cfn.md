---
title: 'Experimenting with Amazon Kendra ML-powered search'
author: 'Alexander Junge'
date: '2021-04-25'
slug: kendra-test-cfn
categories:
  - search
tags:
  - AWS
  - Kendra
  - CloudFormation
draft: true
---

Kendra is ...

CloudFormation templates on [GitHub](https://github.com/JungeAlexander/kbase_search/tree/main/kendra/cloudformation).

## Data 

Podcast episodes show notes and transcripts stored as flat text files in S3

## Results

Kendra default search interface:

![](/posts/2021-04-25/bandit.png)

![](/posts/2021-04-25/data_warehouse.png)

![](/posts/2021-04-25/hadoop.png)

![](/posts/2021-04-25/python_rest.png)


## Conclusion

- easy to get started with a managed service
- many connectors beyond S3
- metadata and Kendra API allow customization via facetted search, access restrictions etc.
- expensive for small user group - this is made for larger enterprises

Open source alternatives like [Jina](https://jina.ai) for neural search or [Haystack](https://haystack.deepset.ai) for Question-Answering but require dealing with infrastructure.
---
title: 'Short: Differential privacy in a RAG setting'
author: 'Alexander Junge'
date: '2024-03-21'
slug: short-diff-privacy-rag
categories:
  - short
tags:
  - ai
  - privacy
  - security
  - 
draft: false
---

## Why

The usefulness of modern AI systems dramatically increases when the underlying AI models have
access to recent, relevant data in addition to the information captured in the models' internal parameters.
This is the core idea behind both in-context learning and Retrieval Augmented Generation.

[Here](https://www.llamaindex.ai/blog/retrieving-privacy-safe-documents-over-a-network) is an interesting LlamaIndex blog post looking at a scenario where
three parties want to share data but cannot to do so freely for privacy reason.

The use of a **differentially-private algorithm** is one solution to this problem.

## What

Image from [this ICLR 2024 paper](https://arxiv.org/abs/2309.11765):

![](/posts/2024-03-21/short-diff-privacy-rag.png)

- LLMs might regurgitate examples given as context or in the prompt, e.g. after prompt injection
- a differentially-private algorithm adds a defined amount of noise to the shared data while providing certain privacy guarantees
- by combining larger, differentially-private datasets, the overall accuracy of the AI system improves, too

## So what?

- enabling data exchange in scenarios where privacy would not allow to do so, e.g., healthcare
- improved performance while enabling privacy-safe data exchange
- further reading in [this ICLR 2024 paper](https://arxiv.org/abs/2309.11765)
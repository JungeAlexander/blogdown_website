---
title: 'Fine-tuning LMs as a way to move compute back from inference to training'
author: 'Alexander Junge'
date: '2024-04-01'
slug: finetuning-inference-training
categories:
  - short
tags:
  - ai
  - fine-tuning
  
draft: false
---

As I was reading this [review paper on tool use for language models (LMs)](https://zorazrw.github.io/files/WhatAreToolsAnyway.pdf)
over the Easter holidays, a thought crossed my mind:

There is an interesting trend when working with LMs in production to perform more and more computations at inference time.
For example, tool-using agents, multiagent systems, elaborate state machines,
and ever more complicated RAG systems like [CRAG](https://arxiv.org/abs/2401.15884) are becoming popular since they tend to
give better responses.
The goal is often to provide deliberative System-2-like responses, rather than instinctive System-1-like responses
(following the System 1 and System 2 distinction coined by the late Daniel Kahneman and Amos Tversky; excuse the anthropomorphism here).

However, this trend leads to increasing response times and inference costs, degrading user experience.
Fine-tuning, on the other hand, can be seen as a way to move compute from inference time back to training time.
**Instead of relying on large, generalist LMs, fine-tuning allows us to reverse this trend and use smaller, faster, and cheaper specialist LMs.**
Fine-tuning of course requires extra compute during training, but allows to run a smaller model at inference time,
reducing inference compute, response times, and inference costs.

Don't think this is a particularly groundbreaking idea but I think it's a useful mental model when thinking about fine-tuning.

PS: a related thought is that as more compute is spent at inference time, the fair evaluation of LM-based systems is becoming a more complex topic.
I guess besides disclosing performance metrics, inference compute spend and latency should start to be widely disclosed as well.
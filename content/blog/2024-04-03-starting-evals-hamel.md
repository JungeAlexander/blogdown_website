---
title: 'Recommended read: Your AI Product Needs Evals by Hamel Husain'
author: 'Alexander Junge'
date: '2024-04-03'
slug: starting-evals-hamel
categories:
  - short
tags:
  - ai
  - evals
  
draft: false
---

I really like this [blog post](https://hamel.dev/blog/posts/evals/) by Hamel Husain entitled 'Your AI Product Needs Evals'.
In particular, what sets this one apart from other posts on the topic is that it is very practical and
actionable (by centering the post around a specific case study).
It helps that this post is not written by an LLM tool provider that primarily tries to sell their new fancy tool to you and
only secondarily provides some useful advice (not mentioning any names here).

The key points in the post are:

1. You do not need a whole lot of usage of your product to start bootstrapping evals. It helps of course to get a few pilot users on board but you can go a long way with just an idea of usage patterns in mind and then using a powerful LM to generate test cases for you based on these ideas. This gives you Level 1 evals already. 
2. Build tools that make it as smooth as possible for you to eyeball samples and hand label them. I wrote about the idea of building custom tools before [here](/blog/ai-tools-for-one/).
3. Never stop looking at data. It is easy to get lost in the weeds of building the next fancy feature but if you do not have a good idea of what your data looks like, you are likely to be in for a surprise. Also love Hamel's point about reusing your evaluation framework to create finetuning data if you need to.

Again, this is a great post and I cannot do it justice in a short summary. Go read it! 
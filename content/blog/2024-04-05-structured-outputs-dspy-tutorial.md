---
title: 'Recommended tutorial on achieving Structured Outputs in DSPy'
author: 'Alexander Junge'
date: '2024-04-05'
slug: structured-outputs-dspy-tutorial
categories:
  - short
tags:
  - ai
  - structured outputs
  - dspy
  - instructor
  
draft: false
---

I am a big fan and active user of [instructor](https://jxnl.github.io/instructor/) to make interactions
with inherently probabilistic LMs more structured and reliable.
Without approaches like this, making LMs work reliably in certrain production settings would be a true nightmare.

This is a really cool [intro video](https://www.youtube.com/watch?v=tVw3CwrN5-8) by Connor Shorten (of Weaviate)
to structured outputs in general and
specifially using instructor and [DSPy](https://dspy-docs.vercel.app). 
DSPy is another really cool project and idea that, simply put, makes working with LM-based systems feel again like
ML engineering and not like a game of tipping LMs to produce the right output or threatening to
harm a kitten (aka prompt engineering).

So I recommend watching this video. In particular, pay attention to:

1. the example of the structure outputs (using instructor) in the beginning,
2. DSPy's approach to structured outputs using DSPy's TypedPredictor,
3. how to use DSPy's Assertions and Guardrails to implement a similar, (in the Mistral case) more robust behavior.

There is a lot going on in this video, so maybe I will try to break these topics down in a future post.
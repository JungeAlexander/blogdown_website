---
title: 'Multi-label classification using 🤗 Hugging Face Transformers AutoModelForSequenceClassification'
author: 'Alexander Junge'
date: '2022-06-12'
slug: til-multi-label-AutoModelForSequenceClassification
categories:
  - TIL
  - NLP
tags:
  - Python
  - PyTorch
  - Transformers
draft: false
---

🤗 Hugging Face Transformers `AutoModelForSequenceClassification` offers a quick way to fine-tune
a pre-trained language model for a text classification task.
`AutoModelForSequenceClassification` supports multi-label classification via its
`problem_type` argument:

```python
from transformers import AutoModelForSequenceClassification

model_ckpt = "distilbert-base-uncased"  # etc.
num_labels = 10  # etc.

model = AutoModelForSequenceClassification.from_pretrained(
    model_ckpt,
    num_labels=num_labels,
    problem_type="multi_label_classification",  # this is important
)
```

However, this comes with a few additional requirements on the dataset that I did not find good documentation for online. 
Since I struggled to make it work, I want to capture what I learned along the way here:

1. The model expects the target variable of the dataset to be named `labels`.
2. `labels` need to be binary vectors of length #labels, indicating which labels are true/false for a given sample, i.e., a multi-hot label encoding.
3. When using PyTorch in the backend, the `labels` vectors need to be floating-point numbers, not integers. This is because `AutoModelForSequenceClassification` uses [BCEWithLogitsLoss](https://pytorch.org/docs/stable/generated/torch.nn.BCEWithLogitsLoss.html?highlight=bce) and no automatic type casting takes place.

Another problem I encountered:
The dataset I worked with already came with a `labels` feature that did not follow the above requirements.
The easiest way to fix this was renaming the old labels, e.g., to `labels_`, and introducing a new `labels` feature
following the requirements.

Hope this helps.
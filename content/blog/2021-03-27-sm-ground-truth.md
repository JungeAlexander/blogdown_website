---
title: 'Labeling data in Amazon SageMaker Ground Truth'
author: 'Alexander Junge'
date: '2021-03-27'
slug: sm-ground-truth
categories:
  - Podcasts
  - NER
  - Data labeling
tags:
  - SageMaker
draft: true
---

Tried and nice experience, like hotkeys 

![](/posts/2021-03-27/sm_ground_truth_ner_podcast.png)

Used NER but other tasks around text, image, video labeling supported, too.

# Worflow

create manifest file and upload to S3. Example format of manifest:

```
{"source": "Lorem ipsum dolor sit amet"}
{"source": "consectetur adipiscing elit"}
   ...
{"source": "mollit anim id est laborum"} 
```

# Further reading

https://docs.aws.amazon.com/sagemaker/latest/dg/sms-input-data-input-manifest.html

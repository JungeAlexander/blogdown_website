---
title: 'Recommending scientific articles interactively'
author: 'Alexander Junge'
date: '2023-03-26'
slug: s2-recommender
categories:
  - papers
  - recommenders
tags:
  - embedding
  - machine learning
  - streamlit
draft: true
---

There are simply **too many** interesting research papers out there to read and more are getting published every day.
Even sort-of keeping up with the literature by skimming titles and abstracts of relevant work
on [PubMed](https://pubmed.ncbi.nlm.nih.gov),
[arXiv](https://arxiv.org), [bioRxiv](https://www.biorxiv.org), medRxiv(https://www.medrxiv.org), and so on is close to impossible.
To find interesting papers, I mostly rely on recommendations from friends, colleagues, or social media, especially 
Twitter but LinkedIn is sometimes surfacing relevant queries, too.

In this project, I wanted to prototype my own, **personal research recommendation system** that is 
based on my preferences.
I wanted to go beyond text-based searches and use semantic vector representations to separate interesting
from uninteresting articles. 
In short, I want to be able to self-curate my own literature recommendations to make better-informed decisions
regarding the papers I invest time in reading.

This project is inspired by [Semantic Scholar's research feeds](https://www.semanticscholar.org/faq#research-feeds).
Here I wanted to explore extra features in the paper recommendations showing how my likes
and dislikes influence the papers recommended interactively.

Note that this is an early **prototype** that I am sure will break.
Nevertheless, please leave feedback below if you have any.

## How it works

### 1. Search for a paper and similar papers

The search is based on your favorite paper's DOI and searches using the [Semantic Scholar API](https://api.semanticscholar.org/.
In addition to the query paper, the search returns similar papers as selected by Semantic Scholar.

### 2. Select papers you like and dislike

Select the papers you like and dislike among all papers returned in Step 1 in a super simple user interface:

![](/posts/2023-03-26/query_like_dislike.png)
    
### 3. Get and explore recommendations based on the papers you liked and disliked

We first search for a topic of interest and ask for 5 paper recommendations:

![](/posts/2023-03-26/recommendation_search.png)

Afterward, the user can interactively explore how recommendations, liked, disliked, and your initial favorite paper
relate to each other:

![](/posts/2023-03-26/umap.png)

The interactive 2D visualization of the papers is created using [bokeh]() after [UMAP]() projection. 

### 4. Repeat satisfied with the recommendations or come back another time

Putting it all together, the recommendation flow looks like this:
    

### The math behind finding recommendations based on liked and disliked papers

For each candidate recommendation paper, I calculate the cosine distance between the paper's embedding and
the embeddings of the liked and disliked papers.

I am using [SPECTER](https://aclanthology.org/2020.acl-main.207/) document-level embeddings here that, in short, are based on SciBERT-document embeddings but further incorporate the literature citation graph to
generate similar embeddings for papers citing each other
using a triplet loss. See the illustration here from the specter paper:

![](/posts/2023-03-26/specter-overview.png)

I then sort papers in increasing order by score $\mathcal{S}$ (loosely inspired by triplet loss):
  
$\mathcal{S}(x, L, D) = \\text{min}_{l \in L} \  d(x, l) \ - \ \\text{min}_{d \in D} \ d(x, d)$

where $x$ is the query paper, $L$ the set of liked papers, $D$ the set of disliked papers, $d$ is the cosine distance between two embeddings. I.e. we look for papers that are similar to the liked papers but dissimilar to the disliked papers.
This is a simple heuristic that I am sure could be improved but it seems to work reasonably well for now.

## Availability

The app is available [here](https://jungealexander-rr-apps2-api-n6v2v3.streamlit.app) and written using [Streamlit](https://streamlit.io).
It is hosted for free on Streamlit Cloud which comes with certain resource limitations.
Please be patient if the app should break but let me know when it does, e.g., by leaving a comment below.
The app's source code is available on [GitHub](https://github.com/JungeAlexander/rr) with CI/CD setup, i.e., whenever I 
push to the `main` branch, the app is automatically redeployed.

## Next steps

There are many improvements to be explored ranging from switching to [SPECTER 2](https://huggingface.co/allenai/specter2)
embeddings to adding a chat-like interface to interact with the surfaced papers or testing better filtering
heuristics.
From a backend perspective, I would love to add different user accounts, multiple feeds per user
and a proper backend database to store recommendations across sessions.
But this goes beyond an early prototype for now.


## TODO 

screen recording
SM posts
link repo -> post
link app -> post
TODO: dark theme for bokeh?
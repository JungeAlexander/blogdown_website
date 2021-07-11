---
title: 'Generating fake tokens to find out about security breaches'
author: 'Alexander Junge'
date: '2021-07-11'
slug: trap-tokens
categories:
  - security
tags:
  - AWS
draft: false
---

I recently came across [Canarytokens](https://canarytokens.org/generate#),
a service generating trap tokens (URLs, images, PDF, and much more) that notify the owner when used.
From their [documentation](https://blog.thinkst.com/p/canarytokensorg-quick-free-detection.html):

> Canary tokens are a free, quick, painless way to help defenders discover they've been breached (by having attackers announce themselves.)

The option to [generate fake AWS API Keys](https://docs.canarytokens.org/guide/aws-keys-token.html)
is very interesting.
Adding those to private repositories or storing them on dev machines could give an nice early warning
in detecting a breach.
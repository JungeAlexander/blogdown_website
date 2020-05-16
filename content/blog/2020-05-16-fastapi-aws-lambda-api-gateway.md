---
title: 'Deploying fastAPI to AWS Lambda via Amazon API Gateway'
author: 'Alexander Junge'
date: '2020-05-16'
slug: fastapi-aws-api-gateway
categories:
  - kbase
tags:
  - kbase
  - Python
  - AWS
  - AWS Lambda
  - Amazon API Gateway
  - AWS SAM
  - FastAPI
draft: false
---

I started experimenting with AWS and as a first tiny project I deployed a fastAPI-based REST API to [AWS Lambda](https://aws.amazon.com/lambda/), a serverless framework.
[Amazon API Gateway](https://aws.amazon.com/api-gateway/) acts as a front-door to the Lambda instance.
The architecture roughly looks like this:

![](/posts/2020-05-16/fastapi-aws-api-gateway.png)

The deployed API is a simplified version of the REST API described in a [previous post](/blog/arxiv-airflow-fastapi-psql/).
My code is available on [GitHub](https://github.com/JungeAlexander/kbase_db_api/tree/blog_fastapi-aws-api-gateway).

Using [AWS SAM](https://aws.amazon.com/serverless/sam/), the deployment works like this:

```shell
sam validate
sam build --use-container --debug
sam package --s3-bucket <S3-BUCKET-NAME> --output-template-file out.yml --region eu-west-1
sam deploy --template-file out.yml --stack-name example-stack-name --region eu-west-1 --no-fail-on-empty-changeset --capabilities CAPABILITY_IAM
```

I found this [post](https://iwpnd.pw/articles/2020-01/deploy-fastapi-to-aws-lambda) very
helpful when working on my project.
The post describes in a lot more detail which AWS accesses need to be created to make this work - check it out.

A great continuation of my tiny project would be setting up
continuous deployment of the API using, for example, GitHub Actions.
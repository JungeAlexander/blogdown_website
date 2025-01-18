---
title: 'Basic async performance testing with FastAPI and Locust'
author: 'Alexander Junge'
date: '2025-01-18'
slug: fastapi-async-perf
categories:
  - performance
tags:
  - fastapi
  - locust
  
draft: false
---

Performance optimization is a crucial aspect of web application development, especially when dealing with concurrent requests.
When writing [FastAPI](https://fastapi.tiangolo.com/) applications in Python, you can use the `async` and `await` keywords to improve performance in many use-cases by writing asynchronous code.
However, most tutorials focus on sprinkling in a bunch of `async`s and `await`s and hoping for the best.
Between some very basic concurrency [explanations](https://fastapi.tiangolo.com/async/#asynchronous-code) and reading whole text books,
I have come across little helpful, practical guidance on how to actually measure (let alone improve) async performance in a real fastAPI application.

This blog post does not aim to give an exhaustive analysis but rather provide a really basic performance analysis of different approaches to using async in FastAPI.
We will use [Locust](https://locust.io/) for load testing.

## What we will test

Our FastAPI app is available [here](https://github.com/JungeAlexander/fastapi-async-perf/blob/blogpost_20250118/app.py) - it is super simple so I recommend you just look at the code.
It has four `GET` endpoints as well as functions using `asyncio.sleep(5)`, or its synchronous counterpart `time.sleep(5)`, to sleep for 5 seconds to simulate a slow task,
e.g., database lookup, external API call (including to a language model or other AI service).
Each endpoint approaches (a)synchronicity in a different way:

1. `sync_baseline`: 3 sync tasks in order. The tasks are executed sequentially and should take 3*5=15 seconds to complete, if all requests are served as they are received.
2. `async_independent`: 3 async tasks in order, producing independent outputs, meaning that the tasks do not depend on each other's output.
3. `async_dependent`: 3 async tasks in order, producing dependent outputs, meaning that the tasks **do** depend on each other's output.
4. `async_concurrent`: 3 async tasks concurrently, producing independent outputs. It uses `asyncio.gather()` to run the tasks concurrently. (`asyncio.as_completed()` is similar but returns the results as they are completed.)

We will be using `uvicorn` with 2 workers to serve the app, as explained in the [FastAPI docs](https://fastapi.tiangolo.com/deployment/server-workers/#multiple-workers) on my local machine.

Our Locust file is super simple and simply calls each endpoint.
It is available [here](https://github.com/JungeAlexander/fastapi-async-perf/blob/blogpost_20250118/locustfile.py). We simulate 500 users, each making requests for a total of 5 minutes.

# Results

| Approach | # reqs | # fails | Avg (ms) | Min (ms) | Max (ms) | **Median (ms)** | **req/s** | failures/s |
| -------- | ------ | ------- | ----- | ----- | ----- | ----- | ------- | ----------- |
| `sync_baseline` | 1520 | 0 | 79082 | 15026 | 119366 | **76000** | **5.29** | 0.00 |
| `async_independent` | 9500 | 0 | 15021 | 15002 | 15140 | **15000** | **32.24** | 0.00 |
| `async_dependent` | 9500 | 0 | 15020 | 15003 | 15093 | **15000** | **32.25** | 0.00 |
| `async_concurrent` | 29157 | 0 | 5018 | 5001 | 5477 | **5000** | **97.33** | 0.00 |

The clear winner is `async_concurrent` with a throughput of 97.33 req/s and a median latency of 5000ms.
Using async/await alone gives you the expected 15s latency as each request still has to wait for the three 5 second tasks to complete.

All raw data is available here: https://github.com/JungeAlexander/fastapi-async-perf/tree/blogpost_20250118

## Key takeaways

In this example:

1. Using async/await is enough to get a significant performance improvement - and easy to implement.
2. Using `asyncio.gather()` (or its alternatives) gives you additional performance improvements. But it is harder to implement and requires that tasks do not depend on each other's output and that you refactor your code accordingly.
3. YMMV - Please do not take this as general advice but rather as a rough guide and encouragement to do your own testing. I have not tested other configurations and deployment scenarios. But tools like Locust make it really easy to do your own testing.

Again, the raw data and code is available [here](https://github.com/JungeAlexander/fastapi-async-perf/tree/blogpost_20250118).


Final note: I tried to work out the math behind this to come with an estimate but cannot make it work...
I guess there is just too much complexity even in this simple setup for a back of the envelope calculation to make sense.
Of course, practical results are what matter.
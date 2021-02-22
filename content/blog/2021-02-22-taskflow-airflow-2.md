---
title: 'Spotlight: New TaskFlow API in Apache Airflow 2'
author: 'Alexander Junge'
date: '2021-02-22'
slug: taskflow-airflow-2
categories:
  - Data Engineering
tags:
  - Python
  - Airflow
  - TaskFlow
draft: false
---

I recently switched to version 2 of Apache Airflow which was released in December 2020.
I am a big fan of the new TaskFlow API and want to highlight it here.

The TaskFlow API allows users to write DAGs in a much more efficient way, requiring less boilerplate code. 
Specifying task dependencies and exchanging data between tasks via XComs is also much easier now.

A minimal example of a DAG using the TaskFlow API looks something like this:

```python
from airflow.decorators import dag, task
from airflow.utils.dates import days_ago

@dag(default_args={'owner': 'airflow'}, schedule_interval=None, start_date=days_ago(1))
def tutorial_taskflow():

   @task
   def foo() -> dict:
       return {"data": 3.12}
       
   @task()
   def bar(data_dict: dict):
       print(f"Data is: {data_dict['data']}.")

   my_data = foo()
   bar(my_data)

dag = tutorial_taskflow()
```

Give it a try if this looks useful!

## Further reading

- [TaskFlow tutorial in the Airflow docs](https://airflow.apache.org/docs/apache-airflow/stable/tutorial_taskflow_api.html)
- [TaskFlow concept in the Airflow docs](https://airflow.apache.org/docs/apache-airflow/stable/concepts.html#taskflow-api)
- [on YouTube](https://www.youtube.com/watch?v=A4kxouVhZWA)



---
title: 'Deploying a custom Python machine learning model as an AWS SageMaker endpoint using MLflow'
author: 'Alexander Junge'
date: '2020-12-02'
slug: mlflow-sagemaker-deploy
categories:
  - Deployment
  - MLOps
  - NLP
tags:
  - Python
  - MLflow
  - AWS
  - SageMaker
  - Docker
draft: false
---

Deploying a trained machine learning model behind a REST API endpoint is an important
problem that needs to be solved on the last mile to getting the model into production.
The [MLflow](https://mlflow.org) package provides a nice abstraction layer that makes deployment
via AWS SageMaker (or Microsoft Azure ML or Apache Spark UDF) really easy.

Here is a minimal example that illustrates how a PyTorch-based pre-trained
[HuggingFace transformers](https://huggingface.co/transformers)
[Extractive Question Answering NLP model](https://huggingface.co/bert-large-uncased-whole-word-masking-finetuned-squad/tree/main)
can be deployed to an AWS SageMaker endpoint.
Note that MLflow provides many additional MLOps features which I will not address here.

## Packages and tools required

All code below is tested using Python 3.8 and with the following packages:

```
boto3==1.16.25
mlflow==1.12.0
pandas==1.1.4
torch==1.7.0
transformers==3.5.1
```

Furthermore, Docker needs to be installed and AWS credentials need to be set up on the host system.

## Defining the model as a Python function in MLflow

Let's first download and store the pre-trained model and its tokenizer locally:

```python
from transformers import AutoTokenizer, AutoModelForQuestionAnswering, AutoConfig

pretrained_model = "bert-large-uncased-whole-word-masking-finetuned-squad"

tokenizer = AutoTokenizer.from_pretrained(pretrained_model)
tokenizer_out_dir = "tokenizer_" + pretrained_model
tokenizer.save_pretrained(tokenizer_out_dir)

model = AutoModelForQuestionAnswering.from_pretrained(pretrained_model, return_dict=True)
model_out_dir = "model_" + pretrained_model
model.save_pretrained(model_out_dir)
```

These model artifacts will later be passed on to MLflow as a dictionary:

```python
artifacts = {
    "tokenizer_dir": tokenizer_out_dir,
    "model_dir": model_out_dir
}
```

The model inference logic is now wrapped in a class that inherits from `mlflow.pyfunc.PythonModel`.
There are two functions to implement:

- `load_context()` loads the model artifacts defined above from a [PythonModelContext](https://mlflow.org/docs/latest/python_api/mlflow.pyfunc.html#mlflow.pyfunc.PythonModelContext).
- `predict()` runs model inference on the given input and returns the model's output; here an input pandas DataFrame with column `question` specifying the query question and column `text` from which the answer will be extracted is turned into a pandas Series of predicted answers.

Full documentation is available [here](https://mlflow.org/docs/latest/python_api/mlflow.pyfunc.html#mlflow.pyfunc.PythonModel).

```python
import os

import mlflow.pyfunc

class TransformersQAWrapper(mlflow.pyfunc.PythonModel):

    def load_context(self, context):
        from transformers import AutoTokenizer, AutoModelForQuestionAnswering, AutoConfig
        self.tokenizer = AutoTokenizer.from_pretrained(context.artifacts["tokenizer_dir"],
                                                       config=AutoConfig.from_pretrained(
                                                           os.path.join(context.artifacts["tokenizer_dir"], "tokenizer_config.json")))
        self.tansformer_model = AutoModelForQuestionAnswering.from_pretrained(context.artifacts["model_dir"], return_dict=True)

    def predict(self, context, model_input):
        import pandas as pd
        import torch
        answers = []
        for _, row in model_input.iterrows():
            question = row["question"]
            text = row["text"]
            inputs = self.tokenizer(question, text,
                                    add_special_tokens=True, return_tensors="pt")
            input_ids = inputs["input_ids"].tolist()[0]
            text_tokens = tokenizer.convert_ids_to_tokens(input_ids)
            model_output = self.tansformer_model(**inputs)
            answer_start_scores = model_output.start_logits
            answer_end_scores = model_output.end_logits
            answer_start = torch.argmax(
                answer_start_scores
            )  # Get the most likely beginning of answer
            answer_end = torch.argmax(answer_end_scores) + 1  # Get the most likely end of answer
            answer = tokenizer.convert_tokens_to_string(
                tokenizer.convert_ids_to_tokens(input_ids[answer_start:answer_end]))
            answers.append(answer)
        return pd.Series(answers)
```

Lastly, we save the model locally, including a conda environment specifying its dependencies:

```python
from sys import version_info

PYTHON_VERSION = "{major}.{minor}.{micro}".format(major=version_info.major,
                                                  minor=version_info.minor,
                                                  micro=version_info.micro)
import torch
import pandas
import transformers

conda_env = {
    'channels': ['defaults'],
    'dependencies': [
      'python={}'.format(PYTHON_VERSION),
      'pip',
      {
        'pip': [
          'mlflow',
          'pandas=={}'.format(pandas.__version__),
          'torch=={}'.format(torch.__version__),
          'transformers=={}'.format(transformers.__version__),
        ],
      },
    ],
    'name': 'transformers_qa_env'
}


# Save the MLflow Model
mlflow_pyfunc_model_path = "transformers_qa_mlflow_pyfunc"
mlflow.pyfunc.save_model(
        path=mlflow_pyfunc_model_path, python_model=TransformersQAWrapper(), artifacts=artifacts,
        conda_env=conda_env)
```

And we can then load and query the model locally:

```python
loaded_model = mlflow.pyfunc.load_model(mlflow_pyfunc_model_path)

import pandas as pd
test_data = pd.DataFrame({"text": ["My name is Alex, I am 32 and live in Copenhagen."] * 3,
                          "question": ["What is my name?", "How old am I?", "Where do I live?"]})
test_predictions = loaded_model.predict(test_data)
print(test_predictions)
```

which returns as pandas Series of predictions:

```
0          alex
1            32
2    copenhagen
dtype: object
```

## Deploying the model

Deployment works via docker and follows three steps:

```python

```

## Querying the model


## Further documentation

- [MLflow deployment tools docs](https://mlflow.org/docs/latest/models.html#id16)
- [MLflow example packaging an XGBoost model](https://mlflow.org/docs/latest/models.html#example-saving-an-xgboost-model-in-mlflow-format)
- [Extractive Question Answering](https://huggingface.co/transformers/task_summary.html#extractive-question-answering)


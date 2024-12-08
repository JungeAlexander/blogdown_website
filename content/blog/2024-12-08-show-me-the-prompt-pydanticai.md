---
title: 'Show me the prompt: PydanticAI'
author: 'Alexander Junge'
date: '2024-12-08'
slug: show-me-the-prompt-pydanticai
categories:
  - prompts
tags:
  - ai
  - llm
  - agents
  - pydantic
  - pydanticai
  
draft: false
---

I am a happy user of [pydantic](https://github.com/pydantic/pydantic) and [instructor](https://useinstructor.com),
the latter being a well scoped tool with a well-defined surface area to use pydantic for structured outputs and validation for large language models.
This week saw the release of [PydanticAI](https://ai.pydantic.dev) that, to me, looks like a nice abstraction to build agents on top of pydantic -
I think this islargely because of my preexisting bias to use pydantic and instructor already.

## More agent frameworks than production-ready agents?

However, I am pretty skeptical of the current state of agent frameworks and if and how they are used in production.
To me, these frameworks feel like they are not quite hitting the right abstraction level.
They make it *really* easy to build an agent for your social media demo
but then you spend *days* just trying to figure out how it works behind the scences and how to do that one
seemingly minor change in your prompt flow, put in some extra validation, etc.
I think it is even fair to argue that there now exist more Python AI agent frameworks, than value-adding AI agents exist in production.

So in the spirit of Hamel Husain's great blog post from February entitled ['F*ck You, Show Me The Prompt.'](https://hamel.dev/blog/posts/prompt/),
let's take a look at what's really going on behind a simple prompt in PydanticAI.

## Show me the prompt, PydanticAI

This example uses PydanticAI's [weather agent example](https://ai.pydantic.dev/examples/weather-agent/)
which I slightly modified (my code is [here](https://github.com/JungeAlexander/demo-pydantic-ai/blob/main/examples/weather_agent.py)).
Since I wanted to see the raw JSON request sent to OpenAI (or my litellm proxy),
I used [langfuse](https://langfuse.com) to get to the prompt.
I also tried using Pydantic Co.'s new [logfire](https://pydantic.dev/logfire) (just to take a look)
but could not figure out a way to see the raw JSON request, so I stayed with langfuse.

So here it what it looks like (comments are my annotation)
```json
{
    "tools": [
        { # 1) The first tool/function derives from the lat/long Python function
            "type": "function",
            "function": {
                "name": "get_lat_lng",
                "parameters": {
                    "type": "object",
                    "required": [
                        "location_description"
                    ],
                    "properties": {
                        "location_description": {
                            "type": "string",
                            "title": "Location Description",
                            "description": "A description of a location."
                        }
                    },
                    "description": "Get the latitude and longitude of a location.",
                    "additionalProperties": false
                },
                "description": "Get the latitude and longitude of a location."
            }
        },
        { # 2) The second tool/function derives from the weather Python function
            "type": "function",
            "function": {
                "name": "get_weather",
                "parameters": {
                    "type": "object",
                    "required": [
                        "lat",
                        "lng"
                    ],
                    "properties": {
                        "lat": {
                            "type": "number",
                            "title": "Lat",
                            "description": "Latitude of the location."
                        },
                        "lng": {
                            "type": "number",
                            "title": "Lng",
                            "description": "Longitude of the location."
                        }
                    },
                    "description": "Get the weather at a location.",
                    "additionalProperties": false
                },
                "description": "Get the weather at a location."
            }
        },
        { # 3) The response model is just a tool/function, too!
            "type": "function",
            "function": {
                "name": "final_result",
                "parameters": {
                    "type": "object",
                    "title": "AnswerModel",
                    "required": [
                        "answer"
                    ],
                    "properties": {
                        "answer": {
                            "type": "string",
                            "title": "Answer"
                        }
                    }
                },
                "description": "Model for the answer."
            }
        }
    ],
    "messages": [ # Nothing interesting here.
        {
            "role": "system",
            "content": "Be concise, reply with one sentence."
        },
        {
            "role": "user",
            "content": "What is the weather like in London and in Wiltshire?"
        }
    ]
}
```

Unsuprisingly, Python functions are turned into tools/functions via JSON schemas.
Where it gets interesting is that the response model is turned into a tool/function named `final_result`.
So as a developer, you need to be aware that *your response model docstring and Field descriptions need to reflect that*.

## So what?

I think the most satifying piece of information is that PydanticAI is not doing any magic here.
It is just using the same JSON schemas derived from Python functions or  pydantic models that you already know and love from instructor.
Looking ahead, I am curious to see where PydanticAI is heading next.
My hope is that the library will stay focused on the things it is already doing, rarther than being a tool trying to cover all the things people do with agents.
For example, I like their focus on [testing and evals](https://ai.pydantic.dev/testing-evals/).
On the other hand, I see some things that feel like they might be a step too far, e.g. [appending system prompts](https://ai.pydantic.dev/agents/#system-prompts) at runtime based on execution order - thi is just screaming for trouble.

## Code

If you want to play with the code, here it is:
[https://github.com/JungeAlexander/demo-pydantic-ai](https://github.com/JungeAlexander/demo-pydantic-ai)

For the actual implementation of how response models are handled,
go [here](https://github.com/pydantic/pydantic-ai/blob/13b9382b388bc717757b5597556f92fd6072ab4b/pydantic_ai_slim/pydantic_ai/agent.py#L129).
Although since the library is still young this link will probably be outdated rather quickly.

# TODO:

- Blog post publish
- Link to post from GitHub
- LinkedIn message draft, X, Mastodon, Bluesky -> short, engineering heavy
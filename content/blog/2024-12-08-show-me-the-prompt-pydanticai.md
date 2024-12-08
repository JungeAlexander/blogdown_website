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

- Hamel Husain has a great blog post from February entitled ['F*ck You, Show Me The Prompt.'](https://hamel.dev/blog/posts/prompt/)
- Big fan of [pydantic](https://github.com/pydantic/pydantic) and [instructor](https://useinstructor.com) (structured outputs using pydantic in a very well scoped tool with a well-defined surface area)
- Interested in [PydanticAI](https://ai.pydantic.dev) that looked like a nice abstraction - I think largely because of my preexisting bias to use pydantic and instructor already.
- However, nagging uncertainty that I have with all Python agentic AI framework is that they have not found the right abstraction-level in that they make it possible to get a basic agent done in an hour but then you spend days just figuring out how to to simple things. In other words, they are too inflexible for anything custom (which is what I mostly care about)
- especially since if feels like there are more 'AI agent frameworks' than value-adding AI agents exist in production
- Looking at the examples I thought PydanticAI might have that right abstraction level
- Also looking for inspiration to roll custom agentic framework using pydantic in a while - so PydanticAI could be great inspiration.

- used PydanticAI's weather agent example: https://ai.pydantic.dev/examples/weather-agent/
- getting to the prompt using [langfuse](https://langfuse.com)
- tried using pydantic new [logfire](https://pydantic.dev/logfire) (just to take a look) but could not figure out a way to see the raw JSON request sent to OpenAI (or my litellm proxy), so I switched to langfuse

What does the prompt look like?

```json
{
    "tools": [
        {
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
        {
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
        {
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
    "messages": [
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

- there is no magic here - but that's a good thing
- response model ist just a tool - so be aware that your response model docstring and Field descriptions need to reflect that
- how differentiate tool schemas and response schemas
- Looking forward to seeing where PydanticAI is heading next and keeping an eye on it, I do believe that most of the 'AI agent frameworks' are rather getting in the way of engineering agents, so I am somewhat skeptical but maybe Pydantic AI can change that, for example, I like their focus on [testing and evals](https://ai.pydantic.dev/testing-evals/) on the other hand, [appending system prompts](https://ai.pydantic.dev/agents/#system-prompts) at runtime based on execution order feels like it can easily get you into trouble

Code

[https://github.com/JungeAlexander/demo-pydantic-ai](https://github.com/JungeAlexander/demo-pydantic-ai)

For the actual implementation in PydanticAI, go here (although since the library is still young this link will probably be outdated rather quickly): https://github.com/pydantic/pydantic-ai/blob/13b9382b388bc717757b5597556f92fd6072ab4b/pydantic_ai_slim/pydantic_ai/agent.py#L129

# TODO:

- Blog post publish
- Link to post from GitHub
- LinkedIn message draft, X, Mastodon, Bluesky -> short, engineering heavy
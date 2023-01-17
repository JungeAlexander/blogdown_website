---
title: 'TIL: f-string formatting - a cheat sheet'
author: 'Alexander Junge'
date: '2023-01-17'
slug: til-yes
categories:
  - TIL
tags:
  - Python
  - f-strings
draft: false
---

I am a big fan of [f-strings](https://docs.python.org/3/tutorial/inputoutput.html#formatted-string-literals) in Python.
If you are not using them yet, you [should](https://realpython.com/python-f-strings/#go-forth-and-format)!

f-strings come with a string formatting syntax that makes it very convenient to create nicely formatted strings.
For example, you can introduce padding:

```python
>>> v = "test"
>>> f"{v:>20}"
'                test'
```

Or you can format dates and print weekdays instead:

```python
>>> from datetime import datetime
>>> print(f"Happy {datetime.now():%A}!")
Happy Monday!
```

Or round you can round floating point numbers:

```python
>>> print(f"{1.1111:.2f}")
1.11
```

The trouble is that I ALWAYS forget this syntax and need to search online every single time.
BUT: this is now solved thanks to this awesome cheatsheet I discovered today: https://fstring.help/cheat/
🤩

I also recommend reading the underlying [article](https://www.pythonmorsels.com/string-formatting/) from
Python Morsel (which provides awesome weekly Python exercises but that's a post for another day). 
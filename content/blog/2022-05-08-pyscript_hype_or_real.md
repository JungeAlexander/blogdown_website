---
title: 'PyScript: beyond the hype'
author: 'Alexander Junge'
date: '2022-05-08'
slug: pyscript_hype_or_real
categories:
  - programming
  - beginners
  - democratization
tags:
  - Python
  - PyScript
  - JavaScript
draft: false
---

PyScript was announced on April 30th at PyCon US by Peter Wang, CEO of Anaconda Inc., and in an accompanying [blog post](https://engineering.anaconda.com).
The announcement sparked a lot of interest and excitement in the Python community and, frankly, generated a lot of hype.
To be clear, PyScript is not a JavaScript replacement (and it does not want to be).
Instead, PyScript aims to further democratize the *superpower* of programming by bringing Python to every browser - which is a SUPER important goal and I very much hope this will work out.

In this post, I will summarize my experience with PyScript so far and give some examples to help you get started:

## PyScript brings Python and its ecosystem to the browser

At is core, PyScript allows you add Python code to your HTML by introducing a new `<py-script>` tag:

```html
<html>
     ...
    <py-script> print('Hello World!') </py-script>
</html>
```

A "Hello World" example is rendered via GitHub Pages[here]( https://jungealexander.github.io/pyscript-demo/) with source code [here](https://github.com/JungeAlexander/pyscript-demo/blob/main/index.html).

What is HUGE is that you can simply use your favorite Python packages by defining a `<py-env>` tag:

```html
<html>
  ...
      <py-env>
        - numpy
        - pandas
        - seaborn
      </py-env>
      ...
</html>
```

Which looks like this:
![seaborn in the browser using PyScript](/posts/2022-05-08/seaborn_pyscript.png)

Again, this is Python running in a browser - including numpy, pandas, and seaborn/matplotlib - 🤯
See the [html version](https://jungealexander.github.io/pyscript-demo/seaborn_pairplot.html) and [source code](https://github.com/JungeAlexander/pyscript-demo/blob/main/seaborn_pairplot.html).

You can even load your own `.py` files:

```html
<html>
  ...
  <py-env>
    - paths:
      - https://raw.githubusercontent.com/JungeAlexander/pyscript-demo/main/helpers.py
  </py-env>
  <body>
    <py-script>
from helpers import say_hello

print(say_hello("Stranger")) 
    </py-script>
      ...
</html>
```

## It feels like a tech demo

From a tech perspective, PyScript builds on [Pyodide](https://pyodide.org/), a port of CPython to WebAssembly/WASM which in turn is supported by modern browsers.
For now, PyScript feels like a (really cool) tech demo: loading PyScript websites is a little slow, it is experimental and there is a growing list of [issues and fixes](https://github.com/pyscript/pyscript/issues).

## Summary

PyScript for sure is real and cool but there is a lot of hype, too.
Only time will tell if PyScript will live up to its goals and
democratize programming further by bringing Python to every browser out there.
There is still [a lot to be done](https://twitter.com/pwang/status/1521138505983959040) but PyScript for sure is fun to explore and I look
forward to seeing where things are going.

## Further reading

- [GitHub repository for this post](https://github.com/JungeAlexander/pyscript-demo)
- [Official Getting started with PyScript on GitHub](https://github.com/pyscript/pyscript/blob/main/GETTING-STARTED.md)
- [A few slides from the PyConUS announcment talk in a Twitter thread](https://twitter.com/pwang/status/1521137668003880961)
- [PyCon US YouTube channel where the announcement talk should be posted soon](https://www.youtube.com/channel/UCMjMBMGt0WJQLeluw6qNJuA)

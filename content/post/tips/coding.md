---
title: coding
date: 2019-09-06T17:31:12+08:00
lastmod: 2019-09-06T17:31:12+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---

# Coding memo

Write down code that I should be able to write at will

## python logging basic setup

```python
import logging

logger = logging.getLogger()
logging.basicConfig(
	level=logging.INFO,
	format="[%(asctime)s] [%(process)d] [%(levelname).3s] %(message)s",
	datefmt="%Y-%m-%d %H:%M:%S %z")
```

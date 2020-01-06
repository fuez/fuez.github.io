---
title: "kafka"
date: 2019-10-30T18:32:54+08:00
lastmod: 2019-10-30T18:32:54+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## How to view kafka headers?

```sh
kafkacat -b kafka-broker:9092 -t my_topic_name -C \
  -f '\nKey (%K bytes): %k
  Value (%S bytes): %s
  Timestamp: %T
  Partition: %p
  Offset: %o
  Headers: %h\n'
```

---
title: redis
date: 2019-06-08T10:36:33+08:00
lastmod: 2019-08-12T18:01:23+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## Trouble shooting: "Asynchronous AOF fsync is taking too long (disk is busy?). Writing the AOF buffer without waiting for fsync to complete, this may slow down Redis"

The blogs about this issue, [redis 持久化 AOF和 RDB 引起的生产故障](https://www.cnblogs.com/yangxiaoyi/p/7806406.html)
A quick fix is to tune `vm.dirty_bytes` by `sysctl vm.dirty_bytes=33554432` or `echo vm.dirty_bytes=33554432 >> /etc/sysctl.conf` and `sysctl -p`

## redis-cli basic comands:
	- List all keys: `KEYS *`, or some patther `KEYS sess*`
	- Get value for a key: `GET <key>`
	- Set value for a key: `SET <key> <value>`

## connect to redis host with password: `redis-cli -h 10.233.39.39 -a redis0111`

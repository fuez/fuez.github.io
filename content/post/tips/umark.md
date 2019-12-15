---
title: umark
date: 2019-03-07T16:33:33+08:00
lastmod: 2019-06-21T18:24:23+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---

 for trouble shooting

## Chaoyang block height around 458000, orderer around 10G, and peer around 12G

## k8s2 is down for unkonwn reason

Even when the second master server is down, k8s cluster will not work correctly: helm command won't work, ceph-rbd and cephfs provisioning does not work, some kubedns which tries to connect to that master server will crash.

So the lesson is not to schedule any task to the secondary master server.

## When build server on HongKong can't build docker image due to DNS resolve issue such as `Could not resolve host: yarnpkg.com`

1. From host machine, it was OK to access and `curl` target url
2. Started intermediate container, and checked its resolve file, which turneed out to be the same as the host
3. Used `dig` command to check target DNS name, and it shows the name could be resolved by the first DNS server
4. Suspected that it was because of docker engine issue, so restart the docker engine, and the issue disappeared

## [API explorer to manage resources on tencent cloud?](https://console.cloud.tencent.com/api/explorer?Product=cvm&Version=2017-03-12)

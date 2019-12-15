---
title: ppc64
date: 2017-06-13T11:48:45+08:00
lastmod: 2017-06-13T11:48:45+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---


## [How to set up yum repo for centos?](https://wiki.centos.org/SpecialInterestGroup/AltArch/ppc64le)

## [How to deploy Kubernets?][101]

## How to install docker on centos?
First add the internal  yum repo for docker: 
```
[docker]
name=Docker
baseurl=http://ftp.unicamp.br/pub/ppc64el/rhel/7_1/docker-ppc64el/
enabled=1
gpgcheck=0
```
Then install docker with yum: `yum install -y docker-io`


## References:
## [Build and Deploy an OpenPower based Kubernetes Cluster](http://cloudgeekz.com/890/build-and-deploy-kubernetes-cluster-openpower.html)

[101]: http://cloudgeekz.com/890/build-and-deploy-kubernetes-cluster-openpower.html
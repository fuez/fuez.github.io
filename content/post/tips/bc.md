---
title: bc
date: 2017-06-13T11:48:45+08:00
lastmod: 2017-06-13T11:48:45+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---


## Code study
## [viper](https://github.com/spf13/viper) is great configuration framework for golang.

## Setup
## Need to install docker on ubuntu base image
In China, first need to update sources.list (using aliyun repo:  https://launchpad.net/ubuntu/+mirror/mirrors.aliyun.com-archive),
`sudo vim /etc/apt/sources.list +:%s/us\.archive\.ubuntu\.com/mirrors\.aliyun\./com/`
Then `sudo apt-get update` to update apt
```
# add apt key for packages from docker.com
curl -fsSL https://get.docker.com/gpg | sudo apt-key add -
# install
curl -fsSL https://get.docker.com/ | sh
```

## In order to pull image such as _hyperledger/fabric-baseimage:x86_64-0.0.10_, try to first pull on laptop through socks5 proxy (socks5://localhost:10086), and then save to a file, and load inside vagrant box

## Need to install docker compose on base image
Docker compose is actually a python module.
```
sudo apt-get install -y python-pip
sudo pip install docker-compose
```

# Reference
## [ethereum](https://github.com/ethereum/go-ethereum)
## [ethereum white-paper](https://github.com/ethereum/wiki/wiki/White-Paper)
## [hyperledger white-paper](https://docs.google.com/document/d/1Z4M_qwILLRehPbVRUsJ3OF8Iir-gqS-ZYe7W-LE9gnE/pub)
## [Next Consensus Architecture Proposal](https://github.com/hyperledger/fabric/wiki/Next-Consensus-Architecture-Proposal)
## [How The Blockchain Will Transform Everything From Banking To Government To Our Identities](http://www.forbes.com/sites/laurashin/2016/05/26/how-the-blockchain-will-transform-everything-from-banking-to-government-to-our-identities/#1c40654b65d9)

## Blogs
## [Banking Is Only The Start: 20 Big Industries Where Blockchain Could Be Used](https://www.cbinsights.com/blog/industries-disrupted-blockchain/)

## Build
## build and start with docker-compose
Just type `sudo docker-compose up`

## Research
### RocksDB
## [Basics](https://github.com/facebook/rocksdb/wiki/RocksDB-Basics)
## [Merge Operator](https://github.com/facebook/rocksdb/wiki/Merge-Operator)

### Secondary Index
## [NoSQL is Great, But You Still Need Indexes](https://www.percona.com/blog/2013/02/20/nosql-is-great-but-you-still-need-indexes/)

## [Diff-Index: Differentiated Index in Distributed Log-Structured Data Stores](http://researcher.ibm.com/researcher/files/us-wtan/DiffIndex-EDBT14-CR.pdf)

### LSM
## [How does the Log-Structured-Merge-Tree work?](https://www.quora.com/How-does-the-Log-Structured-Merge-Tree-work)
## [The Log-Structured Merge-Tree (LSM-Tree)](http://www.cs.umb.edu/~poneil/lsmtree.pdf)

### Nosql
## [A deep dive into NoSQL: A complete list of NoSQL databases](http://bigdata-madesimple.com/a-deep-dive-into-nosql-a-complete-list-of-nosql-databases/)

---
title: "mongodb"
date: 2018-08-28T13:18:38+08:00
lastmod: 2019-11-12T08:57:46+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## [Install mongodb on mac](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

```sh
brew tap mongodb/brew
brew install mongodb-community@4.2
```

## [Mastering MongoDB — Introducing multi-document transactions in v4.0](https://hackernoon.com/mongodb-transactions-5654cdb8fd24)

The script to create 1 member replica set mongod:

```sh
curl -O https://downloads.mongodb.com/osx/mongodb-osx-x86_64-enterprise-4.0.0-rc0.tgz
tar -xvzf mongodb-osx-x86_64-enterprise-4.0.0-rc0.tgz
rm mongodb-osx-x86_64-enterprise-4.0.0-rc0.tgz
mv mongodb-osx-x86_64-enterprise-4.0.0-rc0  v4.0.0-rc0
mkdir data
v4.0.0-rc0/bin/mongod --dbpath data --logpath data/mongod.log --fork --replSet rs0 --port 38000
v4.0.0-rc0/bin/mongo --port 38000 --eval "rs.initiate()"
```

## Create a new db user on a certain db:

First logon to that mongodb instance's shell, like this: ` mongo mongodb://localhost:27017/admin -uroot`, then

```sh
use tide
db.createUser({ user:"tide", pwd: "Abcd1234", roles:["readWrite", "dbAdmin"]})

```

## [Configuration File Settings and Command-Line Options Mapping](https://docs.mongodb.com/manual/reference/configuration-file-settings-command-line-options-mapping/)

## How to connect to auth enabled server? like this: ` mongo 127.0.0.1:27017/admin -u root -p www.umarkcloud.com`

## How to grant user access to some database?
To grant user *pass* created in *admin* access to *bass* db, run the following command:

```
use admin
db.grantRolesToUser('paas',[{ role: "readWrite", db: "bass" }])
```

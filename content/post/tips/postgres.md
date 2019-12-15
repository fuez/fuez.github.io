---
title: postgres
date: 2017-06-13T11:48:45+08:00
lastmod: 2017-06-13T11:48:45+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---


## Postgres equivalent to MySQL's \G?
It's a toggle. Just input `\x` to turn on/off.

## Install on centos:
```
yum install -y postgresql-server
postgresql-setup initdb
```
After installation and init, logon as _postgres_ user and run _psql_ to get started by `sudo -u postgres psql`

## How to fix _psql: FATAL: Ident authentication failed for user “username”_?

More from [ident auth failed](http://www.cyberciti.biz/faq/psql-fatal-ident-authentication-failed-for-user/)
Edit file _vi /var/lib/pgsql/data/pg_hba.conf_, and allow login via localhost:
```
# "local" is for Unix domain socket connections only
local   all             all                                     trust
# IPv4 local connections:
host    all             all             127.0.0.1/32            trust
# IPv6 local connections:
host    all             all             ::1/128                 trust
```

## [How to allow remote access?](http://www.cyberciti.biz/tips/postgres-allow-remote-access-tcp-connection.html)

First, need to edit _/var/lib/pgsql/data/postgresql.conf_
```
# - Connection Settings -

listen_addresses = '*'      # what IP address(es) to listen on;
                    # comma-separated list of addresses;
                    # defaults to 'localhost'; use '*' for all
                    # (change requires restart)
port = 5432         # (change requires restart)
```
And also add the following line to allow access from network 9.111.0.0/16 to __vi /var/lib/pgsql/data/pg_hba.conf__
```
host    all     all         9.111.0.0/16        trust
```

## How to switch to another database inside _psql_?
Connect to another database by `\c dbname`

## References:
## [How To Use PostgreSQL with your Django Application on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-use-postgresql-with-your-django-application-on-ubuntu-14-04)
    + On RHEL, need to install _postgresql-devel_ instead of _libpq-dev_
## [Install and Configure PostgreSQL](http://www.marinamele.com/taskbuster-django-tutorial/install-and-configure-posgresql-for-django)
## [How To Install and Use PostgreSQL on CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-centos-7)

---
title: mysql
date: 2017-06-13T11:48:45+08:00
lastmod: 2018-06-07T13:20:10+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---

###Tips
## [How to dump mysql schema and data?](https://dev.mysql.com/doc/mysql-backup-excerpt/5.7/en/mysqldump-sql-format.html)
dump commands:
```
shell> mysqldump [arguments] > file_name
shell> mysqldump --all-databases > dump.sql
```
Load commands:
```
shell> mysql < dump.sql
or
mysql> source dump.sql

```

## How to connect to mysql client?
Like this: ` mysql -u USERNAME -pPASSWORD -h HOSTNAMEORIP DATABASENAME`
NOTE: please do not use *localhost*, instead you should use *127.0.0.1* for local access.
Refer to [ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock'](https://help.memsql.com/hc/en-us/articles/115001215603-ERROR-2002-HY000-Can-t-connect-to-local-MySQL-server-through-socket-var-run-mysqld-mysqld-sock-)

## [mysql quick start](http://www.coderguide.com/Guides:MySQL/MySQL_Quick_Start_Guide)
    + `show databases`
    + `use database_name`
    + `show tables`

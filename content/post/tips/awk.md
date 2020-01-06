---
title: "awk"
date: 2017-06-13T11:48:45+08:00
lastmod: 2019-09-12T08:32:55+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## Example: How to match line and do something?

Run `go fmt` to format code for hejia-pilot project:`git ls-files | awk '$1 ~ /go/ { system( "go fmt $GOPATH/src/github.ibm.com/bc/hejia-pilot/"$1)}'`

Run `cat big_files/oxford_dict.txt  | awk '$1 ~ /[^a-z\-.]+/ {print $0, NR}'` to show abnormal words

## How to use shell variable in awk?
Use *-v* option to define a awk varible like the following:

```
VAR=3
echo | awk -v env_var="$VAR" '{print "The value of VAR is " env_var}'
```

## How to test awk without explicit input?
Just use keyword _BEGIN_, like the following:
`awk 'BEGIN { print "Here is a single quote <'\''>" }'`

## [How to add quote or double quote into output?](https://www.gnu.org/software/gawk/manual/html_node/Quoting.html)
Just use _\42_ for double quote, and _\47_ for quote, the following is an example:
`awk '{db=$10; sub(/ds/,"d",db);print("INSERT INTO clouddb_tenant(db_name, dbspace_name) values(\42"db"\42,\42" $10"\42);")}' old.txt  > add.sql`

---
title: "php"
date: 2017-06-13T11:48:45+08:00
lastmod: 2017-06-13T11:48:45+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---

### Tips
## [Compiling shared PECL extensions with phpize](http://php.net/manual/en/install.pecl.phpize.php)

```
$ cd extname
$ phpize
$ ./configure
$ make
# make install
```

## How to diagnose php?

```sh
cat << EOF > test.php
<?php phpinfo() ?>
EOF
php test.php

```

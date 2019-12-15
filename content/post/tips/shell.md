---
title: shell
date: 2018-08-13T10:41:52+08:00
lastmod: 2019-12-13T19:20:17+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## How can I make "press any key to continue"?

`read -p "Press enter to continue"` to press enter to continue

`read -n 1 -s -r -p "Press any key to continue"` to press any key to continue, where `-n` defines the required character count to stop reading, `-s` hides the user's input, `-r` causes the string to be interpreted "raw" (without considering backslash escapes)

## `SCRIPTDIR=$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )` to get the current script dir

## use `read` to read input from stdin like this: `read -p "you name please:" -r NAME`

## Why `#!/usr/bin/env bash` is better than `#!/bin/bash`

> `#!/usr/bin/env` searches PATH for bash, and bash is not always in /bin, particularly on non-Linux systems.

## [Bash Automated Testing System](https://github.com/bats-core/bats-core) could be used to test bash scripts like this:

```sh
#!/usr/bin/env bats
@test "addition using bc" {
  result="$(echo 2+2 | bc)"
  [ "$result" -eq 4 ]
}

@test "addition using dc" {
  result="$(echo 2 2+p | dc)"
  [ "$result" -eq 4 ]
}
```

It is a nodejs package, and can be installed via `brew` like this: `brew install bats`, or `npm install -g bats`

## [About shell parameter substituation](https://www.cyberciti.biz/tips/bash-shell-parameter-substitution-2.html)
The syntax is:
```
${var#Pattern}
${var##Pattern}
```
You can strip $var as per given pattern from front of $var. The first syntax removes shortest part of pattern and the second syntax removes the longest part of the pattern.
For example, for k8s sts created POD, it will have POD name like _peer-0_, to get the index of that POD, use `${HOSTNAME##*-}`, please note that _*_ is needed here to match any char before _-_

## Difference between `${varname:=value}` and `${varname=value}`

The first one sets varname to value if varname is either unset or null, while the other only sets the value of varname if varname is currently unset

## What does colon *:* means in bash?
```
: is a shell builtin that is basically equivalent to the true command. It is often used as a no-op eg after an if statement. 
```

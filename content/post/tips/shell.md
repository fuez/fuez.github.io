---
title: "shell"
date: 2018-08-13T10:41:52+08:00
lastmod: 2020-01-06T13:14:28Z
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---

## Mac iso date format: `date -u +"%Y-%m-%dT%H:%M:%SZ"`

## [Single Brackets vs Double Brackets](https://scriptingosx.com/2018/02/single-brackets-vs-double-brackets/)
> The single bracket [ is actually a command. It has the same functionality as the test command, except that the last argument needs to be the closing square bracket ]
> A single bracket test will fail when one of its arguments is empty and gets substituted to nothing. You can prevent this error by quoting the variables (always a prudent solution)
> Double brackets in bash are not a command but a part of the language syntax.
> With double brackets you can also use two equals characters == for a more C like syntax. (or, better, use ((…)) syntax for arithmetic expressions)
> Since the single bracket is a command, many characters it uses for its arguments need to be escaped to work properly; Double brackets interpret these characters properly. You can also use the (again more C like) && and || operators instead of -a and -o: ` [[ ( $a == $b ) || ( $a == $c ) ]]`
> With double brackets you can compare to * and ? wildcards, and bracket globbing […]. `a=hat; [[ $a = ?at ]] && echo match; [[ $a = [chrp]at ]] && echo match`
> You can also use < and > to compare strings lexicographically
> And you get an operator =~ for regular expressions

## How to check string starts with some sub-string?
> The == comparison operator behaves differently within a double-brackets
> test than within single brackets. See [Other Comparison Operators](http://tldp.org/LDP/abs/html/comparison-ops.html)

```shell
[[ $a == z* ]]   # True if $a starts with a "z" (wildcard matching).
[[ $a == "z*" ]] # True if $a is equal to z* (literal matching).
```

## `LC_ALL` 
> It forces applications to use the default language for output
> and forces sorting to be byte-wise
> `LC_ALL` is the environment variable that overrides all the other localisation settings

## What does `echo 'POWERLEVEL9K_DISABLE_CONFIGURATION_WIZARD=true' >>! ~/.zshrc` means

> cmd >>! file will append stdout to file, creating the file if it does not already exist
> If noclobber is not set then the ! has no effect

## Mac iso date format: `date -u +"%Y-%m-%dT%H:%M:%SZ"`

## [Single Brackets vs Double Brackets](https://scriptingosx.com/2018/02/single-brackets-vs-double-brackets/)
> The single bracket [ is actually a command. It has the same functionality as the test command, except that the last argument needs to be the closing square bracket ]
> A single bracket test will fail when one of its arguments is empty and gets substituted to nothing. You can prevent this error by quoting the variables (always a prudent solution)
> Double brackets in bash are not a command but a part of the language syntax.
> With double brackets you can also use two equals characters == for a more C like syntax. (or, better, use ((…)) syntax for arithmetic expressions)
> Since the single bracket is a command, many characters it uses for its arguments need to be escaped to work properly; Double brackets interpret these characters properly. You can also use the (again more C like) && and || operators instead of -a and -o: ` [[ ( $a == $b ) || ( $a == $c ) ]]`
> With double brackets you can compare to * and ? wildcards, and bracket globbing […]. `a=hat; [[ $a = ?at ]] && echo match; [[ $a = [chrp]at ]] && echo match`
> You can also use < and > to compare strings lexicographically
> And you get an operator =~ for regular expressions

## How to check string starts with some sub-string?
> The == comparison operator behaves differently within a double-brackets
> test than within single brackets. See [Other Comparison Operators](http://tldp.org/LDP/abs/html/comparison-ops.html)

```shell
[[ $a == z* ]]   # True if $a starts with a "z" (wildcard matching).
[[ $a == "z*" ]] # True if $a is equal to z* (literal matching).
```

## `LC_ALL` 
> It forces applications to use the default language for output
> and forces sorting to be byte-wise
> `LC_ALL` is the environment variable that overrides all the other localisation settings

## Redirect output to stderr: `exec 1>&2`

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

---
title: "makefile"
date: 2018-09-06T13:50:45+08:00
lastmod: 2019-12-06T17:10:09+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## [Two flavours of variables](https://www.gnu.org/software/make/manual/html_node/Flavors.html)
> The first flavor of variable is a recursively expanded variable. Variables of this sort are defined by lines using ‘=’ (see Setting Variables) or by the define directive.
> If it contains references to other variables, these references are expanded whenever this variable is substituted.
> To avoid all the problems and inconveniences of recursively expanded variables, there is another flavor: simply expanded variables.
> Simply expanded variables are defined by lines using ‘:=’ or ‘::=’. Both forms are equivalent in GNU make; however only the ‘::=’ form is described by the POSIX standard
> The value of a simply expanded variable is scanned once and for all, expanding any references to other variables and functions, when the variable is defined. The actual value of the simply expanded variable is the result of expanding the text that you write. It does not contain any references to other variables; it contains their values as of the time this variable was defined.
> When a simply expanded variable is referenced, its value is substituted verbatim.

## [Setting Variables](https://www.gnu.org/software/make/manual/html_node/Setting.html)
> If you’d like a variable to be set to a value only if it’s not already set, then you can use the shorthand operator ‘?=’ instead of ‘=’
> The shell assignment operator ‘!=’ can be used to execute a shell script and set a variable to its output. This operator first evaluates the right-hand side, then passes that result to the shell for execution. If the result of the execution ends in a newline, that one newline is removed; all other newlines are replaced by spaces. The resulting string is then placed into the named recursively-expanded variable. Like this: `hash != printf '\043'`
> Alternatively, you can set a simply expanded variable to the result of running a program using the shell function call, which I think is better. Like this: `hash := $(shell printf '\043')`

## You can make template make target together with `%` like this:

```shell
.PHONY: push-%
push-%:
	$(MAKE) GOARCH=$* docker
	docker push $(DOCKER_IMAGE_NAME):$(DOCKER_IMAGE_TAG)-$*

.PHONY: push
push: manifest-tool $(addprefix push-,$(ALL_ARCH)) manifest-push
```

## Should learn something about Makefile [file name functions](https://www.gnu.org/software/make/manual/html_node/File-Name-Functions.html)
> Each of the following functions performs a specific transformation on a file name. The argument of the function is regarded as a series of file names, separated by whitespace.
	- `$(dir names…)` to Extracts the directory-part of each file name in names. 
	- `$(notdir names…)` to Extracts all but the directory-part of each file name in names.
	- `$(suffix names…)` to Extracts the suffix of each file name in names. 
	- `$(basename names…)` to Extracts all but the suffix of each file name in names.
	- `$(addsuffix suffix,names…)` - The value of suffix is appended to the end of each individual name and the resulting larger names are concatenated with single spaces between them
	- `$(addprefix prefix,names…)`
	- `$(join list1,list2)`
	- `$(wildcard pattern)` - The result of wildcard is a space-separated list of the names of existing files that match the pattern. 
	- `$(realpath names…)`
	- `$(abspath names…)`

## [What does **?=** means?](http://www.gnu.org/software/make/manual/html_node/Setting.html)

> Variables defined with ‘=’ are recursively expanded variables. Variables defined with ‘:=’ or ‘::=’ are simply expanded variables, only the ‘::=’ form is described by the POSIX standard.

> If you’d like a variable to be set to a value only if it’s not already set, then you can use the shorthand operator ‘?=’ instead of ‘=’.

> The shell assignment operator ‘!=’ can be used to execute a shell script and set a variable to its output.

## [Getting environment variables into GNU Make](https://www.cmcrossroads.com/article/basics-getting-environment-variables-gnu-make)

A simple test example:

```sh
echo "\$(warning \$(FOO) \$(origin FOO))" > /tmp/Makefile
FOO=hello make -f /tmp/Makfile -e FOO=world
```

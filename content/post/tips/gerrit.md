---
title: gerrit
date: 2019-04-30T13:03:29+08:00
lastmod: 2019-04-30T13:03:29+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## [Enable github OAUTH](https://hub.packtpub.com/using-gerrit-github/)

>We can register a new application in GitHub through the URL https://github.com/settings/applications/new, where the following three fields are requested:
Application name : It is the logical name of the application authorized to access GitHub, for example, Gerrit.
Main URL : The Gerrit canonical web URL used for redirecting to GitHub OAuth authentication, for example, https://myhost.mydomain:8443.
Callback URL : The URL that GitHub should redirect to when the OAuth authentication is successfully completed, for example, https://myhost.mydomain:8443/oauth.

## Configure gerrit to replicate from/to github

```sh
# logon as root to gerrit container
# make sure to change exampleorg to your github name
alias sug="su-exec gerrit2"
sug git config -f "${GERRIT_SITE}/etc/replication.config" gerrit.autoReload "true"
sug git config -f "${GERRIT_SITE}/etc/replication.config" gerrit.replicateOnStartup "true"
# make sure to use single quote for the following configuration to avoid ${name} being expanded by shell
sug git config -f "${GERRIT_SITE}/etc/replication.config"  remote.github.url 'git@github.com:<exampleorg>/${name}.git'
sug git config -f "${GERRIT_SITE}/etc/secure.config" remote.github.password  20fbc89e4c66d92382a05ec0351c2e29ef8f1443

#  if you use nodePort, you need to change 29418 to that nodePort
alias gssh="ssh -p 29418 <USERNAME>@<GERRIT_HOST>"
# install plugin from latest build, but might fail
# in this case, logon to that serrver, copy it to gerrit/plugins directory, and restart Gerrit.
gssh gerrit plugin install -n replication.jar   https://gerrit-ci.gerritforge.com/job/plugin-replication-bazel-stable-2.14/lastSuccessfulBuild/artifact/bazel-genfiles/plugins/replication/replication.jar

# if the above nmethod does not work
# Try to add GERRIT_INIT_ARGS="--install-plugin=replication" to env

```

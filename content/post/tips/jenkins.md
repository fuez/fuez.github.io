---
title: "jenkins"
date: 2018-10-25T13:29:32+08:00
lastmod: 2019-05-15T15:48:26+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## Add/override environment variables in pipeline job

```groovy
  withEnv(["IMAGE_FABRIC_PEER=${imageTag}", "JOB_NAME=fabric-pr-${env.CHANGE_ID}", "CI_NAMESPACE=staging-0"]) {
	sh "pytest --tb=line -v -s -x -m paas_smoke"
  }
```

## Jenkins pipeline [*Lockable Resource Plugin*](https://jenkins.io/doc/pipeline/steps/lockable-resources/) provides way to lock a certain global resource to avoid race condition.

This could be used to prevent concurrent execution of dedicated resource such as docker engine.

## Jenkins slave for *kubernetes* plugin is: *jenkins/jnlp-slave:alpine*

## How to delete *Blue Ocean* access token?

>Steps to delete the Access-Token:
Go to the 'old' interface (i.e. not Blue Ocean)
Click on your username in the top right
Click on Credentials on the left
You should see a entry with the domain blueocean-github-domain the id github and the name `<username>/******` (Github Access Token). Delete it.

## [How to migrate jenkins jobs?](https://wiki.jenkins.io/display/JENKINS/Administering+Jenkins)

Just migrate jobs from `$JENKINS_HOME/jobs` should work.

## Add a linux jenkins slave node

  Need to install `ssh-slaves` plugin to allow *lauch agents via ssh*
  Need to add private key to a ssh credential to allow master to connect to slave, and before that, make sure rsa public key is authorized on slave.

## Issue: fail to resolve update server when starting jenkins master by `docker-compose`

Fixed by adding [`dns`](https://docs.docker.com/compose/compose-file/#dns) entries to docker-compose file. The strange thing is that 

## [Jenkins Job DSL API viewer](https://jenkinsci.github.io/job-dsl-plugin/#)

## [Introduction to Jenkins CasC](https://docs.google.com/presentation/d/1VsvDuffinmxOjg0a7irhgJSRWpCzLg_Yskf7Fw7FpBg/edit#slide=id.g3c22764f3b_2_1217)

And its project in [github](https://github.com/jenkinsci/configuration-as-code-plugin)

## How to fix error when connecting to url such as *https://jenkins.umarkcloud.com/job/fabric/job/PR-111/4/display/redirect* so show result?

The problem here might be because `Jenkins Blue Ocean plugin` could not be loaded due to some out of date dependencies. So the fix is to update plugins.

## How to enable PR build for multibranch pipeline job?
>  had this issue as well. I tried what joey suggested, but that did not work. I found out that if you add PR-.+ (or `PR-*` as a wildcard) to your regex filter for branches to include, the pull requests "magically" appear.

The officail support document mentioned this [here](https://support.cloudbees.com/hc/en-us/articles/115003019232-GitHub-Webhook-Pipeline-Multibranch)

At the same time, need to enable *Pull requests* event for github webhook setting for that repository.
And need to add an additional configuration section in *Branch Sources* called *Discover pull requests from origin*.
For *Strategy*, better to select *Exclude branches that are also filed as PRs*

## Official introduction about [github branch source plugin](https://go.cloudbees.com/docs/plugins/github-branch-source/)

## Check out code from certain branch

```groovy
checkout changelog: false, poll: false,
            scm: [$class: 'GitSCM', branches: [[name: "${GERRIT_BRANCH}"]],,
        extensions: [[$class: 'CleanBeforeCheckout']],
        doGenerateSubmoduleConfigurations: false,
        userRemoteConfigs: [[credentialsId: credentialsId, refspec: "${GERRIT_REFSPEC}", url: project_repo_url]]]

```

## [Jenkins env vars](https://jenkins.umarkcloud.com/env-vars.html/)

## [How to add github ssh key to jenkins](https://mohitgoyal.co/2017/02/27/configuring-ssh-authentication-between-github-and-jenkins/)

## [10 best practices about Jenkins pipeline](https://www.cloudbees.com/blog/top-10-best-practices-jenkins-pipeline-plugin)

## In jenkins pipeline, there is a global variable called currentBuild which is defined in [RunWrapper.java](https://github.com/jenkinsci/workflow-support-plugin/blob/master/src/main/java/org/jenkinsci/plugins/workflow/support/steps/build/RunWrapper.java)

## How to configure dingding notification for Jenkins?
From [Jenkins plugin](https://wiki.jenkins.io/display/JENKINS/Dingding+Notification+Plugin), which could only be applied to freestyle job. Still, its [source code](https://github.com/jenkinsci/dingding-notifications-plugin/tree/master/src/main/java/com/ztbsuper/dingding) is worth reading.

For pipeline job, referred to [github gist](https://gist.github.com/dongfg/a171bcbba33a871b2f1eb2d73d8fd10e), but need to install [HTTP Request plugin](https://stackoverflow.com/questions/37945370/how-to-post-json-data-in-body-with-jenkins-http-request-plugin-and-pipeline)

For message format,  refer to [钉钉开放平台- 自定义机器人](https://open-doc.dingtalk.com/docs/doc.htm?spm=a219a.7629140.0.0.karFPe&treeId=257&articleId=105735&docType=1)
## How to [Add linux slave node in the Jenkins](https://mohitgoyal.co/2017/02/14/add-linux-slave-node-in-the-jenkins/)

```
# Install docker engine

# Install jdk
yum install -y java-1.8.0-openjdk
#install git
yum install http://opensource.wandisco.com/centos/6/git/x86_64/wandisco-git-release-6-1.noarch.rpm
yum install -y git
# add jenkins user on slave machine
```

When adding agent node, please also specify *java* path in *Advance* option like this:
```
/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.171-8.b10.el7_5.x86_64/jre/bin/java
```

## How to fix error in Jenkins pipeline:*No such DSL method 'findFiles'*

```
Turns out I first had to install the `pipeline-utility-steps` plugin before these steps became available.
```
More: checkout more [pipeline step plugins](https://jenkins.io/doc/pipeline/steps/)

## Find a good write about how to write declarative pipeline and easily enable various notifications: [Declarative Pipeline: Notifications and Shared Libraries](https://jenkins.io/blog/2017/02/15/declarative-notifications/)

## Migrate Jenkins notes:
  + Looks like directly copy a thinBackup full back to JENKINS_HOME does not work, caused by file permission issue
  +  Work around by changing permission: `chown -R 1000:2003 FULL-2018-01-11_06-59`

## Fix Kubernetes slave could not connect to Jenkins as slave issue
  +  It is not easy to change NodePort range as that might crash API server
  +  Jenkins agent listening port could be configured by environment var: *JENKINS_SLAVE_AGENT_PORT* 


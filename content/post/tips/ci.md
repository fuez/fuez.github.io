---
title: "ci"
date: 2017-06-13T11:48:45+08:00
lastmod: 2017-11-13T15:42:49+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---

## Blocks
### Vagrant
## [Create base box](https://www.vagrantup.com/docs/boxes/base.html)
## [How to set up a self-hosted "vagrant cloud" with versioned, self-packaged vagrant boxes](https://github.com/hollodotme/Helpers/blob/master/Tutorials/vagrant/self-hosted-vagrant-boxes-with-versioning.md)


## CircleCI support s skipping builds
CircleCI supports the [ci skip] or [skip ci] standard for ignoring builds.
When code changes are pushed to your VCS provider (GitHub or Bitbucket), if the last commit has [ci skip] or [skip ci] anywhere in the commit message title or description, then none of the commits in that push will be built automatically by CircleCI.
## [CircleCI: How to create separate steps/jobs for PR/Forks versus branches?](https://discuss.circleci.com/t/create-separate-steps-jobs-for-pr-forks-versus-branches/13419)
```
if [[ ((`echo $CIRCLE_BRANCH | grep -c "pull"` > 0))]]; then 
          echo "Skip doing stuff since it is a PR."
        else
          echo "Not a PR, so now do what I want."
        fi
```

## References:
-[Gerrit Code Review - Quick get started guide](https://gerrit-review.googlesource.com/Documentation/install-quick.html)
## [Gerrit Code Review - Project Configuration](https://gerrit-review.googlesource.com/Documentation/project-configuration.html)
 Manually create "allprojects" like this `git --git-dir=$base_path/allprojects.git init`
 - [HOW TO EDIT THE PROJECT.CONFIG FOR ALL PROJECTS IN GERRIT](http://blog.bruin.sg/2013/04/how-to-edit-the-project-config-for-all-projects-in-gerrit/)
 - [Gerrit access control](https://gerrit-review.googlesource.com/Documentation/access-control.html)
 - [configure gerrit in system level](https://gerrit-review.googlesource.com/Documentation/config-gerrit.html)
 - [gerrit sudo](https://gerrit-review.googlesource.com/Documentation/cmd-suexec.html)

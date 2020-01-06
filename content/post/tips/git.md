---
title: "git"
date: 2017-06-13T11:48:45+08:00
lastmod: 2019-12-17T23:35:05Z
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---

## [How to share git hooks](https://www.darrenlester.com/blog/including-hooks-in-a-git-repository)
> By default hooks are stored in .git/hooks outside of the working tree and are thus not shared between users of the repository. 
> Git versions >= 2.9 provide a helpful configuration option which changes the path Git looks for hooks in. Like this: `git config core.hooksPath <hooks-dir>`

## [How to download a single folder from a Github Repo](https://stackoverflow.com/questions/7106012/download-a-single-folder-or-directory-from-a-github-repo)

Basically there are three ways to do this:
> Git doesn't support this, but Github does via SVN. If you checkout your code with subversion, Github will essentially convert the repo from git to subversion on the backend, then serve up the requested directory.
> For example, modify from `https://github.com/lodash/lodash/tree/master/test` to `https://github.com/lodash/lodash/trunk/test`, then download with `svn checkout https://github.com/lodash/lodash/trunk/test`
> Or, use [GitZip](http://kinolien.github.io/gitzip/)
> Or, use [DownGit](https://minhaskamal.github.io/DownGit/#/home)

NOTE: if it is not master branch, you should use `branches/<branch>` to replace `tree/<branch>`

	SVN trunk --- Git master (refs/heads/master)
	SVN branches/* --- Git branches (refs/heads/*)
	SVN tags/* --- Git tags (refs/tags/*)

## [git subtree教程](https://segmentfault.com/a/1190000012002151)
> add a new subtree: `git subtree add --prefix=sub/libpng https://github.com/test/libpng.git master —squash`

> update subtree: `git subtree pull --prefix=sub/libpng https://github.com/test/libpng.git master —squash`

> push change to subtree: `git subtree push --prefix=sub/libpng https://github.com/test/libpng.git master`

> add a remote to simplify command: `git remote add -f libpng https://github.com/test/libpng.git`, after than: `git subtree push --prefix=sub/libpng libpng master` to push change

## [An example to enable githooks](https://github.com/wspr/fontspec/tree/master/githooks)

## [Work around for `go get` for private repo](https://gist.github.com/dmitshur/6927554)

```sh
git config --global url."git@github.com:".insteadOf "https://github.com/"
# or more specific
git config --global url."git@github.com:<some org>".insteadOf "https://github.com/<some org>"
```

## Login github with access token?

For personal access token, when `git clone`, should use user name as `git`

## How to remove a submodule?

```sh
git submodule deinit <path_to_submodule>
git rm <path_to_submodule>
git commit-m "Removed submodule "
rm -rf .git/modules/<path_to_submodule>
```

## How to update a submodule to latest: `git submodule update --rebase --remote`

## How to show git branch in graph style?

Create two aliases in ~/.gitconfig file like this:

```ini
[alias]
lg1 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all
lg2 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)' --all
lg = !"git lg1"
```

This means I can also add other git alias such as the followings:

```ini
[alias]
pr = ! "git pull -r"
aa = ! "git commit -a --amend"
rs = ! "git reset --soft"
```

BTW, to show all git alias, just type `git config --get-regex alias`

## How to show full history of renamed/moved file?

use `git log` command with *--follow* option, like this: `git log --follow <file>`

## How to fetch remote branch such as refs/meta/config

Use git command like this ( note that remote prefix is *refs/remotes/origin* )

```sh
# the syntax should be something like this
git fetch origin ${remote branch or ref}:${local ref name}"

git fetch origin refs/meta/config:refs/remotes/origin/meta/config
git checkout meta/config

# or like this
git fetch origin refs/meta/config
git checkout FETCH_HEAD -b meta/config

```

## How to connect to multiple github account on mac?

Just configure git to use osxkeychain to store password

```
git config  --global credential.helper osxkeychain
git config  --global credential.useHttpPath true
```

## [Enforce git ssh over HTTPS(443) port](https://help.github.com/articles/using-ssh-over-the-https-port/)
Add the following line to *~/.ssh/config* file:
```
Host github.com
  Hostname ssh.github.com
  Port 443
```
## How to pull all issues from a private repo?
Just call curl like this to get a resulted JSON file:
```
curl -i "https://api.github.com/repos/umarkcloud/devops/issues?access_token=<access token>"
```
## How to cherry pick just some files?
First cherry-pick commit with *-n* option first, and then reset to HEAD by `git reset HEAD` command, after than, `git add` any file you want to include to commit.
## How to merge from other branches and handle conflict?
A recommended way is to first do *merge* and then choose whether to accept *theirs* file by file, like this:
```
Much safer would be to handle the conflict in the regular way:

git merge release-7.15.1 -m "merge release-7.15.1 to master"
...and ensure that the only conflict that arises is the 'release' file. When that happens, do a checkout:

git checkout --ours ./release
```

## [How to lockdown some critical branch?](https://help.github.com/articles/about-protected-branches/)
```
 A protected branch:
Can't be force pushed
Can't be deleted
Can't have changes merged into it until required status checks pass
Can't have changes merged into it until required reviews are approved
Can't be edited or have files uploaded to it from the web
Can't have changes merged into it until changes to files that have a designated code owner have been approved by that owner
```

## [How to auto merge pull request on github?](https://stackoverflow.com/questions/29015464/how-to-auto-merge-pull-request-on-github)
You can most probably add an after_success action to your .travis.yml that would merge the PR using [GitHub API](https://developer.github.com/v3/pulls/#merge-a-pull-request-merge-button). I do not know of any ready to use script for this, but there is no reason for it to be hard. Special care needed for authentication ...
Also refer to [this post](https://stackoverflow.com/questions/40391344/automatically-merge-verified-and-tested-github-pull-requests)
## How to remove a submodule?
```
git submodule deinit <asubmodule>    
git rm <asubmodule>
# Note: asubmodule (no trailing slash)
# or, if you want to leave it in your working tree
git rm --cached <asubmodule>
rm -rf .git/modules/<asubmodule>
```

## How to configure vimdiff as git diff tool?
```
git config --global diff.tool vimdiff
git config --global color.diff auto
git config --global core.editor vim
```
## [How to remove git tag?](https://nathanhoad.net/how-to-delete-a-remote-git-tag)
    + Remove local tag: `git tag -d v1.1.1`
    + Remove remote one: `git push origin :refs/tags/v1.1.1`
## [How to add git tag?](https://git-scm.com/book/en/v2/Git-Basics-Tagging)
    + Add a new tag: `git tag v1.1.1` or with comment `git tag -a v1.1.1 -m "1.1.1 release"`
    + Push tag to server: `git push --tags`
## [How to update submodules?](http://stackoverflow.com/questions/1030169/easy-way-pull-latest-of-all-submodules)
    + `git submodule update --recursive --remote`
    + `git submodule foreach git pull orgin master`
    + First time: `git submodule update --init --recursive`
## [How to manually remove a submodue and fix isse _No submodule mapping found in .gitmodules for path..._?](http://stackoverflow.com/questions/4185365/no-submodule-mapping-found-in-gitmodule-for-a-path-thats-not-a-submodule)
    + `git ls-files --stage | grep 160000`
    + Then `git rm <dangling submodule path>`
## How to show Chinese characters on MacBook?
On terminal, `export LANG="en_US.UTF-8"`
## [GitHub Tricks: Upload Images & Live Demos](http://solutionoptimist.com/2013/12/28/awesome-github-tricks/)
    + Upload images to github issues
    + Create a new orphan branch called gh-pages for demo
## How to merge code when _git rebase_?
To accept theirs:
`git status | awk '$0 ~ /both (added|modified)/ {  system("git checkout --their "$3)}'`
Another option to use strategy:
`git rebase --strategy <s>`
## Fix the is
## sue: _fatal: unable to access 'https://github.com/gmarik/vundle.git/': Peer's Certificate has expired._
    + Make sure you have installed latest ca-certificates: `yum install -y ca-certificates`
    + Check local time in VM, to make sure the time is synced, and install ntpd:
```
sudo yum -y install ntp
sudo chkconfig ntpd on
sudo ntpdate time.apple.com
```

## [Build static documentation from a GitHub Wiki](http://www.embeddedlog.com/static-docs-from-github-wiki.html)
## [Automating GitHub Pages Builds with MkDocs](https://mwop.net/blog/2016-01-29-automating-gh-pages.html)
## [Creating Project Pages manually](https://help.github.com/articles/creating-project-pages-manually/)
## [How to delete remote branch](http://stackoverflow.com/questions/2003505/how-to-delete-a-git-branch-both-locally-and-remotely)
First run `git push origin --delete <branchName>` to delete the branch, and then to propagate change on other machiens, run `git fetch --all --prune`

## [How to remove large files from history](http://stackoverflow.com/questions/2100907/how-to-remove-delete-a-large-file-from-commit-history-in-git-repository)
With help from [BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/)
## How to set upstream branch?
```
git branch -u upstream/foo
Or, if local branch foo is not the current branch:

git branch -u upstream/foo foo
Or, if you like to type longer commands, these are equivalent to the above two:

git branch --set-upstream-to=upstream/foo

git branch --set-upstream-to=upstream/foo foo
```

## How to fetch all branches?

`git fetch --all`

## How to use socket proxy to clone code? (maybe not work)

Just add a global option for that like this:
```
git config --global http.proxy 'socks5://localhost:10086'
git config --global https.proxy 'socks5://localhost:10086'

# unset
# git config --global --unset http.proxy
```

## How to show patch (detail changes)?
Use _-p_ option, such as `git log -p <file>`

## [How to use pull requests?](https://help.github.com/articles/using-pull-requests/)

## How to fix issues when apply patch with `git am`?
http://www.pizzhacks.com/bugdrome/2011/10/deal-with-git-am-failures/

Find the so called 0001 patch file, edit it, and use `git apply` to apply the patch,
then  `git commit ...` and `git am --abort`

## How to clone with proxy?
Use _http_proxy_ environment variable, like the following
`http_proxy=146.184.0.115:8080 go get github.com/hyperledger/fabric`

## Reference
-[Git and Gerrit in Action](https://www.jboss.org/dms/judcon/presentations/Boston2011/JUDConBoston2011_day1track2session6.pdf)
-[Git Submodules: Adding, Using, Removing, Updating](https://chrisjean.com/git-submodules-adding-using-removing-and-updating/)
 + `git submodule update --init --recursive`

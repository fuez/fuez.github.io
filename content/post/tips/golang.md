---
title: golang
date: 2017-11-13T15:42:49+08:00
lastmod: 2019-12-09T17:48:47+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## In golang, channel actually works like a semaphore, see this example code from alertmanager:

```go
type API struct {
	v1                       *apiv1.API
	v2                       *apiv2.API
	requestsInFlight         prometheus.Gauge
	concurrencyLimitExceeded prometheus.Counter
	timeout                  time.Duration
	inFlightSem              chan struct{}
}
```

## Misunderstanding on file write

For normal file, write usually will just overwrite file from starting position, but won't truncate the file, unless you open file with `os.O_TRUNC`

## What is the golang build binary name

For golang, the build binary name is default to current folder name whether main package is put.

## [Tools to embed static assets in Go](https://tech.townsourced.com/post/embedding-static-files-in-go/)

The recommend `go-bindata` fork is `go get -u github.com/shuLhan/go-bindata/...`

## How to fix the "You are not currently on a branch" error for `go get`?
> Delete the source from your $GOPATH and run go get again. There are other ways to fix the error but this is a simple solution.

## golang package managment tool: [Dep](https://golang.github.io/dep/docs/daily-dep.html)

```shell
# ensure command to ensure package sync
dep ensure
# ensure -add to manually add a package
dep ensure -ad github.com/foo/bar
# ensure -update to update all packages or some packages
dep ensure -update
# edit  [Gopkg.toml](https://golang.github.io/dep/docs/Gopkg.toml.html) to control version of packages
# status command together with graphviz to visualize dependencies
brew install graphviz
dep status -dot | dot -T png | open -f -a /Applications/Preview.app
```

## How to fix weird *govendor* package issue?

First move *vendor* folder to somewhere else such as /tmp/, then make sure you can build the package without *vendor* folder.
After that, rebuild *vendor* folder by `govendor init`, and `govendor add +e`
And for `docker build`, make sure golang version matches

## [How to disable some warnings from golint?](https://github.com/golang/lint/issues/186)
Add the following to vscode preference:

```json
"go.lintTool": "gometalinter",
"go.lintFlags": ["--config=${workspaceRoot}/.gometalinter"]
```

And in the _.gometalinter_ file, add _Exclude_ like the following:

```json
  "Exclude": [
      "vendor/",
      "func (\\S*) should be (\\S*['.]*)",
      "exported \\w+ (\\S*['.]*)([a-zA-Z'.*]*) should have comment or be unexported"
  ],

```

## [How to `go get` private repos?](https://michaelheap.com/golang-how-to-go-get-private-repos/)

```
$ git config --global url."git@github.com:".insteadOf "https://github.com/"
$ cat ~/.gitconfig
[url "git@github.com:"]
    insteadOf = https://github.com/
```

## How to forcely run golang test cases?

Run command `go clean -testcache` to expires all test results,
or use environment variable `GOCACHE=off`

## How to fix error: `invalid flag in #cgo LDFLAGS: -I/usr/local/share/libtool`?
Add the following CGO flag to environment variables:

```
CGO_LDFLAGS_ALLOW="-I.*"
```

## How to fix *ltdl.h file not found* error?

```
brew install libtool openssl
```

## How to trouble shooting go build/test freeze?

```
You probably have dependencies that are being recompiled each time. Try go install -a mypackage to rebuild all dependencies.

Removing $GOPATH/pkg also helps to ensure you don't have old object files around.

Building with the -x flag will show you if the toolchain is finding incompatible versions.
```

## How to build go binary with _static link_?
Add _ldflags_ option like `go build --ldflags '-extldflags "-static"'`

## How to build go v1.6 on ppc64le?
  + First install at9.0 (advanced toolchain), which includes gcc5.3.1 and go v1.4
  + Clone go code to path such as ~/dev/go
  + Change current path to ~/dev/go/src
  + Check out to go1.6 brach: `git checkout go1.6 -b v1.6` (you can check with `git tag`)
  + Build go with command: `GOROOT_BOOTSTRAP=/opt/at9.0/ ./make.bash`
  + The above command will install go to _~/dev/go_; to package it, remove __.git__ directory first, or just type:`tar -cvzf ~/go.v1.6.tgz  --exclude .git --exclude test .`
  + Add ~/dev/go/bin to _PATH_ like this: `export PATH=~/dev/go/bin:$PATH`

## What is a good REPL for go?
Check out [gore](github.com/motemen/gore)

## Make sure to enable __Enable vendoring__ mode on _Intellij IDE_ ( in Project Settings tab) to enable symbol lookup, which is critical for code reading.

## [vim plugin for go](https://github.com/fatih/vim-go)

## [How to start a local godoc web server?](https://minhajuddin.com/2012/11/30/automatically-start-a-local-godoc-web-server/)
    + `godoc -http=:8090`
    + Add a upstart daemon file to enable running as daemon at start

    ```
        #Upstart script to start godoc server
        #Author: Khaja Minhajuddin
        #*This obviously works on machines with upstart*
        start on runlevel [2345]
        stop on runlevel [06]
        script
          export HOME=/home/minhajuddin
          export GOROOT=$HOME/go
          export GOBIN=$GOROOT/bin
          export GOPATH=$HOME/gocode
          $GOBIN/godoc -http=:8090 -index=true
        end script
    ```

    + Then seek help by web brower, or type : `godoc net listener`

## KB
## go command: `vet`:
```
Vet examines Go source code and reports suspicious constructs, such as Printf calls whose arguments do not align with the format string. Vet uses heuristics that do not guarantee all reports are genuine problems, but it can find errors not caught by the compilers.
```
## go command: `goimports`:
```
Command goimports updates your Go import lines, adding missing ones and removing unreferenced ones.
```
## [go list: your swiss army knife](https://dave.cheney.net/2014/09/14/go-list-your-swiss-army-knife)
And also [godoc](https://golang.org/cmd/go/#hdr-List_packages) about this.
Example to call *join* function to print in lines: `go list -f "{{ join .Imports \"\\n\"}}"`. 
Note: here *join* actually is *strings.join*.

## Reference:
## [Provided a list of good go books](https://github.com/dariubs/GoBooks)
## [Make a restful json api in golang](http://thenewstack.io/make-a-restful-json-api-go/)
## [effective go](https://golang.org/doc/effective_go.html)
## [go code review comments](https://github.com/golang/go/wiki/CodeReviewComments)
## [go by example](https://gobyexample.com)
    + [string format](https://gobyexample.com/string-formatting)
## [Interfaces: the awesomesauce of Go](http://go-book.appspot.com/interfaces.html)
## [Go Data Structures: Interfaces](http://research.swtch.com/interfaces)
## [The right way to handle YAML in Go](http://ghodss.com/2014/the-right-way-to-handle-yaml-in-golang/)
  + struct "tags" :a Go construct to add metadata to a struct's fields.
  + [deal with both json and yaml](https://github.com/ghodss/yaml)
## [Structuring Applications in Go](https://medium.com/@benbjohnson/structuring-applications-in-go-3b04be4ff091#.19icfx4jf)
## [Creating Web Applications with Go]
## [GoesToEleven on github](https://github.com/GoesToEleven)
## [vim-go tutorial](https://github.com/fatih/vim-go-tutorial)
## [godebug](https://blog.cloudflare.com/go-has-a-debugger-and-its-awesome/)
## [Understanding and using the vendor folder](https://blog.gopheracademy.com/advent-2015/vendor-folder/)
## [govendor](https://github.com/kardianos/govendor)

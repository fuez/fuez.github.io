---
title: "fabric"
date: 2017-06-13T11:48:45+08:00
lastmod: 2018-12-05T12:26:00+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---




## Fix issue: "fatal error: ltdl.h: No such file or directory"

On centos: `yum install libtool-ltdl-devel`
On mac: `brew install libtool`
On ubuntu: `sudo apt install libtool libltdl-dev`

## [Design documents for fabric gossip protocol](https://jira.hyperledger.org/browse/FAB-170)

## How to fix issue: *bzip2 data invalid: bad magic value in continuation file*?
```
cd $GOPATH/src/github.com/hyperledger/fabric
brew install gnu-tar --with-default-names
brew install libtool
make clean
make peer

```
## How to configure logging for node sdk?
`export HFC_LOGGING='{"error": "console","debug": "console","info": "console"}'`

## The reference code for fabric **BLOCK** structure:
The code to assemble a block [CreateSignedTx](hyperledger/fabric/protos/utils/txutils.go)
Another good reference is: [block.js](fabric-client/lib/Block.js), which is not implemented in Java sdk
## How to set up fabri-sdk-node build environment?
    - Needs to install *gulp* and related tools: `npm install gulp gulp-shell gulp-add-src --registry=https://registry.npm.taobao.org`
    - Then just run `gulp`, and it will start build, deployment and test process.

### set up dev env for 1.0
* Fixes on fabric vagrant base image

    ```
# use watson proxy
export http_proxy=http://webproxy.watson.ibm.com:8080
export https_proxy=http://webproxy.watson.ibm.com:8080
# if you when to build fabric, try to build as root,
sudo -i
# Prepare for couchdb
sudo apt update
sudo apt install -y couchdb-bin
    ```

* How to build fabric
    * For devenv, should run scripts in fabric-baseimage repo for common.
    * For build, should first run govendor sync 
    * At home, when there is no direct way to download golang and google packages, you can sync package from remote machine, and then cd to gotools fold, and “make” to build gotools.
    * To force “make” again, use “-B” option.

* How to set up dev environment
    * Build fabric base image for Vagrant
        * Download Vagrant 1.9.1 for Debian: _https://releases.hashicorp.com/vagrant/1.9.1/vagrant_1.9.1_x86_64.deb_
        * Install package: `dpkg -i vagrant_1.9.1_x86_64.deb`
        * Download packer from: _https://releases.hashicorp.com/packer/0.12.2/packer_0.12.2_linux_amd64.zip_
        * Unzip packer to /usr/local/bin:unzip packer_0.12.2_linux_amd64.zip -d /usr/local/bin/
        * Blocked on error: *** Error connecting to atlas server, please check your ATLAS_TOKEN env: authentication failed**
    + Download and set up dev environment: `vagrant init hyperledger/fabric-baseimage; vagrant up --provider virtualbox`
## Tool to show dependency in a Makefile: _https://github.com/lindenb/jsandbox/blob/master/src/sandbox/MakeGraphDependencies.java_, or _https://github.com/lindenb/makefile2graph_
    + Install _makefile2graph_ and _make2graph_ to /usr/local/bin/
    + Install [graphviz](http://www.graphviz.org/Download_macos.php)
    + Run `makefile2graph > a.dot` and open _a.dot_ with _graphviz_

### tips noticed when reading code
## In _docker-env.mak_ make file utility, it defined DUMMY variable as `DUMMY = .dummy-$(DOCKER_TAG)`
## Reason for _.PHONY_ target in Makefile[https://www.gnu.org/software/make/manual/html_node/Phony-Targets.html]:
    + To avoid conflict with a file of the same name
    + Improve performance to avoid build each time

### Deployment steps
## [Deploy based on docker]()
## [Build chain code](https://hyperledger-fabric.readthedocs.io/en/latest/Setup/Chaincode-setup/#chaincode-deploy-via-cli-and-rest): change directory to project go directory and run `go build` to build. For example smart contract project, please refer to `github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02` 
## Run chaincode: 
## Register a chaincode: 
#### Deploy steps for real mode
`docker exec blockchain_vp0_1  mkdir -p /opt/gopath/src/github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02`

`docker cp chaincode_example02.go blockchain_vp0_1:/opt/gopath/src/github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02/`

`docker exec -it blockchain_vp0_1 bash`

`peer chaincode deploy -p github.com/hyperledger/fabric/examples/chaincode/go/chaincode/go/chaincode_example02/chaincode_example02.go -c '{"Function":"init", "Args": ["a","100", "b", "200"]}' -n cc_ex02`

`peer chaincode invoke -l golang -n cc_ex02 -c '{"Args": ["invoke", "a", "b", "10"]}'`
`peer chaincode query -l golang -n cc_ex02 -c '{"Args": ["query", "b"]}'`


#### Trouble shooting guide
## Start cc container failed due to time out error:
```
Error: Error endorsing chaincode: rpc error: code = 2 desc = Timeout expired while starting chaincode hejia1:1.1(networkid:dev,peerid:devorg.easy-sight.com,tx:2858f24706981788550aae91ad4d014753b7351393770424b0483ed5a1dfb319)
```
Trouble shooting tips:
    - Check whether there is cc container created by `docker ps -a`, and if yes, check that container's log by `docker logs -f <container name>`
The code to start VMProcess could be improved to check whether cc container is exited due to error like: fail to connect to peer

---
title: "fe"
date: 2019-06-18T10:12:52+08:00
lastmod: 2019-11-27T13:56:30+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## Need to install geckodriver for firefox webdriver to work: `brew install geckodriver`

## How to select element on chrome?

	- Type in XPath like $x(".//header") to evaluate and validate.
	- Type in CSS selectors like $$("header") to evaluate and validate.

## Change npm registry to taobao:`npm config set registry https://registry.npm.taobao.org`

## w3schools [css selector reference](https://www.w3schools.com/cssref/css_selectors.asp)

## [xpath cheatsheet](https://devhints.io/xpath)
> Some cool stuffs are various *Predicates*, such as *indexing*s: `last()`, `position()`

## Manage nodejs version

Can be managed by [nvm](https://www.sitepoint.com/quick-tip-multiple-versions-node-nvm/) or [n](https://www.npmjs.com/package/n?activeTab=readme)

## How to install nodejs on centos

```sh
# add repo source for node 10
curl -sL https://rpm.nodesource.com/setup_10.x | sudo -E bash -

# install gcc and make
sudo yum install -y gcc-c++ make

# install node
sudo yum install -y nodejs
```

## How to add timestamps to all console messages?

```sh
npm install console-stamp --save
// add timestamps in front of log messages
require('console-stamp')(console, '[HH:MM:ss.l]');
```

## node.js: How to set default npm repostory to taobao?

	- Create a npm configuration file called *.npmrc* under home
	- add this line to the above file: registry = "https://registry.npm.taobao.org"

To check all configurations: `npm config ls -l`

## node.js: How to decode and encode string?

Should take advantage of *Buffer* class.
    - encoding: `let bs = new Buffer('219a0e', 'hex')`
    - decode: `let hs = bs.toString('hex')`

## How to debug nodejs *socket.io* from browser?

It's quite simple. Just open debugger in Chrome, and then in the **console** window, 
create a io socket to the opened socket like this: `var socket = io("http://localhost")`.
Of course, io packge *socket.io-client* should be loaded into client first.

## [How to show deep into a json object?](https://stackoverflow.com/questions/10729276/how-can-i-get-the-full-object-in-node-jss-console-log-rather-than-object)

You need to use `util.inspect()` like the following:

```js
console.log(util.inspect(myObject, {showHidden: false, depth: null}))
// alternative shortcut
console.log(util.inspect(myObject, false, null))
```

See [util.inspect()](https://nodejs.org/api/util.html#util_util_inspect_object_options) doc for detail.

## [Use rewire for easy monkey-patching testing](https://github.com/jhnns/rewire)

## [Understanding requireJS](https://www.sitepoint.com/understanding-requirejs-for-effective-javascript-module-loading/)

## How to fix node package installation failure when compiling with node-gyp on centos?
> Try to install required g++ compiler: `yum upgrade -y && yum install -y gcc-c++ tar`

## How to fix the following *npm install* failure?

```sh
e/Plug-ins/XVim.xcplugin' not present in DVTPlugInCompatibilityUUIDs
  CXX(target) Release/obj.target/native/src/hashtable.o
  In file included from ../src/hashtable.cpp:1:
  ../src/hashtable.h:7:10: fatal error: 'tr1/unordered_map' file not found
  #include <tr1/unordered_map>
           ^
           1 error generated.
           make: *** [Release/obj.target/native/src/hashtable.o] Error 1
```

cause: on macbook, before serrie, the cpp header for `unordered_map` is in tr1.
fix: fork the source code for node-hashtable to `https://github.com/labie/node-hashtable`, and then in package.json file, directly add 
node-hashtable's dependency to this link: `"node-hashtable": "https://github.com/labie/node-hashtable.git",`

## projects

## [How to Create a Chrome Extension in 10 Minutes Flat?](http://www.sitepoint.com/create-chrome-extension-10-minutes-flat/)

## Guide & Blogs

## [Use chrome as your javascript IDE](http://www.jamessugrue.ie/softwaredev/use-chrome-as-your-javascript-ide)

Need to download _Chrome Canary_, and then enable __Developer Tool Experiments__ through _chrome://flags_, lastly, open _Chrome Dev Tools_.

## [10 books on javascript](http://www.alolo.co/blog/2013/10/11/10-books-on-javascript)

## [Setting up a TypeScript + Visual Studio Code development environment](http://blog.wolksoftware.com/setting-up-your-typescript-vs-code-development-environment)

## [JavaScript Promises ... In Wicked Detail](http://www.mattgreer.org/articles/promises-in-wicked-detail/)

## [Four ways of handling asynchronous operations in node.js](http://stevehanov.ca/blog/index.php?id=127)


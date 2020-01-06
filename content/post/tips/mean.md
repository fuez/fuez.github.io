---
title: "mean"
date: 2017-06-13T11:48:45+08:00
lastmod: 2017-06-13T11:48:45+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---


## [What is the difference between a shim and a polyfill?](http://stackoverflow.com/questions/6599815/what-is-the-difference-between-a-shim-and-a-polyfill)
```
Call it shims if you want to keep the directory generic. A polyfill is a type of shim that retrofits legacy browsers with modern HTML5/CSS3 features usually using Javascript or Flash. A shim, on the other hand, refers to any piece of code that performs interception of an API call and provides a layer of abstraction. It isn't necessarily restricted to a web application or HTML5/CSS3.
```
## [How to suppress console output](https://github.com/mochajs/mocha/issues/2107)
    + Use _before_ and _after_ to define functions that would be called before tests and after tests
    + `console.log = function(){}` will make calling `consolo.log` as nop.
## [express generator](https://expressjs.com/en/starter/generator.html)

## reference
## [jade templating language](http://blog.inching.org/2014/03/30/jade/)
## [jade syntax](http://naltatis.github.io/jade-syntax-docs/#basics)
## [learn jade](http://learnjade.com/)
## [start bootstrap](https://startbootstrap.com/template-categories/all/)
## [mongooose schema types](http://mongoosejs.com/docs/2.7.x/docs/schematypes.html)
## [mongo aggregate (join) query](https://docs.mongodb.com/manual/reference/operator/aggregation/lookup/)
## [node debug tool](https://github.com/node-inspector/node-inspector):`npm install -g node-inspector`
## [How to debug with node inspector](http://stackoverflow.com/questions/23340968/debugging-node-js-with-node-inspector)
    + Start node app like: `node --debug app.js`
    + Start node inspector on another session: `node-inspector --web-port=8090`
    + Visit debug web page: _http://127.0.0.1:8090/?port=5858_
    + From _Sources_ tab, set break point as needed.
## [slc to debug nodejs](https://docs.strongloop.com/display/SLC/Debugging+applications): `npm install -g strongloop`
## [Get started with nodejs and mocha](https://semaphoreci.com/community/tutorials/getting-started-with-node-js-and-mocha)

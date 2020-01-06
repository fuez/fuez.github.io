---
title: "cherrypy"
date: 2017-06-13T11:48:45+08:00
lastmod: 2017-06-13T11:48:45+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---

###Tips
## [How to connect your CherryPy server to a database] (http://tools.cherrypy.org/wiki/Databases)
```
1. Use cherrypy.engine.subscribe to tell CherryPy to call a function when each thread starts
2. In that function, create a DB connection for this thread
3. Store the DB connection in cherrypy.thread_data, which is a thread-specific container
4. From your methods, use the DB connection that you stored in cherrypy.thread_data
```

### Reference
## [tutorials](http://docs.cherrypy.org/en/latest/tutorials.html#tutorials)

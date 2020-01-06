---
title: "angularjs"
date: 2017-09-08T10:22:42+08:00
lastmod: 2017-09-08T10:22:42+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---


## [ How to add bootstrap to angular-cli generated projects? ](https://stackoverflow.com/questions/37649164/how-to-add-bootstrap-to-an-angular-cli-project)
```
// First install ngx-bootstrap
npm install ngx-bootstrap bootstrap --save
// Import needed module to src/app/app.module.ts, such as
import { AlertModule } from 'ngx-bootstrap';
...
@NgModule({
...
imports: [AlertModule.forRoot(), ... ],
... 
})
// open .angular-cli.json and insert a new entry into the styles array
 "styles": [
 "styles.css",
 "../node_modules/bootstrap/dist/css/bootstrap.min.css"
 ],
// use it
<alert type="success">hello</alert>

```

---
title: "excel"
date: 2019-02-13T16:14:11+08:00
lastmod: 2019-02-13T16:14:11+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## [How to use if formula to categorize data?](https://www.extendoffice.com/documents/excel/5168-excel-categorize-data-based-on-values.html)
Enter formular like this:

```
IF(A2>90,"High",IF(A2>60,"Medium","Low"))
```

## [How To Categorize Data Based On Values In Excel?](https://www.extendoffice.com/documents/excel/5168-excel-categorize-data-based-on-values.html)
Instead of complex "IF" or "IFS", excel also supports transform data fied by table mapping, with formula *VLOOKUP* like this: `=VLOOKUP(A2,$F$1:$G$6,2,1)` where F and G lie a table for mapping, and the table range is from F1 to G6, and the number 2 indicates the lookup table column number which contains the values you want to return.

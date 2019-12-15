---
title: selenium
date: 2018-11-06T17:28:54+08:00
lastmod: 2018-11-13T12:34:14+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## How to select an item with special text?

Consider the following script:

```html
<myparent>
  <mychild>
    foo
  </mychild>
</myparent>
```

To select the *mychild* element, use `//myparent/mychild[text() = 'foo']` XPath expression;
Alternatively, you can use the shortcut for the self axis:`//myparent/mychild[. = 'foo']`

## How to fix: _selenium.common.exceptions.WebDriverException: Message: 'geckodriver' executable needs to be in PATH_

First of all you will need to download latest executable geckodriver from [here](https://github.com/mozilla/geckodriver/releases) to run latest firefox using selenium.

Similarly, in order to use chrome driver, you need to download its driver first from [here](https://sites.google.com/a/chromium.org/chromedriver/home)



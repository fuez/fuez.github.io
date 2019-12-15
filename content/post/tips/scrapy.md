---
title: scrapy
date: 2017-06-13T11:48:45+08:00
lastmod: 2017-06-13T11:48:45+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## Set up on MacBook
Need to first run `xcode-select --install` to _install requested for command line developer tools_, and then run `pip install scrapy`


## Reference
## [Tutorial: How to use Headless Firefox for Scraping in Linux](http://scraping.pro/use-headless-firefox-scraping-linux/)
```
# install xvfb to simuate hardware display
sudo apt-get install -y xvfb xserver-xephyr
# removes a native Debian browser Iceweasel
apt-get remove iceweasel
# install firefox like this?
echo -e "\ndeb http://downloads.sourceforge.net/project/ubuntuzilla/mozilla/apt all main" | sudo tee -a /etc/apt/sources.list 
sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com C1289A29
sudo apt-get update
sudo apt-get install -y firefox-mozilla-build
sudo apt-get install -y libdbus-glib-1-2 libgtk2.0-0 libasound2
# install python packages, better in a virtualenv
pip install pyvirtualdisplay
pip install selenium
```

## [Web Scraping With Scrapy and MongoDB](https://realpython.com/blog/python/web-scraping-with-scrapy-and-mongodb/)
## [How to get Selenium to wait for page load after a click](http://www.obeythetestinggoat.com/how-to-get-selenium-to-wait-for-page-load-after-a-click.html)
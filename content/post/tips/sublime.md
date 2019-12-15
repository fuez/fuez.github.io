---
title: sublime
date: 2017-06-13T11:48:45+08:00
lastmod: 2017-09-01T18:02:08+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---


## [How to revert to a refreshly installed state?](https://www.sublimetext.com/docs/3/revert.html)
	- Exit program
	- On OSX system, backup and remove *~/Library/Application Support/Sublime Text 3*
	- Start program again
## How to split windows?
Invoke from Menu:  __View > Layout__ or shortcut key (on Mac) : __Alt+CMD+1/2/3__

## How to start from command line?
`ln -s /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl /usr/local/bin/st`

## Show syntax in markdown quote
After _```_, just add syntax alias such as _sh_ for shell, _sql_ for SQL.
It is actually defined in  [languages](https://github.com/github/linguist/blob/master/lib/linguist/languages.yml). Also refer to [syntax highlighting in markdown](https://support.codebasehq.com/articles/tips-tricks/syntax-highlighting-in-markdown)

## How to set cursor size?
Add the following to user/default preference:
```
"caret_extra_bottom": 0, "caret_extra_top": 0, "caret_extra_width": 0,
```
Refer to [cursor too big](https://forum.sublimetext.com/t/cursor-too-big/12179/2) for more detail

## Setup and Good plugins:
## Install package Control
First, invoke command window by _Ctrl+`_, then paste the following python script to command window and run:
```python
import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b4428bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```
Note: you might need to update hash _h_ above for different version of pacakge control.

## How to install new packages?
Press Command-Shift-P (Mac OS X) or Ctrl-Shift-P (Windows) to open the Command Palette

## Enable VIM mode
By removing it from ignored_packages from both "Default" and "User"
"ignored_packages": ["Vintage"], and set  "vintage_start_in_command_mode": true
Refer to [vintage](https://www.sublimetext.com/docs/2/vintage.html) for more details


##  Markdown Editing
Need to customize its color theme by:
```
In order to activate the dark or the yellow theme, put one of these lines to your user settings file of the flavor (Packages/User/[flavor].sublime-settings):

"color_scheme": "Packages/MarkdownEditing/MarkdownEditor-Dark.tmTheme",
"color_scheme": "Packages/MarkdownEditing/MarkdownEditor-Yellow.tmTheme",
```
Actually, the easiest way to fix the color theme is to follow manually installation instructions in [github](https://github.com/SublimeText-Markdown/MarkdownEditing):
```
git clone https://github.com/SublimeText-Markdown/MarkdownEditing.git
ln -s ~/dev/oss/MarkdownEditing/  ~/Library/Application\ Support/Sublime\ Text\ 3/Packages/MarkdownEditing
 #then edit Markdown.sublime-settings manully in the source code
```

## Markdown Preview
## GoSublime
## Anaconda (for python)
## [SublimeCodeIntel](http://sublimecodeintel.github.io/SublimeCodeIntel/)
## [NodeJS](https://packagecontrol.io/packages/Nodejs)

## Reference:
## [Keyboard shortcuts](http://docs.sublimetext.info/en/latest/reference/keyboard_shortcuts_osx.html#text-manipulation)

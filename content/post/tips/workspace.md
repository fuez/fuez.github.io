---
title: "workspace"
date: 2019-06-21T18:24:23+08:00
lastmod: 2019-12-24T23:01:19Z
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---

## How to save and load iterm2 profile?

> If you have a look at Preferences -> General you will notice at the bottom of the panel, there is a setting Load preferences from a custom folder or URL:. There is a button next to it Save settings to Folder.
> So all you need to do is save your settings first and load it after you reinstalled your OS.
> If the Save settings to Folder is disabled, select a folder (e.g. empty) in the Load preferences from a custom folder or URL: text box.
> In iTerm2 3.3 on OSX the sequence is: iTerm2 menu, Preferences, General tab, Preferences subtab

## For IntelliJ IDEA you can select proper profile for maven in this way

> If you import a maven project in intellij, you can choose profiles in the maven tool window (usually on the right of the screen). Once these profiles are selected they will persist in the project configuration for that project and will become effective every time the project loads, until you choose to change them. These profile selections will only affect intellij and instances of maven run within intellij.

## Installed jetbrain plugins list

- BashSupport
- IdeaVim
- Material Theme UI
- Python
- Save Actions
- SonarLint
- Toml


## tmux3 made a breaking change:

> INCOMPATIBLE: tmux's configuration parsing has changed to use yacc(1). There
>  is one incompatible change: a \ on its own must be escaped or quoted as
>  either \\ or '\' (the latter works on older tmux versions).

And also for `style`, see https://github.com/tmux/tmux/wiki/FAQ#how-do-i-translate--fg--bg-and--attr-options-into--style-options

## How to export vscode extention list and bundle install extesions?

`code --list-extensions` to get list of current installed extensions

`code --list-extensions | xargs -L 1 echo code --install-extension` to install listed

## [Google Java Format](https://github.com/google/google-java-format)

My current setting: installed [java format](https://marketplace.visualstudio.com/items?itemName=redhat.java), 
and made the following settings:

```json
    "java.format.enabled": true,
    "java.format.settings.url": "https://raw.githubusercontent.com/google/styleguide/gh-pages/eclipse-java-google-style.xml",
    "java.format.settings.profile": "GoogleStyle",
```

## [The Top 104 Formatter Open Source Projects](https://awesomeopensource.com/projects/formatter)

## [Guides extention for vscode](https://marketplace.visualstudio.com/items?itemName=spywhere.guides)

## How to set different tab size for different file type in vscode?

```json
{
    "[sass]": {
        "editor.tabSize": 2
    },
    "[html]": {
        "editor.tabSize": 4
    },
    "[javascript]": {
        "editor.tabSize": 2
    }
}
```

[More settings](https://code.visualstudio.com/docs/getstarted/settings#_language-specific-editor-settings)

## How to fix vscode Java language server issue to enable intellisense for new project?
> Open the command palette (F1)
> select Java: Clean the Java language server workspace
> click Restart and delete

## The auto-formatting shortcut in IntelliJ

For windows Ctrl+Alt+L.
For ubuntu Ctrl+Alt+windows+L.
For Mac Command+Option+L.

## [vscode python language server fails to start](https://github.com/microsoft/vscode-python/issues/7751)

On my mac, I located directory as "/Users/andyz/.vscode/extensions/ms-python.python-2019.10.41019/languageServer.0.4.71", and add x permission for Microsoft.Python.LanguageServer, but then see error as "The Python Tools server crashed 5 times in the last 3 minutes. The server will not be restarted.", in order to show detail logs, I need to do this: `cmd-shift-p -> Search Show Logs -> Extension Host`, but I can't see anything helpful there. It turns out that I should select "Python Language server" in the dropdown list. It shows detail error:
> Error:
>   An assembly specified in the application dependencies manifest (Microsoft.Python.LanguageServer.deps.json) was not found:
>     package: 'runtimepack.Microsoft.NETCore.App.Runtime.osx-x64', version: '3.0.0'
>     path: 'System.Private.Xml.Linq.dll'"

I installed [dotnet 3.0 sdk](https://dotnet.microsoft.com/download/thank-you/dotnet-sdk-3.0.100-rc1-macos-x64-installer), then in a new terminal window, installed nuget package, but that does not make sense for binary, because nuget is for a source project.

In [this thread](https://github.com/Microsoft/vscode-python/issues/2256), it suggests deleting languageServer folder to let vscode to download it again. And that finally fix all issues. So the fix is to clean install.

## Idea: Enable [CLI](https://www.jetbrains.com/help/idea/working-with-the-ide-features-from-command-line.html)

## vs code: Install cool python code completion for python called [kite](https://github.com/kiteco/vscode-plugin#installation)

You can first install [kite engine](https://kite.com/), then after launch it, go to `plugins` tab and install to IDEs.

To install plugin for vim, first edit `~/.vimrc` by adding additional line: `Bundle 'kiteco/vim-plugin`, and then run `vim +BundleInstall +qall
	- 

## [Find color themes](http://color-themes.com/)

## [editor config which is supported by major IDEs](http://editorconfig.org/)

## Idea: There is common plugin that provides [save actions](https://plugins.jetbrains.com/plugin/7642-save-actions)

Usage could be found on [stackoverflow](https://stackoverflow.com/questions/946993/intellij-reformat-on-file-save)

## Idea: How to change code style for code format?
> Go to IDE Preferences | Editor | Code style, and modify for specified language

## [How to work with gradle?](https://www.jetbrains.com/help/idea/2016.1/working-with-gradle-projects.html)

 for vscode

## How to enable auto-format python code?
	- Install a  tool called *autopep8*: `pip install pep8`
	- Add line to setting: `"python.formatting.provider": "autopep8`
	- Enable auto-saving in user setting: `"editor.formatOnSave": true`

## How to run selected code in python?
Press `Shift+Enter`

## How to run golang test case with verbose output?
Add the following option to preference: "go.testFlags": ["-v"]
## [How to set language specific editor settings](https://code.visualstudio.com/docs/getstarted/settings#_language-specific-editor-settings)?
An example for javascript is:
```
    "[javascript]": {
        "editor.insertSpaces": false
    }
```

*Note*: should also pay attentions to formatter extentions such as "JS-CSS-HTML Formatter" extension, which might do auto-formatting on saving.

## [vscode vim](https://github.com/VSCodeVim/Vim)
## How to set syntax?
```
Press Ctrl + KM and then type in (or click) the language you want.

Alternatively, to access it from the command palette, look for "Change Language Mode" as seen below:
```
## Invoke context menu: _COMMAND+P_
## Install go plugin
    + From context menu, search _ext install go_ and _ext install Go_
    + After installing go plugin, install its dependencies:
        * http_proxy=socks5://localhost:10086 go get -u github.com/rogpeppe/godef
        * http_proxy=socks5://localhost:10086 go get -u github.com/newhook/go-symbols
        * http_proxy=socks5://localhost:10086 go get -u github.com/golang/lint/golint
        * http_proxy=socks5://localhost:10086 go get -u github.com/nsf/gocode
## [Enable node.js intelliSense](https://code.visualstudio.com/Docs/runtimes/nodejs)
    + Add a jsconfig.json file
    + `npm install -g typings`
    + `typings install dt~node dt~express dt~serve-static dt~express-serve-static-core --global --save`
    + `typings install dt~should dt~mocha dt~supertest --global --save`

### basic setup

## Change settings and keyboard binding according to [my dotfiles](https://github.com/leowa/dotfiles)

 for tmux

## How to fix a dead pane?
> Press `<prefix>+:` and then enter `respawn-pane -k`

##  How to resize window when screen resolution is changed?
> tmux limits the dimensions of a window to the smallest of each dimension across all the sessions to which the window is attached. If it did not do this there would be no sensible way to display the whole window area for all the attached clients.

> The easiest thing to do is to detach any other clients from the sessions when you attach: `tmux attach -d`

## How to set history limit
Add the following line to tmux.conf file: `set-option -g history-limit 3000`

## Reference

## [Maintaining Consistent Code Style](http://blog.jetbrains.com/webstorm/2015/08/maintaining-consistent-code-style/)

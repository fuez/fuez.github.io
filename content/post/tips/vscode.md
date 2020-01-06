---
title: "vscode"
date: 2017-06-13T11:48:45+08:00
lastmod: 2019-11-01T10:20:39+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## An extensive example of `launch.json` to run java:

```json
        {
            "type": "java",
            "name": "Flink",
            "request": "launch",
            "console": "internalConsole",
            "classPaths": [
                "/opt/flink-1.9.0/lib/flink-eca-0.0.1-SNAPSHOT.jar",
                "/opt/flink-1.9.0/lib/flink-python_2.12-1.9.0.jar",
                "/opt/flink-1.9.0/lib/flink-table-blink_2.12-1.9.0.jar",
                "/opt/flink-1.9.0/lib/flink-table_2.12-1.9.0.jar",
                "/opt/flink-1.9.0/lib/log4j-1.2.17.jar",
                "/opt/flink-1.9.0/lib/slf4j-log4j12-1.7.15.jar",
                "/opt/flink-1.9.0/lib/flink-dist_2.12-1.9.0.jar"
            ],
            "mainClass": "org.apache.flink.runtime.taskexecutor.TaskManagerRunner",
            "vmArgs": [
                "-Dlog4j.configuration=file:/opt/flink-1.9.0/conf/log4j-console.properties",
                "-Dlogback.configurationFile=file:/opt/flink-1.9.0/conf/logback-console.xml",
                "-Dspring.profiles.active=test",
            ],
            "projectName": "eca",
            "env": {
                "SPRING_PROFILES_ACTIVE": "test"
            },
            "args": [
                "--configDir=/opt/flink-1.9.0/conf",
                "-Djobmanager.rpc.address=job-cluster",
            ],
        }
```

NOTE: after that, you can just set break point.

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

## basic setup
## Change keyboard binding
```
// Place your key bindings in this file to overwrite the defaults
[
    {
        "key": "cmd+shift+j",
        "command": "workbench.action.navigateBack"
    },
    {
        "key": "cmd+shift+k",
        "command": "workbench.action.navigateForward"
    },
    {
        "key": "alt+cmd+b",
        "command": "workbench.action.toggleSidebarVisibility"
    },
    {
        "key": "cmd+b",
        "command": "editor.action.goToDeclaration",
        "when": "editorTextFocus"
    },
    {
        "key": "cmd+shift+k",
        "command": "editor.action.format",
        "when": "editorTextFocus"
    },
    {
        "key": "ctrl+shift+f",
        "command": "cursorRight",
        "when": "editorTextFocus"
    },
    {
        "key": "ctrl+f",
        "command": "extension.vim_ctrl+f",
        "when": "editorTextFocus"
    }
]
```
## Change User Settings
```
// Place your settings in this file to overwrite the default settings
{
    // Controls the font size.
    "editor.fontSize": 15,
    "vim.useCtrlKeys": true
}
```

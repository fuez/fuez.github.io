---
title: "vim"
date: 2017-06-13T11:48:45+08:00
lastmod: 2019-09-12T16:25:43+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## [Markdown Instant preview plugin](https://github.com/suan/vim-instant-markdown)

## Use `:vsp <newfile>` to open a new file in a splitted window (vertical split)

## [Open pdf file in vim](https://vim.fandom.com/wiki/Open_PDF_files)

```sh
# install xpdf
brew install xpdf
# add the following line ot .vimrc
:command! -complete=file -nargs=1 Rpdf :r !pdftotext -nopgbrk <q-args> -
# now you can open pdf file by :Rpdf <pdf-file>

```


## How to format json file?

Use this Vim command to format the contents of a JSON file: `:%!python -m json.tool`

## How to copy from vim to Mac pasteboard?

```
For MacVim and Windows Gvim, simply add the following to your ~/.vimrc:

set clipboard=unnamed
Now all operations such as yy, D, and P work with the clipboard. No need to prefix them with "* or "+.
```

## How to delete all lines before current line? `ddg`

## How to replace spaces by tab?
```
:set tabstop=4      " To match the sample file
:set noexpandtab    " Use tabs, not spaces
:%retab!            " Retabulate the whole file
```
## [How to trouble shooting why vim freezes?](http://stackoverflow.com/questions/24307374/how-to-determine-the-reason-vim-freezes)
```
Also, you can capture a full log of a Vim session with vim -V20vimlog. When a freeze occurs, quit Vim and inspect the last lines of the log to see what Vim was doing, or monitor the log with tail -f.
```

## How to match and replace blank line?
`vim +'%s/^\s\+$//g' +wq <file>`
*Note*: *+* should be escaped here, because I think vim does not consider *+* as special character.
## [How to disable "Entering EX mode"?](http://stackoverflow.com/questions/1269689/to-disable-entering-ex-mode-in-vim)
Add the following configuration to .vimrc file:
`:nnoremap Q <Nop>`
## [How to list loaded scrips and functions?](http://stackoverflow.com/questions/48933/how-do-i-list-loaded-plugins-in-vim)
```
" where was an option set  
:scriptnames            : list all plugins, _vimrcs loaded (super)  
:verbose set history?   : reveals value of history and where set  
:function               : list functions  
:func SearchCompl       : List particular function
```
## How to install new color scheme?
    + Just put new color _.vim_ file to ~/.vim/colors folder and update ~/.vimrc file to apply that.
    + To check the current color scheme: in vim, use command `:colorscheme`
## How to slit window?
```
 :split filename  - split window and load another file
 ctrl-w up arrow  - move cursor up a window
 ctrl-w ctrl-w    - move cursor to another window (cycle)
 ctrl-w_          - maximize current window
 ctrl-w=          - make all equal size
 10 ctrl-w+       - increase window size by 10 lines
 :vsplit file     - vertical split
 :sview file      - same as split, but readonly
 :hide            - close current window
 :only            - keep only this window open
```
## How to avoid weird auto-indent when pasting text into vim?
Before pasting, type command `:set paste`, and after that `:set nopaste`
## How to add key map to switch tab?
Just add keymap like the following one:
`map <C-t> :tabnext<CR>`
More could be found: http://vim.wikia.com/wiki/Alternative_tab_navigation


## Plugins
```
Bundle 'gmarik/vundle'
Bundle 'scrooloose/nerdtree'
Bundle 'scrooloose/syntastic'
Bundle 'airblade/vim-gitgutter'
Bundle 'tyru/open-browser.vim'
Bundle 'altercation/vim-colors-solarized'
Bundle 'Shougo/vimproc'
Bundle 'Shougo/vimshell.vim'
Bundle 'plasticboy/vim-markdown'
Bundle 'fatih/vim-go'
```

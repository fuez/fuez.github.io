---
title: mac
date: 2017-06-13T11:48:45+08:00
lastmod: 2019-12-17T23:35:05Z
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---

## `sed` to avoid backup on mac
> should use like `sed -i '' -e ...`

## How to fix broken package error like: `ImportError: dlopen(/usr/local/Cellar/python/3.7.3/Frameworks/Python.framework/Versions/3.7/lib/python3.7/lib-dynload/_ssl.cpython-37m-darwin.so, 2): Library not loaded: /usr/local/opt/openssl/lib/libssl.1.0.0.dylib`

```shell
brew update && brew upgrade
```

*NOTE*: `brew upgrade` will upgrade all packages installed by brew. For this issue, I guess just upgrading python37 should fix this issue, as it tried to find an old version openssl

## Trouble shooting `brew update` hang

First, run brew with debug and verbose information: `brew update --debug --verbose`, then try those steps:

> ctrl + c to exit the hanging upgrade.
> Run brew doctor. It prompted me to
> run brew cleanup to clean up false symlinks. Then I
> ran brew doctor again and it prompted me
> to install xcode CLT via the command sudo xcode-select --install.
> Finally brew update worked.

## Fix brew error: `fatal: could not read Username for 'https://github.com/Homebrew/homebrew-fuse': terminal prompts disabled`

`brew tab` to show all tap, then disable the errored one: `brew untap fuse`, maybe because its corresponding repository is moved.

## [Mac 怎样快捷地输入 Emoji 表情和颜文字？这里有 3 种方法 ](http://www.sohu.com/a/275275244_602994)
> 第二种方法，调出 emoji 表情页选择，快捷键是「⌃ + ⌘ + ␣」（control + command + space），上面三个功能键的符号也是在这里选择的。

## [使用 lsof 代替 Mac OS X 中的 netstat 查看占用端口的程序](https://tonydeng.github.io/2016/07/07/use-lsof-to-replace-netstat/)

```sh
sudo lsof -nP -iTCP -sTCP:LISTEN

# n: do not parse host name
# P: do not show port convention name, just show its number
# i: select internet address, iTCP means show all TCP address
# s: filter by only list state maching, `sTCP:LISTEN` list only network files with TCP state LISTEN
```

## [How to format disk on mac](https://www.simplifiedios.net/how-to-format-usb-on-mac/)

## [How to ignore some updates?](https://apple.stackexchange.com/questions/178811/how-to-skip-an-update-in-yosemite)

Firt run `softwareupdate --list` to check availabe updates, then run command like this to ignore some: `softwareupdate --ignore Safari13.0.2MojaveAuto-13.0.2`

## [Download ffmpeg binary](https://evermeet.cx/ffmpeg/)

Split mp4 files like this:

```sh
ffmpeg -ss 00:00:00 -t 00:03:00 -i IMG_1.mp4 -acodec copy -vcodec copy p1.mp4
ffmpeg -ss 00:03:00 -t 00:03:00 -i IMG_1.mp4 -acodec copy -vcodec copy p2.mp4

```

## [Fix breaking python2 error: *No module named 'zlib'*](https://github.com/Homebrew/homebrew-core/issues/29176)

```sh
installer -pkg /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg -target /
brew reinstall python@2
```

## brew support [`brew services`](https://github.com/Homebrew/homebrew-services) such as for influxdb

## How to hide/show various bars on adobe reader?

First checkout something in the mene *view*, and then try shutcut keys: *F8* to hide *Tool* bar, *CMD+SHIFT+M* to hide menu bar, and *CMD+CTL+H* for *Read Mode*, *CMD+1* and *CMD+2* to switch page setting, then to disable TAB mode by `In the preferences disable the entry "Open documents as new tabs ..."`

## How to search for certain file type? Just type something like: `kind:epub` in search input.

## To convert epub to mobi for kindle reader, you can use [calibre](https://calibre-ebook.com/download_osx)

##  On Mac book, it is possible to set proxy for docker to speed up package pulling when building docker images.

## [Some useful keyboard shortcuts for Apple notes](https://support.apple.com/en-sg/guide/notes/keyboard-shortcuts-apd46c25187e/mac)

I tried and find those are quite useful:
	- Bold 		- Command + B
	- Italics 	- Command + I
	- Underline	- Command + U
	- Title 	- Command + U
	- Heading 	- Command + H
	- Body 		- Shift + Command + B
	- Indent Right – Command + ]
	- Indent Left – `Command + [`
	- Apply Checklist format
	- Mark as Checked/Unchecked – Shift + Command + U
	- Zoom in on note’s contents - Shift-Command->

## [Keyboard customizer for macOS-Karabiner](https://pqrs.org/osx/karabiner/)

## How to configure notifications, such as for outlook?

>You can access Outlook for Mac’s Notification Center preferences by opening System Preferences, clicking Notifications and then selecting Microsoft Outlook from the list of applications on the left.

## How to add a user to some group?

```sh
sudo dseditgroup -o edit -a $username_to_add -t user wheel
```

## How to configure ip route table on mac?

```sh
# First, install ip route for mac
brew install iproute2mac

# configre a rule like this (write down tunnel name for your target connection)
sudo ip route add 10.10.0.0/24 dev tun0
```

## How to fix warning: "ld: warning: text-based stub file are out of sync. falling back to library file for linking."

```sh
sudo mv /Library/Developer/CommandLineTools /Library/Developer/CommandLineTools.old
xcode-select --install
sudo rm -rf /Library/Developer/CommandLineTools.old
```

## How to flush DNS cache on macbook?
```
sudo killall -HUP mDNSResponder
```

## [How to merge mp3 files?](https://ravingroo.com/1613/merge-mp3-files-mac-os-x-cat-concatenate-command/)

It's deadly simple, just use `cat` command to merge mp3 files to one.

But the above method will cause file broken. Better to use another tool called [mp3wrap](http://mp3wrap.sourceforge.net/)

## How to install sox-[Swiss Army Knife of sound processing utilities](https://sourceforge.net/projects/sox/?source=navbar)

```sh
brew install sox --with-lame --with-flac --with-libvorbis
```

## How to change mp3 speed by sox?

Better to use option *tempo* instead of *speed* to keep pitch (speed will change pitch):

```sh
sox Episode\ 1_\ Return\ of\ the\ Queen.mp3 e1.mp3 tempo 1.33

# To convert all in a folder:
ls *.mp3 | awk '{ system( "sox \""$0"\" \"1.5/"$0"\" tempo 1.5")'}
```

## How to open a file in **finder**? Use key: *CMD+O*
## [How to show listening ports on mac](https://stackoverflow.com/questions/4421633/who-is-listening-on-a-given-tcp-port-on-mac-os-x)

```sh
sudo lsof -iTCP -sTCP:LISTEN -n -P
# -nP means no port name lookup; -i means selecting internet related stuff; -s means protoco state.
```

## [How to play audio on your mac connected to iphone?](https://apple.stackexchange.com/questions/6173/can-i-play-audio-from-my-iphone-on-my-mac)

> Simple, and simply surprising:
Plug in your iPhone to your Mac using your Lightning Cable (doesn't work with 30 pin iPhones, sorry. [Yes I know you said 3GS])
Launch QuickTime Player
Choose File> New Audio Recording
Click the hard-to-see down arrow next to the Record 'button', choose iPhone under 'Microphone'
Play your music, game whatever on your iPhone, sound will come thru your Mac.

## How to avoid brew auto update each time?

set environment variable like this: `HOMEBREW_NO_AUTO_UPDATE=1` before running `brew install...`

## [How to Enable Key Repeating in macOS](https://www.howtogeek.com/267463/how-to-enable-key-repeating-in-macos/)

## How to add system level environment varibles?

Put the line like following to *.bassh_profile* file: `launchctl setenv JAVA_HOME "/Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home"`

## How to enable vim mode for xcode? Try [xvim](https://github.com/XVimProject/XVim)

## How to open unofficial packages?

>若提示“包已损坏”，请：
打开终端输入如下命令：sudo spctl --master-disable并键入您的密码（解除完整性检查）。
再次双击打开程序包。
终端输入sudo spctl --master-enable并键入密码（重新启用完整性检查）。

## How to convert chm file to pdf on mac? Try this tool called [ichm](https://code.google.com/archive/p/ichm/downloads) which is [open source](https://github.com/veryweblog/ichm)

## How to download youtube video and music?

> Download *youtube-dl* by `brew install youtube-dl`
The command to show available media format is *F*, like this: `youtube-dl -F  https://www.youtube.com/watch?v=288rTpd1SDE`
And the command to download selected quality video is *f*, like this: `youtube-dl -f 136+140  https://www.youtube.com/watch?v=288rTpd1SDE`.
The firt number 136 says video quality and the second one says audio quality.

## How to extract music or convert video from video? Use a tool called *Any Video Convert*

## How to crop music? Use a tool called *Audacity*.

## How to convert audio files on mac?

>Use the tool called *afconvert* which is built-in tool on mac. 
Call command like this: `afconvert -hf` to check out audio format.
Call this command to convert: `afconvert -f m4af a.mp3 a.m4r` to convert from a mp3 file to ring tone file.

## How to open a testing port?
  - Open /etc/pf.conf in a text editor.
  - Add a line like this: `# Open port 8080 for TCP on all interfaces`.
pass in proto tcp from any to any port 8080
  - Save the file
  - Load the changes with: `sudo pfctl -f /etc/pf.conf`

*NOTE*:If you need to open a udp port, change tcp to udp, if you need both, add a second line. Additional detail can be found in man pf.conf.
Also make sure your server is listening on the actual interface you want it accessible over (or all interfaces, using 0.0.0.0 or ::0), not localhost (127.0.0.1 or ::1).

## How to open */usr/local/bin* in *open* dialog?
> Press *Press Command+Shift+G* in **Finder**

## How to decode *.flac* and other lossless audios?
> check out [XLD](http://tmkk.undo.jp/xld/index_e.html)

## How to use pasteboard?
> just use `pbcopy` together with `cat` command like this: `cat ~./ssh/id_rsa.pub | pbcopy`

## How to upgrade gcc to 4.9?
> `brew install gcc49`, and then add make sure that _/usr/local/Cellar/gcc@4.9/4.9.3_1/bin/_ is used.

## Caveat to start sevice on mac
> To have launchd start influxdb at login:
`ln -sfv /usr/local/opt/influxdb/*.plist ~/Library/LaunchAgents`

> Then to load influxdb now:
`launchctl load ~/Library/LaunchAgents/homebrew.mxcl.influxdb.plist`

> Or, if you don't want/need launchctl, you can just run:
`influxd -config /usr/local/etc/influxdb.conf`

## [Any Video Converter free](http://www.any-video-converter.com/products/mac_video_converter_freeware/)
## [How to Add External SRT Subtitles to Video](http://www.any-video-converter.com/add-srt-subtitle-to-output-video.php)
## [Show/Hide Hidden Files the Long Way](http://ianlunn.co.uk/articles/quickly-showhide-hidden-files-mac-os-x-mavericks/)

> Open Terminal found in Finder > Applications > Utilities
In Terminal, paste the following: defaults write com.apple.finder AppleShowAllFiles YES
Press return
Hold the ‘Option/alt’ key, then right click on the Finder icon in the dock and click Relaunch.

## [How to remove icon from menu bar](http://osxdaily.com/2012/01/05/remove-icons-menu-bar-mac-os-x/)
>
If your Mac menu bar is starting to resemble an icon farm, remember that you can remove items from the menu bar by holding down the Command key and dragging items out of the menu.

## install _bash-completion_ on mac: `brew install _bash-completion`

## [How to test your own app?](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/TestingYouriOSApp/TestingYouriOSApp.html)

## [General introductions on how to launch your own app](http://www.smartinsights.com/mobile-marketing/app-marketing/how-to-develop-and-launch-your-own-iphoneipad-app/)

## How to rename with awk and with white space in the name?
`find . -iname "*.wav" | awk '{gsub(/ /, "\\ ", $0);system("mv "$0" "$1"mp3")}'`

## How to mount NTFS USB disk?
First get device path from _disk utility_, then call `mount` like the following(create mount point first):
`sudo mount -t ntfs /dev/disk2s1 /portal/msgift/`
## [How to set _DYLD_LIBRARY_PATH_?](http://superuser.com/questions/282450/where-do-i-set-dyld-library-path-on-mac-os-x-and-is-it-a-good-idea)
## What is the alternative for _ldd_ on macos?
It is _otool_. Usage like this: `otool -L  _informixdb.so`

## [How To Completely Uninstall Software under Mac OS X [MacRx]](http://www.cultofmac.com/90060/how-to-completely-uninstall-software-under-mac-os-x-macrx/)
  - /Library
  - /Library/Application Support
  - /Library/Preferences
  - /Library/LaunchAgents
  - /Library/LaunchDaemons
  - /Library/PreferencePanes
  - /Library/StartupItems
  - ~/Library
  - ~/Library/Application  Support
  - ~/Library/LaunchAgents
  - ~/Library/Preferences
  - ~/Library/PreferencePanes
  - ~/Library/StartupItems

## How to check local installed printers?
Visit __http://localhost:631/printers/__

## How to choose default java version?
http://stackoverflow.com/questions/21964709/how-to-set-or-change-the-default-java-jdk-version-on-os-x

> Run `/usr/libexec/java_home -V` to list current installed java, then choose one version by: export JAVA_HOME=`/usr/libexec/java_home -v 1.8`

## How to access menu bar?
ctrl+F2
http://superuser.com/questions/303525/what-is-the-shortcut-to-access-the-menubar-in-mac-os-x

## How to specify inlcude path?
Something like `export DYLD_LIBRARY_PATH=/path/to/preferred/HDF5/lib`
http://stackoverflow.com/questions/27948093/include-search-path-on-mac-osx-yosemite-10-10-1

## How to check default gcc include path?
`echo "" | gcc -xc - -v -E`
http://stackoverflow.com/questions/6715454/what-is-the-default-path-for-osx-system-include-files-when-building-a-c-applic

## To fix "coud not find file values.h" on macos.
```
The standard header file for all this data is limits.h. To fix your problem, you could just create a soft link called values.h that points to limits.h.
```
https://macosx.com/threads/values-h.50056/

 for microsoft office

## How to adjust word line spacing?
>To single-space, press Ctrl+1. Use this command to remove other line-spacing styles.

> To double-space, press Ctrl+2. This setting formats the paragraph with one blank line below each line of text.

> To use 1-1/2-space lines, press Ctrl+5. Yes, this keyboard shortcut is for 1.5 lines not 5 lines. Use the 5 key in the typewriter area of the computer keyboard. Pressing the 5 key on the numeric keypad activates the Select All command.


## Tools

## [Lyrics Finder](https://www.mediahuman.com/lyrics-finder/) from MediaHuman

## [MediaHuman Audio Converter](http://www.mediahuman.com/audio-converter/)

is a freeware application for Mac OS X and Windows. It can help you to convert your music absolutely free to WMA, MP3, AAC, WAV, FLAC, OGG, AIFF, Apple Lossless format and bunch of others.

## [Media player](https://github.com/mpv-player/mpv/)

1. install by brew: `brew install mpv`. In terminal, you can play music with options such as speed to tune playing speed.

2. Install standalone app. [Keyboard control manul](https://github.com/mpv-player/mpv/blob/master/DOCS/man/mpv.rst#keyboard-control)

## vscode

Add vscode to PATH:
`export PATH="$PATH:/Applications/Visual Studio Code.app/Contents/Resources/app/bin"`

## [convert ebook to epub: calibre-ebook](https://calibre-ebook.com/download_osx)

### Troubleshooting

## *can not set DYLD_LIBRARY_PATH*

This is because of SIP (system integration protection)
https://github.com/oracle/node-oracledb/issues/231
http://www.macworld.com/article/2986118/security/how-to-modify-system-integrity-protection-in-el-capitan.html

>
Disabling System Integrity Protection
Power on your Mac and hold down the [command]+[R] keys to access the Recovery Partition.
From the Recovery Partition, click Utilities from the menu bar, and then select Terminal.
Enter the following command into Terminal and press Enter to execute it:
csrutil disable
Once the command has executed, exit the Terminal and reboot the Mac. When you log back into OS X, SIP will be disabled.

### Reference

## [Swift 2 Tutorial: A Quick Start](https://www.raywenderlich.com/115253/swift-2-tutorial-a-quick-start)
## [The Official raywenderlich.com Swift Style Guide.](https://github.com/raywenderlich/swift-style-guide)
## [THE IMPLICITLY UNWRAPPED OPTIONAL](http://krakendev.io/blog/when-to-use-implicitly-unwrapped-optionals)
  - The only difference between them is that implicitly unwrapped optionals are a promise to the swift compiler that it's value will NOT be nil when accessed at runtime.
  - The implicitly unwrapped optional is actually an enum represented by the name ImplicitlyUnwrappedOptional while the optional is an enum represented by the name Optional.
  - implicitly unwrapped optionals promise to have a value when accessed at runtime, but also have the ability to be mutated to nil at runtime as well
  - The nature of Swift is to encourage the developer to be as safe as possible, so it’s highly recommended that we default to using Swift’s Optional Type as much as possible in order to prevent crashes from sending messages to nil. So here’s to writing safer code!

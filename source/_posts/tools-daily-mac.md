---
title: Mac 配置教程-日常篇
date: 2018-12-08 15:29:13
tags: [Mac, Tools]
categories: ToolsDaily
keywords:
  - mac
  - config
  - develop
  - app
  - tutorial
---

![](https://gitee.com/michael_xiang/images/raw/master/computer-mac.jpg)

今年终于在推出 2018 款 MBP 时，看到升级了 CPU，我就果断下手「拔草」。本文记录使用 Mac 的一些配置，会长期更新。

为了控制文章的篇幅，我将 Mac 使用配置分成了两篇：

- [Mac 配置教程-日常篇](https://michael728.github.io/2018/12/08/tools-daily-mac/)
- [Mac 配置教程-开发篇](https://michael728.github.io/2019/04/14/tools-dev-mac/)

<!-- more -->

## 系统配置

记录一些 Mac 系统配置方面的技巧，让使用起来更方便。

### 快捷键整理

- `Enter`：选中一个文件按下 `Enter`，直接改名
- `Ctrl` 多用于命令行快捷键，`Command` 多用于图形化快捷键
- 选中桌面的所有需归档资料，`Command+Shift+n`，会将文件归档到一个文件夹下
- 调度中心设置：右 `Ctrl`，显示桌面
- 触发角

![](https://gitee.com/michael_xiang/images/raw/master/YtXi85.png)

- 辅助功能-鼠标与控制板-触控板选项-启用拖移
- `Option+Command+v` 类似剪切的功能，就是粘贴后，原来文件会被剪切过来。复制文件后，除了用快捷键，还可以在文件夹空白处右键，然后再按住 `「⌥」`，原本的「粘贴项目」就变成了「将项目移动到这里」
- 删除文件：
  - `Command+Delete`
  - 工具栏添加垃圾箱，然后选择待删除文件，点击垃圾箱即可删除
- `Command+Option+c` 复制文件路径
- 按住 `Option` 很多菜单会发生些小变化，比如左上角苹果图标点开，按住 `Option` 『关于本机』会变成『系统信息』
- `Control + Command + Space` emoji 表情
- `Command+Up`：回到上一层文件夹
- `Command+Down`：如果是文件夹就进入文件夹，如果是文件就打开
- `Ctrl + Command + q` 锁屏
- 单词，选中，三指，自动翻译单词
- `Command+Shift+.` 临时性切换显示/隐藏
- `Command+Alt+i`: Chrome开发者工具，F12功能
- `command＋shift＋G` Finder中如何输入路径直接跳转，[参考](https://www.cnblogs.com/wormday/archive/2011/05/04/2035984.html)
  - `command+shift+C` 电脑
  - `command+shift+H` 个人
  - `command+shift+D` 桌面
  - `command+shift+A` 程序

### 终端快捷键

- `Ctrl + A`：移动到行首
- `Ctrl + E`：移动到行尾
- `Ctrl + K`：删除到行尾
- `Ctrl + U`：删除到行头
- `Ctrl + N`：移动到下一行
- `Ctrl + P`：移动到上一行

### 设置允许「任何来源」的应用 —— 解决文件已损坏报错

有时候会遇到这种错误「报错：无法打开已损坏的安装包」，打开终端，键入命令，输入密码，然后回车：

```shell
sudo spctl --master-disable
defaults write com.apple.LaunchServices LSQuarantine -bool false
```

打开「安全性与隐私」，发现久违的「任何来源」回来了。

### 改建

为了将外置键盘和Mac键盘的option键、command键保持一致的顺序，做如下修改：
设置-键盘-修饰键-选择键盘（选择外设键盘）-调整顺序

![](https://gitee.com/michael_xiang/images/raw/master/09O85q.png)

- [Mac系统切换机械键盘win和alt键](https://www.jianshu.com/p/66922b2bc8c3)

### 使用 Shift 键

Shift 键类似于可视模式，比如在某个位置点击光标并按住 Shift 键不松开，再去另一个位置点击一次，就可以选中两次点击位置之间的文本内容。

### 触控板

- 四指捏合，展开全部应用
- 辅助功能-鼠标 开启三指拖拽

参考：

- [用了那么久 Mac，才知道触控板原来还有这些功能](https://sspai.com/post/44173)

### Mac Split View屏幕分割

- 长按左上角放大的绿色按钮，然后就会选择放置应用的区域！

参考：

- [Mac Split View屏幕分割](https://jingyan.baidu.com/article/a3a3f811eb712d8da2eb8a95.html)

### 更改默认文件打开方式

两指轻按文件，显示简介中，可以看到打开方式选项。

### 菜单栏显示投屏的按钮

升级 macOS Big Sur 系统之后，发现菜单栏上找不到投屏的按钮，需要进行设置。系统偏好设置-》程序坞与菜单栏-》屏幕镜像，勾选「在菜单栏中显示」，选择「始终」：

![屏幕镜像](https://gitee.com/michael_xiang/images/raw/master/uPic/c5qdPi.png)
### 电池

- 按住 Option 键并点按菜单栏中的电池图标，以显示电池状态菜单

### 增强 Tab 键 —— Tab 键适用于所有控制

偏好设置->键盘->快捷键 全键盘控制改为「所有控制」

![](https://gitee.com/michael_xiang/images/raw/master/xSF60o.png)

### 电脑改名

电脑改名: 系统偏好设置 - 共享 - 电脑名称

### 其他

- [如何更改 Mac 帐户和个人文件夹的名称](https://mp.weixin.qq.com/s?__biz=MzAxNzcwMTA4Ng==&mid=2247487924&idx=1&sn=af93e65636277a8905a0389cefca2c7f&chksm=9be0d005ac9759134369439e40684ed8b41dfa81bd9a47e82bcd7568b72629dcb15a02df5f4f&mpshare=1&scene=1&srcid=#rd)
- [Mac修改账户名称和个人目录后，进不去系统怎么办？](https://mp.weixin.qq.com/s?__biz=MzAxNzcwMTA4Ng==&mid=2247487924&idx=2&sn=f4af05656734f3743bc2861c56db501b&chksm=9be0d005ac9759134b4d6b103fb9402af27e592fa6c41dbba77491e1fc9d62b0388bb99b262d&mpshare=1&scene=1&srcid=#rd)

## 技巧

### 创建多个桌面

可以创建多个桌面，使用常见快速切换。

四指在触控板往上滑动，然后右上角可以点击加号新建桌面。四指左右滑动，可以在不同桌面之间切换。

## 日常软件

### 分屏软件

目前在用的是一款付费软件 [magnet](http://magnet.crowdcafe.com/#download)，也不贵，6元，感觉还挺好用，下面是它的常用快捷键：

![](https://ws2.sinaimg.cn/large/006tNc79ly1fz4xe7in8kj30au09ajwd.jpg)

[spectacle](https://www.spectacleapp.com/) 这款软件时免费的，也可以体验一下：
- `option+command+←`：窗口左边停靠
- `option+command+→`：窗口右边停靠
- `option+command+f`：窗口全屏

### 视频播放器

- [官宣-IINA播放器](https://lhc70000.github.io/iina/zh-cn/)
- [人人影视客户端](http://app.zimuzu.tv/)

### 截图

- [腾讯-截图](https://jietu.qq.com/)
- [snipaste](https://zh.snipaste.com/) 强烈推荐，Windows 平台也有。F1 截图，F3 贴图

设定了快捷键为 `Ctrl+Shift+a`

### 下载工具

qBittorrent：
- [qBittorrent](https://www.qbittorrent.org/download.php) 下载
- [搜索插键](https://github.com/qbittorrent/search-plugins/wiki/Unofficial-search-plugins)
- [qBittorrent下载の道](https://zshmobi.com/2018/05/10/qbittorrent-usage/)
- [关于 qBittorrent 连接更多 tracker 的一点补充](https://www.bilibili.com/video/av55388579/)
- [qBittorrent 下载安装和高级设置使用教程 | 添加 trackers 优化下载速度](https://go2think.com/how-to-use-qbittorrent/)

tracker 列表：

- [torrents](https://torrents.io/tracker-list/)
- [ngosang/trackerslist](https://github.com/ngosang/trackerslist)
- [newtrackon](https://newtrackon.com/list)

还有一个下载工具，不过没试用过，做个记录：[Photon](https://github.com/alanzhangzm/Photon/blob/master/README.zh-CN.md)

### 视频制作

- [OBS](https://obsproject.com/) 免费的录屏软件
- [ArcTime](https://arctime.org/download.html) 字幕工具，用来给视频添加字幕，免费。

### 状态栏图标管理

- [Dozer](https://github.com/Mortennn/Dozer) 免费开源，`brew cask install dozer`，足够用了，推荐

Dozer 的用法也很简单，只需要按住 Command 键，然后将应用图标拖动至它显示的两个圆点之间，那么这之间的图标就会隐藏。我给它设置了一个快捷键 `Command+Shift+H`

- [vanilla](https://matthewpalmer.net/vanilla/) 免费，按住 Command 键，拖动顶部状态栏图标归档之后，就会隐藏图标了。
- [Bartender](https://www.macbartender.com/)：一款非常实用的 Menubar 菜单栏管理工具，付费的

### CatchMouse

Mac双屏时，通过快捷键快速切换到另外一屏。

- [catchmouse下载](http://macappstore.org/catchmouse/)
- [mac下双屏切换](http://shengbinxu.github.io/mac/2015/06/28/mac下双屏切换/)

### cheatsheet

在使用一些软件时，长按 `Command` 按键，会出现快捷键菜单

- [官宣-cheatsheet](https://www.mediaatelier.com/CheatSheet/)

### [Clipy](https://github.com/Clipy/Clipy)

Github 上面开源免费的一款剪贴板软件，简单好用！关键的是还免费，so hacker~

### Downie

Downie：下载各网站视频且更新频繁；

### encrypto

文件加密软件： [encrypto](https://macpaw.com/encrypto) 免费的加密软件

### firefox

火狐的浏览器还是不错的，尽管主力还是 Chrome，用火狐主要是因为国内不需要梯子也可以账号同步。但是国内和国际版，账号系统没打通的，要想使用国际版的账号，那么可以在设置-》首选项-》同步中，找到「切换至全球服务」，然后登陆即可。

下载：[现在火狐浏览器国际版去哪里下载？ - 马里亚纳的秋刀鱼的回答 - 知乎](https://www.zhihu.com/question/39394751/answer/365943869)

参考：
- [firefox 国际版 账户登录 国内账户 切换 同步问题](https://blog.csdn.net/u014710355/article/details/84665962)

### iMazing

[Mac/Win]备份/管理iOS设备 iTunes 的替代品

### licecap

一款用来制作 Gif 动态图片的小软件：

- [licecap官网](https://www.cockos.com/licecap/)

### manico

默认，长按option，显示dock应用，数字快速切换应用。

### Mos

- [Mos](https://mos.caldis.me/) 鼠标平滑滚动软件，很好用。免费开源

```shell
$ brew cask install mos
$ brew update
$ brew cask reinstall mos
```

### NightOwl

NightOwl ：一款让 Mojave 深色模式更加智能的神器，可以将当前开启的某些软件单独设置成「始终显示为浅色」，这样，即使系统在深色模式下，这些被勾选的软件也会依然显示为浅色界面。

[NightOwl](https://nightowl.kramser.xyz/)

### Onedrive

Onedrive 用来同步文档用的，作为「同步盘」使用，我用来存储电子书，还不错。它的在线版需要梯子才能访问，但是客户端使用是OK的，安卓客户端/、Mac 客户端也都有。

它的汉化有些含义不是很好理解，这里做个简要记录：
- 「始终保留在此设备上」 表示将文件的状态和在线（远端）不关联，及时远端删除了，本设备上的该文件也还在；
- 「释放空间」 表示将文件从本机上删除，节省本机的空间，但是远端文件并没删除；
- 「保持脱机」 脱机保存到你的电脑。这意味着，即使未连接到 Internet，也可以随时打开和更改它们。重新连接网络时，OneDrive 将根据脱机时所做的任何更改更新联机版本。

在没有 Internet 连接时脱机文件会很方便，但它们也会占用你电脑上的空间

参考：
- [OneDrive 图标的含义是什么？](https://support.office.com/zh-cn/article/OneDrive-%E5%9B%BE%E6%A0%87%E7%9A%84%E5%90%AB%E4%B9%89%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F-11143026-8000-44f8-aaa9-67c985aa49b3)

### PopClip

当你在 Mac 上用鼠标选择文字后 PopClip 即会出现。然后即可执行针对内容的特定操作，包括「拷贝」、「复制」以及其它插件操作例如「发送至 Evernote」等。

我安装了扩展：

- `dash` 选中，可以启动 Dash 搜索开发文档
- `terminal`: 复制相关代码时，就可以默认直接去终端执行了，可以设置为默认 iTerm2
- bing
- alfred

这几个扩展，可以到这里下载：[my-config-files/popclip](https://github.com/Michael728/my-config-files/tree/master/popclip)

参考：
- [popclip扩展官网](https://pilotmoon.com/popclip/extensions/)

### Pock

[Pock](https://github.com/pigigaldi/Pock) Mac 的 touchbar 增加一点用途，显示 Dock 的应用图标~推荐！

双击 Ctrl 键在 Pock 和系统触控栏之间切换

###  PinPoint
鼠标指针修改

### Pixelmator

图片处理软件

### snap

- [Snap-Mac平台下的快捷键设置App-在Mac下使用机械键盘！](https://zhuanlan.zhihu.com/p/28602277)

### SketchBook

[SketchBook](https://sketchbook.com/) 画图软件，需要配合手绘板等工具使用。在看教程时，看到老师用这个软件手写一些流程图啥的用到的。

### The Unarchiver

解压软件，在 App Store 中即可下载。

### Typora

[Typora](https://typora.io/)：Markdown 编辑器。

### Telegram

- [Telegram使用指南](https://www.jianshu.com/p/9be7ab548e90)

### TinyPNG4Mac

[kyleduo/TinyPNG4Mac](https://github.com/kyleduo/TinyPNG4Mac/blob/master/README_ZH.md) 进行图片压缩的

```
brew cask install tinypng4mac
```

### keycastr

屏幕上显示你敲击的快捷键，或者也可以设置显示你敲击的任意字母。

- [How can I show typing keyboard in record screen](https://apple.stackexchange.com/questions/52618/how-can-i-show-typing-keyboard-in-record-screen)
- [官宣-keycastr/keycastr](https://github.com/keycastr/keycastr)

`brew cask install keycastr`

## Mac 相关好文

- [推荐-Mac OS X 配置指南 | Mac OS X Setup Guide](https://wild-flame.github.io/guides/docs/mac-os-x-setup-guide/guide-introduction)
- [你每天都能用上的 Mac 快捷键](https://sspai.com/topic/145)
- [少数派-macOS 上都有哪些既免费、又实用的工具？| 新手问号](https://sspai.com/post/41477)
- [柠檬精选](https://lemon.qq.com/lab/)
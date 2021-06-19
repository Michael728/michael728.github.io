---
title: 群晖 NAS 920+ 使用笔记
tags: [数码, 多媒体]
toc: true
date: 2021-05-30 21:29:08
categories: Life
---

## 前言

很早之前就听说过 NAS 设备，但是苦于一直在租房的状态（其实是穷），一直没有入手。今年终于搬到新家了，加上近期老婆抱怨手机存储空间不够，已经塞满了吉宝的照片和视频了，因此，终于狠心剁手了一台群晖 920+ 的 NAS 设备。

本文主要就是围绕 NAS 到手后，我进行了哪些设置以及安装哪些好玩的套件。

## DS video

群晖安装套件：Video Station
电视盒子、手机安装：DS Video

通过在 NAS 上安装好套件、添加好资源之后，可以在其他设备上安装 DS Video 来进行观看。会有视频的海报墙，比直接看文件夹或者视频文件更美观和直观。

经过测试发现，对于 DTS、EAC3 格式的视频，群晖的这个 DS Video 播放就没有声音了。只能在电视盒子安装一个 VLC 播放器，这样，DS Video 不支持的视频资源可以选择用第三方播放器打开。

> 网上有各种解决方案，比如 [群晖 Video Station 支持 DTS 和 eac3 解决方案](https://post.smzdm.com/p/az5e3nep/)，但是在我群晖 920+ 上都没好使。

经过周末的折腾，利用 Jellyfin 搭建了家庭媒体服务中心，然后其他平台也都有 Jellyfin 的客户端，使用下来，效果也还可以，非常值得推荐！

参考：
- [群晖 Video Station 支持 DTS 和 eac3 解决方案](https://woodenrobot.me/2019/08/12/syn-vediostation/)

## kodi

[kodi](https://kodi.tv/) 就是个功能强大的播放器客户端，关于它的教程非常丰富，可玩性也非常高。由于我一开始是将 kodi 安装在电视盒子上，电视盒子性能不是很好，因此，kodi 播放一些视频时，卡顿感比较明显。

![kodi](https://gitee.com/michael_xiang/images/raw/master/uPic/le7QDp.png)

使用视频：
- [B 站司波图/手把手教你组建家庭影院！（KODI+群晖+智能电视）](https://www.bilibili.com/video/BV1R4411A7xi)

### 解决播放视频没有声音

需要在设置-》系统-》音频里，勾选开启一些音频方面设置项的兼容性设置。设置要点：
- 系统-音频-声道数：2.0
- 系统-音频-允许直通输出
- 系统-音频-启用杜比数字（AC3）兼容功放
- 系统-音频-启用杜比数字（AC3）编码转换

参考：
- [Would appreciate any insight into dealing with AAC 5.1 audio](https://forums.whirlpool.net.au/archive/2575344)
- [CSDN/安卓dts音频解码_【日常折腾】KODI的AC3音频转码设置](https://blog.csdn.net/weixin_28785509/article/details/112413648)

### 主菜单的添加与删除

在设置/皮肤设置/主菜单选项中，可以开启剧集、电影等菜单项。

参考：
- [Kodi主菜单功能介绍 主菜单如何添加删除](http://www.kodiplayer.cn/course/2951.html)

### 参考

- [KODI 中文网](https://www.themoviedb.org/) 这个网站提供了很多关于 kodi 的教程

## Transmission 安装与汉化

![Transmission](https://gitee.com/michael_xiang/images/raw/master/uPic/dhA0Nr.png)

[Transmission](https://transmissionbt.com/about/) 是一个开源的下载软件，可以用来下载 PT 站的资源。记得在大学时期使用六维空间时，经常为了做种需要将笔记本一直打开着。现在只需要在 NAS 中安装好这个 APP， 则可以使用 NAS 24 小时挂在后台进行资源的下载和上传。

### 安装 TR

在套件中心添加套件源 `http://packages.synocommunity.com`：

![synocommunity](https://gitee.com/michael_xiang/images/raw/master/uPic/wLH56L.png)

常规中设置信任：

![iCVvS7](https://gitee.com/michael_xiang/images/raw/master/uPic/iCVvS7.png)

在社群中搜索 Transmission，按照提示安装，安装好之后的访问地址：

![dxOVgz](https://gitee.com/michael_xiang/images/raw/master/uPic/dxOVgz.png)

### 汉化

汉化的安装，可以阅读 [ronggang/transmission-web-control](https://github.com/ronggang/transmission-web-control/wiki/Linux-Installation-CN)。以下简要介绍：

在群晖控制中心，开启 NAS SSH 登录的功能：

![SSH](https://gitee.com/michael_xiang/images/raw/master/uPic/KXmeOu.png)

然后在终端命令行窗口即可登录 NAS：

```
# 登录账号名和 IP 得换成你自己的
ssh 用户名@IP
# 切换为 root，密码和你 admin 账户密码一样
sudo -i
```

![p1XRb4](https://gitee.com/michael_xiang/images/raw/master/uPic/p1XRb4.png)

注意：
- 如果想要在下载时指定目录，需要对应目录需要添加群组： `sc-transmission`、`sc-download`

添加常用下载目录：
![eBEGuV](https://gitee.com/michael_xiang/images/raw/master/uPic/eBEGuV.png)

### 参考

- [君子不器/群晖安装Transmission](https://www.colinjiang.com/archives/synology-install-transmission.html)
- [喵斯基俱乐部/群晖NAS安装及美化Transmission(PT)教程](https://www.moewah.com/archives/2420.html)
- [新浪众测/最强下载工具，玩转NAS影音竟然如此简单！](https://zhongce.sina.com.cn/article/view/80527/) 介绍了 Tr/万物下载、Plex/Kodi Jellfin

## Jellyfin 媒体中心

### 介绍

- Jellyfin 支持硬件转码，在使用硬件转码推流的时候可以大幅降低 CPU 占用率，支持**实时转码**。硬件转码功能在 emby 和 plex 都是付费功能。
- Jellyfin 是在它的服务器上搭建影音资料库，这样，在任何客户端来访问资料库时，就不用再建立资料库。Kodi 在不同设备上需要重新建立存储在该设备上的资料库。观看的记录会保存在 Jellyfin 服务端，这样，在各个平台切换观看时，使用同一账户就可以方便继续观看了。

Kodi 播放方式类似电脑上的播放器播放。直接从共享文件夹读取文件流，而非播放视频流。Kodi 的这种方式，占用的网络资源则由具体的文件的码率决定。由于解码由播放设备进行，所以实际效果取决于设备的解码能力。如果播放设备解码能力弱，直接播放视频文件，有时候就会造成卡顿或无法播放。而 Jellyfin 的这种方式，可以理解为你自己在 NAS 上搭建了一个多媒体服务器，它可以进行视频的解码，客户端播放能够流畅很多。

### 安装

通过打开 Docker 套件，在其中的注册表中搜索 `jellyfin` 镜像（映像）进行下载。

![9BNRO6](https://gitee.com/michael_xiang/images/raw/master/uPic/9BNRO6.png)

![muC3tc](https://gitee.com/michael_xiang/images/raw/master/uPic/muC3tc.png)

![35K25i](https://gitee.com/michael_xiang/images/raw/master/uPic/35K25i.png)

进入 jellyfin 服务器地址（NAS IP:8096），即可访问。

需要进行一些设置。可以参考 [SMZDM阿文菌/使用群晖Docker 安装Jellyfin 家庭影院HTPC 比emby plex好用多了](https://post.smzdm.com/p/a6lnxg3g/):
- 国家选项里没有 China，而是要选择 Peoples's Republic of China
- 选择备用字体文件路径：控制台》播放》选择备用字体文件路径，提前在 `config` 下创建好 `font` 文件夹（可以自定义文件夹名），在其中放好下载的[字体 noto.zip](https://github-repository-files.githubusercontent.com/164221924/4434292?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20210606%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20210606T051418Z&X-Amz-Expires=300&X-Amz-Signature=56e41c40dd848673c66029e026ab94ed4ad7c566afcd2ed692c27ca7074bde28&X-Amz-SignedHeaders=host&actor_id=8510410&key_id=0&repo_id=164221924&response-content-disposition=attachment%3Bfilename%3Dnoto.zip&response-content-type=application%2Fx-zip-compressed)。这个主要为了解决 `ASS/SSA` 中文字幕会显示方块乱码。

![meK4M9](https://gitee.com/michael_xiang/images/raw/master/uPic/meK4M9.png)

安装的步骤，[Jellyfin 官网](https://jellyfin.org/docs/general/administration/hardware-acceleration.html)可以看做是如下命令的等同：
```
docker run --name=jellyfin2 \
--device=/dev/dri:/dev/dri  \
--device=/dev/dri/card0:/dev/dri/card0 \
-p 8096:8096 \
-v /volume1/docker/jellyfin/config:/config \
-v /volume1/docker/jellyfin/cache:/cache \
-v /volume1/video:/media \
jellyfin/jellyfin:latest
```

> 之所以映射设备，是为了开启硬件加速

### 添加媒体库

添加媒体库的步骤很简单，注意勾选：

- 将媒体图像保存到媒体所在文件夹：方便将下载的资源归档到视频文件夹中

### 插键

#### 字幕插键 Open Subtitles

安装 `Open Subtitles` 插件，这样的话，可以使用字幕下载的功能。使用该插件

安装好插键之后，需要重启容器。

![重启容器](https://gitee.com/michael_xiang/images/raw/master/uPic/ry2JoO.png)

需要去 [opensubtitles](https://www.opensubtitles.org/zh) 注册账号，有了账号，需要去点击该插键进行配置。

参考：
- [92Nas/Jellyfin中电影外挂ass格式字幕无法显示的解决方法](http://www.92nas.com/forum.php?mod=viewthread&tid=147)
- [All the Chinese plug-in subtitles in ASS format](https://github.com/jellyfin/jellyfin-web/issues/934) 中的解决方案进行配置

### 播放

![播放数据](https://gitee.com/michael_xiang/images/raw/master/uPic/jr7IZR.png)

查看播放数据：
- 播放信息：会显示播放方式，可以看出是转码播放还是直接播放的
- 媒体源信息：表示播放的视频源的信息，可以看到码率、音频编码
- 比特率：码率，视频文件 原本的码率，如果原本码率比较高，我们通过播放时设置低码率，那么，就会被自动转码
- 转码信息：看到这个表示正在进行硬件转码，方便播放设备播放。可以看到，源文件的音频是 `EAC3`，播放时被自动转码成 `AAC` 了。

### 硬件加速

通过如下的设置开启硬件加速：

- 开启转码：控制台》播放，选择硬件加速`Video Acceleration API(VAAPI)`

![5pEBal](https://gitee.com/michael_xiang/images/raw/master/uPic/5pEBal.png)

> 注意，上面能够成功开启的前提是，勾选了「使用高权限执行容器」

![Yrvjxu](https://gitee.com/michael_xiang/images/raw/master/uPic/Yrvjxu.png)

通过 SSH 登录后台，`htop` 命令查看 CPU 占用率高的进程（jellyfin），查看是否开启验证加速：

![RhkpfW](https://gitee.com/michael_xiang/images/raw/master/uPic/RhkpfW.png)

此外，在 NAS 查看资源监控，播放视频时，CPU 如果没有飙升，一般也是开启硬件加速的效果。

参考：
- [分享一种简单得不能再简单的群晖DS918+下Jellyfin调用核显硬解的办法](https://post.smzdm.com/p/a259mgk2/p3/#comments)

### 客户端

Jellyfin 的 APP 死机概率非常高，没有网页版本好用。可以使用手机浏览器直接访问网页。利用 Chrome 访问 Jellyfin 的地址，然后在浏览器页面的右上角的菜单项中，点击「添加到主屏幕」，这样即可在手机桌面创建快捷方式。

> 我的手机进行了权限管理，需要放开 Chrome 创建快捷方式的权限。

#### TV 端设置

[Jellyfin/Clients](https://jellyfin.org/clients/) 官网有提供客户端的下载，其中，有[安卓 TV 的客户端](https://github.com/jellyfin/jellyfin-androidtv/releases/tag/v0.11.5)。

此外，也可以利用 kodi 来访问 Jellyfin 媒体中心。具体的使用方式，可以阅读 [kodi](https://jellyfin.org/docs/general/clients/kodi.html) ，简要步骤如下：
- 添加 jellyfin 源：进入插键菜单，插键浏览器，选择从 zip 文件安装，浏览服务器中已经下载好的压缩包
- 从库安装 jellyfin 插键
- 为了避免之前 kodi 中添加的媒体资源重复，可以使用使用 jellyfin 插键中的重置本地数据库的功能

> 利用 kodi + jellyfin 插键的方式播放资源，不会对视频进行转码，这可能就会导致播放高质量视频时会有卡顿。

具体的设置，可以阅读：
- [B站司波图/Jellyfin的外网访问姿势，如何通过安卓，IOS，电视访问Jellyfin服务器](https://www.bilibili.com/video/BV1AE411k7hr)
- [SMZDM/安卓TV端Kodi部署Jellyfin，使用Jellyfin打造最强媒体中心（篇二）](https://post.smzdm.com/p/a99vlpmp/)

### 其他资源

- 字幕网站：
  - [opensubtitles](https://www.opensubtitles.org/zh)
  - [a4k](https://www.a4k.net/)
  - [subhd](https://subhd.tv)

### 参考

- [喵斯基部落/群晖Docker安装Jellyfin媒体服务器](https://www.moewah.com/archives/1552.html) 利用 Docker 部署 jellyfin 服务
- [SMZDM/使用群晖Docker 安装Jellyfin 家庭影院HTPC 比emby plex好用多了](https://post.smzdm.com/p/a6lnxg3g/)
- [B站司波图/免费开源影音服务器Jellyfin部署全攻略，含群晖，OMV系统下Docker安装并启动硬件转码](https://www.bilibili.com/video/BV1ME411o7H2)


## 电影刮削器 TinyMediaManager

### 参考

- [92NAS/群晖Docker里安装电影剧集刮削器TinyMediaManager](http://www.92nas.com/forum.php?mod=viewthread&tid=98)
- [KODI中文网/抛弃Kodi难用的刮削器 tinyMediaManager(TMM)刮削电影信息更方便](http://www.kodiplayer.cn/course/2945.html)

## PT

### 下载客户端

- [Transmission](https://transmissionbt.com/) Linux/MacOS
- [uTorrent](https://www.utorrent.com/downloads/mac)
- [Azureus](http://www.vuze.com/download.php)

### m-team

馒头，通过捐赠即可获得账号+1 个月的 VIP：
- [账号保留规则](https://wiki.m-team.cc/index.php?title=%E5%B8%B3%E8%99%9F%E4%BF%9D%E7%95%99%E8%A6%8F%E5%89%87_(Account_Parking_,_Keeping_Rules))
- [m-team 馒头捐赠页面](https://pt.m-team.cc/pay.php)
- [登录地址](https://kp.m-team.cc/index.p)
- [M-Team 外站](https://bbs.m-team.cc/)

## NAS 教程

- [花王群晖教程](https://www.huakings.cn/category/nas/8/) 该

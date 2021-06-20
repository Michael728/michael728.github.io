---
title: 小米电视大师 82 寸的使用心得
tags: [数码, 多媒体, APP]
toc: true
date: 2021-06-12 21:29:08
categories: 数码
---

## 前言

![效果](https://gitee.com/michael_xiang/images/raw/master/uPic/bncTZ6.png)

为了让已经拥有不错硬件的电视机发挥它应有的实力，
- **提高片源质量是非常关键的！**
- **提高片源质量是非常关键的！**
- **提高片源质量是非常关键的！**

因此，我购入了一款国外的 TV 盒子，它拥有原生的 Google Android 框架，可以支持安装 Youtube 和 Netflix，并且，该设备是经过 Netflix 认证的，可以看 4K 高清视频。关于这款盒子的使用，可以阅读 [Tivo Stream TV 使用笔记](https://michael728.github.io/2021/04/25/digital-products-tivo-tv/)

除了购入 TV 盒子之外，近期我还购入了 群晖 920+ NAS，可玩性也很高，可以阅读 [群晖 NAS 920+ 使用笔记](https://michael728.github.io/2021/05/30/digital-products-nas-started/)。

<!-- more -->
## APP

言归正传，
### aida64

这个软件可以方便的查看电视机的系统信息、CPU 架构等。

下载地址：[aida64官网](https://www.aida64.com/downloads)

我的电视是小米电视大师 82 寸液晶版本的，架构信息：

![CPU](https://gitee.com/michael_xiang/images/raw/master/uPic/D7AE31.png)

### x-plore

x-plore 真是个神器，不仅方便浏览目录结构，还有可以访问远端服务器上的资源，例如访问家里 NAS 中的共享文件夹、访问网盘中的文件……这样，就不用拿着 U 盘拷贝文件等琐碎的操作。

下载地址：[X-plore File Manager](https://x-plore-file-manager.cn.uptodown.com/android)

如果安装的 apk 在远端服务器上，直接安装时，它会先复制到电视机上的临时目录中，然后开始安装。

临时文件夹目录：
- `内部共享存储空间-》Android-》data-》com.lonelycatgames.Xplor-》cache-》temp`

### vlc

![VLC](https://gitee.com/michael_xiang/images/raw/master/uPic/kfLtWR.png)

对于一些新格式的视频，常见的播放器可能无法播放或者没有声音，而 VLC 这款开源播放器亲测兼容性很强！而且，功能非常丰富，各个平台都有对应客户端。通过它，可以直接播放 NAS 设备中共享的文件夹中的视频。

下载地址：[VideLAN](https://www.videolan.org/vlc/)
### jellyfin

![Jellyfin](https://gitee.com/michael_xiang/images/raw/master/uPic/b74vkw.png)

[Jellyfin](https://jellyfin.org/) 是一个开源的媒体库管理软件。我一开始是安装在 [Tivo TV 盒子](https://michael728.github.io/2021/04/25/digital-products-tivo-tv/)上的，后来想着这个 APP 也不依赖谷歌框架，因此，直接安装在了小米电视上。亲测，可用，这样，就少了一个切换信号源的操作了。

关于它的使用，我在 [群晖 NAS 920+ 使用笔记](https://michael728.github.io/2021/05/30/digital-products-nas-started/) 有详细的介绍。

下载地址：[jellyfin/jellyfin-androidtv](https://github.com/jellyfin/jellyfin-androidtv/releases/tag/v0.11.5)

> 目前，如果想要好看的海报墙，则在 TV 端打开 Jellyfin 客户端，如果图方便和稳定，则直接使用 TV 端的 VLC 播放器链接到 NAS 设备，然后播放视频文件。这两种方式，是目前我探索下来比较稳定、兼容性好的方案。

### emby

![Emby](https://gitee.com/michael_xiang/images/raw/master/uPic/VuoHhX.png)

Emby 是一个家庭媒体库软件，包含服务端和客户端。服务端用于整理电影和剧集，客户端连上服务端后就能播放这些影片。这个软件真是一款**神器**！类似 Jellyfin~

Emby 公益服维护了数量庞大的视频资源，但是它的获取目前比较有门槛，需要经过考试等步骤才可以。网上有热心的网友汇总了题库[语雀/Emby](https://www.yuque.com/ow/emby/ganwxl)。

关于它的资料，可以阅读 
- [Emby 公益服](https://embywiki.911997.xyz/)
- [油管 Bigdongdong/上万部电影和剧集免费来看！完全免费的EMBY公益媒体库分享](https://www.youtube.com/watch?v=NjUQWgN_hKg)

客户端下载：[Emby 公益服](https://embywiki.911997.xyz/use-on-various-devices/use-on-android-tv/using-official-client-on-android-tv.html)

### kodi

下载地址：[kodi](https://kodi.tv/download/android)

### 软件下载地址

- [apkmirror](https://www.apkmirror.com/)
- [aptoide](https://en.aptoide.com/)
- [apkpure](https://apkpure.com/cn/)
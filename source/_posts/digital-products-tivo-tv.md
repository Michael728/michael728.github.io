---
title: Tivo Stream TV 激活配置使用笔记
tags: [数码, 多媒体]
toc: true
date: 2021-04-25 21:29:08
categories: 数码
---

## 前言

搬到新家之后，陆续购入了若干大件电器，其中，电视机作为家庭客厅的一个重要输出终端设备，经过综合比较，最后剁手了小米电视大师 82 寸 4K 的版本。同期，红米也新推出了一款 86 寸的电视。但是，硬件貌似并没有大师系列好，同时，红米的这款不支持远场语音功能，每次要呼叫小爱同学，还需要找到遥控器然后语音控制，不方便。

目前国产电视机的硬件性能其实都还 OK，个人觉得影响观看体验的重要因素反而是片源！国内「优爱腾」流媒体的质量还有待提高，2K/4K 的片源质量堪忧，甚至 1080P 的片源都不行。因为小米电视并不是 Netflix 认证的设备，是无法直接安装 Netflix 官方 APP 的。因此，决定购入一个国外的电视盒子——TiVo Stream TV 盒子，然后在它上面安装 Netflix、Youtube 等 APP 观看世界各地高质量的片源了。

![效果图](https://gitee.com/michael_xiang/images/raw/master/uPic/nFIi8b.png)

上图就是效果图啦，美滋滋，顺带安利一部剧《纸钞屋》，西班牙剧真是给力！

## Tivo TV 激活

Tivo TV 设置激活：
- [悟空/如何激活Tivo Stream 4K](https://didiboy0702.gitbook.io/wukongdaily/test/ru-he-ji-huo-tivo-stream-4k)
- [悟空/Tivo Stream 4K 电视盒子开箱加9步长测评](https://www.youtube.com/watch?v=Rxv0E3kMa4Y)

## ADB 工具

为了解决盒子时间不正确以及提示网络不能联网的问题，需要用到 ADB 工具连接盒子进行设置。

ADB 各种客户端：
- 谷歌应用商店安装 `Remote ADB Shell`，可以通过手机来进行对 TV 盒子的设置
- Windows 平台也有对应的 ADB 工具，查看 [ADB Shell](https://adbshell.com/)、[悟空分享的网盘](https://drive.google.com/drive/folders/1PIT3issyC3qD_mjt9HRVJkM2qTlphXWk)
- Mac：`brew install android-platform-tools`

下载好 ADB 工具之后，怎么连接电视盒子？
- 查看设置，查看系统版本，连续点击「内部版本号」，开启开发者选项
- 在开发者选项里，打开网络调试/USB调试开关
- 查看电视盒子连接的 WiFi 网络，获取电视盒子的链接 IP
- `adb connect IP`

> 电视盒子的ADB开关通常有以下几个名字，例如USB调试开关，远程调试开关、网络调试开关

## 可以正常访问奈菲，但是无法观看 youtube

看一下你的 TV 盒子的系统时间，肯定于当前时间不一致，youtube 会有时间验证，临时的解决办法是打开“设置”-“设备偏好设置”，然后找到“日期和时间”，关闭“自动确定日期和时间”，然后手动更改的时间和日期为当前的正确时间。

一劳永逸的方法是去更改时间同步服务器。

### 修改时间服务器

- [安卓原生电视/盒子 出现已连接但无法访问 要怎么解决？根治请看这里！](https://www.bilibili.com/video/av286661109/)

修复盒子时间不对的问题，因为时间不对，可能会影响证书验证：
``` shell
# 查看盒子 NTP Server
adb shell settings get global ntp_server
# 设置 NTP Server
adb shell settings put global ntp_server ntp1.aliyun.com
```

## 此 WLAN 网络无法连接到互联网

Tivo TV 盒子弹出：您所连接的 WLAN 网络无法访问互联网。

虽然弹出上面的提示，但是实际上有些 APP 是可以正常上网的，唯独 Youtube 就无法打开，显示”现在无法联网“！

### 问题原因

谷歌从 Android 5.0 开始就引入了 `Captive Portal` 机制，主要用来检测 WiFI 网络认证是否正常，默认检测访问的是谷歌服务器（`gstatic.com/google.com`）。通过HTTP返回的状态码是否是 204 来判断是否成功，如果访问得到了 200 带网页数据，那你就可能处在一个需要登录验证才能上网的环境里，比如说校园网，再比如说一些酒店提供的客户才能免费使用的WiFi(其实是通过DNS劫持实现的)，如果连接超时(根本就连接不上)就在 WiFi 图标和信号图标上加一个标志，安卓5和6是叹号，安卓7改成一个叉了。只不过默认访问的是谷歌自家的验证服务器，然而由于你懂的原因，就算你连接上了网络也连不上这个服务器。这就是所说的 `generate_204` 的问题。

国内安卓手机系统都会修改成自家的服务器地址或者高通中国的地址，以此来实现该功能。

### 解决步骤
```
# 查看当前状态：
adb shell settings get global captive_portal_mode
# 关闭检测：
adb shell settings put global captive_portal_mode 0
# 查看机器已有的验证服务器，方便备份 
adb shell settings get global captive_portal_http_url
adb shell settings get global captive_portal_https_url
# 删除验证服务器
adb shell settings delete global captive_portal_http_url
adb shell settings delete global captive_portal_https_url
# 设置验证服务器
adb shell settings put global captive_portal_http_url http://www.google.cn/generate_204
adb shell settings put global captive_portal_https_url https://www.google.cn/generate_204
```

小米的验证服务器在国内应该是延迟最低的。
```
settings put global captive_portal_http_url http://connect.rom.miui.com/generate_204
settings put global captive_portal_https_url https://connect.rom.miui.com/generate_204
```

验证服务器：
- 华为： http://connectivitycheck.platform.hicloud.com/generate_204
- Vivo： http://wifi.vivo.com.cn/generate_204
- 小米：http://connect.rom.miui.com/generate_204
- Google 大陆： www.google.cn/generate_204

> 给 captive_portal_https_url 赋值，改为 https 链接

以上设置了不一定能解决问题，可能还需要清理 play 数据和缓存，重启盒子。

> 其实上面步骤在我的软路由环境下不存在，因为很多科学插键的代理域名中已经包含了谷歌那个验证服务器的域名，因此，TV 盒子就不会报错误了。

### 参考

- [原生 Android 网络去叉／叹号 Android 5.0 - 10.0](https://ericclose.github.io/Captive-Portal-Android.html)
- [摩西数码/shield tv无法访问互联网无法观看youtube/无线ADB教程](http://www.moxishuma.cn/index.php/archives/32/)
- [记录一次 Android TV 网络访问排障](https://a-li.me/844.html)
- [油管悟空/Shield TV 出现已连接但无法访问 要怎么解决？Android TV的通用解决办法看这里！再说一遍~](https://www.youtube.com/watch?v=Hsmp0IfCZfw)
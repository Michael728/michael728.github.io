---
title: R4S 使用笔记
date: 2021-04-24 23:21:24
tags: [网络, 数码, 多媒体]
categories: 数码
keywords: 软路由
---

## 介绍

为了让新家的网络可以自由访问各大流媒体资源，以及方便自己开发环境中下载各种国外资源依赖软件包等，购入了 R4S 软路由设备。

<!-- more -->
## 固件下载

- [官方固件/NanoPi R4S/zh](https://wiki.friendlyarm.com/wiki/index.php/NanoPi_R4S/zh#.E4.B8.8B.E8.BD.BD.E7.B3.BB.E7.BB.9F.E5.9B.BA.E4.BB.B6) 对应的[网盘地址/5fd2](https://pan.baidu.com/share/init?surl=IVKSPo5m4SGGh8z_hY_hTg )
- [klever1988/nanopi-openwrt](https://github.com/klever1988/nanopi-openwrt) Star 2.3K，推荐
- [SuLingGG/OpenWrt-Rpi](https://github.com/SuLingGG/OpenWrt-Rpi) Star 2.1K
- [QiuSimons/R2S-R4S-X86-OpenWrt](https://github.com/QiuSimons/R2S-R4S-X86-OpenWrt)
- [Dongdong/R4S第三方固件](https://bigdongdong.gitbook.io/nanopi-r2s/r4s-gu-jian-ji-gong-ju-xia-zai) 
- [骷髅头/DHDAXCW/NanoPi-R4S-2021](https://github.com/DHDAXCW/NanoPi-R4S-2021/releases/) 

> 语雀上有个文章总结的挺好，推荐 [NanoPi-R2S / NanoPi-R4S 常见第三方固件选择](https://www.yuque.com/friendlyelec/r2s/gd4g3l)

如果想自己编译固件，可以看如下资源：
- [coolsnowwolf/lede](https://github.com/coolsnowwolf/lede) Lean的Openwrt源码仓库，高 Star 项目

## 刷固件

- [洋葱油管/R2S软路由销量之王！R2S安装openwrt攻略 openwrt软路由设置](https://www.youtube.com/watch?v=ZCmbbnIBD78)
- [DongDong/R2S 使用指南](https://www.youtube.com/watch?v=UYl-dgrPnTI&t=114s)

刷固件的软件：
- [balenaEtcher](https://www.balena.io/etcher/?ref=etcher_footer)
- [bigdongdong/R2S/R4S 使用指导](https://bigdongdong.gitbook.io/nanopi-r2s/)

写盘工具：可以使用 Rufus 或者 balenaEtcher，其实，就和之前做 Linux 启动盘一样，就是拿着固件镜像文件做一个启动盘。

注意点：刷完固件的 TF 卡下次插入电脑，可能会弹出需要格式化，千万不要进行格式化，否则可能会丢失容量。下次刷固件，不用格式化就可以直接刷固件即可！

balenaEtcher 刷固件
1. select image：选择要刷入的固件
2. select dirve：选择 tf 卡，也就是要将固件刷入的存储介质
3. flash：开始刷入固件

刷好固件的 TF 卡插入 R4S，SYS 灯常亮之后，通过一根网线将 R4S 的 LAN 口和电脑连接。

接着就可以打开 R4S 的后台，浏览器访问 192.168.2.1。
- 用户名 root
- 密码 password

至此， R4S 的系统已经安装启动完毕，接着开始配置网络。

## 网络配置

要搞清楚是猫拨号还是路由器拨号。进入路由器后台观察它的上网方式设置，如果是 DHCP 的方式，则表示是光猫拨号上网的方式。

连接设备：光猫的线接入 R4S 的 WAN 口，然后用网线将 R4S 的 LAN 口和路由器的 WAN 口相连。

> 个人理解，WAN 口就是信号的入口，LAN 口就是信号的出口，这么理解，就知道上面为何这么连接了。

备注：路由器的上网方式其实就是对 WAN 口进行设置，局域网设置的就是对 LAN 口设置的。

![网络接线](https://gitee.com/michael_xiang/images/raw/master/uPic/8TOcgI.png)

> 图片来自 [语雀/NanoPi R2S / R4S 软路由常见网络接线指引](https://www.yuque.com/friendlyelec/r2s/fh1msa)

### 将无线路由器改为无线交换机，有线中继模式

所有设备都在一个局域网，没有路由功能，少一次转发

判断路由器是否支持场景二的设置：登录路由器后台，判断路由器是否支持无线接入点（AP）模式/有线中继模式，他们一样，只是叫法不同。

> 小米路由器在上网设置菜单下有工作模式的切换

后台管理地址一定要记住：重启之后，这台路由器就不是路由器了，就是一台无线交换机，以后，路由器的网口没有 WAN 口和 LAN 区分了。

- 如果是光猫拨号，网络接口的 WAN 协议就是 DHCP 客户端
- 如果是软路由拨号上网，就切成 PPPoE，配置宽带账号密码（填好重启光猫）

## 路由器设置

- [NanoPi R4S 上手体验](https://blog.qust.me/posts/nanopir4s/) 介绍了 OpenClash 的使用，简略
- [资料：悟空的日常](https://didiboy0702.gitbook.io/wukongdaily/)
- 测速网站：∂https://fast.com/zh/cn/

### 设置桥接模式

如果为了让软路由的 IP 和家里联网设备都处于同一网段，那么，可以将路由器设为桥接模式，这样，路由器就相当于 AP，接入路由器的设备不会处于另外的网段。方便在软路由中针对具体的 IP 设备进行联网控制。

小米路由器进入后台：常用设置-》上网设置-》工作模式切换，选择「有线中继工作模式」：

![工作模式](https://gitee.com/michael_xiang/images/raw/master/uPic/qP6Vec.png)

> 要记住改为中继模式后的管理后台 IP

![管理台](https://gitee.com/michael_xiang/images/raw/master/uPic/T8y7cW.png)

## Passwall 使用

### 基本设置

![启用](https://gitee.com/michael_xiang/images/raw/master/uPic/izrusV.png)

- UDP 一般选择与 TCP 节点一致
- 需要勾选「主开关」保存并应用才真正启用

模式

- TCP 默认代理模式：一般选择「中国列表以外」
- UDP 默认代理模式：默认是「游戏模式」
- 路由器自身 TCP 代理模式：
- 路由器自身 UDP 代理模式：

选项说明：
- 不代理：不启用代理
- 全局代理：全部网站走代理
- GFW 列表：仅那些被屏蔽的网站走代理
- 中国列表以外：只要是外国的都走代理
- 中国列表：这个模式一般是国外的用于访问国内网站的，暂时用不到

### 访问控制

控制连接到局域网中的设备，主要是控制他们使用什么节点或者什么模式来访问网络。

> 要能控制这些设备，有个前提，这些设备要和软路由在同一网段下。

一般家庭网络布置是如下的：

![光猫拨号](https://gitee.com/michael_xiang/images/raw/master/uPic/Zi5HbO.png)

上面这种方式，路由器一般是 「DHCP 方式/动态 IP」。只能保证无线接入的设备和路由器在一个网段，但是，路由器和软路由并不在一个网段。这种情况，「访问控制」是无法使用的，因为对于软路由来说，只有路由器这个设备是它能控制的。除此以外，还多了一个缺点，多了一次转发。

![推荐模式](https://gitee.com/michael_xiang/images/raw/master/uPic/sSGwHY.png)

上图是 UP 主推荐的模式，避免家中网段过多，光猫改为桥接模式，软路由负责拨号，路由器设为「AP模式/桥接模式/有线中继模式」。这样设置，设备就处于同一网段下了。此时，路由器就类似是一个交换机的角色，他不会像 DHCP 那样，产生一个新的网段。

![简化](https://gitee.com/michael_xiang/images/raw/master/uPic/iZOSy2.png)

有时候运宽带运营商是不推荐让光猫改为桥接模式，因为改为桥接模式他们没法直接后台操作网络，升级等。现在光猫的性能都不差，也没必要非要改为桥接模式，让其他设备拨号。上图的模式就比较方便，仅仅是将路由器的模式修改为「AP模式/桥接模式/有线中继模式」，路由器连接的设备也可以和软路由处在同一网段。

如下的设置，可以实现局域网内某些设备是否可以走代理：

![示例](https://gitee.com/michael_xiang/images/raw/master/uPic/nsCBOu.png)

比如我们使用的 NAS，一般都是国外厂商的，当我们模式选择「中国列表以外」时，一般他们的网站都会被代理，这时候想让他们不走代理或者为GFW 列表的方式，就可以使用访问控制的功能来设置了。

### 自动切换

自动切换功能就是添加若干个备用节点，当其中一个节点不可用，会自动切换到备用节点，实现「高可用」。

![自动切换](https://gitee.com/michael_xiang/images/raw/master/uPic/uruSYQ.png)

### 直连/代理名单管理

- 直连列表：可以实现加入的 域名/IP 不走代理，这就可以让一些防火墙中的网址依然不能访问，对所有模式生效，优先级最高
- 代理列表：加入的 域名/IP 将走代理；
- 屏蔽列表：加入的 域名/IP 将屏蔽；

举例，`https://www.speedtest.net/` 这个测速网站是国外网站，如果在使用代理的情况，默认会是在国外节点进行测速，如果我们将它加到「直连列表」中，则会测量我们家中实际运营商的网速，这才是我们关心的：

![测速示例](https://gitee.com/michael_xiang/images/raw/master/uPic/FjVAl8.png)

### 高级设置

设置节点数量，可以使用多个 TCP 节点，然后配合上面介绍的访问控制，可以实现针对某个设备指定使用某个节点：

![节点数量](https://gitee.com/michael_xiang/images/raw/master/uPic/TnR7eT.png)

![指定节点](https://gitee.com/michael_xiang/images/raw/master/uPic/uHBDsU.png)

> 例如使用某些不限流量的节点给 NAS 云盘备份使用

### 分流

实现让特定的网站使用指定的节点，比如奈菲使用新加坡节点，油管使用香港节点。

只需要在「节点列表」-》「添加」，按照如下示例选择：

![分流](https://gitee.com/michael_xiang/images/raw/master/uPic/TP1VOf.png)

当创建好这个「分流节点」之后，需要在「基本设置」中，选择我们刚刚创建的这个节点。这样，就可以实现分流了。

### 参考

- [油管/使用PassWALL 完美掌控内网科学上网](https://www.youtube.com/watch?v=qkga9DN5H08&t=7s)

## 软路由设置重启

在使用一周左右，发现网速降低比较明显，重启软路由之后正常了。因此，决定设置定时任务，让软路由每天清晨自动重启。

- 首先在系统》启动项中，检查 `cron` 服务是启用的状态
- 在系统》计划任务的输入框中，添加 `0 4 * * * sleep 70 && touch /etc/banner && reboot`，然后提交保存。计划每天凌晨 4 点重启

> 上面这个定时任务，其实，直接进入 TTYD 终端界面，`crontab -e` 也可以编辑保存

参考：
- [使用 Cron 定时重启 Openwrt 路由器](https://einverne.github.io/post/2017/03/auto-reboot-openwrt.html)

## 客户端

客户端：
- [Clash andriod](https://github.com/Kr328/ClashForAndroid/releases)
- [smartyoutubetv](https://smartyoutubetv.github.io/) 针对没有谷歌框架的设备可以安装的 Youtube 客户端
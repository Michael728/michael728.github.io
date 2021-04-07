---
title: ss+vps+mac
date: 2019-04-27 14:21:24
tags: [ss,vps]
categories: ToolsDaily
keywords:
---

Vultr 选择的是日本的节点，发现速度比较不错，油管视频 1080P 无压力！而且，感觉 Vultr 的界面也很清新，用着很方便。

<a href="https://www.vultr.com/?ref=7488919"><img src="https://www.vultr.com/media/banners/banner_800x418.png" width="800" height="418"></a>

<!-- more -->

我自搭梯子，选择的是：~~`Shadowsocks-libev+开启simple-obs插键+BBR加速`~~

20191013 更新，发现使用 kcptun 加速效果更明显！于是改为`Shadowsocks-libev+开启simple-obs插键+ kcptun加速`

Vultr 通过我的链接注册充值，你我都可以获得 $10 账户返利，所以，如果有需要，就点击注册吧：

- [Vultr 注册返利链接](https://www.vultr.com/?ref=7488919)

## VPS

vultr速度比较：

- [vultr-downloadspeedtests](https://www.vultr.com/faq/#downloadspeedtests)
- [vultr中文网](https://www.thevultr.org/)

我选择的节点是日本的节点，离得近，速度能快点。

建议选择 Ubuntu 18 或者 CentOS 8 开始的版本，这样就默认开启了 BBR 的设置，有加速的效果。

### 设置时区

时区保持一致很重要，尤其是 v2ray 方式：

``` shell
# 查看时间和本地时间是否一致
date -R
# 设置一致，东八区
sudo -i
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
date -R
```

### BBR

BBR 开启，可以起到加速作用。如果用的是 CentOS 8 或者 Ubuntu 18 的话，可能都不需要开启，因为默认就开启了。

BBR 一键安装：

```shell
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh
```

安装过程，需要重启机器一次。重启之后，输入 `lsmod | grep bbr` 看到输出有 `bbr` 则代表 OK 了。

参考：

- [上网配置](https://github.com/lyf3/ss/issues/1) bbr 开启和 ssr 搭建
- [Ubuntu 18.04/18.10快速开启Google BBR的方法](https://www.moerats.com/archives/612/)

### 辅助工具

- https://tools.ipip.net/traceroute.php
- http://ping.pe/ping.php
- https://www.uuidgenerator.net/

## shadowsocks

### 安装 ss

在 VPS 机器上安装好 ss 客户端：

```shell
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
chmod +x shadowsocks-all.sh
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```

**注意，建议选择 `Shadowsocks-libev` 版本。**

按照提示，设置好端口、密码、选择加载 simple-obfs 插件。

卸载的命令：

```
./shadowsocks-all.sh uninstall
```

### ss 服务端相关配置

启动脚本后面的参数含义，从左至右依次为：启动，停止，重启，查看状态。

- Shadowsocks-Python 版：`/etc/init.d/shadowsocks-python start | stop | restart | status`
- ShadowsocksR 版：`/etc/init.d/shadowsocks-r start | stop | restart | status`
- Shadowsocks-Go 版：`/etc/init.d/shadowsocks-go start | stop | restart | status`
- Shadowsocks-libev 版：`/etc/init.d/shadowsocks-libev start | stop | restart | status`

配置文件：

```shell
Shadowsocks-Python 版：
/etc/shadowsocks-python/config.json

ShadowsocksR 版：
/etc/shadowsocks-r/config.json

Shadowsocks-Go 版：
/etc/shadowsocks-go/config.json

Shadowsocks-libev 版：
/etc/shadowsocks-libev/config.json
```

`netstat -nl | grep 12250` 或 `ss -nl | grep 12250` 可以查看 ss 服务的端口，12250 仅仅是示例，需要换成你自己设置的 ss 端口。

ss 服务端配置参考：

```shell
[root@vultr ss]# cat /etc/shadowsocks-libev/config.json
{
    "server":"0.0.0.0",
    "server_port":12250,
    "password":"xxxx",
    "timeout":300,
    "user":"nobody",
    "method":"aes-256-gcm",
    "fast_open":false,
    "nameserver":"8.8.8.8",
    "mode":"tcp_and_udp",
    "plugin":"obfs-server",
    "plugin_opts":"obfs=http"
}
```

## 加速配置

### kcptun

除了上面介绍的 BBR 加速方式之外，还有 kcptun 的加速。二者选择一种即可。

![kcptun原理](https://gitee.com/michael_xiang/images/raw/master/T0EYuk.jpg)

```shell
mkdir /data && cd /data
wget --no-check-certificate https://github.com/kuoruan/shell-scripts/raw/master/kcptun/kcptun.sh
chmod +x ./kcptun.sh
./kcptun.sh
```

配置：

- 端口：默认29900，即为KCPTUN与其客户端连接使用的端口，默认即可，这个端口号其实也是有用的，客户端启用 kcptun 插件时，端口要写这个，而不是 ss 的端口号！！！
- 要加速的地址：默认 `127.0.0.1`。
- 要加速的端口：设置为你的 SS/SSR 使用的端口，这里要注意！！！
- 密码：自己设置，用于KCPTUN客户端连接使用，不要使用默认密码。
- 加密方式选择：较强的加密方式会影响网速，建议默认aes或不加密。
- 加速模式：默认fast即可。随后可以手动修改为其它模式，测试加速效果。
- MTU：默认1350即可。
- sndwnd：发送窗口大小，与服务器的上传带宽大小有关，这项与rcvwnd的比例会影响加速效果，可以暂时设置为默认的512，服务端的这个值和客户端的 rcvwnd 关系紧密，影响速度，需要调试
- rcvwnd：接收窗口大小，与服务器的下载带宽大小有关，也可以暂设置为默认的512，或者1024也可以。
- 以下几项中，除了数据压缩外，其它保持默认参数即可。建议关闭数据压缩，可以在一定程度上提升传输效率。

其余各项设置，保持默认即可，设置完成后，按任意键开始安装过程。

安装好之后，记录下最后的输出信息，后面有用。

kcptun 服务端配置参考：

```shell
[root@vultr ss]# cat /usr/local/kcptun/server-config.json
{
  "listen": ":39901",
  "target": "127.0.0.1:12250",
  "key": "xxxx",
  "crypt": "aes",
  "mode": "fast2",
  "mtu": 1350,
  "sndwnd": 2048,
  "rcvwnd": 2048,
  "datashard": 10,
  "parityshard": 3,
  "dscp": 0,
  "nocomp": false,
  "quiet": false,
  "tcp": false,
  "pprof": false
}
```

KCPTUN常用功能及命令：

- KCPTUN安装目录：`/usr/local/kcptun`
- KCPTUN的参数配置文件：`/usr/local/kcptun/server-config.json`
- 启动：`supervisorctl start kcptun`
- 停止：`supervisorctl stop kcptun`
- 重启：`supervisorctl restart kcptun`
- 状态：`supervisorctl status kcptun`
- 打印出服务器信息：`./kcptun.sh show` ——— 多亏了这个命令，看到了 `remoteaddr` 值，这个也是启用 kcptun 插键后，你本地 ss 客户端要配置的 IP 和 端口号
- 更新：`./kcptun.sh update`
- 查看日志：`./kcptun.sh log`
- 卸载：`./kcptun.sh uninstall`
- 更多使用说明 `cd /data && ./kcptun.sh help`

配置参数调试：

- 同时在两端逐步增大 client rcvwnd 和 server sndwnd;
- 尝试下载，观察如果带宽利用率（服务器＋客户端两端都要观察）接近物理带宽则停止，否则跳转到第一步。

参考：

- [KCPTUN-网络加速方案](http://www.jouypub.com/2019/9c4df700f0f76848f7042858a2b71a8a/)
- [kuoruan/shell-scripts](https://github.com/kuoruan/shell-scripts)
- [Kcptun 服务端一键安装脚本](https://blog.kuoruan.com/110.html)
- [KCPTUN](https://lvii.gitbooks.io/outman/content/kcptun.html)


## ss 客户端

### 客户端下载

- [Shadowsocks各种客户端](https://shadowsocks.org/en/download/clients.html)
- [ShadowsocksX-NG/releases](https://github.com/shadowsocks/ShadowsocksX-NG/releases) Mac 客户端下载
- [xtaci/kcptun](https://github.com/xtaci/kcptun/releases)
- [shadowsocks/shadowsocks-android](https://github.com/shadowsocks/shadowsocks-android/releases) Android
- [插键-shadowsocks/kcptun-android](https://github.com/shadowsocks/kcptun-android/releases) kcptun 加速需要使用的插键
- [插键-shadowsocks/simple-obfs-android](https://github.com/shadowsocks/simple-obfs-android/releases)

### 客户端配置

shadowsocks 客户端要设置的是你 vps 上 kcptun 的默认端口，例如 29900，而不是你设置的 shadowsocks 端口，否则，你使用 kcptun 插键将不会生效，网络会无法访问，除非你客户端不启用 kcptun 插键。

![ss 配置](https://gitee.com/michael_xiang/images/raw/master/aSx9r8.png)

- 端口号填写 KCPTun 的端口号，这个注意点坑了我好久！
- 加密方式填写 SS 的加密方式
- 密码填写 SS 的密码
- 插件直接手动输入 `kcptun`。
- 插件选项填写 tcptun 安装完成图中的 手机参数可使用 下面的记录即可

- [NG 客户端插键配置](https://github.com/shadowsocks/ShadowsocksX-NG/wiki/SIP003-Plugin)

kcptun 插键选项：

```shell
key=xxx;crypt=aes;mode=fast3;
```

simple-obfs 插键选项：

```shell
obfs=http;obfs-host=www.bing.com
```

## v2ray 方式

在使用 v2ray 之后，发现这个方式更加便捷一点，因为它既支持 shadowsocks 协议，又支持 vmess 协议，提供两种链接方式。

于是，目前（20191103）就采用了 v2ray 的方式+bbr加速+kcptun加速，然后配置了 v2ray 和 ss 的链接方式。

一键安装脚本：

```shell
bash <(curl -s -L https://git.io/v2ray.sh)
```

- v2ray 端口：62666
- 选择安装 ss
- ss 端口：62888

v2ray 配置生成器：https://intmainreturn0.com/v2ray-config-gen/

- 输入 v2ray url 可生成 vmess URL 链接 / 输入 v2ray qr 可生成二维码链接
- 输入 v2ray ssqr 可生成 Shadowsocks 二维码链接

相关路径：

- 在 Linux 中，日志通常在 `/var/log/v2ray/access.log` 文件中

### 教程

- [v2ray 官方文档](https://www.v2ray.com/chapter_00/install.html)
- [白话文教程](https://toutyrater.github.io/)
- [白话文教程-社区版](https://guide.v2fly.org/)
- [V2Ray完全使用教程](https://yuan.ga/v2ray-complete-tutorial/)
- [服务端搭建Shadowsocks和V2Ray详解](https://www.yeahwu.com/posts/8f7ac171.html) 这位博主，貌似真的把可用的账号提供出来了！
- [油管视频-V2Ray官方脚本搭建教程](https://www.youtube.com/watch?v=Mm6OWeo8dYU)

### 客户端

[神一样的工具们](https://www.v2ray.com/awesome/tools.html) 官方文档里也放了链接（这个连接需要 FQ 才能打开），下面列一下我自己用的：

- [yanue/V2rayU](https://github.com/yanue/V2rayU/releases) Mac 最新一次更新 20190920
- [2dust/v2rayNG](https://github.com/2dust/v2rayNG/releases) Andriod 最新一次更新 20191101
- [v2ray/v2ray-core](https://github.com/v2ray/v2ray-core/releases)

## FAQ

### 查看日志

- 查看本地客户端的 ss 日志：点击小飞机-》显示日志-》`~/Libray/Logs/ss-local.log`
- 查看本地 kcptun 插键是否启动： `ps -ef | grep kcptun`
- 查看服务端 kcptun 的日志，如果本地请求正常发送，那么会有日志输出：`cd data && ./kcptun.sh log`

## 其他

Docker方式

- [docker文档](https://docs.docker.com/install/linux/docker-ce/centos/#install-using-the-repository)
- [docker-ss-搜索结果](https://hub.docker.com/search/?isAutomated=0&isOfficial=0&page=1&pullCount=1&q=shadowsocks&starCount=0)

网关方式：
- [油管达人分析的各种方式的优缺点](https://www.youtube.com/watch?v=f9ohvZyQrmY&list=PLqybz7NWybwUgR-S6m78tfd-lV4sBvGFG&index=3) 他最推荐的还是网关方式，此外还有 trojan、软路由等方式

## 参考

- [haoel-科学上网](https://github.com/haoel/haoel.github.io) 耗子叔写的
- [聪聪-SS/SSR 简介](https://congcong0806.github.io/2018/04/20/SS/)
- [写给非专业人士看的 Shadowsocks 简介](http://vc2tea.com/whats-shadowsocks/)
- [VPS+ShadowsocksR 搭建自己的 VPN](https://www.liaoyuqin.com/post/tools/ha-bi-da-ti-zi)
- [Ubuntu 16.04下Shadowsocks服务器端安装及优化](https://www.polarxiong.com/archives/Ubuntu-16-04%E4%B8%8BShadowsocks%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%AB%AF%E5%AE%89%E8%A3%85%E5%8F%8A%E4%BC%98%E5%8C%96.html)
- [秋水逸冰-使用 Docker 快速部署 Shadowsocks-libev + v2ray-plugin](https://teddysun.com/569.html)
- [使用docker快速部署shadowsocks-libev+v2ray-plugin代理](https://ssu.tw/index.php/archives/31/)
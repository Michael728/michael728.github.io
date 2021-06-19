---
title: sdkman 来管理多 JDK 版本的环境
date: 2021-05-18 23:21:24
tags: [ 开发环境, JDK]
categories: ToolsDev
keywords: JDK
---

## 背景

![sdkman](https://gitee.com/michael_xiang/images/raw/master/uPic/cIj20k.png)

[sdkman 官网]是这么介绍它的：

> SDKMAN! is a tool for managing parallel versions of multiple Software Development Kits on most Unix based systems.

sdkman 是一个用来管理大多数类 Unix 系统（例如 Mac OSX、Linux、Cygwin等） SDK 多版本的。例如，现在个人机器上主要还是使用 JDK8 的版本，但是，突然有个项目（比如新版 Elasticsearch 7.10）需要更高版本的 JDK 版本，这时候怎么方便管理你机器上的 JDK 环境呢？

看完下文的操作，你就会用 sdkman 来灵活切换你 SDK 的版本，真的方便！

## 常用命令

安装 sdkman：
```
curl -s "https://get.sdkman.io" | bash
```

<!-- more -->

打开新的终端窗口：
```
source "$HOME/.sdkman/bin/sdkman-init.sh"
```

验证安装：
```
sdk version
```

sdkman 支持多达大约 29 个软件开发包管理，我们也可以使用  命令来查看支持的完整列表：
```
sdk list
```

这个内容比较多，可以使用例如 `sdk list java` 列出我感兴趣的 `candidate` 版本。

管理本地已经安装的 JDK 版本：
```
sdk install java 1.8.0_231 /Library/Java/JavaVirtualMachines/jdk1.8.0_231.jdk/Contents/Home
```

![local](https://gitee.com/michael_xiang/images/raw/master/uPic/tc4ZC6.png)

> 其实就是在 `/Users/michael/.sdkman/candidates/java` 路径下，创建一个软连接 `1.8.0_231` 指向了机器原本的 JDK 安装目录

安装指定的版本：
```
sdk install java 16.0.0.hs-adpt
```

临时使用指定版本（关闭终端之后失效）：
```
sdk use java 1.8.0_231
```

设置默认版本：
```
sdk default java 1.8.0_231
```

查看当前使用的版本：
```
# 查看 java 当前版本
sdk current java
# 查看所有版本
sdk current
```

卸载指定版本的包：
```
sdk uninstall java 16.0.0.hs-adpt
```

> 如果卸载之后想再次安装之前通过 sdkman 卸载的版本，此时不会再次重新下载，会提示 `Found a previously downloaded java 11.0.7.hs-adpt archive. Not downloading it again...`，因为之前删除操作并没有真正的从你计算机上删除源压缩包文件

清理：
```
# 清理广播消息
sdk flush broadcast
# 清理下载的 sdk 二进制文件
sdk flush archives
# 清理临时文件内容
sdk flush temp
```

升级 sdkman
```
sdk selfupdate
```

## 参考

- [sdkman/Usage](https://sdkman.io/usage#installspecific)
- [segmentfault/如何在一台计算机上安装多个 JDK 版本](https://segmentfault.com/a/1190000022666856)
- [segmentfault/Java升级那么快，多个版本如何灵活切换和管理？](https://segmentfault.com/a/1190000021037771?utm_source=sf-similar-article)
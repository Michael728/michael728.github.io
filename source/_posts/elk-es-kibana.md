---
title: Kibana 学习笔记
date: 2021-02-14 23:10:57
categories: ELK
tags:
    - ELK
    - Search
keywords:
  - Elasticsearch
  - Kibana
---

![Search](https://gitee.com/michael_xiang/images/raw/master/uPic/pexels-andrea-piacquadio-3769697.jpg)

## Kibana 安装

官方文档 [Installing Kibana](https://www.elastic.co/guide/en/kibana/current/install.html) 中提供了多种安装包对应的指导链接！本文就先选择 [tar 包](https://www.elastic.co/guide/en/kibana/current/targz.html)的方式安装。

### 下载 Kibana 安装包

同样，Kibana 在我司镜像站上也有对应的软件包：

```shell
https://mirrors.huaweicloud.com/kibana/7.3.2/
wget https://mirrors.huaweicloud.com/kibana/7.3.2/kibana-7.3.2-linux-x86_64.tar.gz
wget https://mirrors.huaweicloud.com/kibana/7.3.2/kibana-7.3.2-linux-x86_64.tar.gz.sha512
shasum -a 512 -c elasticsearch-7.3.2-linux-x86_64.tar.gz.sha512
tar xzf kibana-7.3.2-linux-x86_64.tar.gz
chown -R michael kibana-7.3.2-linux-x86_64
cd kibana-7.3.2-linux-x86_64
```

> Mac 环境，下载的安装包是 kibana-7.3.2-darwin-x86_64.tar.gz。因为我是从华为镜像站下载的，直接运行可能会提示安全告警，运行这样的命令即可去除告警 `xattr -d -r com.apple.quarantine kibana-7.3.2-darwin-x86_64`

### 配置 Kibana

```shell
egrep -v "^#|^$" config/kibana.yml # 如下内容是修改的配置
server.port: 5601
server.host: "0.0.0.0"
elasticsearch.hosts: ["http://127.0.0.1:9200"]
kibana.index: ".kibana"
```

更多配置内容，可以阅读 [Configuring Kibana](https://www.elastic.co/guide/en/kibana/current/settings.html)

Kibana 链接 ES 以后，会把相关的数据写入 `.kibana` 开头的 index 中。

### 运行 Kibana

如下方式可以实现后台运行，避免 Ctrl+C 终止了程序：

```shell
nohup bin/kibana &
```

访问：`http://127.0.0.1:5601/`

这时候可以看到我们之前搭建的集群节点了：

![Kiana-ES](https://gitee.com/michael_xiang/images/raw/master/Dg7DMe.png)

## Dev Tools

Kibana Console 可以执行一些 API 请求，Help 中有相关快捷键的操作说明：

![Dev Tools](https://gitee.com/michael_xiang/images/raw/master/uPic/5GPJYG.png)

快捷键：
- `Ctrl/Cmd + /` API 文档

## Kibana Plugins

安装命令和 ES 类似，提供了如下常用命令：
- `bin/kibana-plugin install plugin_location`
- `bin/kibana-plugin list`

## 补充

Cerebro 可以用来管理查看 ES

除了上述安装方式之外，还有用 Docker 方式安装的，可以参考：
- [onebirdrocks/geektime-ELK](https://github.com/onebirdrocks/geektime-ELK/blob/master/part-1/2.3-%E5%9C%A8Docker%E5%AE%B9%E5%99%A8%E4%B8%AD%E8%BF%90%E8%A1%8CElasticsearch%2CKibana%E5%92%8CCerebro/7.x-docker-2-es-instances/docker-compose.yaml)
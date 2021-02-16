---
title: DevOps 之 ELK 基础设施搭建
date: 2019-08-14 22:10:57
categories: ELK
tags:
    - CICD
    - DevOps
    - ELK
    - ES
keywords:
  - Elasticsearch
  - Kibana
  - Logstash
  - Filebeat
---

## 简介

![ELK](https://gitee.com/michael_xiang/images/raw/master/201909078-elk.png)

## Kibana

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

### 配置 Kibana

```shell
egrep -v "^#|^$" config/kibana.yml # 如下内容是修改的配置
server.port: 5601
server.host: "0.0.0.0"
elasticsearch.hosts: ["http://192.168.3.43:9200"]
kibana.index: ".kibana"
```

更多配置内容，可以阅读 [Configuring Kibana](https://www.elastic.co/guide/en/kibana/current/settings.html)

### 运行 Kibana

如下方式可以实现后台运行，避免 Ctrl+C 终止了程序：

```shell
nohup bin/kibana &
```

访问：`http://192.168.3.43:5601/`

这时候可以看到我们之前搭建的集群节点了：

![Kiana-ES](https://gitee.com/michael_xiang/images/raw/master/Dg7DMe.png)

## Filebeat && Logstash

```shell
https://mirrors.huaweicloud.com/filebeat/7.3.0/
wget https://mirrors.huaweicloud.com/filebeat/7.3.0/filebeat-7.3.0-linux-x86_64.tar.gz
wget https://mirrors.huaweicloud.com/filebeat/7.3.0/filebeat-7.3.0-linux-x86_64.tar.gz.sha512
```

目前项目中是采用的直接往 Elasticsearch 中存储数据 + Kibana 展示数据的方式。所以，[Filebeat](https://www.elastic.co/cn/products/beats/filebeat) 和 Logstash 暂时没接触过。不过，在 B 站上看到一位 UP 主发了很多相关的学习教程，安利一下：

- [EP26 - 安装与初始化配置ELK 6](https://www.bilibili.com/video/av23720863)
- [ELK搜索结果](https://ngx.hk/?s=elk) 上面这位 UP 主的博客

## 总结

本文是通过 tar 包方式安装的，发现，还不如用 RPM 包来的方便。不过配置的内容其实差不多，区别可能就是，RPM 包方式，可以直接用 systemctl 的命令查看状态、重启等。

## 参考

- [程序羊-CentOS7上ElasticSearch安装填坑记](https://www.jianshu.com/p/04f4d7b4a1d3) FAQ 有帮助
- [极客时间-Elasticsearch核心技术与实战](https://time.geekbang.org/course/detail/197-102661) 这篇文章阐述了 ES 集群的主节点的仲裁等知识
- [搭建ELFK日志采集系统](https://jeremy-xu.oschina.io/2018/10/%E6%90%AD%E5%BB%BAelfk%E6%97%A5%E5%BF%97%E9%87%87%E9%9B%86%E7%B3%BB%E7%BB%9F/)
- [静觅—Ubuntu 搭建 Elasticsearch 6 集群流程](https://cuiqingcai.com/6255.html)
- [阮一峰-全文搜索引擎 Elasticsearch 入门教程](https://www.ruanyifeng.com/blog/2017/08/elasticsearch.html)
- [ELK 架构之 Elasticsearch 和 Kibana 安装配置](https://www.cnblogs.com/xishuai/p/elk-elasticsearch-kibana.html)
- [使用 ELK(Elasticsearch + Logstash + Kibana) 搭建日志集中分析平台实践](https://wsgzao.github.io/post/elk/)
- [手把手教你，在CentOS上安装ELK，进行服务器日志收集](http://www.justdojava.com/2019/08/11/elk-install/)

Filebeat
- [博客园-开始使用Filebeat](https://www.cnblogs.com/cjsblog/p/9445792.html)

SpringBoot+ES
- [【ES】Java High Level REST Client 使用示例（增加修改）](https://my.oschina.net/zhaixingzu/blog/3010467)
- [Elasticsearch Java Rest Client 上手指南（上）](https://www.jianshu.com/p/c1f2161a5d22)
- [博客园——springboot elasticsearch 集成注意事项](https://www.cnblogs.com/guozp/archive/2018/04/02/8686904.html)
- [lasticsearch系列七：ES Java客户端-Elasticsearch Java client（ES Client 简介、Java REST Client、Java Client、Spring Data Elasticsearch）](https://www.cnblogs.com/leeSmall/p/9218779.html)

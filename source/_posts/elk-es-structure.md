---
title: Elasticsearch 中的倒排索引
date: 2021-02-16 23:10:57
categories: ELK
tags:
    - ELK
    - Search
keywords:
  - Elasticsearch
  - Kibana
---

![Search](https://gitee.com/michael_xiang/images/raw/master/uPic/pexels-andrea-piacquadio-3769697.jpg)

## 倒排索引

- 正排索引指的是文档 ID 和文档内容的关联,索引号对应索引稳定的内容，比如：书的第一页有啥内容?第二页有啥内容?
- 倒排索引指的是**单词到文档 ID**的对应关系

![倒排索引](https://gitee.com/michael_xiang/images/raw/master/uPic/BTZrxe.png)

## 倒排索引的核心组成

倒排索引包含两个部分：
- 单词词典（Term Dictionary），记录所有文档的单词，记录单词到倒排列表的关联关系
  - 单词词典一般比较大，可以通过 B+ 树或哈希拉链法实现，以满足高性能的插入与查询
- 倒排列表（Posting List），记录了单词对应的文档结合，由倒排索引项组成
  - 倒排索引项（Posting）
    - 文档 ID
    - 词频 TF：该单词在文档中出现的次数，用于相关性评分
    - 位置（Position）：单词在文档中分词的位置。用于语句搜索（pharse query）
    - 偏移（Offset）：记录单词的开始结束位置，实现高亮显示

一个示例，针对 Elasticsearch 倒排索引：

![Posting List](https://gitee.com/michael_xiang/images/raw/master/uPic/rH8GAg.png)

## Elasticsearch 的倒排索引

- Elasticsearch 的 JSON 文档中的每个字段，都有自己的倒排索引
- 可以指定对某些字段不做索引
  - 优点：节省存储空间
  - 缺点：字段无法被搜索

## 参考

- [简书/Elasticsearch是如何做到快速索引的](https://www.jianshu.com/p/ed7e1ebb2fb7)
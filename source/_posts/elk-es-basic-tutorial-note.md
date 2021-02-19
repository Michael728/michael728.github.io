---
title: Elasticsearch 学习笔记
date: 2021-02-14 23:10:57
categories: ELK
tags:
    - ELK
    - Search
keywords:
  - Elasticsearch
  - ES
---

![Search](https://gitee.com/michael_xiang/images/raw/master/uPic/pexels-andrea-piacquadio-3769697.jpg)

## 基本概念

### 文档 Document

ES 是面向文档的，文档是所有课搜索数据的最小单位。例如：
- 日志文件中的日志项
- 一本电影的具体信息

文档会被序列化为 JSON 格式，保存在 ES 中
- JSON 对象由字段组成
- 每个字段都有对应的字段类型

每个文档都有一个 Unique ID
- 可以自定义 ID
- 或者 ES 自动生成

#### 文档的元数据

元数据，用于标准文档的相关信息
- `_id`：一篇文档的唯一 ID
- `_index`：文档所属的索引名
- `_source`：文档的原始 JSON 数据
- `_version`：文档版本信息
- `_score`：相关性打分
- `_all`：整合所有字段内容到该字段，已被废除

### 索引

Index 索引是文档的容器，是一类「文档」的集合
- Index：体现了逻辑空间的概念：每个索引都有自己的 Mapping 定义，用于定义包含的文档的字段名和字段类型
- Shard：体现了物理空间的概念：索引中的数据分散在 Shard 上

索引的 Mapping 与 Settings：
- Mapping 定义文档字段的类型
- Setting 定义不同的数据分布

#### 索引的不同语义

索引这个词在不同的上下文中是有不同的含义的。

> 索引（动词）文档到 ES 的索引（名词）中。

- 名词：ES 集群中可以创建很多个不同的索引
- 动词：保存文档到 ES 的过程也叫索引（indexing），ES 中创建一个倒排索引的过程

### Type

- 在 ES7 之前，一个 Index 可以设置成多个 Types
- 在 ES7 开始，一个 Index 仅可以创建一个 Type `_doc`

### 关系型数据库与 ES 的比较

类比：

| RDMS   | ES       |
| ------ | -------- |
| Table  | Index    |
| Row    | Document |
| Column | Field    |
| Schema | Mapping  |
| SQL    | DSL      |

传统关系型数据库与 ES 的区别：
- ES：相关性/高性能全文检索
- RDMS：事务性/Join

### 分片（Primary Shard & Replica Shard）

- 主分片，用以解决数据水平扩展的问题。通过主分片，可以将数据分不到集群内的所有节点之上
  - 一个分片就是一个运行的 Lucene 的实例
  - 主分片数在索引创建时指定，**后续不允许修改**，除非 Reindex
- 副本，用以解决数据高可用的问题。副本分片是主分片的拷贝。当主分片丢失，集群会选择对应的副本分片称为主分片
  - 副本分片数是可以动态调整的
  - 增加副本数，还可以在一定程度上提高服务的可用性（读取的吞吐 ）。
  
增删改属于写操作，增加副本只可能降低写入速度，但是会提高数据安全性。从写的角度看，会把请求分发到不同的副本，只要这些副本在不同的机器，机器资源又足够，那就实现了水平的扩展，提高了读取的并发性。

补充：
- 一个 ES node 对应一个 ES 实例
- 一个 ES node 可以有多个 index
- 一个 index 可以有多个 shard
- 一个 shard 是一个 Lucene index（这里的 index 是 Lucene 自己概念，和 ES 中的 index 不是一个概念）

#### 分片的设定

- 对于生产环境中分片的设定，需要提前做好容量规划
  - 分片数设置过小
    - 导致后续无法实现增加节点实现水平扩展
    - 单个分片的数据量太大，导致数据重新分配耗时
  - 分片数设置过大，7.0 开始，默认主分片数设置成1，解决了 over-sharding 的问题
    - 影响搜索结果的相关性打分，影响统计结果的准确性（因为 idf 是基于分片上的数据进行计算的，并不是基于所有分片计算，所以数据量少，容易出现不准的情况）
    - 单个节点上过多的分片，会导致资源浪费，同时也会影响性能

从数据量、写入速度、是写为主还是查询为主等等，考虑分片的设置：
- 磁盘推荐 SSD
- JVM 最大 Xmx 不要超过 30G
- 副本分片至少设置为 1
- 主分片单个存储不要超过 30GB，按照这个推算出分片数


查看集群的健康状况：`GET _cluster/health`
- Green：主分片与副本都正常分配
- Yellow：主分片全部正常分配，有副本分片未能正常分配
- Red：有主分片未能分配
  - 例如，当服务器磁盘容量超过 85%时，去创建了一个新的索引
### 参考
- [极客时间-](https://time.geekbang.org/course/detail/100030501-102667)

## 倒排索引

- 正排索引指的是文档 ID 和文档内容的关联,索引号对应索引稳定的内容，比如：书的第一页有啥内容?第二页有啥内容?
- 倒排索引指的是**单词到文档 ID**的对应关系

![倒排索引](https://gitee.com/michael_xiang/images/raw/master/uPic/BTZrxe.png)

### 倒排索引的核心组成

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

### Elasticsearch 的倒排索引

- Elasticsearch 的 JSON 文档中的每个字段，都有自己的倒排索引
- 可以指定对某些字段不做索引
  - 优点：节省存储空间
  - 缺点：字段无法被搜索

### 参考

- [简书/Elasticsearch是如何做到快速索引的](https://www.jianshu.com/p/ed7e1ebb2fb7)
## Analysis 与 Analyzer


- Analysis：文本分析是把全文本转换一系列单词（term/token）的过程，也叫分词
- Analysis 是通过 Analyzer 来实现的
  - 可以使用 Elasticsearch 内置的分析器或者按需定制化分析器
- 除了在数据写入时转换词条，匹配 Query 语句时也需要使用相同的分析器对查询语句进行分析

### Analyzer 的组成

分词器是专门处理分词的组件，Analyzer 由三部分组成：
  - Character Filters 针对原始文本处理，例如去除 HTML 标签
  - Tokenier 按照规则切分为单词
  - Token Filter 将切分的单词进行 二次加工，小写，删除 stopwords，增加同义词

![demo](https://gitee.com/michael_xiang/images/raw/master/uPic/rjTlvk.png)

### ES 的内置分词器

Elasticsearch 内置了挺多[分词器](https://www.elastic.co/guide/en/elasticsearch/reference/7.x/analysis-analyzers.html)的：
- Standard Analyzer：默认分词器，按词切分，小写处理
- Simple Analyzer：按照非字母切分（符号被过滤），小写处理
- Stop Anlyzer：小写处理，停用词过滤（the,a,is）
- Whitespace Analyzer：按照空格切分，不转小写
- Keyword Analyzer：不分词，直接将输入当做输出，★★★
- Pattern Analyzer：正则表达式，默认 \W+（非字符）
- Language：提供 30 多种常见语言的分词器
- Customer Analyzer：自定义分词器

#### Standar Analyzer
默认分词器，按词切分，小写处理

![standard](https://gitee.com/michael_xiang/images/raw/master/uPic/qxeLOF.png)

停用词，例如 `in` 这些，也没有去除

```
GET _analyze
{
  "analyzer": "standard",
  "text": ["2 running Quick brown-foxes dogs in the summer"]
}
```

返回体：
```
{
  "tokens" : [
    {
      "token" : "2",
      "start_offset" : 0,
      "end_offset" : 1,
      "type" : "<NUM>",
      "position" : 0
    },
    {
      "token" : "running",
      "start_offset" : 2,
      "end_offset" : 9,
      "type" : "<ALPHANUM>",
      "position" : 1
    },
    {
      "token" : "quick",
      "start_offset" : 10,
      "end_offset" : 15,
      "type" : "<ALPHANUM>",
      "position" : 2
    },
    {
      "token" : "brown",
      "start_offset" : 16,
      "end_offset" : 21,
      "type" : "<ALPHANUM>",
      "position" : 3
    },
    {
      "token" : "foxes",
      "start_offset" : 22,
      "end_offset" : 27,
      "type" : "<ALPHANUM>",
      "position" : 4
    },
    {
      "token" : "dogs",
      "start_offset" : 28,
      "end_offset" : 32,
      "type" : "<ALPHANUM>",
      "position" : 5
    },
    {
      "token" : "in",
      "start_offset" : 33,
      "end_offset" : 35,
      "type" : "<ALPHANUM>",
      "position" : 6
    },
    {
      "token" : "the",
      "start_offset" : 36,
      "end_offset" : 39,
      "type" : "<ALPHANUM>",
      "position" : 7
    },
    {
      "token" : "summer",
      "start_offset" : 40,
      "end_offset" : 46,
      "type" : "<ALPHANUM>",
      "position" : 8
    }
  ]
}
```

#### Simple Analyzer
- 按照非字母切分（例如 空格啊、中划线 - 这样的），非字母的都被去除
- 小写处理

![Simple](https://gitee.com/michael_xiang/images/raw/master/uPic/3K3q1d.png)

#### Stop Analyzer

相比 Simpler Analyzer 多了 stop filter，会把 the/a/is 等词去除

![Stop](https://gitee.com/michael_xiang/images/raw/master/uPic/EF8Fcj.png)

#### Keyword Analyzer

不分词，直接将输入当一个 term 输出

![Kerword](https://gitee.com/michael_xiang/images/raw/master/uPic/DFgKBg.png)

```
GET _analyze
{
  "analyzer": "keyword",
  "text": ["2 running Quick brown-foxes dogs in the summer"]
}
```

返回体：
```
{
  "tokens" : [
    {
      "token" : "2 running Quick brown-foxes dogs in the summer",
      "start_offset" : 0,
      "end_offset" : 46,
      "type" : "word",
      "position" : 0
    }
  ]
}
```

### _analyzer API

#### 直接指定 Analyzer 进行测试

```
GET _analyze
{
  "analyzer": "standard",
  "text": "Mastering Elasticsearch, elasticsearch in Action"
}
```

返回体：
```
{
  "tokens" : [
    {
      "token" : "mastering",
      "start_offset" : 0,
      "end_offset" : 9,
      "type" : "<ALPHANUM>",
      "position" : 0
    },
    {
      "token" : "elasticsearch",
      "start_offset" : 10,
      "end_offset" : 23,
      "type" : "<ALPHANUM>",
      "position" : 1
    },
    {
      "token" : "elasticsearch",
      "start_offset" : 25,
      "end_offset" : 38,
      "type" : "<ALPHANUM>",
      "position" : 2
    },
    {
      "token" : "in",
      "start_offset" : 39,
      "end_offset" : 41,
      "type" : "<ALPHANUM>",
      "position" : 3
    },
    {
      "token" : "action",
      "start_offset" : 42,
      "end_offset" : 48,
      "type" : "<ALPHANUM>",
      "position" : 4
    }
  ]
}
```

#### 指定索引的字段进行测试

```
POST my_index/_analyze
{
  "field":"comment",
  "text":"Mastering Elasticsearch, elasticsearch in Action"
}
```

返回体：
```
{
  "tokens" : [
    {
      "token" : "mastering",
      "start_offset" : 0,
      "end_offset" : 9,
      "type" : "<ALPHANUM>",
      "position" : 0
    },
    {
      "token" : "elasticsearch",
      "start_offset" : 10,
      "end_offset" : 23,
      "type" : "<ALPHANUM>",
      "position" : 1
    },
    {
      "token" : "elasticsearch",
      "start_offset" : 25,
      "end_offset" : 38,
      "type" : "<ALPHANUM>",
      "position" : 2
    },
    {
      "token" : "in",
      "start_offset" : 39,
      "end_offset" : 41,
      "type" : "<ALPHANUM>",
      "position" : 3
    },
    {
      "token" : "action",
      "start_offset" : 42,
      "end_offset" : 48,
      "type" : "<ALPHANUM>",
      "position" : 4
    }
  ]
}

```

#### 自定义分词器进行测试

```
POST /_analyze
{
  "tokenizer": "standard",
  "filter":["lowercase"],
  "text": "Mastering Elasticsearch, elasticsearch in Action"
}
```

返回体：
```
{
  "tokens" : [
    {
      "token" : "mastering",
      "start_offset" : 0,
      "end_offset" : 9,
      "type" : "<ALPHANUM>",
      "position" : 0
    },
    {
      "token" : "elasticsearch",
      "start_offset" : 10,
      "end_offset" : 23,
      "type" : "<ALPHANUM>",
      "position" : 1
    },
    {
      "token" : "elasticsearch",
      "start_offset" : 25,
      "end_offset" : 38,
      "type" : "<ALPHANUM>",
      "position" : 2
    },
    {
      "token" : "in",
      "start_offset" : 39,
      "end_offset" : 41,
      "type" : "<ALPHANUM>",
      "position" : 3
    },
    {
      "token" : "action",
      "start_offset" : 42,
      "end_offset" : 48,
      "type" : "<ALPHANUM>",
      "position" : 4
    }
  ]
}
```

### 中文分词器

中文分词的难点：
- 中文句子，需要切分成一个一个词（不是一个一个字）。英文中，单词有自然的空格作为分隔，而中文却没有。
- 一句中文，在不同的上下文，有不同的理解
  - 这个苹果，不大好吃/这个苹果，不大，好吃
  - 他说的确实在理/这事的确定不下来

#### ICU Analyzer

![icu](https://gitee.com/michael_xiang/images/raw/master/uPic/iYkbh5.png)

```
POST _analyze
{
  "analyzer": "icu_analyzer",
  "text": "他说的的确在理"
}
```

返回体：
```
{
  "tokens" : [
    {
      "token" : "他",
      "start_offset" : 0,
      "end_offset" : 1,
      "type" : "<IDEOGRAPHIC>",
      "position" : 0
    },
    {
      "token" : "说的",
      "start_offset" : 1,
      "end_offset" : 3,
      "type" : "<IDEOGRAPHIC>",
      "position" : 1
    },
    {
      "token" : "的确",
      "start_offset" : 3,
      "end_offset" : 5,
      "type" : "<IDEOGRAPHIC>",
      "position" : 2
    },
    {
      "token" : "在",
      "start_offset" : 5,
      "end_offset" : 6,
      "type" : "<IDEOGRAPHIC>",
      "position" : 3
    },
    {
      "token" : "理",
      "start_offset" : 6,
      "end_offset" : 7,
      "type" : "<IDEOGRAPHIC>",
      "position" : 4
    }
  ]
}
```

如果是默认的分词器呢？

```
GET _analyze
{
  "analyzer": "standard",
  "text": "他说的的确在理"
}
```

返回体：
```
{
  "tokens" : [
    {
      "token" : "他",
      "start_offset" : 0,
      "end_offset" : 1,
      "type" : "<IDEOGRAPHIC>",
      "position" : 0
    },
    {
      "token" : "说",
      "start_offset" : 1,
      "end_offset" : 2,
      "type" : "<IDEOGRAPHIC>",
      "position" : 1
    },
    {
      "token" : "的",
      "start_offset" : 2,
      "end_offset" : 3,
      "type" : "<IDEOGRAPHIC>",
      "position" : 2
    },
    {
      "token" : "的",
      "start_offset" : 3,
      "end_offset" : 4,
      "type" : "<IDEOGRAPHIC>",
      "position" : 3
    },
    {
      "token" : "确",
      "start_offset" : 4,
      "end_offset" : 5,
      "type" : "<IDEOGRAPHIC>",
      "position" : 4
    },
    {
      "token" : "在",
      "start_offset" : 5,
      "end_offset" : 6,
      "type" : "<IDEOGRAPHIC>",
      "position" : 5
    },
    {
      "token" : "理",
      "start_offset" : 6,
      "end_offset" : 7,
      "type" : "<IDEOGRAPHIC>",
      "position" : 6
    }
  ]
}
```

#### IK

[IK Analysis for Elasticsearch](https://github.com/medcl/elasticsearch-analysis-ik) 支持自定义词库，支持热更新分词词典

安装插键：
```
bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.3.2/elasticsearch-analysis-ik-7.3.2.zip
```

测试：
```
POST _analyze
{
  "analyzer": "ik_smart",
  "text": "他说的的确在理"
}
```

返回体：
```
{
  "tokens" : [
    {
      "token" : "他",
      "start_offset" : 0,
      "end_offset" : 1,
      "type" : "CN_CHAR",
      "position" : 0
    },
    {
      "token" : "说",
      "start_offset" : 1,
      "end_offset" : 2,
      "type" : "CN_CHAR",
      "position" : 1
    },
    {
      "token" : "的",
      "start_offset" : 2,
      "end_offset" : 3,
      "type" : "CN_CHAR",
      "position" : 2
    },
    {
      "token" : "的确",
      "start_offset" : 3,
      "end_offset" : 5,
      "type" : "CN_WORD",
      "position" : 3
    },
    {
      "token" : "在理",
      "start_offset" : 5,
      "end_offset" : 7,
      "type" : "CN_WORD",
      "position" : 4
    }
  ]
}
```

`ik_max_word` 与 `ik_smart` 的区别：
- ik_max_word: 会将文本做最细粒度的拆分，比如会将“中华人民共和国国歌”拆分为“中华人民共和国,中华人民,中华,华人,人民共和国,人民,人,民,共和国,共和,和,国国,国歌”，会穷尽各种可能的组合，适合 Term Query；
- ik_smart: 会做最粗粒度的拆分，比如会将“中华人民共和国国歌”拆分为“中华人民共和国,国歌”，适合 Phrase Query。

Term Query 与 Phrase Query 的区别：
Term Query，表示查询时，将查询的词分词之后，他们之间关系是 OR 的关系，例如请求为`GET /movies/_search?q=title:(Beautiful Mind)`，意思就是查询 title 中包括 Beautiful 或者 Mind
Phrase Query 表示查询时他们是一个整体短语，需要用引号包起来，例如请求为 `GET /movies/_search?q=title:"Beautiful Mind"`

综上，创建索引的时候可以使用 ik_max_word，查询的时候使用 ik_smart，例如：

```
# 创建索引的同时指定 mapping
PUT /ik_index
{
  "mappings": {
        "properties": {
            # 字段
            "content": {
                "type": "text",
                "analyzer": "ik_max_word",
                "search_analyzer": "ik_smart"
            }
        }
    }
}
```

#### 其他

- [THULAC](https://github.com/microbun/elasticsearch-thulac-plugin) 清华大学的一套中文分词器，[官网主页](http://thulac.thunlp.org/)
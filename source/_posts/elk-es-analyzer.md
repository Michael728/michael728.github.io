---
title: 初识 Elasticsearch 中的分词器
date: 2021-02-16 12:10:57
categories: ELK
tags:
    - ELK
    - Search
keywords:
  - Elasticsearch
  - Kibana
---

![Search](https://gitee.com/michael_xiang/images/raw/master/uPic/pexels-andrea-piacquadio-3769697.jpg)

## Analysis 与 Analyzer


- Analysis：文本分析是把全文本转换一系列单词（term/token）的过程，也叫分词
- Analysis 是通过 Analyzer 来实现的
  - 可以使用 Elasticsearch 内置的分析器或者按需定制化分析器
- 除了在数据写入时转换词条，匹配 Query 语句时也需要使用相同的分析器对查询语句进行分析

## Analyzer 的组成

分词器是专门处理分词的组件，Analyzer 由三部分组成：
  - Character Filters 针对原始文本处理，例如去除 HTML 标签
  - Tokenier 按照规则切分为单词
  - Token Filter 将切分的单词进行 二次加工，小写，删除 stopwords，增加同义词

![demo](https://gitee.com/michael_xiang/images/raw/master/uPic/rjTlvk.png)

## ES 的内置分词器

Elasticsearch 内置了挺多[分词器](https://www.elastic.co/guide/en/elasticsearch/reference/7.x/analysis-analyzers.html)的：
- Standard Analyzer：默认分词器，按词切分，小写处理
- Simple Analyzer：按照非字母切分（符号被过滤），小写处理
- Stop Anlyzer：小写处理，停用词过滤（the,a,is）
- Whitespace Analyzer：按照空格切分，不转小写
- Keyword Analyzer：不分词，直接将输入当做输出，★★★
- Pattern Analyzer：正则表达式，默认 \W+（非字符）
- Language：提供 30 多种常见语言的分词器
- Customer Analyzer：自定义分词器

### Standar Analyzer
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

### Simple Analyzer
- 按照非字母切分（例如 空格啊、中划线 - 这样的），非字母的都被去除
- 小写处理

![Simple](https://gitee.com/michael_xiang/images/raw/master/uPic/3K3q1d.png)

### Stop Analyzer

相比 Simpler Analyzer 多了 stop filter，会把 the/a/is 等词去除

![Stop](https://gitee.com/michael_xiang/images/raw/master/uPic/EF8Fcj.png)

### Keyword Analyzer

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

## _analyzer API

### 直接指定 Analyzer 进行测试

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

### 指定索引的字段进行测试

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

### 自定义分词器进行测试

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

## 中文分词器

中文分词的难点：
- 中文句子，需要切分成一个一个词（不是一个一个字）。英文中，单词有自然的空格作为分隔，而中文却没有。
- 一句中文，在不同的上下文，有不同的理解
  - 这个苹果，不大好吃/这个苹果，不大，好吃
  - 他说的确实在理/这事的确定不下来

### ICU Analyzer

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

### IK

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
- ik_smart: 会做最粗粒度的拆分，比如会将“中华人民共和国国歌”拆分为“中华人民共和国,国歌”，适合 Phrase 查询。

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


### 其他

- [THULAC](https://github.com/microbun/elasticsearch-thulac-plugin) 清华大学的一套中文分词器，[官网主页](http://thulac.thunlp.org/)
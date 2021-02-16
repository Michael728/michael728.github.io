---
title: Elasticsearch常见API
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

## 文档 CRUD

| Index  | API 示例                                                     |
| ------ | ------------------------------------------------------------ |
| Index  | PUT my_index/_doc/1 <br/>{"user":"mike","comment":"You know,for search"} |
| Create | PUT my_index/_create/1<br/>{"user":"mike","comment":"You know,for search"}<br/>POST my_index/_doc （不指定 ID，则自动生成 ID）<br/>{"user":"mike","comment":"You know,for search"} |
| Read   | GET my_index/_doc/1                                          |
| Update | POST my_index/_update/1<br/>{<br/>  "doc": {<br/>    "user":"mike",<br/>    "comment": "You know, Elasticsearch"<br/>  }<br/>} |
| Delete | DELETE my_index/_doc/1_                                      |

> 示例中，my_index 是新建的索引名

- Type 名，约定都用 `_doc`
- Create：创建新文档，如果 ID 已经存在，则会失败
- Index：如果 ID 不存在，则新建文档；如果 ID 存在，则会先删除已有的文档，在创建新的文档，版本会增加
- Update：文档必须已经存在，更新只会对相应字段做增量修改

### Index 文档

Index 和 Create 不一样的地方：
- index：如果文档不存在，就索引新的文档。否则现有文档会被删除，新的文档被索引。版本信息 +1
- update：update 方法不会删除原来的文档，而是实现真正的数据更新，POST 方法请求体需要包含在 `doc` 中

```
POST my_index/_update/1
{
  "doc": {
    "user":"mike",
    "comment": "You know, Elasticsearch"
  }
}
```

### Get 文档

```
GET my_index/_doc/1
{
  "_index" : "my_index",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 6,
  "_seq_no" : 6,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "user" : "mike",
    "comment" : "You know, Elasticsearch"
  }
}
```

- 找到文档，返回 HTTP 200
  - 文档元信息
    - _index/_type
    - 版本信息，同一个 ID 的文档，及时被删除，Version 号也会不断增加
    - _source 中默认包含了文档的所有原始信息
- 找不到文档，返回 HTTP 404

## Bulk API

- 支持在一次 API 调用中，对不同的索引进行操作
- 支持四种类型操作
  - Index
  - Create
  - Update
  - Delete
- 可以在 URI 中指定 Index，也可以在请求体中进行
- 操作中单条操作失败，并不影响其他操作
- 返回结果包含每一条操作执行的结果

```
POST  _bulk
{"index": {"_index":"test","_id":"1"}}
{"field1": "value1"}
{"delete": {"_index": "test","_id":"2"}}
{"create": {"_index":"test2","_id":"3"}}
{"field1":"value3"}
{"update":{"_index":"test","_id":1}}
{"doc":{"field2":"value2"}}
```

说明：
- index 和 create 操作，需要在紧接着的下一行提供 source，以便添加文档或者更新
- delete 不需要 source
- update 需要在下一行指定 doc，指明如何更新

返回体：
```
{
  "took" : 350,
  "errors" : false,
  "items" : [
    {
      "index" : {
        "_index" : "test",
        "_type" : "_doc",
        "_id" : "1",
        "_version" : 1,
        "result" : "created",
        "_shards" : {
          "total" : 2,
          "successful" : 2,
          "failed" : 0
        },
        "_seq_no" : 0,
        "_primary_term" : 1,
        "status" : 201
      }
    },
    {
      "delete" : {
        "_index" : "test",
        "_type" : "_doc",
        "_id" : "2",
        "_version" : 1,
        "result" : "not_found",
        "_shards" : {
          "total" : 2,
          "successful" : 2,
          "failed" : 0
        },
        "_seq_no" : 1,
        "_primary_term" : 1,
        "status" : 404
      }
    },
    {
      "create" : {
        "_index" : "test2",
        "_type" : "_doc",
        "_id" : "3",
        "_version" : 1,
        "result" : "created",
        "_shards" : {
          "total" : 2,
          "successful" : 2,
          "failed" : 0
        },
        "_seq_no" : 0,
        "_primary_term" : 1,
        "status" : 201
      }
    },
    {
      "update" : {
        "_index" : "test",
        "_type" : "_doc",
        "_id" : "1",
        "_version" : 2,
        "result" : "updated",
        "_shards" : {
          "total" : 2,
          "successful" : 2,
          "failed" : 0
        },
        "_seq_no" : 2,
        "_primary_term" : 1,
        "status" : 200
      }
    }
  ]
}
```

## 批量读取 mget

批量操作，可以减少网络连接所产生的开销，提高性能。`mget` 是通过文档 ID 来查询文档信息。

```
GET _mget
{
  "docs": [
    {
      "_index": "my_index",
      "_id": 1
    },
    {
      "_index": "test",
      "_id": 1
    }
    ]
}
```

返回体：
```
{
  "docs" : [
    {
      "_index" : "my_index",
      "_type" : "_doc",
      "_id" : "1",
      "_version" : 10,
      "_seq_no" : 10,
      "_primary_term" : 1,
      "found" : true,
      "_source" : {
        "user" : "mike2",
        "comment" : "You know, Elasticsearch",
        "age" : 28
      }
    },
    {
      "_index" : "test",
      "_type" : "_doc",
      "_id" : "1",
      "_version" : 2,
      "_seq_no" : 2,
      "_primary_term" : 1,
      "found" : true,
      "_source" : {
        "field1" : "value1",
        "field2" : "value2"
      }
    }
  ]
}
```

## 批量查询 msearch

`msearch` 是根据查询条件、搜索得到相应文档。

```
POST my_index/_msearch
{}
{"query": {"match_all": {}}, "from": 0, "size": 10}
{"index":"test"}
{"query":{"match_all":{}}}
```

- 上面的查询涉及了 2 个索引，一个是 my_index 索引，还有一个是 test 索引

## 常见错误返回

| 问题         | 原因               |
| ------------ | ------------------ |
| 无法连接     | 网络故障或集群挂了 |
| 连接无法关闭 | 网络故障或节点出错 |
| 429          | 集群过于繁忙       |
| 4xx          | 请求体格式有错     |
| 500          | 集群内部错误       |


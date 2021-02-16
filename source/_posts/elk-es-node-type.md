---
title: Elasticsearch集群中节点角色的介绍
date: 2020-09-20 15:10:57
categories: ELK
tags:
    - ELK
    - ES
keywords:
  - Elasticsearch
---

## Node

**默认情况**，一个节点 [Node](https://www.elastic.co/guide/en/elasticsearch/reference/7.3/modules-node.html#voting-only-node) 包含下面所有的角色：`master-eligible, data, ingest, and machine learning (if available)`。

| 节点类型          | 配置参数    | 默认值                       |
| ----------------- | ----------- | ---------------------------- |
| master eligible   | node.master | true                         |
| data              | node.data   | true                         |
| ingest            | node.ingest | true                         |
| coodrinating only | 无          | 设置上面三个参数全部为 false |
| machine learning  | node.ml     | true (需要 enable x-pack)    |

{% note info %}
随着群集的增长，尤其是如果您有大量机器学习工作，需要考虑将具有主机资格（`master-eligible`）的节点专用，将它与专用数据的节点（`data nodes`）和专用机器学习的节点（`machine learning nodes`）分开。

我理解，这里的专用（`dedicated`）一词就是表示，将一个节点的角色不要设置那么多，主节点资格的节点就是用于当主节点，而不是同时设置数据节点的角色！
{% endnote%}

- 开发环境中，一个节点可以承担多种角色
- 生产环境中，应该设置单一的角色的节点（dedicated node）

本文 ES 是 7.3.0 版本，查看官方文档，发现 ES 7.9.0 的配置和它是有点区别的。

## master-eligible node

`master-eligible node`：默认值就是 `true`，意味着每个节点启动后，默认就是一个 Master eligible 节点。具有该角色的节点表示它有资格被选举为 `mater node`。备注：`eligible` 一词表示符合条件的含义。

主节点负责维护集群的状态，集群范围内的轻量级操作（只有 Master 节点才能修改集群的状态信息），例如：
- 创建或删除索引，
- 跟踪哪些节点是集群的一部分
- 确定将哪些分片分配给哪些节点
- 分片的路由信息

> 任意节点都能修改信息，会导致数据的不一致性

**拥有稳定的主节点对于群集健康非常重要。**

在[主节点选举过程中](https://www.elastic.co/guide/en/elasticsearch/reference/7.3/modules-discovery.html)，任何一个非 `voting-only` 的 `master-eligible` 节点都可能被选为主节点。

> 很好理解，如果已经是「仅是选举」用途的节点，它怎么能称为主节点呢？

索引和搜索数据是占用大量CPU，内存和 I/O 的工作，这可能给节点的资源带来压力。为了确保主节点稳定且不受压力，在较大的群集中，最好将符合角色的专用主节点和专用数据节点分开。

尽管主节点还可以充当协调节点（`coordinating nodes`），将搜索和索引请求从客户端路由到数据节点，但最好不要为此目的使用专用的主节点。对于符合主节点要求的节点，其工作量应尽可能少，这对于群集的稳定性很重要。

{% note warning %}
概括来讲就是，主节点的机器不要身兼数职，不要给主节点配置其他节点角色，以免影响主节点的稳定性！
{% endnote%}

```yaml
# 可选为主节点（默认是 true）
node.master: true
# voting_only 角色不启用（默认也是 false） 
node.voting_only: false
# data 角色不启用 （默认是 true） 
node.data: false 
# inges 角色不启用（默认是 true）
node.ingest: false 
# ml 角色不启用（默认是 true）
node.ml: false 
# xpack.ml.enabled 默认是启用的
xpack.ml.enabled: true 
# 默认是 true
cluster.remote.connect: false 
```

> xpack 功能时付费功能，因此，一般情况下，可以忽略 xpack 的设置。

一个集群中可以有多个 master 节点，active 的 master 节点只有一个！

## Voting-only master-eligible node

一个具有 `voting-only` `master-eligible` 两种角色的节点，它是参与主节点的选举但不会充当主节点的节点。特别指出，`voting-only` 可以在选举中为决胜局服务。

要配置一个具有主机资格的节点为 `voting-only` 节点，需要如下配置

```yaml
# 默认是 false
node.voting_only: true 
```

{% note warning %}
注意：OSS-only 分布式版本不支持 voting-only 角色。**只有 master-eligible 的节点才能标记为 voting-only 角色！这里虽然有点难理解，但是由于历史原因造成的。**
{% endnote%}

**一个高可用的集群（HA —— High Avaliablity clusters）至少需要 3 个 `master-eligible` 角色的节点、至少 2 个不是 `voting-only` 节点。这样的集群将能够在一个主节点故障时选出一个主节点。**

一个具有 `voting-only` `master-eligible` 两种角色的节点同样也可以赋予其他的角色。例如，一个节点可以同时赋予 `voting-only` `master-eligible` `data` 3 种角色。专用于 `voting-only` `master-eligible` 的节点配置如下：

```yaml
node.master: true
# 默认不启用，需要设置为 false 
node.voting_only: true 
node.data: false 
node.ingest: false 
node.ml: false 
xpack.ml.enabled: true 
cluster.remote.connect: false 
```

## Data Node

数据节点包含您已建立索引的文档的分片。数据节点处理与数据相关的操作，例如：
- CRUD，
- 搜索和聚合。

> 数据节点在数据扩展上起到了至关重要的作用

这些操作是 I/O，内存和 CPU 密集型的。**监视这些资源并在过载时添加更多数据节点非常重要。**

具有专用数据节点的主要好处是将主节点角色和数据节点角色分开。 参考如下配置：
```yaml
# 默认是 true
node.master: false 
# 默认是 false
node.voting_only: false 
# 默认是 true
node.data: true 
# 默认是 true
node.ingest: false 
# 默认是 true
node.ml: false 
# 默认是 true
cluster.remote.connect: false 
```

## Ingest Node

接收节点（`Ingest nodes`）可以执行由一个或多个接收处理器组成的预处理管道。根据摄取处理器执行的操作类型和所需的资源，拥有专用的摄取节点可能会很有意义，这些节点仅会执行此特定任务。

> Ingest 节点也称为预处理节点。

```yaml
node.master: false 
node.voting_only: false 
node.data: false 
node.ingest: true 
node.ml: false 
cluster.remote.connect: false
```

Ingest Node，可以用来承担一些数据管道的数据转换工作，例如，为你的文档增加一个新的字段，例如帮转换文档中的字段。

## Coordinating only nodes

如果您不具备处理主节点职责、保存数据和预处理文档的能力，那么你就剩下一个仅可路由请求的协调节点角色（`coordinating only nodes`），处理搜索缩减阶段、分配批量索引的。本质上，coordinating only nodes 可充当智能负载平衡器。

- 负责接受 Client 的请求，将请求分发到合适的节点，最终把结果汇集到一起。
- 每个节点默认都起到了 Coordinating Node 的职责

> 仅协调节点也称为负载均衡节点或 Client 节点

{% note warning %}
在集群中添加过多的仅协调节点可能会增加整个集群的负担，因为选择的主节点必须等待每个节点的集群状态更新确认！仅协调节点的好处不应被夸大 —— 数据节点可以很好地达到相同的目的。
{% endnote%}

为了创建一个仅协调节点（coordinating only node），可以如下配置：

```yaml
node.master: false 
node.voting_only: false 
node.data: false 
node.ingest: false 
node.ml: false 
cluster.remote.connect: false 
```

## 其他节点类型

- Hot & Warm Node
  - 不同硬件配置的 Data Node，用来实现 Hot & Warm 架构，降低集群部署的成本
- Machine Learning Node
  - 负责跑机器学习的 Job，用来做异常检测 

## 单一职责节点

一个节点只承担一个角色，总结图：

![单一职责](https://gitee.com/michael_xiang/images/raw/master/uPic/dt7Hon.png)

## 参考

- [官宣-Elasticsearch Reference [7.3] » Modules » Node](https://www.elastic.co/guide/en/elasticsearch/reference/7.3/modules-node.html#coordinating-node)
- [learnku-笔记五十二：集常见的集群部署方式](https://learnku.com/articles/40718)
- [Elasticsearch集群规划求助](https://elasticsearch.cn/question/8487)
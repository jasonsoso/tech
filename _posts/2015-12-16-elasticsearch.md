---
layout: post
title: 搜索引擎ElasticSearch
tags:
    - 搜索引擎
    - ElasticSearch
    - 大数据
---


## 第一章	搜索引擎 ##

#### 搜索引擎简单结构 ####
![搜索引擎简单结构](http://cdn.jasonsoso.com/2015/index.png)


#### 搜索引擎Lucene简介 ####

1. Logo    
![搜索引擎Lucene简介](http://cdn.jasonsoso.com/2015/lucene_logo_green_300.png)

2. 简介    
Lucene是一套用于全文检索和搜寻的开源程式库，由Apache软件基金会支持和提供。Lucene提供了一个简单却强大的应用程式接口，能够做全文索引和搜寻。在Java开发环境里Lucene是一个成熟的免费开源工具。


#### 搜索引擎Lucene优点  ####

1. 索引文件格式独立于应用平台。Lucene定义了一套以8位字节为基础的索引文件格式，使得兼容系统或者不同平台的应用能够共享建立的索引文件。
2. 在传统全文检索引擎的倒排索引的基础上，实现了分块索引，能够针对新的文件建立小文件索引，提升索引速度。然后通过与原有索引的合并，达到优化的目的。
3. 优秀的面向对象的系统架构，使得对于Lucene扩展的学习难度降低，方便扩充新功能。
4. 设计了独立于语言和文件格式的文本分析接口，索引器通过接受Token流完成索引文件的创立，用户扩展新的语言和文件格式，只需要实现文本分析的接口。
5. 已经默认实现了一套强大的查询引擎，用户无需自己编写代码即使系统可获得强大的查询能力，Lucene的查询实现中默认实现了布尔操作、模糊查询（Fuzzy Search[11]）、分组查询等等。



#### 搜索引擎与数据库的比较  ####

| 对比项 | 全文检索库（Lucene） | 关系型数据库（Oracle） |
| :--- | :--- | :---|
| 核心功能 | 以文本检索为主，插入（insert）、删除（delete）、修改（update）比较麻烦，适合于大文本块的查询。 | 插入（insert）、删除（delete）、修改（update）十分方便，有专门的SQL命令，但对于大文本块（如CLOB）类型的检索效率低下。 |
| 库    | 与Oracle类似，都可以建多个库，且各个库的存储位置可以不同。      | 可以建多个库，每个库一般都有控制文件和数据文件等，比较复杂。     |
| 表    | 没有严格的表的概念，比如Lucene的表只是由入库时的定义字段松散组成。      | 有严格的表结构，有主键，有字段类型等。     |
| 记录    | 由于没有严格表的概念，所以记录体现为一个对象，在Lucene里记录对应的类是Document。     | Record，与表结构对应。     |
| 字段    | 字段类型只有文本和日期两种，字段一般不支持运算，更无函数功能。在Lucene里字段的类是Field，如document（field1,field2…）     | 字段类型丰富，功能强大。record（field1,field2…）   |
| 查询结果集    | 在Lucene里表示查询结果集的类是Hits，如hits（doc1,doc2,doc3…）   | 在JDBC为例， Resultset（record1,record2,record3...）   |
| 字符串的搜索    | 非常快  | 相对慢|
| 分词   | 可以中文分词  | 不可以分词 |



## 第二章	Elasticsearch简介 ##

#### 简介 ####

1.	ElasticSearch是一个基于Lucene构建的开源，分布式，RESTful搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。支持通过HTTP使用JSON进行数据索引。
2.	它基本上所有我想要的特性都包含了，分布式搜索，分布式索引，零配置，自动分片，索引自动负载，节点自动发现，restful风格接口。
3.	扩展性非常好，插件丰富，有很多官方和第三方开发的插件，有分词，同步，数据传输，脚本支持，站点，其他等等；
4.	支持多种语言的客户端，有Perl，Python,Ruby,PHP,Java,NodeJS,.Net,Scala,Go,Erlang…



#### 国内外优秀案例 ####

1.	Github：“GitHub使用ElasticSearch搜索20TB的数据，包括13亿文件和1300亿行代码”
2.	Foursquare：“实时搜索5千万地理位置信息？Foursquare每天使用ElasticSearch做到了”
3.	SoundCloud：“SoundCloud使用ElasticSearch为1.8亿用户提供即时而精准的音乐搜索服务”
4.	Fog Creek ： “Elasticsearch使Fog Creek可以在400亿行代码中进行一个月3千万次的查询”
5.	StumbleUpon : "Elasticsearch是StumbleUpon的关键部件，它每天为社区提供百万次的推荐服务"
6.	Sony：Sony公司使用elasticsearch 作为信息搜索引擎
7.	Infochimps：“在 Infochimps，我们已经索引了25亿文档，总共占用 4TB的空间”。
Infochimps是一家位于德克萨斯州奥斯丁的创业公司，为大数据平台提供商。它主要提供基于hadoop的大数据处理方案。
8.	1号店：每天搜索过亿的商品。

![国内外优秀案例](http://cdn.jasonsoso.com/2015/es1.png)


#### Elasticsearch的一些概念 ####

1.	集群 (cluster)
在一个分布式系统里面,可以通过多个elasticsearch运行实例组成一个集群,这个集群里面有一个节点叫做主节点(master),elasticsearch是去中心化的,所以这里的主节点是动态选举出来的,不存在单点故障。
在同一个子网内，只需要在每个节点上设置相同的集群名,elasticsearch就会自动的把这些集群名相同的节点组成一个集群。节点和节点之间通讯以及节点之间的数据分配和平衡全部由elasticsearch自动管理。
在外部看来elasticsearch就是一个整体。

2.	节点(node)
每一个运行实例称为一个节点,每一个运行实例既可以在同一机器上,也可以在不同的机器上.所谓运行实例,就是一个服务器进程.在测试环境内,可以在一台服务器上运行多个服务器进程,在生产环境建议每台服务器运行一个服务器进程
node会使用广播（或指定的多播）来发现一个现有的cluster，并且试图加入该cluster。

3.	索引(index)
这里的索引是名词不是动词,在elasticsearch里面支持多个索引。类似于关系数据库里面每一个服务器可以支持多个数据库一样。在每一索引下面又支持多种类型，类似于关系数据库里面的一个数据库可以有多张表。但是本质上和关系数据库有很大的区别。这里暂时可以这么理解

4.	分片(shards)
把一个索引分解为多个小的索引，每一个小的索引叫做分片。分片后就可以把各个分片分配到不同的节点中。 Elasticsearch在集群中分布所有的shards，并且在添加删除节点时，自动重新分配。

5.	副本(replicas)
代表索引副本，es可以设置多个索引的副本，副本的作用一是提高系统的容错性，当个某个节点某个分片损坏或丢失时可以从副本中恢复。二是提高es的查询效率，es会自动对搜索请求进行负载均衡。



#### 特点优势 ####

1.	官网与社区
2.	Apache Lucene（基于 Lucene）
3.	Schema Free(模式自由)
4.	Document Oriented(面向文档型的设计)
5.	Real Time Data & Analytics（实时索引数据）
6.	Distributed（分布式）
7.	High Availability（高可靠性）
8.	其他特性：RESTful API；JSON format；multi-tenancy；full text search；conflict management；per-operation persistence


#### Elasticsearch与Solr ####

1.	realtime-search-solr-vs-elasticsearch    
参照http://blog.socialcast.com/realtime-search-solr-vs-elasticsearch/
2.	Apache Solr vs ElasticSearch    
参照http://solr-vs-elasticsearch.com/



## 第三章	ElasticSearch使用与说明 ##

#### ElasticSearch相关知识网站 ####

1.	官网与社区
http://www.elasticsearch.org/
http://www.elasticsearch.com/
2.	中国Elasticsearch官网
http://www.elasticsearch.cn/
3.	Elasticsearch相关博客
http://log.medcl.net/


#### Elasticsearch内部架构图 ####
![es内部架构图](http://cdn.jasonsoso.com/2015/es2.jpg)

#### ElasticSearch部署架构图 ####
![es部署架构图](http://cdn.jasonsoso.com/2015/e3.png)


## 第四章	ElasticSearch部署 ##

#### Linux启动 ####

    cd elasticsearch/bin/service
    ./elasticsearch console


#### window启动 ####

    cd elasticsearch/bin/service
    elasticsearch.bat


####  工具访问 ####

    http://localhost:9200/_plugin/rtf/









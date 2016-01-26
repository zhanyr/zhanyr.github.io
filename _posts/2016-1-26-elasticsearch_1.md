---
layout: post
category: elasticsearch
title: (一)Elasticsearch简介
tagline: by Zhanyr
tags: [elasticsearch, lucene]
---
Elasticsearch是一个基于Lucene的搜索服务器，提供了一个分布式多用户能力的全文搜索引擎。ES用于云计算中，能够达到实时搜索，具有稳定，可靠，快速，安装使用方便等优点。

<!--more-->

###elasticsearch介绍
Lucene是当今最先进，最高效的全功能开源搜索引擎框架,但是使用起来特别麻烦。Elasticsearch使用Lucene作为内部引擎，只需要了解统一的API就可以可以使用。
当然Elasticsearch并不仅仅是Lucene这么简单，它不但包括了全文搜索功能，还可以进行以下工作:

a.分布式实时文件存储，并将每一个字段都编入索引，使其可以被搜索。

b.实时分析的分布式搜索引擎。

c.可以扩展到上百台服务器，处理PB级别的结构化或非结构化数据。

这么多的功能被集成到一台服务器上，你可以轻松地通过客户端或者任何你喜欢的程序语言与ES的RESTful API进行交流。

###安装

#####1、安装jdk
确保安装Java SE环境，1.7以上版本
#####2、安装elasticsearch
从[elasticsearch官网](https://www.elastic.co/downloads/elasticsearch)下载，解压。即完成安装。
#####3、配置
ES解压之后，到ES对应的目录中发现如下的目录结构：

	bin:运行ES实例和插件管理需要的脚本
	config:配置文件目录
	lib:ES使用的库
	
config目录下有两个配置文件：elasticsearch.yml和logging.yml。

第一个文件主要负责服务器的默认配置值。`(一些配置值可能会在运行中更改,所以这个文件中的值可能是不准确的。)` 其中cluster.name和node.name不会在运行中被更改，分别代表集群名字和节点名字。

第二个文件定义了将多少信息写入系统日志，它定义了日志文件并定期创建新文件。

#####4、运行
bin目录下执行./elasticsearch，就将ES运行起来了。它工作时使用两个端口号，默认情况下，使用http协议与rest api通信的端口为9200，在集群内及java客户端和集群之间通信的端口为9300。

在浏览器中打开http://localhost:9200/
浏览器显示如下：
	
	{
		"status" : 200,
		"name" : "node_zhyr",
		"cluster_name" : "elasticsearch",
		"version" : {
			"number" : "1.7.3",
			"build_hash" : "05d4530971ef0ea46d0f4fa6ee64dbc8df659682",
			"build_timestamp" : "2015-10-15T09:14:17Z",
			"build_snapshot" : false,
			"lucene_version" : "4.10.4"
		},
		"tagline" : "You Know, for Search"
	}
	
运行之后，ES对应的目录下将会被自动的创建如下目录：

	data:elasticsearch使用的所有数据的存储位置
	logs:关于事件和错误记录的文件
	plugins:安装的插件
	work:elasticsearch使用的临时文件

#####5、关闭
a.如果节点是连接到控制台，ctrl+c

b.kill 命令

c.使用rest api:

关闭整个集群curl -XPOST http://localhost:9200/_cluster/nodes/_shutdown

关闭单一节点curl -XPOST http://localhost:9200/_cluster/nodes/节点标识符/_shutdown

###插件：
集群管理工具：./plugin -install mobz/elasticsearch

集群监控工具：./plugin -install lukas-vlcek/bigdesk



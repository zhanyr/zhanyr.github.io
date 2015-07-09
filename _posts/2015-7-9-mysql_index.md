---
layout: post
category: SQL
title: MySql索引结构基础
tagline: by zhanyr
tags: [Mysql]
---
索引是帮助mysql高效获取数据的数据结构。它相当于一本书的目录，我们通过目录可以知道一本的整体情况，可以检索所需要的资料。

<!--more-->

##唯一索引

唯一索引强调唯一，即索引值必须唯一。

####创建索引：

	create unique index 索引名 on 表名 (列名);
	alter table 表名 add unique index 索引名 (列名);

####删除索引：

	drop index 索引名 on 表名;
	alter table 表名 drop index 索引名；

###主键

主键是唯一索引的一种，主键要求建表时指定，一般用auto_increment列，关键字是`primary key`。

####主键创建：

	create table test2 (id int not primary key auto_increment);

##全文索引

全文索引一般在CHAR、VARCHAR或TEXT列创建。

	create table 表名(
		id int not null primary key auto_increment,
		title varchar(100),FULLTEXT(title)
	)type=MyISAM;

##单列索引与多列索引

索引可以是单列索引也可以是多列索引(复合索引)。多列索引的创建如下：

	create table 表名(
		id int not null primary key auto_increment,
		uname char(8) not null default '',
		password char(12) not null,
		INDEX(uname,password)
	)type=MyISAM;

`注：INDEX(a,b,c)可以当作a或(a,b)的索引来用，但不能当作b、c或(b,c)的索引来使用。这是一个左前缀的优化方法。`

##聚簇索引

该索引中键值的逻辑顺序决定了表中相应行的物理顺序，可以确定表中数据的物理顺序。

###查看表的索引

	show index from 表名；

####结果：

`Key_name`:什么类型的索引

`Column_name`:索引列的字段名

`Cardinality`:索引基数，平均数组值=索引基数/数据总行数，平均数组值越接近1就越有可能利用索引



	

	


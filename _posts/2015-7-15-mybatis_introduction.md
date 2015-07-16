---
layout: post
category: SQL
title: MyBatis之一、入门
tagline: by zhanyr
tags: [MyBatis, SQL]
---
MyBatis是支持定制化SQL、存储过程以及高级映射的持久层框架。它避免了JDBC中手动设置参数以及获取结果集的缺点。可以对配置和原生Map使用简单的XML或注解，将接口和Java的POJO映射成数据库中的记录。

<!--more-->

##安装

要使用MyBaitis,首先要保证我们的开发环境中有它的开发包，有两种方式：

#####直接引入jar包

将mybatis-x.x.x.jar文件放入classpath中。

#####maven中添加依赖
	
	<dependency>
		<groupId>org.mybatis</groupId>
		<artifactId>mybatis</artifactId>
		<version>x.x.x</version>
	</dependency>	
	
##Mybatis流程
![流程图](http://img.my.csdn.net/uploads/201306/09/1370783456_4126.JPG)

####创建SqlSessionFactory实例

Mybatis应用程序的入口是SqlSessionFactoryBuilder，他的作用是通过xml配置文件或者是通过java程序创建Mybaits的实例中心SqlSessionFactory。

#####通过xml创建SqlSessionFactory

	String resource = "org/mybatis/example/mybatis-config.xml";
	InputStream inputStream = Resources.getResourceAsStream(resource);//加载配置文件
	SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);//创建sqlSessionFactory
	
xml配置文件中包含了一些核心设置，包括获取数据库连接实例的数据源等。下面是一个xml配置的简单示例。

	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE configuration
	PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-config.dtd">
	<configuration>
		<environments default="development">
			<environment id="development">
      			<transactionManager type="JDBC"/>
      			<dataSource type="POOLED">
        			<property name="driver" value="${driver}"/>
        			<property name="url" value="${url}"/>
        			<property name="username" value="${username}"/>
        			<property name="password" value="${password}"/>
      			</dataSource>
    		</environment>
		</environments>
        <mappers>
    		<mapper resource="org/mybatis/example/BlogMapper.xml"/>
    	</mappers>
	</configuration>
	
#####不通过xml创建SqlSessionFactory
	
	DataSource datasource = createDataSource();//创建dateSource数据源
	TransactionFactory transactionFactory = new JdbcTransactionFactory();//创建事务
	Environment environment = new Environment("development", transactionFactory, dataSource);
	Configuration configuration = new Configuration(environment);
	SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(configuration);

SqlSessionFactoryBuilder这个类的实例只用来创建SqlSessionFactory,一旦创建完成,sqlSessionFactoryBuilder就不再需要了(方法范围)sqlSessionFactory一旦被创建，就应该在应用的运行期间一直存在(应用范围)，整个应用最好只有一个SqlSessionFactory实例。

####创建SqlSession实例

当创建了sqlSessionFactory工厂实例之后，我们就可以通过它获得SqlSession的实例了。SqlSession包含了面向数据库执行 SQL命令所需的所有方法。
方式一：

	SqlSession session = 	sqlSessionFactory.openSession();
	try {
		Blog  blog = (Blog)session.selectOne(
			"org.mybatis.example.BlogMapper.selectBlog", 101);
		} finally {
    		session.close();
    	}
    	
方式二：

	SqlSession session = sqlSessionFactory.openSession();
	try {
		BlogMapper mapper = session.getMapper(BlogMapper.class);//映射器实例
		Blog blog = mapper.selectBlog(101);
		} finally {
		session.close();
		}
		
SqlSession有一个方法getMapper用来获取Mapper对象。这个方法是联系应用程序和Mybatis纽带，应用程序访问getMapper时，Mybatis会根据传入的接口类型和对应的XML配置文件生成一个代理对象，这个代理对象就叫Mapper对象。应用程序获得Mapper对象后，就应该通过这个Mapper对象来访问Mybatis的SqlSession对象，这样就达到里插入到Mybatis流程的目的。

每个线程都应该有它自己的SqlSession实例。SqlSession实例不是线程安全的，所以不能共享。

####Executor对象

Executor对象在创建Configuration对象的时候创建，并且缓存在Configuration对象里。Executor对象的主要功能是调用StatementHandler访问数据库，并将查询结果存入缓存中(如果配置了缓存的话)。

####StatementHandler对象

StatementHandler是真正访问数据库的地方，并调用ResultSetHandler处理查询结果。

####ResultSetHandler对象

处理查询结果。
	


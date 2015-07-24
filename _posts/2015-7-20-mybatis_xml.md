---
layout: post
category: SQL
title: MyBatis之(二)Xml映射配置文件
tagline: by zhanyr
tags: [MyBatis, Mysql]
---

MyBatis的配置文件包含了影响MyBatis行为的设置(settings)和属性(properties)信息。

<!--more-->

##顶层结构

	configuration 配置
		properties 属性　
		settings 设置
		typeAliases 类型命名
		typeHandlers 类型处理器
		objectFactory 对象工厂
		plugins 插件
		environments 环境
			environment 环境变量
				transactionManager 事务管理器
				dataSource 数据源
		databaseIdProvider 数据库厂商标识
		mappers 映射器

##各种配置的详细介绍

###properties

属性可以通过各种不同的方式进行配置，包括使用java常用的配置文件、properties元素,方法参数。

如果属性在不只一个地方进行了配置，那么 MyBatis 将按照下面的顺序来加载：

1. 在 properties 元素体内指定的属性首先被读取。
2. 根据 properties 元素中的 resource 属性读取类路径下属性文件或根据 url 属性指定的路径读取属性文件，并覆盖已读取的同名属性。
3. 读取作为方法参数传递的属性，并覆盖已读取的同名属性。

也就是说，通过方法参数传递的属性值具有最高优先级，使用properties指定的属性优先级最低。

###settings

该设置可以改变mybatis的运行时行为，完整配置如下：

	<settings>
		/*所有映射器中配置的缓存的全局开关*/
		<setting name="cacheEnabled" value="true"/>
		/*延迟加载，配置为true,则所有关联对象都会延迟加载*/
		<setting name="lazyLoadingEnabled" value="false"/>
		/*允许单一语句返回多结果集*/
		<setting name="multipleResultSetsEnabled" value="true"/>
		/*使用列标签代替列名*/
		<setting name="useColumnLabel" value="true"/>
		/*支持自动生成主键*/
		<setting name="useGeneratedKeys" value="false"/>
		/*自动映射列到字段或属性的方式*/
		<setting name="autoMappingBehavior" value="PARTIAL"/>
		/*设置默认执行器*/
		<setting name="defaultExecutorType" value="SIMPLE"/>
		/*默认语句超时时间*/
		<setting name="defaultStatementTimeout" value="null"/>
		/*允许在嵌套语句中使用分页*/
		<setting name="safeRowBoundsEnabled" value="false"/>
		/*开启自动驼峰命名规则映射*/
		<setting name="mapUnderscoreToCamelCase" value="false"/>
		/*利用本地缓存机制防止循环引用和加速重复嵌套查询*/
		<setting name="localCacheScope" value="SESSION"/>
		/*当没有为参数提供特定的 JDBC 类型时，为空值指定 JDBC 类型*/
		<setting name="jdbcTypeForNull" value="OTHER"/>
		/*指定那个对象的方法触发一次延迟加载*/
		<setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
	</settings>
	
###typeAliases

为java别名设置一个短的名字，存在的意义在于减少类完全限定名冗余。
	
	<typeAliases>	
		<typeAlias alias="Author" type="domain.blog.Author"/>
	</typeAliases>
	
后面就可以在任何使用domain.blog.Author的地方使用Author。

###typeHandlers

类型处理器。无论是MyBatis在预处理语句（PreparedStatement）中设置一个参数时，还是从结果集中取出一个值时，都会用类型处理器将获取的值以合适的方式转换成Java类型。

MyBatis不会窥探数据库元信息来决定使用哪种类型，所以你必须在参数和结果映射中指明那是VARCHAR类型的字段，以使其能够绑定到正确的类型处理器上。这是因为:MyBatis直到语句被执行才清楚数据类型。

###objectFactory

MyBatis 每次创建结果对象的新实例时，它都会使用一个对象工厂(ObjectFactory)实例来完成。 默认的对象工厂需要做的仅仅是实例化目标类，要么通过默认构造方法，要么在参数映射存在的时候通过参数构造方法来实例化。 如果想覆盖对象工厂的默认行为，则可以通过创建自己的对象工厂来实现。

###plungins

MyBatis 允许你在已映射语句执行过程中的某一点进行拦截调用。

###environment

MyBatis可以配置成适应多种环境，这种机制有助于将SQL映射应用于多种数据库之中。例如，开发、测试和生产环境需要有不同的配置；或者共享相同 Schema 的多个生产数据库，想使用相同的SQL映射。
`尽管可以配置多个环境，每个 SqlSessionFactory 实例只能选择其一。`

指定环境

	SqlSessionFactory factory = sqlSessionFactoryBuilder.build(reader, environment);
	SqlSessionFactory factory = sqlSessionFactoryBuilder.build(reader, environment,properties);
	
默认环境

	SqlSessionFactory factory = sqlSessionFactoryBuilder.build(reader);
	SqlSessionFactory factory = sqlSessionFactoryBuilder.build(reader,properties);
	
配置：

	<environments default="development">
		<environment id="development">
			<transactionManager type="JDBC">
				<property name="..." value="..."/>
			</transactionManager>
			<dataSource type="POOLED">
				<property name="driver" value="${driver}"/>
				<property name="url" value="${url}"/>
				<property name="username" value="${username}"/>
				<property name="password" value="${password}"/>
			</dataSource>
		</environment>
	</environments>

#####transactionManager

在 MyBatis 中有两种类型的事务管理器（也就是 type=”[JDBC|MANAGED]”）：

JDBC 这个配置就是直接使用了 JDBC 的提交和回滚设置，它依赖于从数据源得到的连接来管理事务范围。
MANAGED 这个配置几乎没做什么。它从来不提交或回滚一个连接，而是让容器来管理事务的整个生命周期（比如 JEE 应用服务器的上下文）。 默认情况下它会关闭连接，然而一些容器并不希望这样，因此需要将 closeConnection 属性设置为 false 来阻止它默认的关闭行为。

如果使用 Spring + MyBatis，则没有必要配置事务管理器， 因为 Spring 模块会使用自带的管理器来覆盖前面的配置。

#####dataSource

dataSource 元素使用标准的 JDBC 数据源接口来配置 JDBC 连接对象的资源。


###databaseIdProvider

根据不同的数据库厂商执行不同的语句，这种多厂商的支持是基于映射语句中的 databaseId 属性。 MyBatis 会加载不带 databaseId 属性和带有匹配当前数据库 databaseId 属性的所有语句。 如果同时找到带有 databaseId 和不带 databaseId 的相同语句，则后者会被舍弃。

###mappers

定义sql映射语句。但是首先要告诉Mybatis去哪里找这些语句。最佳的方式是告诉 MyBatis 到哪里去找映射文件。有三种方式：
使用resource、url、class、package


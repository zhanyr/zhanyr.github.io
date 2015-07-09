---
layout: post
category: Language
title: Scala数组操作
tagline: by zhanyr
tags: [Scala]
---
数组是定义数据类型很基础的容器,是一种非常重要的数据结构。围绕数组的操作也非常重要。

<!--more-->

###定义数组类型

	var array1 = new Array[Int](10)
	var array2 = new Array[String](10)
	
###数组元素赋值
Scala具有强大的类型推导能力去判断数组中赋值的类型:

	val arrayInt = Array(1,2,3,4)
	var arrayString = Array("hello","world")
	val array = Array(1,"hello","4",4)
	arrayInt(0) = 0
	arrayString(1) = "Scala"

以上定义都是合法的。

###数组元素读取及遍历

####读取

	println(arrayInt(0))
	println(arrayString(0))

####遍历

	for(elem <- arrayString)
		println(elem)
	for(elem <- arrayInt)
		println(elem)
	for(elem <- array)
		println(elem)

###数组基本操作
	val b = ArrayBuffer[Int]() //可修改
	b += 1 //(1)
	b += (1,2,3,5) //(1,1,2,3,5)
	b ++= Array(6,7,8) //(1,1,2,3,5,6,7,8)
	b.trimEnd(5) //(1,1,2)
	b.insert(2,6) //(1,1,6,2)
	b.insert(2,7,8,9) //(1,1,7,8,9,6,2)
	b.remove(2) //(1,1,8,9,6,2)
	b.remove(2,3) //从索引为2的位置移除3个元素(1,1,2)
	val c = b.toArray //变为不可修改数组

###数组进阶操作
	//把数组每个元素乘2并返回集合(4,6,8,10)
	var c = Array(2,3,4,5)
	var result = for(elem <- c) yield 2 * elem
	//所有偶数乘2 (4,8)
	val result1 = for(elem <- c if elem %2 == 0)yield 2 * elem
	//所有偶数乘2 (4,8)
	val result2 = c.filter(_ % 2 == 0).map(2 * _)
	//9
	Array(1,2,3,3).sum
	//长度最长的(little)
	ArrayBuffer("Marry","had","a","little","dog").max
	//默认排序升序(1,2,7,9)
	val d = Array(1,7,2,9)
	val bSorted = d.sorted
	//快排 e变成(1,2,7,9)
	val e = Array(1,7,2,9)
	scala.util.Sorting.quickSort(e)
	//拼接字符串
	e.mkString(" and ") // 1 and 2 and 7 and 9
	e.mkString("<",",",">") // <1,2,7,9>
	//矩阵,多维数组
	val matrix = Array.ofDim[Double](2,3) //定义一个2行三列的数组
	matrix(1,2) = 2 //给第二行第三列赋值
	val triangle = new Array[Array[Int]](10)//数组中嵌套数组，其中内部数组的长度待定

  


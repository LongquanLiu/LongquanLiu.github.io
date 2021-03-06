---
title: Imooc Java Primary III Chap456 集合框架
date: 2018-11-30 19:12:00
tags: Java Imooc
---

## 4.1 集合框架概述

1. 集合的概念：
  * 现实生活中：很多事物凑在一起。（购物车，商品的集合；军队，军人的集合）
  * 数学中的集合：具有共同属性的事物的总体（有理数、整数）
  * Java中的集合：是一种工具类，就像是**容器**，存储任意数量的**具有共同属性的对象**。
2. 集合的作用：
  * 在类的内部，对数据进行组织；
  * 简单而快速的搜索大数量的条目；
  * 有的集合接口，提供了一系列排列有序的元素，并且可以再序列中间快速的插入或者删除有关元素；
  * 有的集合接口，提供了映射关系，可以通过关键字（key）去快速查找到对应的唯一对象，而这个关键字可以是任意类型。
3. 与数组的对比-为何选择集合而不是数组
  * 数组的长度固定，集合长度可变（金箍棒、打狗棒）
  * 数组只能通过下标访问元素，类型固定，而有的集合可以通过任意类型查找所映射的具体对象
4. 集合框架体系结构：
![集合框架体系结构]((/img/Imooc Java Primary III/Collection_Map.png)

List(序列)、Queue(队列)：排列有序可重复；Set(集)：无序不可重复
map:<key,value>Entry键值对

## 4.2 Collection interface & List interface

1. Collection接口：
  * 是List、Set和Queue接口的父接口
  * 定义了可用于操作List、Set和Queue的方法——增删改查
2. List接口及其实现类——ArrayList
  * List是元素有序并且可以重复的集合，被称为序列
  * List可以精确的控制每个元素的插入位置，或删除某个位置元素
  * ArrayList——数组序列，是List的一个重要实现类
  * ArrayList底层是由数组实现的

## 4.3 学生选课

1. 对象存入集合都变成Object类型，取出时需要进行类型转换
2. Iterator依赖于某个集合存在，不具备存储集合的功能
3. ClassCastException 泛型，eg：在未定义List泛型时，往List中添加字符串；

## 4.9 泛型

1. 集合中的元素，可以是任意类型的对象(对象的引用)
  * 如果把某个对象放入集合，则会忽略他的类型，而把她当做Object处理
2. 泛型则是规定了某个集合只可以存放特定类型的对象
  * 会在编译期间进行类型检查
  * 可以直接按指定类型获取集合元素
3. 泛型集合：泛型类型、泛型的子类型
4. 泛型集合中的限定类型不能是基本数据类型(int,long,boolean);但可以通过包装类来限定数据类型(Integer,Long,Boolean).

## 4.11 Set集合

1. Set接口及其实现类——HashSet
  * Set是元素无序并且不可重复的集合，被称为集
  * HashSet——哈希集，是Set一个重要实现类
2. 遍历Set中的元素只能用ForEach、Iterator;不能用get()-无序
3. Set 中可以添加空对象

## 5.1 Map & HashMap 简介

1. Map接口：
  * Map提供了一种映射关系，其中的元素是以键值对(Key-Value)的形式存储的，能够实现根据key快速查找value
  * Map中的键值对以Entry类型的对象实例形式存在
  * key值不可重复，value值可以
  * 每个key最多只能映射到一个值
  * Map接口提供了分别返回key值集合、value值集合以及Entry集合的方法
  * Map支持泛型，形式如：Map<K,V>

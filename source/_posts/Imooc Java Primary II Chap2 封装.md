---
title: Imooc Java Primary II Chap2 封装
date: 2018-11-26 16:40:00
tags: Java Imooc
---

## 2.1 封装

1. 面向对象三大特性：封装、继承、多态
2. 封装：隐藏类的信息，只能通过类提供的方法实现对隐藏信息的操作和访问
3. 封装实现步骤：
  1. 修改属性可见性，设为private
  2. 创建getter/setter方法，用于属性的读写
  3. 在getter/setter方法中加入属性控制语句，对属性值得合法性进行判断

## 2.2 包管理Java中的类

1. 包的作用：管理Java文件、解决同名文件(类)冲突
2. 定义包：package 包名
  * 必须放在Java源程序第一行
  * 包名间可以使用“.”隔开，eg:com.imooc.MyClass
3. 系统中的包：
  1. java.(功能).(类)
  2. java.lang.(类) 包含java语言基础的类
  3. java.util.(类) 包含java语言中各种工具类
  4. java.io.(类) 包含输入、输出相关功能的类
4. 包的使用：
  1. import
  2. 全小写字母
  3. com.imooc.*

## 2.3 访问修饰符

1. 修饰属性和方法的访问范围
2. 访问修饰符：private,default,protected,public
3. 对应范围：本类，同包，子类，其他

## 2.4 this关键字

1. this关键字代表当前对象
  * this.属性 操作当前对象的属性
  * this.方法 调用当前对象的方法
2. 封装对象属性的时候，经常使用this关键字

## 2.5 内部类

1. 内部类的作用
  1. 内部类提供了更好的封装，把内部类隐藏在外部类之内，不允许同一个包中的其他类访问该类。
  2. 内部类可以直接访问外部类的所有数据，包括私有数据。
  3. 内部类所实现的功能外部类同样可以实现，只是有时使用内部类更方便
2. 内部类分为以下几种：
  * 成员内部类
  * 静态内部类
  * 方法内部类
  * 匿名内部类

## 2.6 成员内部类

1. 成员内部类的使用方法：
  1. Inner 类定义在 Outer 类的内部，相当于 Outer 类的一个**成员变量的位置**，Inner 类可以使用任意访问控制符，如 public 、 protected 、 private 等
  2. Inner 类中定义的 test() 方法可以**直接访问 Outer 类中的所有数据**，而不受访问控制符的影响，如直接访问 Outer 类中的私有属性a
  3.  定义了成员内部类后，**必须使用外部类对象来创建内部类对象**，而不能直接去 new 一个内部类对象，即：内部类 对象名 = 外部类对象.new 内部类( );
  4. 编译后：内部类名.class;外部类名$内部类名.class;
2. **外部类是不能直接使用内部类的成员和方法**滴，即先创建内部类的对象，然后通过内部类的对象来访问其成员变量和方法。
3. 如果外部类和内部类具有相同的成员变量或方法，内部类默认访问自己的成员变量或方法，如果要访问外部类的成员变量，可以使用 this 关键字。

## 2.7 static内部类

1.  创建static内部类的对象时，不需要外部类的对象，可以直接创建 内部类 对象名= new 内部类();
2. static内部类不能直接访问外部类的**非静态成员**，但可以通过 new 外部类().成员 的方式访问
3. 如果外部类的static成员与内部类的成员名称相同，可通过**类名.静态成员**访问外部类的静态成员；如果外部类的静态成员与内部类的成员名称不相同，则可通过“成员名”直接调用外部类的静态成员

## 2.8 方法内部类

1. 方法内部类就是内部类定义在外部类的方法中，方法内部类只在**该方法的内部可见**，即只在该方法内可以使用。
2. 由于方法内部类不能在外部类的方法以外的地方使用，因此方法内部类**不能使用访问控制符和 static 修饰符**。

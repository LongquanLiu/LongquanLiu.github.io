---
title: Imooc Java Primary II Chap3 继承
date: 2018-11-26 16:40:00
tags: Java Imooc
---

## 3.1 继承

1. 继承：是类与类的一种关系，是一种“is a”的关系 Dog is a Animal
  * Java中的继承是单继承的
2. 继承的好处：
  * 子类拥有父类的所有属性和方法（富二代）
    private修饰的无效（私生子）
  * 实现代码复用

## 3.2 方法重写

1. 语法规则：以下三条都要与父类相同
  * 返回值类型
  * 方法名
  * 参数类型及个数

## 3.3 继承的初始化顺序

1. 父类对象-父类属性初始化-父类构造方法---子类对象-子类属性初始化-子类构造方法

## 3.4 final关键字

1. final 修饰类，则该类**不允许被继承**
2. final 修饰方法，则该方法**不允许被重写**
3. final 修饰属性，则该类的属性不会进行隐式初始化（即初始化属性必须有值）or在构造方法中赋值
4. final 修饰变量，则该变量只能赋一次值，即为常量

## 3.5 super关键字

1. 在对象的内部使用，可以代表父类对象
2. 访问父类属性 super.age; 访问父类的方法 super.eat();
3. 子类的构造过程当中必须调用其父类的构造方法（隐式）
  * 显示调用必须在第一行
  * 隐式默认调用父类无参构造方法
  * 若父类定义了有参的构造方法（代表父类将没有默认无参的构造方法），子类又无显式调用父类的构造方法，则编译出错

## 3.6 Object类I

1. toString()方法：
  * 返回对象的**hash码**（对象地址字符串）,return 包名@地址
  * 可以通过重写toString()方法表示出对象的属性
2. equals()方法:
  * 比较的是对象的**引用**是否指向同一块内存地址,类似于==, return boolean
  * 比较两个对象的值是否一致，即进行重写
  ``` java
  public boolean equals(Object obj){
  @Override
    if(this == obj)
      return true;
    if(obj == null)
      return false;
    if(getClass() != obj.getClass())
      // obj.getClass代表获取'类对象'！即判断该对象的类型
      return false;
    Dog other = (Dog)obj;
    if(age != other.age)
      // 判断'类的对象'
      return false;
    return true;
  }
  ```

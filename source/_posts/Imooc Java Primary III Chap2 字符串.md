---
title: Imooc Java Primary III Chap2 字符串
date: 2018-11-28 15:45:00
tags: Java Imooc
---

## 2.1 字符串

1. 字符串被作为 String 类型的对象处理。 String 类位于 java.lang 包中。默认情况下，该包被自动导入所有的程序。
2. 创建String对象的方法：
  1. String s1 = "imooc"
  2. String s2 = new String();
  3. String s3 = new String("imooc");

## 2.2 字符串的不变性

1.String 对象创建后则不能被修改，是不可变的，所谓的修改其实是创建了新的对象，所指向的内存空间不同。
```java
String s1 ="爱慕课";
String s2 ="爱慕课";
String s3 = new String("爱慕课");
String s4 = new String("爱慕课");
// 多次出现的字符串常量，java程序只创建一个，所以返回true
System.out.println(s1 == s2);
// s1和s3是不同的对象，所以返回false
System.out.println(s1 == s3);
// s3和s4是不同的对象，所以返回false
System.out.println(s3 == s4);
s1 = "欢迎来到：" + s1;
// 字符串s1被修改，指向新的内存空间
System.out.println(s1);
```

2. 一旦一个字符串在内存中创建，则这个字符串将不可改变。如果需要一个可以改变的字符串，我们可以使用StringBuffer或者StringBuilder
3. 每次 new 一个字符串就是产生一个新的对象，即便两个字符串的内容相同，使用 ”==” 比较时也为 ”false” ,如果只需比较内容是否相同，应使用 ”equals()” 方法

## 2.3 String 类的常用方法 I

1. String类的常用方法:
![String类的常用方法](/img/Imooc Java Primary III/String.jpg)
2. 方法使用:
![方法使用](/img/Imooc Java Primary III/String_eg.jpg)
3. 字符串 str 中字符的索引从0开始，范围为 0 到 str.length()-1
4. 使用 indexOf 进行字符或字符串查找时，如果匹配返回位置索引；如果没有匹配结果，返回 -1
5. 使用 substring(beginIndex , endIndex) 进行字符串截取时，包括 beginIndex 位置的字符，不包括 endIndex 位置的字符

## 2.4 String 类的常用方法 II

1. ==: 判断两个字符串在内存中首地址是否相同，即判断是否是同一个字符串对象；equals(): 比较存储在两个字符串对象中的内容是否一致
2. 字节是计算机存储信息的基本单位，1 个字节等于 8 位， gbk 编码中 1 个汉字字符存储需要 2 个字节，1 个英文字符存储需要 1 个字节。所以我们看到上面的程序运行结果中，每个汉字对应两个字节值，如“学”对应 “-47 -89” ，而英文字母 “J” 对应 “74” 。同时，我们还发现汉字对应的字节值为负数，原因在于每个字节是 8 位，最大值不能超过 127，而汉字转换为字节后超过 127，如果超过就会溢出，以负数的形式显示。

## 2.5 StringBuilder类

1. StringBuffer 是线程安全的，而 StringBuilder 则没有实现线程安全功能，所以性能略高。
2. StringBuilder方法:
![StringBuilder方法](/img/Imooc Java Primary III/StringBuilder.jpg)
3. 在需要频繁对字符串进行修改操作时使用 StringBuilder 的效率比 String 要高

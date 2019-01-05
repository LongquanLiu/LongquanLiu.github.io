---
title: Imooc Java Primary III Chap3 常用类
date: 2018-11-30 19:12:00
tags: Java Imooc
---

## 3.1 包装类

1. 基本类型和包装类之间的对应关系：
![基本类型和包装类之间的对应关系](/img/Imooc Java Primary III/packageClass.jpg)
2. 包装类主要提供了两大类方法：
  * 将本类型和其他基本类型进行转换的方法
  * 将字符串和本类型及包装类互相转换的方法

3. Integer 包装类的构造方法：
![](/img/Imooc Java Primary III/Integer.jpg)

4. Integer包装类的常用方法：
![](/img/Imooc Java Primary III/IntegerMethod.jpg)

## 3.2 基本类型和包装类之间的转换

1. JDK1.5 引入自动装箱和拆箱的机制
2. 装箱：把基本类型转换成包装类，使其具有对象的性质，又可分为手动装箱和自动装箱
```java
int i = 10
Integer x = new Integer(i);
Integer y = i;
```
3. 拆箱：和装箱相反，把包装类对象转换成基本类型的值，又可分为手动拆箱和自动拆箱
```java
Integer j = new Integer(8);
int m = j.intValue();
int n = j;
```

## 3.3 基本类型和字符串之间的转换

1. 基本类型转换为字符串有三种方法：
  * 使用包装类的 toString() 方法
  * 使用String类的 valueOf() 方法
  * 用一个空字符串加上基本类型，得到的就是基本类型数据对应的字符串
```java
int c = 10;
String str1 = Integer.toString(c);
String str2 = String.valueOf(c);
String str3 = c + "";
```

2. 将字符串转换成基本类型有两种方法：
  * 调用包装类的 parseXxx 静态方法
  * 调用包装类的 valueOf() 方法转换为基本类型的包装类，会自动拆箱
```java
String str = "9";
int d = Integer.parseInt(str);
int e = Integer.valueOf(str);
```

## 3.4 使用 Date 和 SimpleDateFormat 类表示时间

1. java.util 包中的 Date 类。这个类最主要的作用就是获取当前时间，Date 类的使用：
```java
Date d = new Date();
System.out.println(d);
// 默认无参构造函数为当前时间
```
2. java.text 包中的 SimpleDateFormat 类, 可以使用 SimpleDateFormat 来对日期时间进行格式化，如可以将日期转换为指定格式的文本，也可将文本转换为日期。
  * 使用 format() 方法将日期转换为指定格式的文本:

  ```java
  Date d = new Date();
  //创建SimpleDateFormat对象，指定目标格式
  SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
  //调用format方法，格式化时间，转换为指定格式字符串
  String today = sdf.format(d);
  System.out.println(today);
  ```

  *使用 parse() 方法将文本转换为日期:

  ```java
  //创建日期格式的字符
  String day = "2018年12月01日 16:39:10";
  //创建SimpleDateFormat对象,指定字符串的日期格式
  SimpleDateFormat df = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
  //调用parse()方法，将字符串转换为日期
  Date date = df.parse(day);
  System.out.println("当前时间：" + date);
  ```

3. 调用 SimpleDateFormat 对象的 parse() 方法时可能会出现转换异常，即 ParseException ，因此需要进行异常处理
4. 使用 Date 类时需要导入 java.util 包，使用 SimpleDateFormat 时需要导入 java.text 包

## 3.5 Calendar类的应用

1. java.util.Calendar 类是一个抽象类，可以通过调用 getInstance() 静态方法获取一个 Calendar 对象，此对象已由当前日期时间初始化，即默认代表当前时间，如 Calendar c = Calendar.getInstance();
```java
Calendar c = Calendar.getInstance();
int year = c.get(Calendar.YEAR);
int month = c.get(Calendar.MONTH) + 1; // 0 is 1 MONTH
int day = c.get(Calendar.DAY_OF_MONTH);
int hour = c.get(Calendar.HOUR_OF_DAY);
int minute = c.get(Calendar.MINUTE);
int second = c.get(Calendar.SECOND);
System.out.println("当前时间: " + year + "-" + month + "-" + day + " " + hour + ":" + minute + ":" + second);
```

2. Calendar 类提供了 getTime() 方法，用来获取 Date 对象，完成 Calendar 和 Date 的转换，还可通过 getTimeInMillis() 方法，获取此 Calendar 的时间值，以毫秒为单位。
```java
Date date = c.getTime(); //将Calendar对象转换为Date对象
Long time = c.getTimeInMillis(); //获取当前毫秒数
```

## 3.6 使用 Math 类操作数据

1. Math 类位于 java.lang 包中，包含用于执行基本数学运算的方法， Math 类的所有方法都是静态方法，所以使用该类中的方法时，可以直接使用类名.方法名，如： Math.round();
2. 常用的方法：
![](/img/Imooc Java Primary III/MathMethod.jpg)
3. 使用：
![](/img/Imooc Java Primary III/MathEG.jpg)

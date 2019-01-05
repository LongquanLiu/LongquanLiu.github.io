---
title: Imooc Java Primary III Chap1 异常和异常处理
date: 2018-11-28 10:45:00
tags: Java Imooc
---


## 1.1 异常简介

1. 异常：有异于常态，阻止当前方法或作用域，称之为异常。
2. 异常的正确处理：提交给编程人员or用户，从而使原本中断的程序可继续运行or退出，并且保存当前操作or数据回滚，最后释放占用资源
3. Throwable:
  1) Error: 程序崩溃（硬伤无法处理）
    * VirtualMachineError:虚拟机错误
    * ThreadDeath:线程错误
  2) Exception: 编程、环境、用户操作输入出现问题
    1. RunTimeException: 非检查异常 Java虚拟机自动抛出捕获，逻辑上检查
      * NullPointerException: 空指针异常
      * ArrayIndexOutOfBoundException: 数组下标越界异常
      * ClassCastException: 类型转换异常
      * ArithmeticException: 算术异常
      ...
    2. CheckException: 检查异常 手动添加捕获处理语句
      * IOException: 文件异常
      * SQLExecption: SQL异常

## 1.2 处理异常

1. try-catch:
  ```java
  try{
    //一些会抛出异常的方法
  }catch(Exception e){
    e.printStackTrace();
    //处理该异常的代码块
  }catch(Exception2 e){
    //处理Exception2的代码块
  }...finally{
    //最终要执行的代码
  }
  ```
  * 异常顺序注意子类-父类，就近寻找匹配
  * return 在try、catch、finally中，按顺序都执行完,eg:finally是在catch里return执行完，返回调用该方法之前执行
  * return 不在try、catch、finally中，则执行到块外的return
  * try 语句块不可以独立存在，必须与 catch 或者 finally 块同存

## 1.3 异常抛出与自定义异常

1. throw, 将产生的异常抛出（动作）
2. throws, 声明将要抛出何种类型的异常（声明）
  ```java
  public void 方法名(参数列表) throws 异常列表{
    //调用会抛出异常的方法或者：throw new Exception();
  }
  ```

3. 自定义异常
```java
class 自定义异常类 extends 异常类型{
  public 自定义异常类(){

  }
  public 自定义异常类(String message){
    super(message);
  }
}
```

## 1.4 异常链

1. 捕获的异常包装成新的异常，在新的异常里添加对原始异常的引用，再抛出
```java
try{

}catch(DrunkException e){
  RunTimeException newExc = new RunTimeException("...");
  newExc.initCause(e);
  throw newExc;
}
try{

}catch(DrunkException e){
  RunTimeException newExc = new RunTimeException(e);
  // newExc.initCause(e);
  throw newExc;
}
```

2. 捕获到的异常，可以在当前方法的 catch 块中处理，也可抛出给调用者去处理

## 1.5 经验总结

1. 处理运行时异常时，采用逻辑去合理规避同时辅助try-catch处理
2. **在多重catch块后面，可以加一个catch(Exception)来处理可能会被遗漏的异常**
3. 对于不确定的代码，也可以加上try-catch,处理潜在的异常
4. 尽量去处理异常，切忌只是简单的调用printStackTrace()去打印输出
5. 具体如何处理异常，要根据不同的业务需求和异常类型去决定
6. **尽量添加finally语句块去释放占用的资源**

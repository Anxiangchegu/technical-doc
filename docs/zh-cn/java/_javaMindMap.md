## 简介

[Java11新特性](https://www.oracle.com/technetwork/java/javase/11-relnote-issues-5012449.html#NewFeature)  

?> 以下导图均为图片格式，可单独打开放大查看。

![Java导图](../_images/java/1-javaIntro.png "简介")
## 特性  
![Java导图](../_images/java/2-javaCharacter.png "Java特性")
## 发展历史
![Java导图](../_images/java/3-historyOfDevelopment.png "Java发展历史")
## 开发环境
![Java导图](../_images/java/4-developmentEnvironment.png "开发环境")
## 基础语法
![Java导图](../_images/java/5-basicGrammar.png "基础语法")

## 基本概念
![Java导图](../_images/java/6-basicConception.png "基本概念")

## 面向对象
![Java导图](../_images/java/7-OOP.png "面向对象")


![Java导图](../_images/java/8-OOPCharacter.png "Java发展历史")

![Java导图](../_images/java/9-OOP-C.png "Java发展历史")
## 数据类型 

| 大类 | 数据类型 | 存储大小 | 取值 | 示例 |
| :---         |     :---:      |          ---: |    ---: | ---: |
| 逻辑型   | boolean     | 2个字节    | true/false |   false|
| 文本型     | char      | 2个字节/字符     | 字符集| "沐之杰" |
| 整数型     | byte      | 8 位      | -128~127 | 100|
| 整数型     | short      | 16 位      |-32768~32767|0|
| 整数型     | int      | 32 位      |-2^31-1~2^31 （21 亿）|0|
| 整数型     | long      | 64 位      |-9223372036854775808~9223372036854775807(19位)|0L|
| 浮点型     | float      | 32 位      |后缀 F 或 f，1 位符号位，8 位指数，23 位有效尾数 |0.0F|
| 浮点型     | double      | 64 位      |后缀 D 或 d，1 位符号位，11 位指数，52 位有效尾数|0.0D|

![Java导图](../_images/java/10-java基本数据类型.png "数据类型")
![Java导图](../_images/java/11-java基本数据类型B.png "数据类型")
## 变量类型
![Java导图](../_images/java/12-Java变量类型.png "变量类型")
## 修饰符
![Java导图](../_images/java/13-java修饰符.png "修饰符")
## 运算符
![Java导图](../_images/java/14-java运算符A.png "运算符")
![Java导图](../_images/java/15-java运算符B.png "运算符")
## 循环结构
![Java导图](../_images/java/16-java循环结构.png "循环结构")
## 分支结构
![Java导图](../_images/java/17-java分支结构.png "分支结构")
## Number & Math
![Java导图](../_images/java/18-javaNumber&Math类.png "Number & Math")
## 常用JavaString类
![Java导图](../_images/java/19-常用javaString类.png "常用JavaString类")
## StrignBuffer和StringBuilder类
![Java导图](../_images/java/20-javaStringBuffer和StringBuilder类.png "StrignBuffer和StringBuilder类")

| String | 	StringBuffer | StringBuilder |
| :---         |     :---:      |          ---: |
| String的值是不可变的，这就导致每次对String的操作都会生成新的String对象，不仅效率低下，而且浪费大量优先的内存空间   | StringBuffer是可变类，和线程安全的字符串操作类，任何对它指向的字符串的操作都不会产生新的对象。每个StringBuffer对象都有一定的缓冲区容量，当字符串大小没有超过容量时，不会分配新的容量，当字符串大小超过容量时，会自动增加容量| 可变类，速度更快 |
| 不可变     | 可变      | 可变   |
|      | 线程安全      | 	线程不安全   |
|      | 多线程操作字符串      | 单线程操作字符串 |    

## 数组
![Java导图](../_images/java/21-java数组.png "数组")
## 日期时间
![Java导图](../_images/java/22-java日期时间.png "日期时间")
## 异常处理
![Java导图](../_images/java/23-java异常处理.png "异常处理")
## IO流
![Java导图](../_images/java/24-java流(Stream)、文件(File)和IO.png "java流(Stream)、文件(File)和IO")
## 集合
![Java导图](../_images/java/26-java集合框架.png "集合")
## 泛型
![Java导图](../_images/java/27-java泛型.png "泛型")
## 序列化
![Java导图](../_images/java/28-java序列化.png "序列化")
## 网络编程
![Java导图](../_images/java/29-java网络编程.png "网络编程")
##### 示例
![Java导图](../_images/java/29-NetworkingProgramming.png "网络编程")
## 多线程
![Java导图](../_images/java/30-java多线程.png "网络编程")
---
title: Java学习笔记入门篇（一）
date: 2017-12-11 19:34:12
tags: ["Java编程"]
---
摘要：Java学习笔记入门，第一部分

<!-- more -->

## 学习过程

### Helloworld

```
public class Main {

    public static void main(String[] args) {

        System.out.println("Hello World!");

    }
}

```

``` bash
$ javac Main.java
$ ls
Main.class  Main.java
$ java Main
Hello World!
```

### 数据类型

#### 基本数据类型

##### 数值型

###### 整数型

byte、short、int、long
默认值：0

###### 小数型

float、double
默认值：0.0

##### 字符型

char
默认值：'\u0000'

##### 布尔值

boolean
默认值：false

#### 引用数据类型

数组、类、接口
默认值：null

#### 以上类型范围

![006.png](006.png)

#### 数据类型的选用建议

1. 在程序开发中描述整数用int，描述小数用double

2. long这种类型一般会描述日期时间、内存或文件大小（字节）

3. 如果需要进行编码方式转换，或者二进制文件传输，使用byte（-128~127)

4. char一般在描述中文时使用到

5. boolean在描述程序逻辑时用到

### 数据溢出问题

用更大的数据类型来解决

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

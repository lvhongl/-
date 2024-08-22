[TOC]

# Java发展史

## 计算机编程语言分类

* **机器语言**：电子机器能够直接识别的语言，无需经过翻译，计算机内部就有相应的电路来完成它；从使用的角度来看，机器语言是最低级的语言。
* **汇编语言**：面向机器的程序设计语言，符号语言；人工操作起来较简易的方式来设计程序的语言，通过不同的符号代替机器指令，特定的汇编语言与特定的机器语言指令集是一一对应的。
* **高级语言**：更容易被人们所理解的高级程序语言，符合人类思维模式的程序设计语言，如：C、Java，JavaScript、Python、Go[等](https://www.tiobe.com/tiobe-index/)。

## Java诞生及发展

- [James Gosling](https://zh.wikipedia.org/wiki/%E8%A9%B9%E5%A7%86%E6%96%AF%C2%B7%E9%AB%98%E6%96%AF%E6%9E%97)

- 版本

  | 版本                      | 代号                 | 发行日期   |
  | ------------------------- | -------------------- | ---------- |
  | JDK Beta                  |                      | 1995年     |
  | JDK 1.0                   | Oak(橡树)            | 1996年1月  |
  | JDK 1.1                   |                      | 1997年2月  |
  | J2SE 1.2                  | Playground（运动场） | 1998年12月 |
  | J2SE 1.3                  | Kestrel（美洲红隼）  | 2000年5月  |
  | J2SE 1.4                  | Merlin（灰背隼）     | 2002年2月  |
  | Java SE 5 (1.5)           | Tiger（老虎）        | 2004年9月  |
  | Java SE 6 (1.6)           | Mustang（野马）      | 2006年12月 |
  | Java SE 7 (1.7)           | Dolphin（海豚）      | 2011年7月  |
  | **Java SE 8 (1.8) (LTS)** | Spider（蜘蛛）       | 2014年3月  |
  | Java SE 9                 |                      | 2017年9月  |
  | Java SE 10                |                      | 2018年3月  |
  | **Java SE 11 (LTS)**      |                      | 2018年9月  |
  | Java SE 12                |                      | 2019年3月  |
  | Java SE 13                |                      | 2019年9月  |
  | Java SE 14                |                      | 2020年3月  |
  | Java SE 15                |                      | 2020年9月  |
  | Java SE 16                |                      | 2021年3月  |
  | **Java SE 17 (LTS)**      |                      | 2021年9月  |
  | Java SE 18                |                      | 2022年3月  |

  * **JavaSE**：标准版本，也称之为( J2SE)，具备了基本的库，用于在pc端进行开发。
  * **JavaEE**：企业版本，也称之为( J2EE)，具备了开发网站的功能，用于开发网站。
  * **JavaME**：最小版本，也称之为( J2ME)，在移动端开发使用，嵌入式设备上使用。

# JDK、JRE、JVM

## 基本概述

* Jvm：Java Virtual Machine（Java虚拟机）的缩写，用于执行字节码文件(.class)，相当于 Java 语言运行的一个容器。
* Jre：Java Runtime Environment，Java运行时环境。
* Jdk：Java Development Kit，Java语言的软件开发工具包。


## 三者关系

* Jre = JVM + 运行时需要的类库
* Jdk = Jre + Java程序开发工具

## Java可移植性

- 什么是可移植性？

  Java程序可以做到一次编译，到处运行。可移植性也被叫做跨平台性。
  例如：Java程序可以在Windows操作系统上运行，在不做任何修改的情况下，也可以在Linux操作系统下运行。

- Java的可移植性是如何做到的？

  - **C语言编译执行过程：**源码文件 --> 编译成机器能够识别的语言 --> 机器执行
    依赖平台编译器：**一次编写，到处编译**
- **Java语言编译执行过程：**源码文件 --> 编译成字节码文件(.class) --> 运行在Java虚拟机中
    依赖Java虚拟机(JVM)：**一次编译，到处执行**
  
- Java跨平台原理：

  - 在不同的操作系统平台上，安装了不同版本的jvm
  - 不同的jvm虚拟机，在不同的操作系统平台上，营造出来相同的运行环境，所以具备了跨平台性、
  
  ![image-20230709221734273](https://s2.loli.net/2023/07/09/BoKO6UDATwz4vLy.png)

## 安装JDK

[下载](https://www.oracle.com/in/java/technologies/javase/javase-jdk8-downloads.html)

[OpenJDK](https://en.wikipedia.org/wiki/OpenJDK#OpenJDK_versions)

[Zulu下载](https://www.azul.com/downloads/?version=java-8-lts&os=windows&package=jdk#zulu)

# Hello, World!

## 编写代码

创建`HelloWorld.java`文件并编写代码如下：

```java
class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

## 编译代码

控制台输入命令：

```shell
javac HelloWorld.java
```

无任何提示即代表编译成功

## 运行代码

```shell
java HelloWorld
```

## 注释

### 注释的作用

注释可以帮助其他人阅读程序，通常用于概括算法、确认变量的用途或者阐明难以理解的代码段。

注释并不会增加可执行程序的大小，编译器会忽略所有注释。

### 单行注释

```java
// 单行注释
```

### 多行注释

```java
/*
多行注释
多行注释
多行注释
*/
```

### 文档注释

文档注释允许你在程序中嵌入关于程序的信息。

并可以使用 javadoc 工具软件来生成 Java 文档文件，通常写在类、方法、属性上。

```java
/**
* HelloWorld类
* @author kobe
* @version: 1.0
*/
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

javadoc   -d  文件夹名称  文件名.java

javadoc   -d  文件夹名称  -author   -version 文件名.java
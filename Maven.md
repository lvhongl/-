[TOC]

# Maven介绍

## 什么是Maven

Maven 的正确发音是[ˈmevən]，而不是“马瘟”以及其它什么瘟。Maven 在美国是一个口语化的词语，代表专家、内行的意思。

一个对 Maven 比较正式的定义是这么说的：Maven 是一个项目管理工具，它包含了一个**项目对象模型** (**POM**：**Project Object Model**)，一组标准集合，一个项目生命周期(Project Lifecycle)，一个依赖管理系统 (Dependency Management System)

## Maven能解决什么问题

我们知道，项目开发不仅仅是写写代码而已，期间会伴随着各种必不可少的事情要做，列举几个感受一下：

1、我们需要引用各种 jar 包，尤其是比较大的工程，引用的 jar 包往往有几十个乃至上百个， 每用到一种 jar 包，都需要手动引入工程目录，而且经常遇到各种让人抓狂的 jar 包冲突，版本冲突。

2、世界上没有不存在 bug 的代码，计算机喜欢 bug ，就和人们总是喜欢美女帅哥一样。为了追求美为了减少 bug，因此写完了代码，我们还要写一些单元测试，然后一个个的运行来检验代码质量。

3、再优雅的代码也是要出来卖的。我们后面还需要把代码与各种配置文件、资源整合到一起，定型打包，如果是 web 项目，还需要将之发布到服务器，供人蹂躏。

​        试想，如果现在有一种工具，可以把你从上面的繁琐工作中解放出来，能帮你构建工程，管理 jar包，编译代码，还能帮你自动运行单元测试，打包，甚至能帮你部署项目，生成 Web 站点，你会心动吗，Maven 就可以解决上面所提到的这些问题。

## 构建工具的作用

Maven的定位是项目构建工具，因此需要了解什么是项目构建工具。

构建工具是将软件项目构建相关的过程自动化的工具。

构建一个软件项目通常包含以下一个或多个过程：

- 生成源码（如果项目使用自动生成源码）；

- 从源码生成项目文档；
- 编译源码；
- 将编译后的代码打包成JAR文件或者ZIP文件；
- 将打包好的代码安装到服务器、仓库或者其它的地方；

有些项目可能需要更多的过程才能完成构建，这些过程一般也可以整合到构建工具中，因此它们也可以实现自动化。

自动化构建过程的好处是将手动构建过程中犯错的风险降到最低。而且，自动构建工具通常要比手动执行同样的构建过程要快。

## Maven的依赖管理

​         Maven 的一个核心特性就是依赖管理。当我们涉及到多模块的项目（包含成百个模块或者子项目），管理依赖就变成

一项困难的任务。Maven 展示出了它对处理这种情形的高度控制。

如下图：

![image-20230218221536095](https://s2.loli.net/2023/02/18/52DC1UXidE6cvZy.png)

你会发现其实我们maven需要的jar包会存放在本地的一个jar包仓库，这样就不需要存放在项目中了，

但是说来说去jar包还是占用了电脑的磁盘空间，只是传统方式放在项目中，maven放在jar包仓库，这也并没有节约磁盘空间。如果是这样的话，我们试想一下，如果有10个项目都需要这个jar包呢。

如果有10个项目都需要这些jar包。我们就可以使用maven方式创建java项目了。

其实我们就是在做一件事儿，就是代码可重用。这也就是maven的第一个特性：依赖管理

依赖管理：maven工程对jar包的管理过程。我们只需要在项目中存放jar包的坐标即可，坐标一般都是放在pom.xml文件中。

# Maven的使用

## Maven软件下载

为了使用 Maven 管理工具，我们首先要到官网去下载它的安装软件。通过百度搜索“Maven“如下：

下载[Maven](https://maven.apache.org/download.cgi)

![image-20230218222123619](https://s2.loli.net/2023/02/18/Eiwa9Nrjx4KFfhC.png)

进入到官网页面，然后download

![image-20230218222626196](https://s2.loli.net/2023/02/18/pRIiPTJfV4XbrDt.png)

其实这个maven下载下来就是一个压缩包，只需要解压到一个没有中文路径的文件夹即可。比如放在D盘下。

我们只需要做一些简单的配置即可。 

## Maven软件安装

Maven 下载后，将 Maven 解压到一个没有中文没有空格的路径下，比如 D:\software\maven 下面。

解压后目录结构如下：

![image-20230218223205141](https://s2.loli.net/2023/02/18/1gKeGtlb8IWmnfc.png)

bin:存放了 maven 的命令

boot:存放了一些 maven 本身的引导程序，如类加载器等

conf:存放了 maven 的一些配置文件，如 setting.xml 文件

lib:存放了 maven 本身运行所需的一些 jar 包

至此我们的 maven 软件就可以使用了，前提是你的电脑上之前已经安装并配置好了 JDK。匹配环境变量

 配置环境变量

声明：配置环境变量目的只是为了能在任意目录下访问maven下的命令。其实不匹配也可以。

配置步骤和配置jdk步骤几乎一样，只是路径变了。

![image-20230218223746421](https://s2.loli.net/2023/02/18/ypvrTwmehDoU3Gs.png)

![image-20230218223045488](https://s2.loli.net/2023/02/18/1sWq5iXnDIG2M7A.png)

值得注意的是：maven是依赖于jdk的，所以jdk必须安装好。

export MAVEN_HOME=/Users/a0000/maven/apache-maven-3.9.8

export PATH=$PATH:$MAVEN_HOME/bin



![image-20230218223647367](https://s2.loli.net/2023/02/18/L8wKyJSBPUVkerG.png)

### 2. 4 Maven仓库

三类：

1. 本地仓库
2. 远程仓库
3. 中央仓库

![image-20230218224241276](https://s2.loli.net/2023/02/18/u9IxsXN2gJwiohP.png)

中央仓库地址：[mvn](http://mvnrepository.com/)

## 配置：

但以上就是作为了解，其实现实中我们只需要访问阿里云的镜像即可，阿里云就是个远程仓库 (私服)，我们都去访问阿里云。

我们本地仓库默认在setting.xml配置文件中会放到c盘很深的目录下，我们可以修改配置文件，把本地仓库放到自己专属的位置。

如下图：

![image-20230218224748676](https://s2.loli.net/2023/02/18/YMcbjKSN1Jsr4QW.png)

```xml
<localRepository>C:\maven_repository</localRepository>
```

上图意味着每次启动项目时候都会去自己的本地目录找需要的jar包。而不会直接去中央仓库找需要的jar包了。

如果本地仓库没有我们想要的jar包，那么本地仓库就会去中央仓库去找。

如下图就是阿里云的镜像地址，把如下代码粘贴早自己项目的setting文件中，即可。

```xml
<mirror>
    <id>alimaven</id>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    <mirrorOf>central</mirrorOf>        
</mirror>
```

# IDEA配置Maven

## 修改项⽬的默认配置

![image-20230218225917954](https://s2.loli.net/2023/02/18/2jtPTbuYsZ4amXE.png)

## 进⼊Maven配置⻚

改Maven⽬录以及settings⽂件位置

<img src="https://s2.loli.net/2023/02/18/lr6MAfQEV1YStgX.png" alt="image-20230218225959500" style="zoom: 33%;" />

修改为自己安装路径：

![image-20230218230233624](https://s2.loli.net/2023/02/18/l3oQk2diYCWS86K.png)

## IDEA**创建**Maven项⽬

### 第一步：

<img src="https://s2.loli.net/2023/02/18/DgcYtAN3Wdb5Eow.png" alt="image-20230218231223085" style="zoom:50%;" />

### 第二步：

填写项⽬名称、GroupId、ArtifactId以及Version，

GroupId通常为公司域名倒置，ArtifactId通常与项⽬名保持⼀致，Version为项⽬版本号，

此三项被称为Maven三⼤坐标，后⾯详细说。

<img src="https://s2.loli.net/2023/02/18/RZMG94Yv6g73rfd.png" alt="image-20230218231337359" style="zoom:50%;" />

### 第三步：

确认各项配置

![image-20230218231437643](https://s2.loli.net/2023/02/18/wKdPHZLC6EImQ8f.png)

### 第四步：

创建项⽬后需要⼀段时间⾃动下载Maven所需要的资源，BUILD SUCCESS代表创建完成。

<img src="https://s2.loli.net/2023/02/18/5HXFvVYyxn9Mdwr.png" alt="image-20230218233047545" style="zoom:50%;" />

### Maven工程目录结构

<img src="https://s2.loli.net/2023/02/19/NJgXpaTDutCPYO8.png" alt="image-20230219221505659" style="zoom:50%;" />

### 目录说明

```
src/main/java —— 存放项目的.java 文件

src/main/resources —— 存放项目资源文件，如 spring, hibernate，jdbc 配置文件

src/test/java —— 存放所有单元测试.java 文件，如 JUnit 测试类

src/test/resources —— 测试资源文件

pom.xml——maven 项目核心配置文件

注意：如果是普通的 java 项目，那么就没有 webapp 
```

中央仓库地址 ：

https://mvnrepository.com/

# Pom文件

Maven项⽬中最重要的是pom.xml⽂件，所谓pom意为Project Object Model，即项⽬对象模型。

Maven体现了⾯向对象的思想，将每个项⽬看作是⼀个对象，并⽤⼀个pom⽂件来具体描述。

每个项⽬有⾃⼰的属性，项⽬之间也会有⼀些关系，⽐如依赖、继承、聚合。

![image-20230219221746906](https://s2.loli.net/2023/02/19/gKImjOWwHv5MYk6.png)

## 项目三大坐标

在项⽬pom的属性中，有三个属性是最重要的，分别是groupId、artifactId以及version，它们统称为Maven三⼤坐标，

我们可以通过这三个属性来查找、定位到某个具体的项⽬。

1. groupId：Maven对项⽬是分组管理的，groupId通常为项⽬所属公司或组织的域名倒置，⽐如com.codingfuture。

2. artifactId：⽤来表示项⽬ID，即为项⽬名。maven识别的名字

3. version项⽬的版本号，可以为任意值，有两个常⽤的后缀⽅式需要了解：

   RELEASE：发⾏版、正式版

   SNAPSHOT：快照版、测试版

![image-20230219222147953](https://s2.loli.net/2023/02/19/jLgAuhpl2CxHXNk.png)

## 依赖

依赖是Maven中项⽬之间的⼀种关系，⽐如A项⽬的运⾏需要引⼊B项⽬，如果不使⽤Maven，

我们需要在A项⽬中引⼊B项⽬的jar包，⽽在Maven中，我们只需要在pom中配置项⽬A与项⽬B之间的依赖关系就好，

实际的引⼊操作由Maven来帮我们完成。

![image-20230219223553780](https://s2.loli.net/2023/02/19/6XEd1wcLR7TPASe.png)

发现c3p0还依赖于mchange包

![image-20230219223701372](https://s2.loli.net/2023/02/19/yPXY2WRQauq75wG.png)

### 查看依赖关系图:

![image-20230219223920264](https://s2.loli.net/2023/02/19/uxIZFU2mJQMpXio.png)

### 查看本地仓库：

![image-20230219224410336](https://s2.loli.net/2023/02/19/FmWDACcOGSaJVxY.png)

### 依赖的强大之处：

```xml
C(2.0) <- A -> B(1.0) -> C(1.0)
```

A项目依赖于C的2.0版本，同时依赖于B(1.0版本)，我们就需要在项目的pom.xml文件中写两个配置，C的2.0和**B**的1.0，

至于间接需要依赖的C的1.0，maven 就帮我们解决了。

如果没有maven，我们自己就需要在A项目中把其它三个jar包都倒入进来，这样会导致在A项目中有2个不同版本的C jar包，

这样会冲突有问题的，artifactId在pom文件中 只能有一个。不可以有多个。



`<scope></scope>`为依赖的作用域，具体可选值如下：

- compile

  **默认就是compile**，什么都不配置也就是意味着compile。compile表示被依赖项目需要参与当前项目的编译，当然后续的测试，运行周期也参与其中，是一个比较强的依赖。打包的时候通常需要包含进去。

- test

  scope为test表示依赖项目仅仅参与测试相关的工作，包括测试代码的编译，执行。比较典型的如junit。

- runtime

  runtime表示被依赖项目无需参与项目的编译，不过后期的测试和运行周期需要其参与。通常来说项目之间通过反射进行依赖则可以设置为runtime，最典型的就是mysql驱动。

- provided

  provided意味着打包的时候可以不用包进去，别的设施(Web Container)会提供。事实上该依赖理论上可以参与编译，测试，运行等周期。相当于compile，但是在打包阶段做了exclude的动作。

- system

  从参与度来说，也provided相同，不过被依赖项不会从maven仓库抓，而是从本地文件系统拿，一定需**要配合systemPath属性使用**。

# Maven



## 1、maven相关主要内容

* maven的简介
* maven的安装配置和目录结构
* idea编辑器集成Maven环境
* maven项目的创建
* maven仓库的基本概念
* maven环境下构建多模块项目
* maven的打包操作
* maven依赖的基本概念

## 2.Maven的简介

Maven是一个Java项目管理和构建工具，它可以定义项目结构、项目依赖，并使用统一的方式进行自动化构建，是Java项目不可缺少的工具。

### 2.1项目构建工具

**Ant构建**

**Maven（java）**

**Gradle**：Android御用的，结合了ant和Maven的特点，采取了DSL格式

### 2.2Maven的四大特性

* 依赖管理系统



## 3.Maven的结构目录

```textascii
a-maven-project
├── pom.xml
├── src
│   ├── main
│   │   ├── java
│   │   └── resources
│   └── test
│       ├── java
│       └── resources
└── target
```

解释一下：`a-maven-project`为项目名；`pom.xml`为项目描述文件,`ser/main/java` 为源码目录，`src/main/resources` 为资源目录，`src/test/java`存放测试源码目录，`src/test/resources`存放测试资源，最终所有的编译、打包生成文件都放在`target`目录里。这些就是一个maven项目标准的目录结构

`porm.xml `文件格式

```xml
<project ...>
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.itranswarp.learnjava</groupId>
	<artifactId>hello</artifactId>
	<version>1.0</version>
	<packaging>jar</packaging>
	<properties>
        ...
	</properties>
	<dependencies>
        <dependency>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
            <version>1.2</version>
        </dependency>
	</dependencies>
</project>
```

* `groupId`类似于Java的包名，通常是公司或组织名称，

* `artifactId`类似于Java的类名，通常是项目名称，

*  再加上`version`

**总结**：一个Maven工程就是由`groupId`，`artifactId`和`version`作为唯一标识。

一个标准的`dependency`标准

```xml
<dependency>
    <groupId>commons-logging</groupId>
    <artifactId>commons-logging</artifactId>
    <version>1.2</version>
</dependency>
```

## 4.安装Maven

* 1.在[官网](https://maven.apache.org/)下载对应的maven版本

* 2.配置环境变量 MAVEN_HOME ,path ![image-20220309165315578](img\image-20220309165315578.png)

  ![image-20220309165604693](img\image-20220309165604693.png)

* 3.命令行检测是否配置成功

> mvn -version

![image-20220309165806349](img\image-20220309165806349.png)

## 5.依赖管理

```ascii
┌──────────────┐
│Sample Project│
└──────────────┘
        │
        ▼
┌──────────────┐
│     abc      │
└──────────────┘
        │
        ▼
┌──────────────┐
│     xyz      │
└──────────────┘
```

**Q**：如上面所描述的，如果项目依赖了abc库，而`abc`库又依赖了`xyz`库，那么是否在项目中需要在申请`abc`库的同时又申请`xyz`库吗？

answer：在`maven`环境下不需要，当已经配置`abc`的依赖时，`xyz`会一起被依赖

### 5.1依赖关系

Maven定义了几种依赖关系，分别是`compile`、`test`、`runtime`和`provided`：

| scope    | 说明                                          | 示例            |
| :------- | :-------------------------------------------- | :-------------- |
| compile  | 编译时需要用到该jar包（默认）                 | commons-logging |
| test     | 编译Test时需要用到该jar包                     | junit           |
| runtime  | 编译时不需要，但运行时需要用到                | mysql           |
| provided | 编译时需要用到，但运行时由JDK或某个服务器提供 | servlet-api     |

`compile`是最常用的，Maven会把这种类型的依赖直接放入`classpath`中

`test`依赖表示仅在测试时使用，正常运行时并不需要

`runtime`依赖表示编译时不需要，但运行时需要

`provided`依赖表示编译时需要，但运行时不需要。

*Q*:maven从哪里下载所需依赖？

Answer：https://repo1.maven.org/ 这个是maven的中心仓库，然后将所有的第三发库和自身的jar库托管在中央仓库，本地仓库（前面配置的地址，若没有，就在用户目录的 .m目录下）只要下载一次，下次使用可直接使用本地仓库。

### 5.2唯一ID

那么是如何定位一个依赖的

- groupId：属于组织的名称，类似Java的包名；

- artifactId：该jar包自身的名称，类似Java的类名；

- version：该jar包的版本。

  Maven通过对jar包进行PGP签名确保任何一个jar包一经发布就无法修改。修改已发布jar包的唯一方法是发布一个新版本。

### 5.3镜像仓库

由于maven中央仓库在外网上，所以下载速度会很慢，我们还可以配置镜像仓库加速依赖构建

```ascii
          slow    ┌───────────────────┐
    ┌─────────────>│Maven Central Repo.│
    │              └───────────────────┘
    │                        │
    │                        │sync
    │                        ▼
┌───────┐  fast    ┌───────────────────┐
│ User  │─────────>│Maven Mirror Repo. │
└───────┘          └───────────────────┘
```

上面为配置镜像后的依赖路径

```xml
<settings>
    <mirrors>
        <mirror>
            <id>aliyun</id>
            <name>aliyun</name>
            <mirrorOf>central</mirrorOf>
            <!-- 国内推荐阿里云的Maven镜像 -->
            <url>https://maven.aliyun.com/repository/central</url>
        </mirror>
    </mirrors>
</settings>
```

上面为阿里云的镜像maven仓库

### 5.4 如何寻找第三方库

* 1.https://search.maven.org/ 好用就是每次都需要人机校验
* 2、https://u.tools/ utools 一个工具非常好用，加载maven插件很好用
* 其他工具

### 5.5 常用命令行

* 本地发布

  * > mvn clean install -DskipTests

* 平台发布

  * > mvn clean deploy -Dmaven.test.skip

* 命令行编译

  * > mvn clean package

## 6.maven构建流程

Maven的生命周期(lifecycle）由一系列阶段（phase）构成，以内置的生命周期`default`为例，它具备以下phase：

- validate
- initialize
- generate-sources
- process-sources
- generate-resources
- process-resources
- compile
- process-classes
- generate-test-sources
- process-test-sources
- generate-test-resources
- process-test-resources
- test-compile
- process-test-classes
- test
- prepare-package
- package
- pre-integration-test
- integration-test
- post-integration-test
- verify
- install
- deploy

例如，运行 `mvn package`,maven执行顺序如下

- validate
- ...
- package

运行`mvn compile`,maven执行顺序如下

- validate
- ...
- compile



maven另一个声明周期是`clean`

- pre-clean
- clean （注意这个clean不是lifecycle而是phase）
- post-clean



更高级的例子是同时使用多个phase，例如：`mvn clean package`;

* Maven先执行`clean`生命周期并运行到`clean`这个phase，然后执行`default`生命周期并运行到`package`这个phase，实际执行的phase如下：
  * pre-clean
  * clean （注意这个clean是phase）
  * validate
  * ...
  * package



### 6.1常用的命令

* `mvn clean`：清理所有生成的class和jar；
* `mvn clean compile`：先清理，再执行到`compile`；
* `mvn clean test`：先清理，再执行到`test`，因为执行`test`前必须执行`compile`，所以这里不必指定`compile`；
* `mvn clean package`：先清理，再执行到`package`。

### 6.2 Goal

执行一个phase又会触发一个或多个goal：

| 执行的Phase | 对应执行的Goal                     |
| :---------- | :--------------------------------- |
| compile     | compiler:compile                   |
| test        | compiler:testCompile surefire:test |

goal的命名总是`abc:xyz`这种形式。

**总结**

- lifecycle相当于Java的package，它包含一个或多个phase；
- phase相当于Java的class，它包含一个或多个goal；
- goal相当于class的method，它其实才是真正干活的。


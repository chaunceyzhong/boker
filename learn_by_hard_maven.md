---
title: Maven learn by hard
date: 2017-08-23 19:33:22
tags: 
	- maven
	- learn-by-hard
---
## 第1章 Maven简介
(略)
## 第2章 Maven安装
(略).

## 第3章 Maven使用
### 3.1 pom.xml 文件基本框架	
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.learn.mvn</groupId>
  <artifactId>mvn-newer</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  <scope>..</scope>
  <optional></optional>
  <name>mvn-newer Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <dependencies>
	<dependency>
  ...
	<exclusions>
		<exclusion>
	...
		</exclusion>
    </exclusions>
   </dependency>
 </dependencies>
</project>
```


### 3.2 Maven 常见命令解释
mvn jar:jar(第一个jar表示jar插件，第二个jar目标) 意思是jar插件的jar目标将代码打包成jar文件
mvn archetype:genertate(使用archetype插件**快速**生成WEB项目骨架)
mvn resources:resources
......

### 3.3 Maven项目约定
在项目根目录放置pom.xml文件，在src/main/java目录放置项目主代码,在src/test/java放置项目测试代码。

## 第4章 Maven坐标与依赖

### 4.1 Maven坐标元素:
1. groupId(通常与组织域名对应)
2. artifactId(实际项目中的一个Maven项目(模块))
3. version
4. packaging(打包方式)
5. classfier(定义附属构件 eg：*-javadoc.jar，*-sources.jar).

### 4.2 依赖范围
1. compile ：编译范围依赖（默认）。 对编译、测试、运行三种classpath有效。eg: spring-core
2. test    ：测试范围依赖。只对测试classpath有效。 eg：Junit
3. provided：已提供依赖范围。对编译、测试classpath有效，运行时无效。eg：servlet.api（容器已提供）
4. runtime ：运行时依赖范围。对测试、运行classpath有效，但运行时无效。eg：JDBC驱动实现(编译时候只需要JDK提供JDBC接口，测试、运行才会具体现).
5. system
6. import


### 4.3 传递依赖
1. （没指定scope,默认为compile）假设项目A->项目B，项目B->项目C,则项目A->项目C.
依赖范围与classpath关系
|         | compile | test    | provided | runtime |
| :------ | ------  | --------|----------|
| compile | compile |   -     | provided | runtime | 
| test    | test    |   -     |    -     | runtime |
| provided| provided|   -     |    -     | runtime |
| runtime | runtime |

### 4.4 依赖调节
1. 路径最近优先(假设有2条依赖路径:1.A->B->C(1.0) 2.A->C(2.0) 最终会选择C(2.0)).
2. 第一声明者优先(假设有2条先后依赖路径:1.A->C(1.0) 2.A->C(2.0) 最后会选择C(1.0)).

### 可选依赖
1. 定义 A->B,B->X(optional),B->Y(optional),则X,Y对A没有任何影响.
2. 背景:可能B需要实现2中不同特性，例如X实现Mysql驱动，Y实现PGSQL驱动.因此，如果A要是基于Mysql的数据库，在项目A需要显式说明mysql-connector-java的依赖。
3. 理想情况不使用可选依赖。更好的做法是为基于Mysql、PGSQL项目分别建一个Maven项目，根据传递依赖性，因此在项目A就不需要显式说明mysql-connector-java的依赖。

### 最佳实践
1. 排除依赖。很多隐式依赖会引进不稳定版本的包，因此要排除不稳定版本的包，然后重新指定稳定版本的。
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
 ...
  <dependencies>
	<dependency>
  	<groupId>com.learn</groupId>
	<artifactId>project-A</artifactId>
    <version>1.0</version>
 	<exclusions>
		<exclusion>
		<groupId>com.learn</groupId>
		<artifactId>project-B</artifactId>
		</exclusion>
  	</exclusions>
   </dependency>
   <!--重新指定project-B版本-->
	<dependency>
		<groupId>com.learn</groupId>
		<artifactId>project-B</artifactId>
		<version>1.1.0<version>
	</dependency>
 </dependencies>
</project>
```

2. 归类依赖:即把版本号定义成一个常量，方便管理.（略）



（待续）...
### 第6章 仓库
### 第7章 生命周期和插件
### 第8章 聚合和继承





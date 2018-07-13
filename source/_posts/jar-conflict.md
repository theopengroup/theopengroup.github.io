title: Jar包冲突治理
date: 2017-06-05 20:56:34
categories: java
tags:
---
> 近来，多个项目出现上线后Jar包冲突导致上线失败回滚情况，而在开发、测试过程中都没有提前发现，因此需要提出一种简单、可靠的方案来规避这种情况。

<!--more-->
## 现象
错误日志出现nested exception is Java.lang.NoSuchMethodError

## 问题排查
在程序启动的脚本中加入JVM启动参数 -verbose:class，然后重启应用。在启动日志中可以看到加载的类来自哪个包。

例如：[Loaded org.apache.velocity.runtime.parser.node.ASTTrue from file:/home/admin/app/.default/deploy/app.war/WEB-INF/lib/velocity-1.6.4.jar]

如果在启动日志中没有发现加载冲突的类，执行触发异常的操作，这时就有加载类的信息了。最后，查看加载的类所在的包，与正确的包比较。

## 问题分析
Jar包冲突主要包括以下两类：

1、相同Jar包不同版本导致冲突

>Maven管理依赖遵循两条原则，最短路径优先原则和首先声明原则。假设 A->B->C->D1, E->F->D2, G->H->D3，其中D1,D2,D3分别为D的不同版本。如果pom.xml文件中引入了A、E和G之后，先按最短路径优先原则保留D2和D3，再按首先声明原则，最终保留D2。如果此时开发期望的是D1，而D1和D2在接口实现上有所差别，则会出现不符合预期的问题。
>
>由于规则是固定的，线下环境可以复现线上问题，然而如果测试点覆盖的地方恰好没有调用到D，则上线后会出现失败。

2、相同ClassPath下的同名类出现在不同Jar包中

>Classloader查找class文件的过程取决于文件系统返回的顺序（linux文件系统inode的顺序）。如果相同ClassPath下的同名类出现在不同Jar包，一旦Classloader先加载的文件不是我们期望的，则基本一定会发生异常。
>
>由于文件系统返回文件的顺序在线上、线下环境很可能不一样（操作系统、文件系统、JVM等因素），因此线下很难发现问题，只有到线上排查，为时已晚。

## 解决方案
1、在pom中增加冲突检测插件，在build阶段发现问题，消灭问题。

```xml
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-enforcer-plugin</artifactId>
	<version>1.4.1</version>
	<executions>
		<execution>
			<id>enforce</id>
			<configuration>
				<rules>
					<dependencyConvergence/>
				</rules>
			</configuration>
			<goals>
				<goal>enforce</goal>
			</goals>
		</execution>
		<execution>
			<id>enforce-ban-duplicate-classes</id>
			<goals>
				<goal>enforce</goal>
			</goals>
			<configuration>
				<rules>
					<banDuplicateClasses>
						<ignoreClasses>
							<ignoreClass>javax.*</ignoreClass>
							<ignoreClass>org.junit.*</ignoreClass>
						</ignoreClasses>
						<findAllDuplicates>true</findAllDuplicates>
					</banDuplicateClasses>
				</rules>
				<fail>true</fail>
			</configuration>
		</execution>
	</executions>
	<dependencies>
		<dependency>
			<groupId>org.codehaus.mojo</groupId>
			<artifactId>extra-enforcer-rules</artifactId>
			<version>1.0-beta-6</version>
		</dependency>
	</dependencies>
</plugin>
```

2、优雅管理依赖版本对于 A->B->C->D1, E->F->D2, G->H->D3，其中D1,D2,D3分别为D的不同版本这种情况，通常的做法是用exclusion标签排除不需要的jar包，然而这种做法的后果是每次引入新的jar包都可能传递依赖到导致冲突的jar包，虽然冲突检测插件可以检测出，但需要开发者逐一排除，非常麻烦。

推荐的做法是在parent pom文件中通过dependencyManagement来进行版本管理，一劳永逸。例如，如果开发时确定使用poi 3.9版本，则配置如下：

```xml
<dependencyManagement>
	<dependency>
		<groupId>org.apache.poi</groupId>
		<artifactId>poi</artifactId>
		<version>${poi.version}</version>
	</dependency>
</dependencyManagement>
<properties>
	<poi.version>3.9</poi.version>
</properties>
```
title: 解决windows下phoenix查询出现的PhoenixIOException
date: 2016-03-21 11:32:22
categories: 工作
tags:
---
> 问题描述：当hbase数据量较大时，在windows下用phoenix查询出现异常问题(org.apache.phoenix.exception.PhoenixIOException 系统找不到指定的路径)

<!--more-->
## 问题分析
当通过phoenix方式查询hbase数据时，hbase会通过多线程的方式并发查询结果，并由客户端进行合并。数据量增大会导致内存的需求增加，内存不足时会写入磁盘临时文件。
phoenix-core 4.6.0版本下磁盘的临时文件路径默认为/tmp目录，如下图所示，导致了在windows下找不到该目录，进而出现PhoenixIOException。

![图1](http://7xnvrl.com1.z0.glb.clouddn.com/DEFAULT_SPOOL_DIRECTORY.png)

**在phoenix-core 4.7.0中已修复该问题，路径由java.io.tmpdir环境变量决定。**

![图2](http://7xnvrl.com1.z0.glb.clouddn.com/phoenix-core4.7.0.png)

## 解决方法
1、（推荐策略）在resin或tomcat所处的盘符（分区）的根路径下创建tmp文件夹，或者暴力地在每个分区的根路径下创建tmp文件夹！

2、（中等推荐）增加JVM的-Xmx配置，或增加hbase-site.xml中的maxGlobalMemoryPercentage，减少spoolThresholdBytes。麻烦！有时候不一定奏效！
>Xmx设置的大小=50*spoolThresholdBytes(兆)/(maxGlobalMemoryPercentage/100) 

3、升级phoenix-core版本

4、用反射的方法修改QueryServicesOptions中的static final DEFAULT_SPOOL_DIRECTORY的值，傻子才会这么做！不可行！

## 问题结论
Linux爽！Windows Low！不服？找[他](http://veryyoung.me/)！

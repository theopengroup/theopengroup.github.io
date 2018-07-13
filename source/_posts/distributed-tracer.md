title: 分布式追踪系统行业对比
date: 2017-01-15 22:23:11
categories: java
tags:
---
> 分布式追踪系统在微服务架构中有着非常重要的地位。

<!--more-->
## ZipKin
Zipkin是由Twitter 公司开发贡献的开源的分布式实时数据追踪系统，应用较为广泛。与之配合使用的另一个重要开源组件是Brave，提供了面向 Standard Servlet、Spring MVC、Http Client、JAXRS、Jersey、Resteasy 和 MySQL 等接口的跟踪信息埋点，可以通过编写简单的配置和代码，让基于这些框架构建的应用可以向 Zipkin报告数据。

Zipkin目前只是展示调用链，并没有做进一步的统计分析，功能相对比较原生，其优势在于通用性强，积极跟进OpenTracing的标准，可以与spring cloud方便地集成。

## 鹰眼
鹰眼是由淘宝于2013年上线的分布式跟踪系统，是目前业内功能最完善的跟踪系统，不仅接入了大部分中间件（前端请求接入，分布式session，远程服务框架，异步消息通讯, 分库分表，分布式缓存，分布式文件系统等等），也提供了路由分布、QPS、调用次数、服务依赖度、平均耗时等统计分析功能，为后续的系统优化提供参考。

![图1](http://7xnvrl.com1.z0.glb.clouddn.com/yingyan.png)
![图2](http://7xnvrl.com1.z0.glb.clouddn.com/yingyan2.png)

目前，国内其它公司的跟踪系统如下（超链接是对应的详细介绍）：


>京东的Hydra：与dubbo框架集成，开源，不维护；
>
>唯品会的[Microscope](http://weibo.com/ttarticle/p/show?id=2309404006474413149166)：规划合理的一整套监控方案；
>
>窝窝网的[Tracing](http://zhengyun-ustc.iteye.com/blog/2167052)：系统中文名也叫鹰眼；
>
>新浪的[Watchman](http://www.infoq.com/cn/articles/weibo-watchman)：无特色；
>
>美团的[MTrace](http://tech.meituan.com/mt-mtrace.html)：功能比较完善，未来有望开源；
>
>点评的[Cat](http://tech.meituan.com/CAT_in_Depth_Java_Application_Monitoring.html)：参考eBay的CAL，部分开源，几乎不维护；


它们技术上各有千秋，功能上都没有做更多的创新。我们的跟踪系统在统计分析的功能上也有在借鉴鹰眼，但目前统计分析功能的利用率不高，个人觉得需要根据统计的结果推进服务质量的提升，才能提高统计功能的利用率。

## SKY-WALKING
SKY-WALKING是一个由国人开发者开源的全链路监控追踪系统，目前维护较为活跃，支持很不错，点赞！他也是积极参与了opentracing标准的制定，文档翻译地址，也看到他推进其它开源组件遵从这个标准。下图是weibo开源的motan rpc框架。
![图3](http://7xnvrl.com1.z0.glb.clouddn.com/skywalking.png)

下图是sky-walking的进展情况，可借鉴有意义的功能是“邮件报警”以及“通过log4j扩展在日志中快速展现trace id”。

目前我们的跟踪系统使用率较低，通过跟踪系统的调用链只能看到来源、去向和时间等信息，如果想进一步调试问题，需要去翻日志，如果此时日志里带有traceid，可以更快地把相关日志都找到。
![图4](http://7xnvrl.com1.z0.glb.clouddn.com/skywalking2.png)

同样，目前只有在服务向调用者抛异常时，邮件报警的日志内容会带有trace id，在业务中打的error日志并没有trace id，因此也降低了跟踪系统的使用率。下图是sky-walking的报警截图。
![图5](http://7xnvrl.com1.z0.glb.clouddn.com/skywalking3.png)
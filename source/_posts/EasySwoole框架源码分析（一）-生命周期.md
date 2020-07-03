---
title: EasySwoole框架源码分析（一）-生命周期
date: 2020-07-02 17:10:44
tags:
    - PHP
    - Swoole
---

[Swoole](https://www.swoole.com)近期的流行趋势不亚于一门语言，比如说Go，针对于目前的大环境可以说是大势所趋，基于以后应用的角度选取[EasySwoole](https://www.easyswoole.com)框架进行源码分析。

首先简单说下Swoole是什么，它的优势和特性在哪？

先看下Swoole的官方解释，
>Swoole使PHP开发人员可以编写高性能高并发的TCP、UDP、Unix Socket、HTTP、WebSocket等服务，让PHP不再局限于Web领域。Swoole4协程的成熟将PHP带入了前所未有的时期，为性能的提升提供了独一无二的可能性。Swoole可以广泛应用于互联网、移动通信、云计算、网络游戏、物联网（IOT）、车联网、智能家居等领域。使用PHP + Swoole可以使企业IT研发团队的效率大大提升，更加专注于开发创新产品。

这里我们总结出几个关键词，高性能、高并发、多服务、协程机制、广泛应用领域，这些方面与PHP高效的开发效率结合就不难看出它所做的事情和优势了。

随后再从[Swoole官方文档](https://wiki.swoole.com)角度来看Swoole，虽然是标准的PHP扩展，但实际上又与普通扩展只提供一个库函数不同（这就有了Swoole既是扩展又是框架的说法），它所做的是接管PHP的控制权，进入事件循环，当IO事件发生后底层会自动回调指定的PHP函数。

最后从文档总结Swoole有以下几个特性：
1. 常驻内存，避免重复加载带来的性能损耗，提升海量性能。。
2. 协程异步，提高对I/O密集型场景并发处理能力；
3. 内置服务，方便地开发Http、[WebSocket](https://www.ruanyifeng.com/blog/2017/05/websocket.html)、TCP、UDP等应用，可以与硬件通信；

以上就是Swoole的简单介绍，因为工作和兴趣的关系，选取基于Swoole Server开发的常驻内存型的分布式PHP框架EasySwoole，进行源码分析学习。

##### 从生命周期开始

![life-cycle](/images/lifecycle.png "生命周期")
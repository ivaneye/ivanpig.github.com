---
layout: post
title: zookeeper入门指南
categories: zookeeper
tags: [java,zookeeper]
avatarimg: "/img/head.jpg"
published: false

---

本文翻译自[ZooKeeper Getting Started Guide](http://zookeeper.apache.org/doc/trunk/zookeeperStarted.html)

# 入门：使用ZooKeeper协调分布式应用

本文是ZooKeeper的快速入门。主要包括节点的ZooKeeper服务安装，一些操作命令和一个简单的例子程序。最后
 Finally, as a convenience, there are a few sections 
regarding more complicated installations, for example running replicated deployments, 
and optimizing the transaction log. However for the complete instructions for commercial deployments, 
please refer to the ZooKeeper Administrator's Guide.
---
layout: post
title: 一致性协议
categories: [consensus]
tags: [consensus,paxos,zookeeper]
avatarimg: "/img/head.jpg"
published: false

---

# 简介

一致性协议主要是为了解决分布式环境下的数据一致性问题。

对于单节点应用来说，数据的同步相对较简单。例如数据库的事务，多线程中的锁，都是为了保证数据的一致性。

而对于分布式环境来说，没有完善的强一致性解决方案。

在2000年，加州大学伯克利分校Eric Brewer教授提出了CAP猜想(后被证明可行)：一个分布式系统不可能同时满足一致性(C:Consistency),
可用性(A:Abailability)和分区容错性(P:Partition tolerance)这三个基本需求，最多只能同时满足其中的两项。

后eBay架构师Dan Pritchett提出了BASE理论，即Basically Available(基本可用),Soft state(软状态)和Eventually consisent(最终一致性)。
其核心思想即无法做到强一致性，但系统可以根据自身做到最终一致性。

故对于分布式系统设计，依照保证可用性和分区容错性的基础上去实现最终一致性。

而对于最终一致性处理，比较著名的协议和算法便是：二阶段提交协议(2PC)，三阶段提交协议(3PC)以及Paxos算法。

# 2PC

二阶段提交主要是引入了协调者来协调各个节点。





























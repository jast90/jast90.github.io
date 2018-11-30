---
layout: bootstrap
title: 'Java HashMap涉及的数据结构及实现'
description: 'Data structure of Java HashMap'
date: '2018-11-30 16:40:00'
author: Jast
---
# 提供的功能
- 基于哈希表实现的Map;
- 非线程安全的Map实现；
- 键和值都可以为null(因为有处理null的情形)；
- 基本操作`get()`和`put()`的时间消耗是固定的；
- 数据存储结构会随着HashMap的数量而变换成不同的数据结构。

# 涉及到的概念
- 默认初始化容量
- 最大容量
- 默认的负载系数（load factor）
- 树形化的阈（yù）值
- 非树形化的阈值
- 最小树形化的容量


# 涉及的基本概念
- 单链表
- 红-黑树
- 链表、红-黑树互转

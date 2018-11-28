---
layout: bootstrap
title: 'Java Hashtable 源码分析'
description: 'Think in Java Hashtable'
date: '2018-11-28 11:19:00'
author: Jast
---
# Hashtable 提供的功能
- Hashtable是一个线程安全的Map，其线程安全是通过在各个方法上加上synchronized关键字实现的，即：该类只能被一个线程所使用，其他调用该类时会阻塞等待;
- 实现了哈希表，映射key到value；
- key和value都不能为null，key类型必须实现hashCode()和equals()方法；
- put(K k,V v); 
- get(K k,V v);
- Hashtable没有实现hash冲突的解决方案，冲突需要按自己的逻辑实现，它只提供了哈希表自动扩容的功能。
# Hashtable 涉及到的概念
- 初始化容量：key数组初始长度，默认值是11
- 负载系数（load factor）：即到达容量的百分比时，哈希表就会重新hash到一个新容量的哈希表中,取值范围是：<1.0 的百分数 默认取值是.75(75%，当加入的键值对数量达到初始容量的75%时，继续加入的话会重新hash到一个新的容量的哈希表中)
- 阈(yù)值：threshold=初始化容量*0.75和Integer.MAX_VALUE-8（注：Integer.MAX_VALUE=0x7FFFFFFF）
	中较小值
- 重新Hash：加入元素时，当数组大小大于等于阈值时，key数组的容量会左移2位后+1即：原容量*4+1，然后按新的容量hash后放入新的数组中。
- 哈希函数：(key.hashCode() & 0x7FFFFFFF) % key数组现在的长度

# Hashtable常用方法分析
- put(K k,V v);
	添加元素到哈希表时，先判断是否存在哈希冲突，如果存在的话，就会原来的元素覆盖掉，并返回原来的值,如果不存在的话，就会直接新建一个元素。
- get(K k);
	获取元素时，先根据key的hash获取到对应key数组的下标，获取对应的元素。

# 涉及的基本概念
- 位运算
- 数组
- 哈希表
- Java transient 关键字原理
- Java 序列化、反序列化以及自定义序列化、反序列话逻辑（writeObject(ObjectOutputStream o)和readObject(ObjectInputStream i)）

# 存在未理解之处
	Hashtable的count字段是什么时候初始化的？从赋值来看是通过readObject()方法来时先的。但是具体实现需要回去取研读下《Java编程思想》序列化章节内容
	```
	private transient int count;
	```
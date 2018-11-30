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
- key和value都不能为null（因为需要调用hashCode()方法，如果为null的话就会抛出NPE），key类型必须实现hashCode()和equals()方法；
- put(K k,V v); 
- get(K k,V v);
- ~~Hashtable没有实现hash冲突的解决方案，冲突需要按自己的逻辑实现，它只提供了哈希表自动扩容的功能；~~
- 出现hash冲突时，采用单向链表来储存冲突的元素。

# Hashtable 涉及到的概念
- 初始化容量：key数组初始长度，默认值是11
- 负载系数（load factor）：即到达容量的百分比时，哈希表就会重新hash到一个新容量的哈希表中,取值范围是：<1.0 的百分数 默认取值是.75(75%，当加入的键值对数量达到初始容量的75%时，继续加入的话会重新hash到一个新的容量的哈希表中)
- 阈(yù)值：threshold=初始化容量*0.75和Integer.MAX_VALUE-8（注：Integer.MAX_VALUE=0x7FFFFFFF）
	中较小值
- 重新Hash：加入元素时，当数组大小大于等于阈值时，key数组的容量会左移2位后+1即：原容量*4+1，然后按新的容量hash后放入新的数组中。
- 哈希函数：(key.hashCode() & 0x7FFFFFFF) % key数组现在的长度

# Hashtable常用方法分析
- put(K k,V v);
	添加元素到哈希表时，判断元素是否存在，如果存在（key的hash相同并且值也相同）的话，就会覆盖掉原来的元素，并返回原来的值；如果不存在的话，就会直接新建一个元素，若产生hash冲突则将旧元素链接到新元素的尾部。
- get(K k);
	获取元素时，先根据key的hash获取到对应key数组的下标，获取对应的元素（因为数组元素的值是一个单链表，所以如果当前的值不匹配时就需要判断链表的下一个元素）。

# 处理 Hash冲突
Hashtable 处理 Hash冲突 时通过单链表。。

# 涉及的基本概念
- 位运算
- 数组
- 哈希表
- 单链表
- Java transient 关键字原理
- Java 序列化、反序列化以及自定义序列化、反序列话逻辑（writeObject(ObjectOutputStream o)和readObject(ObjectInputStream i)）

# 存在未理解之处
Hashtable的count字段是什么时候初始化的？从赋值来看是通过readObject()方法来实现的。但是具体实现需要回去取研读下《Java编程思想》序列化章节内容。

	private transient int count;

# 未理解之处的解答
- Hashtable的count字段是什么时候初始化的？因为该变量是类变量编译的时候会自动赋值。可以总结下Java各种类型的变量。

# 模拟hash冲突
可以根据hash函数`(key.hashCode() & 0x7FFFFFFF) % key数组现在的长度`来模拟hash冲突，只要key的hashCode是相同但是又不equals()的就是Hash冲突。如下实例：

	public class MyKey implements Serializable {
	    private int i;

	    public MyKey(int i) {
	        this.i = i;
	    }

	    @Override
	    public int hashCode() {
	        if (i % 2 == 0) {
	            return 1;
	        } else {
	            return 2;
	        }
	    }

	}

# Hashtable处理hash冲突
如下实例：

	public static void main(String[] args) {
        Hashtable<MyKey, Integer> map = new Hashtable<>();
        for (int i = 0; i < 11; i++) {
            map.put(new MyKey(i), i);
        }
        map.get(new MyKey(1));
    }

# 如何在IDEA调试模式下查看储存结构？
Hashtable在IDEA下的默认视图：

![Java-Hashtable-data-structure-default-view](https://raw.githubusercontent.com/jast90/jast90.github.io/master/img/Java-Hashtable-data-structure-default-view.png)

如何查看对象视图？如下图操作：

![Java-Hashtable-data-structure](https://raw.githubusercontent.com/jast90/jast90.github.io/master//img/Java-Hashtable-data-structure.png)

对象视图如下

![Java-Hashtable-data-structure-object-view](https://raw.githubusercontent.com/jast90/jast90.github.io/master//img/Java-Hashtable-data-structure-object-view.png)

从如上对象视图可以看出Hashtable的table字段的具体存储方式：
table数组中有两个元素，一个是`MyKey.i=10`，一个是`Mykey.i=9`,按如上查看对象视图的方法查看这两个元素的对象视图查看`java.util.Hashtable.Entry#next`属性，可以看到`MyKey.i=10`的next属性值是`MyKey.i=8`...
各元素的具体结构如下：

	MyKey.i=10.next -> MyKey.i=8
	MyKey.i=8.next -> MyKey.i=0
	MyKey.i=0.next -> MyKey.i=2
	MyKey.i=2.next -> MyKey.i=4
	MyKey.i=4.next -> MyKey.i=6
	MyKey.i=6.next -> null


	MyKey.i=9.next -> MyKey.i=1
	MyKey.i=1.next -> MyKey.i=3
	MyKey.i=3.next -> MyKey.i=5
	MyKey.i=5.next -> MyKey.i=7
	MyKey.i=7.next -> null

为什么顺序不是按加入的顺序的呢，而是一部分到过来的？  
因为Hashtable的key数组默认大小是11，当加入11个元素时，会自动扩容，在加入第8个元素时会rehash一次，rehash时是将新哈希表中的元素作为后面加入元素的next的，所有就会出现部分元素顺序相反。
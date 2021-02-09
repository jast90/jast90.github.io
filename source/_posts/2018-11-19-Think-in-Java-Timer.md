---
layout: post
title: 'Java Timer 源码分析'
description: 'Think in Java Timer'
date: '2018-11-19 16:54:00'
author: Jast
---
# 1  Java Timer实现功能、原理分析
## 1.1 功能
- 延时、单次执行任务（java.util.Timer#schedule(java.util.TimerTask, long)，单次执行，周期传0）
- 指定时间、单次执行任务（java.util.Timer#schedule(java.util.TimerTask, java.util.Date)）
- 延时、周期性执行任务（java.util.Timer#schedule(java.util.TimerTask, long, long)）
- 指定时间、周期性执行任务（java.util.Timer#schedule(java.util.TimerTask, java.util.Date, long)）
- 延时、固定频率周期性执行任务（java.util.Timer#scheduleAtFixedRate(java.util.TimerTask, long, long)，周期的取值为正数）
- 指定时间、周期性执行任务（java.util.Timer#scheduleAtFixedRate(java.util.TimerTask, java.util.Date, long)，周期的取值为正数）
- 通用执行任务方法（根据传入周期来确定定时器任务的执行时间；如果传入的周期是负数的话，是相对于系统当前时间的，非负数的话是相对于任务的执行时间的即固定频率是相对于执行时间的）

### 1.1.1 周期性执行任务和固定频率周期性执行任务的区别
周期性执行任务是要考虑任务的执行时间的(任务的下一个执行时间是用调度时系统时间-)；  
固定频率周期性执行任务是不考虑任务执行时间的；  
可以参考`java.util.TaskQueue#rescheduleMin()`方法调用的地方，如下：
```
queue.rescheduleMin(
	task.period<0 ? currentTime   - task.period
		: executionTime + task.period);
```

## 1.2 涉及到的类
### 1.2.1 主要类
- java.util.Timer  
	初始化定时任务存放队列；  
	初始化定时调度线程并启动；  
	安排（计划）定时任务（安排完后需要唤醒定时调度线程，运用了生产者-消费者模式）；  
	维护任务的状态。
- java.util.TimerTask  
	同步锁；  
	任务状态：未使用的、已调度、已执行、已取消；  
	任务执行间隔时间；  
	定义具体的执行的任务；  
	取消该任务。  
### 1.2.2 辅助类
- java.util.TimerThread  
	定时调度线程，通过死循环及生产者消费者模式实现，当任务队列为空时跳出死循环，用于消费定时器生产出的定时任务，并维护定时任务执行、修改下次执行时间、移除。
- java.util.TaskQueue  
	任务队列，底层是数组实现的，当数组数量达到数组长度数-1时继续添加任务会使用数组复制功能来复制到一个长度是现在2倍的数组中；  
	提供获取执行时间最小的任务和执行时间第n小的功能；  
	提供移除执行时间最小的任务和执行时间第n小的功能；  
	数组中个任务往前移动的功能；  
	设置执行时间最小任务下次执行时间；  
	清空队列任务。

## 1.3 涉及到的概念
### 1.3.1 数据结构
- 动态数据
- 队列
### 1.3.2 操作系统概念
- 生产者-消费者问题
### 1.3.3 Java 并发机制
- 锁机制


## 任务执行所需时间、调度时间和下个执行时间区分

---
layout: post
title: '堆排序'
description: '堆排序'
date: '2020-05-07 17:32:00'
author: Jast
---

#### 参考

- [堆排序](https://www.bilibili.com/video/BV1Eb41147dK)

#### 堆的概念

1. 完全二叉树
2. 父节点`>`或`<`子节点

#### 分析

1. 计算第n个节点的父节点
2. 计算第n个节点的子节点
3. 将某个节点堆化：将其自己堆化，并且将交换位置的子节点也进行堆化
4. 将一个数组构建成堆：从 最后一颗子树父节点`((n-1)/2)` 往上依次将各节点堆化
5. 堆化后的数组打印顺序是：从上往下，从左往右
6. 堆排序：将堆顶元素和最底层的最后一个叶子节点交换并将交换的的最底层的叶子节点移除（并非真正的移除），然后将对顶元素堆化，重复步骤6

#### 方法拆分

1. 将某个节点堆化
2. 元素交换
3. 将数组构建成堆
4. 堆排序

#### 实现

```java
package cn.jast.sort;

import java.util.Arrays;

public class Heap {
    /**
     * 交换元素
     */
    public void swap(int a[] , int i , int j){
        int temp = a[i];
        a[i] = a[j];
        a[j] = temp;
    }

    //将某个节点堆化：将其自己堆化，并且将交换位置的子节点也进行堆化确保交换后还是一个堆
    // a：数组； n: 数组长度；i：数组中第i个元素，待堆化的元素
    public void heapify(int a[] , int n , int i){
        //递归出口
        if(i >= n){
            return ;
        }
        int c1 = 2 * i + 1;//左子节点，确保存在（不超过数组长度）
        int c2 = 2 * i + 2;//右子节点，确保存在（不超过数组长度）
        int max = i;
        if(c1 < n && a[c1] > a[max]){
            max = c1;
        }
        if(c2 < n && a[c2] > a[max]){
            max = c2;
        }
        if(max != i){
            swap(a,i,max);
            heapify(a,n,max);
        }
    }

    // 将数组构建成堆
    // a :待构建的数组，n：数组的长度
    public void build_heap(int a[] , int n){
        int last_node = n-1;
        int last_node_parent = (last_node-1)/2;
        for(int i = last_node_parent ; i >= 0 ; i--){
            heapify(a,n,i);
        }
    }

    // 堆排序
    public void heap_sort(int a[],int n){
        //数组堆化
        build_heap(a,n);
        //交换最后一个元素
        for(int i = n-1 ; i>=0 ; i--){
            //交换第0和最后一个元素
            swap(a,0,i);
            //对堆顶元素堆化
            heapify(a,i,0);
        }
    }

    public static void main(String[] args) {
        Heap heap = new Heap();
        int[] a = {2,1,3,4,6,7,5,9,8};
        heap.heap_sort(a,a.length);
        System.out.println(Arrays.toString(a));
    }
}
```

---
title: Interview Java
date: 2022-04-28 12:23:34
tags: Computer Science
---
1. ConcurrentHashMap原理：HashMap是线程不安全的，ConcurrentHashMap采用分段锁技术将数据分段存储，每段分配一个锁。
   1. 存储结构：
      1.  Segments数组：数组的每个元素是一个segment数组——存储元素，segment内部存在竟态条件，同时访问一个segment的时候才会抢占锁；
      2.  一个Segment相当于一个小的hashmap，里边的table数组继承自ReentrantLock；
      3.  JDK1.8之后，取消了segments字段，直接采用transient HashEntry<K,V>[] table保存数据，采用table数组元素作为锁，从而实现对每一行数据加锁，并发控制使用Synchronized和CAS加锁；
    1. 初始化：
       1. 除了第一行的数组，其他数组都在添加第一元素时才被初始化；
       2. 第一次添加元素时初始长度为16；
       3. 当添加位置冲突达到8时，如果数组长度小于64就扩容，否则链表转化为红黑树；
       4. 每个数组的负载因子为0.75；
       5. 

2. HashTable也是线程安全的，get/put等操作是synchronized，实现效率较低；
3. 

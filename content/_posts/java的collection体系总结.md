---
title: "Java 的 Collection 体系总结"
date: 2020-02-04T13:59:17+08:00
draft: false
---

Collection 是一个接口，由它衍生出的类在 Java 中用途非常广。下图就是关于 Collection 的继承体系。（图片可能加载很慢）

<!--more-->

![](/imgs/collection.png)

## List

List 下的 ArrayList、LinkedList、Vector 相当于增强了的「数组」。其中 ArrayList、Vector 是使用数组实现的，支持常量时间的查找操作；LinkedList 是用链表实现的，在指定位置插入、删除操作很高效。

由于 LinkedList 使用链表实现，所以不用考虑容量问题，但是 ArrayList 却不同，它是用数组实现的，需要支持动态增加或减少容量。

那它动态变化容量是如何实现的呢？以动态扩容为例。ArrayList 会在当前容量满了的时候，新建一个 1.5 倍于当前数组大小的临时数组，并把之前的数据一个个拷贝过去，拷贝完成后舍弃旧的数组，而使用临时数组作为新的数组。

Vector 和 ArrayList 在大多数情况下是类似的，不过，在扩容的时候它会选择变为当前数组规模的 2 倍。更多不同可以点击这个文章：[Difference between Vector and ArrayList in java?](https://javapapers.com/core-java/java-collection/difference-between-vector-and-arraylist-in-java/)

## Queue

Queue 是一种基础的数据结构，一般来讲，它最基本的操作是先入先出操作。

优先队列(PriorityQueue)是一种用二叉树存储的数据结构，我们能利用它快速取到队列中最大或最小的元素，如果我们需要实现一个热搜榜，它是在合适不过的一个数据结构了。

Deque 有丰富的 API，即可以单独作为一个栈，也可以作为一个队列来使用。Java 的文档推荐使用 Deque 来代替 Vector 下 的 Stack

## Set

Set 最显著的特征是不允许两个相同的元素出现。它很适合查找元素。

在 Set 下实现了 三个类：HashSet、LinkedHashSet、TreeSet。

就表象而言，TreeSet 内部的元素总是有序的，LinkedHashSet 保持元素和插入的顺序一致，HashSet 并不保证任何有序性。

以上就是对 Collection 体系的简单分析。

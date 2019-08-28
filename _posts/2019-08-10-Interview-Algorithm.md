---
layout:     post         
title:      "Algorithm"
date:       2019-08-10 12:00:00             # 时间
author:     "Alex"                          # 作者
header-img: "img/animate-bg/bg1-min.jpg"
catalog: true                               # 是否归档
tags:                                       # 标签，可多个
    - Interview
---

## 树

#### [二叉树](https://blog.csdn.net/google19890102/article/details/53926704)

[前序、中序、后序遍历](https://blog.csdn.net/m0_37698652/article/details/79218014)

二叉查找树（BST）:

* 左子树上所有结点的值均小于或等于它的根结点的值。
* 右子树上所有结点的值均大于或等于它的根结点的值。
* 左、右子树也分别为二叉排序树。

#### [红黑树](https://github.com/julycoding/The-Art-Of-Programming-By-July/blob/master/ebook/zh/03.01.md)

本质上来说就是一棵二叉查找树，同时:

* 每个结点要么是红的，要么是黑的。  
* 根结点是黑的。  
* 每个叶结点（叶结点即指树尾端NIL指针或NULL结点）是黑的。  
* 如果一个结点是红的，那么它的俩个儿子都是黑的。  
* 对于任一结点而言，其到叶结点树尾端NIL指针的每一条路径都包含相同数目的黑结点。

#### B树与B+树

## 排序

[十大经典排序算法（动图演示）](https://www.cnblogs.com/onepixel/p/7674659.html)

#### 快速排序

* 从数列中挑出一个元素，称为 “基准”（pivot）；
* 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；
* 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序。

## 查找

#### DFS & BFS



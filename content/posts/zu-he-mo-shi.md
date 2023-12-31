---
title: 组合模式
date: 2022-03-28
tags:
  - 设计模式-结构型
---

## 定义


组合模式是一种结构型设计模式，你可以使用它将对象组合成树状结构，并且能像使用独立对象一样使用他们。


## UML图


![%E7%BB%84%E5%90%88%E6%A8%A1%E5%BC%8F%20c0f43acfe0724e0da4e024a882cb571b](https://peierlong-blog.oss-cn-hongkong.aliyuncs.com/uPic/20c0f43acfe0724e0da4e024a882cb571b.svg)


## 使用场景


如果你需要实现树状对象结构，可以使用组合模式。

如果你希望客户端以相同的方式来处理简单和复杂的元素，可以使用该模式。


## 实现思路


1. 确保应用的核心模型能够以树状结构表示。

2. 声明组件接口及其一系列方法，这些方法对简单和复杂元素都有意义。

3. 创建一个叶子节点类表示简单元素。

4. 创建一个容器类表示复杂元素。

5. 最后，在容器中定义添加和删除子元素的方法。


## 优点


可以利用多态和递归机制更方便的使用复杂树结构

符合开闭原则


The end！

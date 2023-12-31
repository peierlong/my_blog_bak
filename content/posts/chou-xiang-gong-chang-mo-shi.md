---
title: 抽象工厂模式
date: 2021-12-20
tags:
  - 设计模式-创建型
---

## issue


什么是**产品等级结构**？产品等级结构即产品的继承结构。

什么是**产品族**？在抽象工厂模式中，产品族指的是同一个工厂生产的，位于不同产品等级结构中的一组产品。


## 角色


AbstractFactory 抽象工厂

ConcreteFactory 具体工厂

AbstractProduct 抽象产品

Product 具体产品


## 类图


![https://raw.githubusercontent.com/peiel/oss/master/uPic/FSqOHD.jpg](https://raw.githubusercontent.com/peiel/oss/master/uPic/FSqOHD.jpg)


## 时序图


![https://raw.githubusercontent.com/peiel/oss/master/uPic/Vo8Hky.jpg](https://raw.githubusercontent.com/peiel/oss/master/uPic/Vo8Hky.jpg)


## 模式要点


增加新的具体工厂和产品族都很方便，符合”开闭原则“

缺点：开闭原则的倾斜性（增加新的产品等级结构比较复杂）


> 什么是开闭原则的倾斜性？搜索笔记。

> 


## 适用环境


系统中有多个产品族，且只使用其中一种产品。


## 实际使用场景举例


更换主题的场景，要求界面中的文本框，背景颜色，按钮样式等一起改变，可以使用抽象工厂模式进行设计。

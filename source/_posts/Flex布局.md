---
title: Flex布局
date: 2020-05-11 15:18:18
tags:
 - CSS
 - 伸缩布局
categories: UI设计开发
---

页面CSS样式之Flex布局

---

## 2020/5/11 14:56 Write

关键词：UI

使用flex布局

```bash
在父容器设置属性 display: flex
```
![Flex基本概念](Flex基本概念.png)
```bash
Flex的核心概念就是容器和轴。容器包括外层的父容器和内层的子容器，轴包括主轴和交叉轴，Flex的布局特性构建在这两个概念上（如上图）。
```
## 1.容器
```bash
容器具有这样的特点：父容器可以统一设置子容器的排列方式，子容器也可以单独设置自身的排列方式，如果两者同时设置，以子容器的设置为准。
```
![Flex容器](Flex容器.png)
### 1.1父容器
·设置子容器沿主轴排列：justify-content
justify-content 属性用于定义如何沿着主轴方向排列子容器。
![justify-content](justify-content.png)
```bash
flex-start:  起始端对齐
flex-end：末尾段对齐
center：居中对齐
space-around：子容器沿主轴均匀分布，位于首尾两端的子容器到父容器的距离是子容器间距的一半
space-between：子容器沿主轴均匀分布，位于首尾两端的子容器与父容器相切
```
·设置子容器如何沿交叉轴排列：align-items
align-items 属性用于定义如何沿着交叉轴方向分配子容器的间距
![align-items](align-items.png)
```bash
flex-start：起始端对齐
flex-end：末尾段对齐
center：居中对齐
baseline：基线对齐，这里的 baseline 默认是指首行文字，即 first baseline，所有子容器向基线对齐，交叉轴起点到元素基线距离最大的子容器将会与交叉轴起始端相切以确定基线
stretch：子容器沿交叉轴方向的尺寸拉伸至与父容器一致
```
### 1.2子容器
·在主轴上如何伸缩：flex
子容器是有弹性的（flex 即弹性），它们会自动填充剩余空间，子容器的伸缩比例由 flex 属性确定。
 flex 的值可以是无单位数字（如：1, 2, 3），也可以是有单位数字（如：15px，30px，60px），还可以是 none 关键字。子容器会按照 flex 定义的尺寸比例自动伸缩，如果取值为 none 则不伸缩。
 虽然 flex 是多个属性的缩写，允许 1 - 3 个值连用，但通常用 1 个值就可以满足需求，它的全部写法可参考下图。
![flex](flex.png)
·单独设置子容器如何沿交叉轴排列：align-self
![align-self](align-self.png)
每个子容器也可以单独定义沿交叉轴排列的方式，此属性的可选值与父容器 align-items 属性完全一致，如果两者同时设置则以子容器的 align-self 属性为准。
```bash
flex-start：起始端对齐
flex-end：末尾段对齐
center：居中对齐
baseline：基线对齐
stretch：拉伸对齐
```
## 2.轴
如图所示，轴 包括 主轴 和 交叉轴，我们知道 justify-content 属性决定子容器沿主轴的排列方式，align-items 属性决定子容器沿着交叉轴的排列方式。那么轴本身又是怎样确定的呢？在 flex 布局中，flex-direction 属性决定主轴的方向，交叉轴的方向由主轴确定。
![轴](轴.png)
### 主轴
主轴的起始端由 flex-start 表示，末尾段由 flex-end 表示。不同的主轴方向对应的起始端、末尾段的位置也不相同。
```bash
向右：flex-direction: row
向下：flex-direction: column
向左：flex-direction: row-reverse
向上：flex-direction: column-reverse
```
### 交叉轴
主轴沿逆时针方向旋转 90° 就得到了交叉轴，交叉轴的起始端和末尾段也由 flex-start 和 flex-end 表示。

上面介绍的几项属性是 flex 布局中最常用到的部分，一般来说可以满足大多数需求，如果实现复杂的布局还需要深入了解更多的属性。

参考链接https://juejin.im/post/58e3a5a0a0bb9f0069fc16bb
深入理解Flex布局:https://juejin.im/post/5dedb28ef265da33b12e98cd

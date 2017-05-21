---
title: MySQL5.6使用Notifier无法停止或重启服务
date: 2016-12-04 19:21:26
toc: true
categories: 编程
tags:
- mysql
---

## 前言
以前用mysql的时候, 一直用的是WAMP集成开发环境, 前两天心血来潮, 打算自己配一下环境。Apache, php都没有什么问题, 很顺利的就装好了。但是到了Mysql这, 出了点小小的问题, 装好之后发现无法通过`notifier`工具停止服务, 提示`the service MySQL56 was not found in the windows services`。不甘心的我去windows的services里找了下, 发现明明存在`MySQL56`的服务。一脸懵逼。。。

<!-- more -->

## 解决方案
今天找到了解决方案, 其实很简单

### 打开管理面板
点击右下角的`notifier`, 选择`Actions` -> `Manage Monitored Items...`

![](http://7xqoa3.com1.z0.glb.clouddn.com/notifier.png)

### 删除当前服务

选中当前服务, 点击右边的`Delete`即可
![](http://7xqoa3.com1.z0.glb.clouddn.com/notifier2.png)

### 重新添加`MySQL56`服务
点击 `Add`->`Windows Services`, 在列表中找到`MySQL56`, 添加即可

### 测试
这时候, 再次停止或重启服务, 就会发现没有问题了

## 后记

其实这也不算是什么大的问题, 就是做为一个有强迫症的人来说, 一直看着这个服务运行停不下来, 简单没有办法忍受...
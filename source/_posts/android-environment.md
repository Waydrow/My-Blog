---
title: Android开发：环境配置
date: 2016-02-06 23:24:27
categories: Android
tags:
- Android
- Android Studio
- java
- jdk
---

## 前言
&emsp;由于最近参加了学校的一个创新创业训练项目，鬼使神差的入了一个android开发小组...当时心里默默的想，“我是搞前端的啊！！”。没办法，只能寒假恶补了。幸好时间不算紧，半年多的开发周期。  
&emsp;开发android首先要需要配开发环境咯
<!-- more -->
## 安装jdk
因为android的开发是基于java的，所以我们需要先配置好java环境, 这里是我的网盘分享地址  
[下载链接](http://pan.baidu.com/s/1hrwHXzA)
安装过程没有什么好说的，下一步大法好！

## 安装Android Studio
### 下载
毫无疑问，我们需要一个强大的android开发IDE, 首选自然是Google指定的开发工具——Android Studio。由于官方下载链接需要翻墙，这里附上国内的源，最新稳定版本为1.5.1  
[官方下载链接](https://developer.android.com/sdk/index.html)  
[国内下载链接](http://pan.baidu.com/s/1nuhv3qp#path=%252F1.5.1)
这是官网截图：

![](http://7xqoa3.com1.z0.glb.clouddn.com/images%2Fandroid-develop.png)
### 安装
一路安装即可，记得把`Android SDK`和`Android Virtual Device`选项勾选上
安装成功后，打开
![](http://7xqoa3.com1.z0.glb.clouddn.com/images%2Fandroid-studio.png)

## 创建新项目
我们来创建一个`HelloAndroid`新项目测试一下

![](http://7xqoa3.com1.z0.glb.clouddn.com/images%2Fandroid-first-test.png)

## 运行
我们先用官方的avd模拟器运行一下，直接点击那个绿色的运行箭头就好了, 如下图

![](http://7xqoa3.com1.z0.glb.clouddn.com/images%2Favd-test.png)


一开始的话我们可能需要新建一个模拟器

加载过程：  

![](http://7xqoa3.com1.z0.glb.clouddn.com/images%2Floading.png)  


模拟器启动界面：  

![](http://7xqoa3.com1.z0.glb.clouddn.com/images%2Frunning.png)

主界面：  

![](http://7xqoa3.com1.z0.glb.clouddn.com/images%2Fview.png)

程序运行界面：  

![](http://7xqoa3.com1.z0.glb.clouddn.com/images%2Fhello-world.png)

看起来还不错

## 后记
这样一个基本的开发环境就配置好了，我自己总结了几点比较重要的：  
- 一定要学会翻墙！！一定要学会翻墙！！一定要学会翻墙！！重要的事说三遍，不然的话在Android开发的路上寸步难行，官方API, SDK更新等等，虽说有国内的源，但是总觉得用官方的心里舒服！谁让这是Google的亲儿子呢！谁让我们生活在大陆地区呢！说多了都是泪...   
- 英文能力好的话是有很大的帮助的，可以直接去看Google官方的文档, 掌握第一手资料  
- 电脑配置是硬伤，有钱的话直接上mac吧，不然加块SSD, 升级下内存。确实对电脑要求蛮高的，毕竟谁都不想写个代码卡半天  
- 官方模拟器avd很难用，推荐Genymotion, 快很多

就说这么多吧，如果有问题欢迎在下方直接评论，当然也可直接联系我  
邮箱：<waydrow@163.com>
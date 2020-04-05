---
title: Android日志系统
date: 2016-03-03 22:53:28
toc: true
categories: coding
tags:
- android
---

此文为个人学习记录所用

### 使用日志API
#### Java :
- 错误日志-> `System.err.println()`
- 普通日志-> `System.out.println()`

<!-- more -->

#### Android :
- 错误信息-> `Log.e()`
- 警告信息-> `Log.w()`
- 普通信息-> `Log.i()`
- 调试信息-> `Log.d()`
- 无用信息-> `Log.v()`

>由下到上 优先级升高

可添加标签，如:

```java
    private static String TAG = "MainActivity";

    Log.e(TAG, "错误信息");
```

### 日志分类
- 根据优先级
- 根据包名
- 通过日志tag, 即上文中自定义的`TAG`
- 根据日志内容

### 使用DDMS查看日志
Android Device Monitor
---
title: 为Hexo博客添加顶部加载条
date: 2016-01-26 13:42:20
toc: true
categories: Blog
tags:
- blog
- css
---

&emsp;有时候会看到有的网页加载时上方出现进度条，一直想加到自己的博客上来，今天折腾了一番，终于成功了，记录下来。
## 开始制作
我使用的是[Pace](http://github.hubspot.com/pace/), 不过听别人说[NProgress](http://ricostacruz.com/nprogress/)更好用，改天试下
<!-- more -->
### 链入相关的`css` `js`文件
在`head.ejs`里加入中的head里
```
<script src="//cdn.bootcss.com/pace/1.0.2/pace.min.js"></script>
<link href="//cdn.bootcss.com/pace/1.0.2/themes/pink/pace-theme-flash.css" rel="stylesheet">
```
>`head.ejs`路径一般在博客当前目录下的`themes/xxx/layout/_partial/`中
我使用的cdn外链，也可以下载下来，本地链入

这个时候我们刷新一下博客，发现已经成功了，不过有个问题，默认的颜色为粉红色...让我们来改一改

### 更改默认样式
还是在`head.ejs`中,在<head></head>中加入
```
<style>
    .pace .pace-progress {
    	background: #1E92FB; /*进度条颜色*/
    	height: 3px;
    }
    .pace .pace-progress-inner {
     	box-shadow: 0 0 10px #1E92FB, 0 0 5px #1E92FB; /*阴影颜色*/
    }
    .pace .pace-activity {
    	border-top-color: #1E92FB;	/*上边框颜色*/
    	border-left-color: #1E92FB;	/*左边框颜色*/
    }
  </style>
```
上面标注的颜色位置可以任意修改成自己喜欢的颜色
>友情提示：这段代码要在上面link链接的下面加入（学过一些css的朋友应该都了解这个问题）

## 参考文章
[Hexo博客美化之动画效果加雪花](http://www.netcan666.com/2016/01/05/Hexo%E5%8D%9A%E5%AE%A2%E7%BE%8E%E5%8C%96%E4%B9%8B%E5%8A%A8%E7%94%BB%E6%95%88%E6%9E%9C%E5%8A%A0%E9%9B%AA%E8%8A%B1/)

如有问题欢迎大家下方留言，也可邮箱联系我 <Waydrow@163.com>
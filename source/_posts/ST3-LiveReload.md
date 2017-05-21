title: Sublime Text3 无法使用LiveReload插件的解决方法
date: 2015-08-16 16:10:40
toc: true
categories: 编程
tags:
- sublime
- web
---

## 问题更新
最近电脑系统换为ubuntu后，发现原来这个问题的解决办法和windows中略有不同，及时记录下来
## 前言
以前一直在用`sublime text2`, 有一款插件感觉非常好用，就是`LiveReload`，
在sublime中写完代码，按下ctrl+s保存后，浏览器自动刷新页面，可直接查看效果，
而不用切换到浏览器中再按F5刷新。这对于做web开发的真心方便，最爽的莫过于双屏开发，这边写完代码，保存，那边直接查看效果。
然而，前段时间开始尝试`sublime text 3`, 没想到通过`package control`安装的`livereload`插件无法使用了，简直不能忍啊！！在网上搜索了好久，也看了国外的一些解答，总算找到了解决方法。
<!-- more -->

## 安装LiveReload
### chrome
1.在chrome浏览器中安装扩展插件LiveReload，安装完成后，可看到右上角出现livereload图标，
如下图所示，此时中心的圆点是空心的，代表还未开启
![](/images/20150816/livereload-chrome.jpg)
2.进入chrome扩展程序页面，将livereload中的`允许访问文件网址`打上勾
![](/images/20150816/livereload-chrome2.jpg)

### sublime安装`LiveReload`
>sublime text3的package control中的livereload插件存在bug, 不知道什么时候能够修复

**windows**
<https://github.com/Grafikart/ST3-LiveReload>
可以从上面地址下载到可用的livereload，直接下载到本地或clone均可
你会得到一个插件包，将该文件夹重命名为`LiveReload`，然后将其手工放入
sublime text3的Packages目录中，此文件夹是隐藏文件夹，默认地址为
`C:\Users\your_user_name\AppData\Roaming\Sublime Text 3\Packages`
博主是用的[Everything](http://www.voidtools.com/)查找的，直接输入sublime package 关键字即可查找到。
>顺道在此打个广告，Everything是一款特别好用的windows平台下的搜索工具，比windows自带的搜索功能好用上千倍，附上下载地址，有需自取
<http://www.voidtools.com/downloads/>

**Linux**

```
cd ~/.config/sublime-text-3/Packages
rm -rf LiveReload
git clone https://github.com/Grafikart/ST3-LiveReload.git LiveReload
```

接下来，打开sublime text 3

**windows**
`preferences -> Packge Settings -> LiveReload -> Settings - User`
![](/images/20150816/sublime-livereload.jpg)
**Linux**
`preferences -> Packge Settings -> LiveReload -> Settings - Default`
输入以下内容保存即可

```
{
    "enabled_plugins": [
        "SimpleReloadPlugin",
        "SimpleRefresh"
    ]
}
```

>有很多教程是要每次打开sublime后，通过`package control`手动启用上述组件，这样也是可以的，但总归麻烦了一些，可以直接添加上述代码，使之默认加载。

## 最后
例如，打开某个html文件，在chrome中打开，点击LiveReload图标，可以看到中心由空心圆点变为实心圆点，代表启动成功，这时返回sublime中，可以看到左下角出现LiveReload连接成功的提示
![](/images/20150816/sublime-livereload2.jpg)
最后，开始享受敲代码的愉悦感吧！

>如有问题，欢迎留言或者发送邮件到<Waydrow@163.com>
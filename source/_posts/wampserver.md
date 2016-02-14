title: Windows下WampServer初体验
date: 2015-11-10 12:18:02
categories: 技术漫谈
tags:
- WampServer
- 服务器
---

## 前言
最近初步涉及php及服务器的搭建，曾花了一个晚上搭建`Apache`, 由于没有基础，搞的头都大了最终也没成功…不过，在这里介绍一款软件`WampServer`,这款软件在安装的过程中就已经把Apache、MySQL、PHP继承好了，而且也做好了相应的配置,安装时直接下一步大法就好了，简直是小白的福音！
<!-- more -->

## 安装WampServer
直接去官方网站下载就好了，不过貌似很慢，是法国的网站
>官网地址<http://www.wampserver.com/> 默认法语，可选英语

这里给大家分享此软件原版安装程序(抱歉只有64位版本的)
><http://pan.baidu.com/s/1ntxnK4x>

安装时很简单，一直下一步next就好了。以下有两点可能困扰大家的地方
![](/images/20151110/wamp-install1.png)  
上图为选择默认浏览工具：安装过程中会提示要选择默认浏览工具，如上图所示，不过要注意哦，这个浏览工具指的可不是浏览器哦，它指的是windows的浏览器，也就是explorer.exe，默认的就是这个，直接点击“打开”就可以了。

![](/images/20151110/wamp-install2.png)  
如上图所示，会提示一个输入管理员邮箱以及邮箱SMTP服务器的窗口，这个可以自己填写一下，要可以直接下一步，不影响安装

安装完成后打开`WampServer`,在任务栏中会出现wampserver图标，右键可选择语言
![](/images/20151110/wamp3.png)

左键点击出现如下界面
![](/images/20151110/wamp4.png)
需要使用时点击**启动所有服务**，使用完毕选择**停止所有服务**，右键退出即可

浏览器打开`localhost:80`可测试服务是否开启成功，若出现如下界面即可
![](/images/20151110/wamp5.png)

## 使用方法
左键点击wamp图标，选择`www目录`
![](/images/20151110/wamp6.png)
此时会打开www目录，将你需要测试的页面放入这个目录即可，例如我放入了一个文件夹名为`test`,其中有`index.html`,那我如果想要打开这个页面，只需在浏览器中输入`localhost:80/test/index.html`即可

## 后记
由于博主本身对后端和服务器端知识了解不多，一些wampserver详细的配置请google，baidu。这篇文章可供一些做前端的朋友需要在服务器端测试页面所用。
>如有问题欢迎留言或直接联系我，我的邮箱地址为: <Waydrow@163.com>

参考链接：
><http://www.360doc.com/content/
13/1113/09/426480_328813961.shtml>
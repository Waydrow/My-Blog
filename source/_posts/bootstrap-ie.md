title: 如何让bootstrap兼容ie8+
date: 2015-11-10 10:26:43
toc: true
categories: 编程
tags:
- web
- boostrap
---

想必做前端开发的都应该听说过`Bootstrap`, 一款优秀的前端开发框架。前段时间博主做的一个项目便尝试着用了boostrap来搭建，确定非常快，而且响应式做的特别好，省去了许多麻烦。不过由于我用的是Bootstrap 3.0,结果发现在ie8下崩掉了，心塞啊！又翻出bootstrap官方API，细读之……这才明白由于bootstrap做响应式所用的栅格布局在ie8下不被支持，而且也不支持html5的新标签和css3的一些效果，如圆角、阴影、一些过渡效果等……(详情见下图) 翻了好多教程，总结出以下方法，与大家分享！
<!-- more -->
![](/images/20151110/bootstrap-ie8+.png)
>bootsrap已经对ie9渲染的很好了，亲测
>听说bootstrap2.0对ie8支持蛮好，不过没有用过。有用过的朋友欢迎留言告诉我

## DOCTYPE
有些人可以不太注意html开头的doctype声明，其实这是非常重要的。DOCTYPE会告诉浏览器使用什么样的HTML或XHTML规范来解析HTML文档，具体会影响：
>标记、attributes 、properties的约束规则
>对浏览器的渲染模式产生影响，不同的渲染模式会影响到浏览器对于CSS 代码甚至 JavaScript 脚本的解析

所以千万不要忘记在html文档首先键入：
```
<!DOCTYPE html>
```
>而且注意doctype前后不要有空行

## 使用meta标签来调节浏览器的渲染方式
### IE 兼容模式
Bootstrap 不支持 IE 古老的兼容模式。为了让 IE 浏览器运行最新的渲染模式下，建议将此 <meta> 标签加入到你的页面中：
> `<meta http-equiv="X-UA-Compatible" content="IE=edge">`

按 F12 键打开 IE 的调试工具，就可以看到 IE 当前的渲染模式是什么。
此 meta 标签被包含在了所有 Bootstrap 文档和实例页面中，为的就是在每个被支持的 IE 版本中拥有最好的绘制效果。

### 国产浏览器高速模式
国内浏览器厂商一般都支持兼容模式（即 IE 内核）和高速模式（即 webkit 内核），不幸的是，所有国产浏览器都是默认使用兼容模式，这就造成由于低版本 IE （IE8 及以下）内核让基于 Bootstrap 构建的网站展现效果很糟糕的情况。幸运的是，国内浏览器厂商逐渐意识到了这一点，某些厂商已经开始有所作为了！

将下面的 <meta> 标签加入到页面中，可以让部分国产浏览器默认采用高速模式渲染页面：
> `<meta name="renderer" content="webkit">`

目前只有360浏览器支持此 <meta> 标签。希望更多国内浏览器尽快采取行动、尽快进入高速时代！

## Respond.js
Respond.js是什么？官方解释`A fast & lightweight polyfill for min/max-width CSS3 Media Queries (for IE 6-8, and more)`
其实就是使media query(媒体查询)兼容ie6-8
>附上链接<https://github.com/scottjehl/Respond>
然后将其引入到页面中，一般是放在head中。
>注意：
	1、一定要在所以css引入完毕后再引入respond.js
	2、不要用@import方式引入css文件，respond.js还不支持

引入方式如下：
```
<!--[If lt IE 9]>
  <script src="js/respond.min.js"></script>
<![endif]-->
```

使用ie8测试，咦，好像并没有什么用，页面一点变化都没有！
是了，如果你是用file://方式打开的，页面有变化才怪，来看一下bootstrap给出的解释
![](/images/20151110/respond-js.png)
看不懂？说白了就是respond.js只有在服务器端才可以使用，直接在本地打开html文件，是无法测试的！！
那要怎么办？简单，在本地配个服务器就行了。
你可能感觉配服务器好难啊！不要担心，我们只需使用第三方软件就好了，`WampServer`，好心的软件开发者们已经帮你们配置好了一切，如何使用请看我的另一篇博客
[Windows下WampServer初体验 ](http://blog.waydrow.com/2015/11/10/wampserver/)

好了，解决了这个问题，再次测试一下，发现还不错，布局基本没问题了，但是你如果使用了html5新标签(如header,nav,footer等)的话，可能会发现这些标签并不被支持。这时就需要`html5shiv`出场了！

## HTML5 Shiv
先附上github链接<https://github.com/aFarkas/html5shiv>
使用方法：
	这个没有respond.js那么麻烦，直接引入就好，再加上面的respond.js，代码就变成了下面这样

```
<!--[If lt IE 9]>
  <script src="js/respond.min.js"></script>
  <script src="js/html5shiv.min.js">
<![endif]-->
```

## CSS3
通过`respond.js`和`html5shiv`，你的页面已经基本兼容ie8+了，当然你如果追求更高的话，想要解决css3的支持问题，可以采用一些hack方法，比较流行的如[CSS3 PIE](http://css3pie.com/),可以支持border-radius、box-shadow、border-image、multiple background images、linear-gradient等。具体使用方法参照官方文档就好

## placeholder
ie8下不支持html5的属性placeholder，可以使用jquery插件来解决这个问题
<https://github.com/mathiasbynens/jquery-placeholder>

## 后记
些文只列出了一些ie8下的兼容问题，还有诸好background-size,last-child,inline-block,max-width等问题没有提供详细的解决方案，这篇文章会继续更新。如果一些问题你有更好的解决方法，欢迎在下面留言！也可以直接联系我, 我的邮箱为: <Waydrow@163.com>

参考链接：
><http://hustlzp.com/post/2014/01/ie8-compatibility>
><http://blog.csdn.net/chenhongwu666
/article/details/41513901>

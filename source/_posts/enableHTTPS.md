---
title: 拥抱 HTTPS
toc: true
date: 2017-05-24 16:30:48
categories: blog
tags:
- https
- CloudFlare
---

由于我使用的是独立域名, 以前天真地以为部署在`Github`上的博客是没有办法启用`HTTPS`的, 今天才知道我错了。偶然间发现了 [CloudFlare](https://www.cloudflare.com), 其提供的个人免费套餐可以为我们的博客启用`HTTPS`

## 配置方法

### Github Pages
如果你是使用的 Github Pages 默认提供的域名, 如 `waydrow.github.io`, 那么可以直接在设置中启用 HTTPS, 以下内容可直接忽略

### 注册
不多说

### 添加网站
注册成功后会提示你添加网站, 如下图
![](http://7xqoa3.com1.z0.glb.clouddn.com/cf_add.png)

> 此处直接输入你的独立域名 (例: waydrow.com), 注意不要填写子域名, 例(blog.waydrow.com)

### 确认 DNS 解析列表
下一步后其会扫描你的域名的 DNS 解析记录, 你需要做的就是确认下面的列表是否完整

> 这个步骤我配置的时候很奇怪, 扫描到的列表为空, 只好手动全部添加进去了

### 更换 Name Server
这时会提示让你更换 Name Server, 以我所使用的万网为例
![](http://7xqoa3.com1.z0.glb.clouddn.com/cf_change.png)

如上图所示, 将 Name Server 修改为 CloudFlare 所提供的

### 等待确认
CloudFlare 提示的时间需要等待几个小时, 但实际好像不需要这么长时间, 我就等了几分钟就可以了
在配置面板中点击 `Recheck Nameservers`, 会发现网站已经激活, 如下图
![](http://7xqoa3.com1.z0.glb.clouddn.com/cf_active.png)

接下来就可以使用 [https://blog.waydrow.com](https://blog.waydrow.com) 访问本站点了

### ~~强制 HTTPS 跳转~~
~~但是此时你会发现, 只能手动输入 https 才可以, 所以我们输入在博客的模板文件中使用 js 强制跳转~~

~~以我所使用的 maupassant 主题为例, 在 `layout\_partial\head.jade` 中添加如下代码~~

```jade
script(type='text/javascript').
       var host = "blog.waydrow.com";
       if ((host == window.location.host) && (window.location.protocol != "https:"))
           window.location.protocol = "https";
```

> ~~注意, 要添加在 head 标签中, 其他主题类似~~

### 使用 CloudFlare 强制 HTTPS
感谢 [Matriks](https://www.lyeec.me) 提供的方法, 可以不用在客户端强制 HTTPS 跳转, 直接在 CloudFlare 的 `Page Rules` 页面中添加一条规则。

以本站为例, 填写 `http://blog.waydrow.com/*`, Setting 中选择 `Always Use HTTPS` 即可。

## 部署
之后就可以愉快的使用 https 啦~

## 参考文章
[给github pages上ssl(hexo可用)](https://blog.xingoxu.com/2015/04/github-pages-ssl/)
[为Github的Hexo博客启用SSL/TLS](https://g2ex.github.io/2015/10/14/Hexo-with-SSL-Hosted-on-Github-Page/)

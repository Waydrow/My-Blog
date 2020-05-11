---
title: 用 Travis CI 自动部署Hexo博客到 GitHub (二)
date: 2016-02-01 21:51:22
toc: true
categories: blog
tags:
- travis ci
- hexo
---

上文简单介绍了一些基本的概念和我们的实现思路，思路明确后，具体操作也就不难理解了

## 准备 Dev repo 与 Pages repo
如果你已经在使用hexo博客，可以将`master`分支作为`Dev repo`, `gh-pages`作为`Pages repo`。建立Hexo博客的方法可以参照我的另一篇文章[使用Hexo在Github上搭建你的博客](http://blog.waydrow.com/2015/08/14/%E4%BD%BF%E7%94%A8Hexo%E5%9C%A8Github%E4%B8%8A%E6%90%AD%E5%BB%BA%E4%BD%A0%E7%9A%84%E5%8D%9A%E5%AE%A2/)
<!-- more -->
## Deploy Key
生成ssh-key请参见官网教程：[Generating an SSH key](https://help.github.com/articles/generating-an-ssh-key/)
这里我们假设生成的两个文件名为`id_rsa.pub` 和 `id_rsa`，其中`.pub`是公钥，我们需要将其添加到github上。
注意：这个 SSH key 不应成为你账号的全局 SSH key（因为这样 Travis CI 就获得了你所有代码库的提交权限，这是不严谨的），而应该添加至 https://github.com/username/username.github.io/settings/keys ，这样能更好的限制 Travis CI 的提交权限。即下图所示位置：
![](https://raw.githubusercontent.com/Waydrow/PicGo/master/img/deploy-key.png)

## 申请Travis CI
在<https://travis-ci.org/>,用github帐号登录，找到你的博客仓库，开启Travis CI服务，如下图：
![](https://raw.githubusercontent.com/Waydrow/PicGo/master/img/2016-02-01_221125.png)
在其中的设置页面作如下设置：
![](https://raw.githubusercontent.com/Waydrow/PicGo/master/img/2016-02-01_221211.png)

## 加密 Private Key
下面的操作需要 Ruby 和 gem 环境，`Windows`下配置有很多问题，建议在`Linux下`，下面以`Ubuntu 14.04 LTS`为例来向大家介绍

### 安裝 Travis
```
$ gem install travis
```
这个时候你可能会发现好久没有响应，或者出现提示说连接错误。
这便是由于我们伟大的**墙**了，可以采用下面的方法解决
```
$ gem sources --add https://ruby.taobao.org/ --remove https://rubygems.org/
$ gem sources -l
*** CURRENT SOURCES ***

https://ruby.taobao.org
# 请确保只有 ruby.taobao.org
```
我们将gem包的镜像源换为国内的即可，然后再运行
```
gem install travis
```

### 命令行登录Travis CI
```
$ travis login --auto
```
会让你输入github帐号

如此一来，我们就能通过 Travis 提供的命令列工具加密刚刚所制作的 Private key，并把它上传到 Travis 上供日後使用。

### 建立文件
将一开始得到的`id_rsa`文件复制到`Dev repo`下，并建立`.travis.yml`文件，内容暂时为空即可

### 加密私钥并上传至 Travis CI.
```
$ travis encrypt-file id_rsa --add
```
成功后会生成`id_rsa.enc`文件，我们就可以将`id_rsa`文件手动删除，保证安全，同时上述指令还会在`.travis.yml`文件中插入解密指令：
`
- openssl aes-256-cbc -K $encrypted_xxxxxxxxxx_key -iv $encrypted_xxxxxxxxxx_iv
  -in id_rsa.enc -out id_rsa -d
`
其中xxxxxxxxxx部分便是你的解密参数，不要去改动它

### 修改`.travis.yml`文件
`
- openssl aes-256-cbc -K $encrypted_xxxxxxxxxx_key -iv $encrypted_xxxxxxxxxx_iv
  -in id_rsa.enc -out ~/.ssh/id_rsa -d
`

## 建立ssh_config文件
内容为：
```
Host github.com
  User git
  StrictHostKeyChecking no
  IdentityFile ~/.ssh/id_rsa
  IdentitiesOnly yes
```

## 完善 Travis CI 的脚本文件 `.travis.yml`
下面对 .travis.yml 文件各块添加了注释。有几个地方必须要修改：
两处 xxxxxxxxxx 修改为你之前获得的解密参数；你的姓名 和 你的邮箱 与你 Github 上的信息保持一致。
同时附上本博客的 `.travis.yml` 源文件，以供参考：[.travis.yml](https://github.com/Waydrow/My-Blog/blob/master/.travis.yml)

```
# 指定环境
language: node_js

node_js:
- '0.12' #指定使用 node.js 最新的稳定版0.12

# 指定分支
branches:
  only:
  - master #这个分支应当使用自己的 Dev repo

before_install:
#注意将xxxx内容修改为你之前获得的解密参数
- openssl aes-256-cbc -K $encrypted_xxxxxxxxxx_key -iv $encrypted_xxxxxxxxxx_iv
  -in id_rsa.enc -out ~/.ssh/id_rsa -d
- chmod 600 ~/.ssh/id_rsa //修改目录权限
- eval $(ssh-agent)//将密钥加入系统
- ssh-add ~/.ssh/id_rsa
- cp ssh_config ~/.ssh/config //修改 git 信息
- git config --global user.name "你的姓名"
- git config --global user.email 你的邮箱

# 配置 Hexo
install:
- npm install hexo-cli -g
- npm install
- npm install hexo-generator-feed --save
- npm install hexo-generator-sitemap --save
- npm install hexo-deployer-git --save

# 执行 Hexo 的编译操作
script:
- hexo clean
- hexo g
- hexo d

```

## Push 到 Dev repo
将改动push到Dev repo上，在<https://travis-ci.org>页面可以查看构建状态，
如果成功的话就能在自己的 pages 上查看刚生成的博客了；如构建失败，Travis CI 会显示出哪步脚本导致了构建失败，本地源里修改它，然后再次 push 即可。

## 后记
这样一来，我们以后写博客或者改配置，只需要push即可，Travis CI会帮助我们自动部署，是不是比较方便？
其实我感觉这样最大的好处重装系统之后不需要重新配置hexo环境了，直接clone到本地，就可以了。

如有问题欢迎在下面留言
或者直接联系我：<Waydrow@163.com>
我会在第一时间回复
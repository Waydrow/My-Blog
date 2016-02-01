---
title: 用 Travis CI 自动部署Hexo博客到 GitHub (一)
date: 2016-02-01 21:22:51
categories: 博客
tags:
- travis ci
- Hexo
---

## 前言
使用hexo静态博客已经半年多了，总体感觉挺好用的。但是有一点一直让我很苦恼，由于这是静态博客，所以每次写完博客都需要先`generate`,再发布`deploy`。而且博客的基本配置信息没办法同时更新到github，还需要我再`push`上去，这样一番下来，虽然花费不了太长时间，但是时间长了就比较难以忍受了。
特别是重装系统之后或者用别人的电脑，需要重新搭建环境，对像我这种喜欢捣腾系统的人，简直了...
前几天偶然看到了`Travis CI`，可以用来自动部署博客，心甚喜之，来与大家分享。
<!-- more -->
> 注：个人建议使用`Linux`来搭建下面的环境，在win下我尝试了很多次，有很多问题
> 以下教程使用环境: `Ubuntu 14.04 LTS`

## Travis CI
先简单介绍一下持续集成，这是一种软件开发实践。在持续集成中，团队成员频繁集成他们的工作成果，每人每天可能集成一次，甚至多次。每次集成会经过自动构建（包括自动测试）的检验，以尽快发现集成错误。许多团队发现这种方法可以显著减少集成引起的问题，并可以加快团队合作软件开发的速度。
自动构建工具则是持续集成的一种出色实践。代码提交后，由软件自动完成代码的测试、构建，并将过程中状态与构建物产出才是持续集成的意义。
Travis CI就是一个在线的、分布式的持续集成服务，用来构建及测试在GitHub托管的代码。利用Travis CI 会在每一次push后生成一个虚拟机来执行事先安排好的自动构建任务，从来进行发布。

## 构思
Travis CI 自动构建 Hexo 的工作流的构思是：
本地向 Github 上 push 代码后，如果该代码属于目标源（我们暂时称它为 dev repo），Travis CI 就自动构建 Hexo 环境编译它，并将产出的静态博客 push 回我们的 Github pages 源（我们就称它为 pages repo）。然后即可在 pages 上查看新发布的博客。
如下图：
![](http://7xqoa3.com1.z0.glb.clouddn.com/images%2Ftravis-hexo-flowing.png)

下面分解一下：

1. User - push -> Dev repo
事先在 Github 上建立好 repository 即可, 我所采用的方案是建立 `username.github.io` 源，将`master`分支作为 Dev repo. 而 `gh-pages` 分支就是 Pages repo. 这样的好处是只用维护一个 repository.

2. Dev repo - sync -> Travis CI
在 Travis CI 中开启 Dev repo 的同步开关，然后在 Dev repo 中添加 `.travis.yml` 文件。这样 Travis CI 就能自动同步之后 push 的代码了。
另外记得在 Travis CI 的同步设置中启用 `Build only if .travis.yml is present` 项，这样能在 repository 中有多个 branch 时，让 Travis CI 只构建放置了`.travis.yml` 文件的 branch.

3. Travis CI - build and push -> Pages repo
这里再分解为 build 和 push 两步：
- build
Travis CI 的自动化构建完全依靠唯一的 `.travis.yml` 脚本文件。需要在此文件中添加构建环境、构建 Hexo、生成博客及后续 push 到 Pages repo 的全部脚本。
- push
这一步是最麻烦的。要做到 `Travis CI` 向 Pages repo 自动推送就必须用到 `Github SSH Key`. 但是如果直接放置 SSH 私钥在 Dev repo 中，等于向所有人开放了代码仓库的提交权限！
没有一点点防备，也没有一丝顾虑，你就这样出现在我的世界里，带给我惊喜——大概就会出现这种状况。
这不符合程序员的严谨美学（即使这个项目除了自己外根本无人 care)。
我们要把私钥加密并上传到 Travis CI. 然后会得到一个加密过得公钥和一段解密脚本。这个公钥只能被 Travis CI 解密，所以可以放心地把公钥放置于 Dev repo 中。
在 `.travis.yml` 中添加解密公钥、SSH 加密 push 等步骤的脚本。

这就是我们大致的思路，具体如何操作请看下篇文章[用 Travis CI 自动部署Hexo博客到 GitHub (二)](http://blog.waydrow.com/2016/02/01/travis-ci-for-hexo-2/)
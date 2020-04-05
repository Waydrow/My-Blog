title: 使用Hexo在Github上搭建你的博客
date: 2015-08-14 16:53:00
toc: true
categories: blog
tags:
- blog
- hexo
- markdown
---

自从投身于互联网学习的怀抱中后，一直想搭建一个属于自己的个人博客。了解过WordPress
，不过因为不懂服务器的知识只得暂时作罢;也看过csdn、博客园等，方便是方便，不过主题样式无法个人定制，这对于美工出身的我简直无法忍受。听朋友说起hexo，并且能够部署在github上，甚是方便，照着教程，一步一步搭建了这个博客。在搭建博客过程中遇到了很多问题，写下这篇教程的原因就是希望能够给需要的人些许帮助。
<!-- more -->
## hexo简介
__Hexo__ 是一个快速、简洁且高效的博客框架。出自台湾大学生[tommy351](http://twitter.com/tommy351) 之手，是一个基于Node.js的静态博客程序。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（关于markdown在后面会有更详细的介绍）或其他渲染引擎解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

## 开发环境
### 安装[Node.js](https://nodejs.org/)
直接到官网下载安装即可 或 通过 [nvm](https://github.com/creationix/nvm)
	cURL:
	```bash
	$ curl https://raw.github.com/creationix/nvm/master/install.sh | sh
	```
Wget:
	```bash
	$ wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh
	```
安装完成后，重启终端并执行下列命令即可安装 Node.js
	```bash
	$ nvm install 0.10
	```
### 安装[Git](http://git-scm.com/)
- windows: 下载并安装[Git](https://git-scm.com/download/win)
- Mac: 使用 [Homebrew](http://mxcl.github.com/homebrew/), [MacPorts](http://www.macports.org/) 或下载 [安装程序](http://sourceforge.net/projects/git-osx-installer/) 安装
- Linux (Ubuntu, Debian)：`sudo apt-get install git-core`
- Linux (Fedora, Red Hat, CentOS)：`sudo yum install git-core`

### 安装Hexo
安装完node.js和git之后，即可以通过npm安装hexo了
打开**git bash**,键入以下命令
```bash
$ npm install -g hexo-cli
```
安装完成后可以通过 `$ hexo version`查看hexo版本信息

## 初始化
安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件
```bash
$ hexo init <folder>
$ cd <folder>
$ npm install
```
>或者通过cd命令进入你想放置的文件夹
>然后 `$ hexo init`

初始化完成后，你会在该文件夹下得到如下的目录结构
```
.

├── _config.yml
├── package.json
├── scaffolds
├── scripts
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

## 本地启动
执行命令
```bash
$ hexo server
```
成功启动则会看到反馈信息
`[info] Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.`
此时端口4000被打开，打开浏览器，输入上面所示网址
出现了默认的主题界面，心情是不是有点激动~
![](/images/20150814/hexo-web.png)
>有的电脑通过命令`$ hexo server`本地启动后，查看到的反馈信息中的网址可能如下所示
>http://0.0.0.0:4000/  (如博主自己的电脑…)
>经测试发现直接输入该网址是无法访问的，还是需要通过 http://localhost:4000/ 来访问

>至此，Hexo的安装工作基本完成，下面介绍hexo的使用方法

## 博客配置
**_config.yml**
这是网站的配置信息，可以在些配置网站的基本参数
**网站**
![](/images/20150814/config.jpg)
**网址**
![](/images/20150814/config2.jpg)
>若您的网站存放在子目录中，例如 `http://yoursite.com/blog`，则请将您的 url 设为 `http://yoursite.com/blog` 并把 root 设为 `/blog/`。

**目录**
![](/images/20150814/config3.jpg)
**文章**
![](/images/20150814/config4.jpg)
**分类 & 标签**
![](/images/20150814/config5.jpg)
**日期/时间格式**
![](/images/20150814/config6.jpg)
**分页**
![](/images/20150814/config7.jpg)
**扩展**
![](/images/20150814/config8.jpg)

## hexo指令
### init
```bash
$ hexo init [folder]
```
新建一个网站，也可以先进入目录，再`hexo init`
### new
```bash
$ hexo new [layout] <title>
```
新建一篇文章。如果没有设置 `layout` 的话，默认使用 `_config.yml` 中的 `default_layout` 参数代替。如果标题包含空格的话，请使用引号括起来。
>`layout`在此意为布局。Hexo 有三种默认布局：`post`、`page` 和 `draft`，它们分别对应不同的路径，而您自定义的其他布局和 `post` 相同，都将储存到 `source/_posts` 文件夹。
>![](/images/20150814/layout.jpg)

### generate
```bash
$ hexo generate
```
生成静态文件
### server
```bash
$ hexo server
```
本地启动服务器
### deploy
```bash
$ hexo deploy
```
部署网站
### list
```bash
$ hexo list <type>
```
列出网站资料

>以上指令是可以简写的，如
>`hexo n` == `hexo new`
>`hexo g` == `hexo generate`
>`hexo s` == `hexo server`
>`heox d` == `hexo deploy`
>同时，hexo还支持复合命令，如 `hexo d -g` 意为 先静态化处理，再部署
>平常写博客上面的这些指令就差不多够用了
>详细指令说明可参阅<https://hexo.io/zh-cn/docs/commands.html>

## 创建新文章
下面我们开始写第一篇博文，进入安装hexo的目录，`git bash`或本地命令行均可
```bash
$ hexo new "我的第一篇博文"
```
你会发现在_posts目录下生成文件 “我的第一篇博文.md”
![](/images/20150814/myfirstblog.jpg)
然后我们可以开始以markdown语法编写文章
>markdown的语法说明可以参照
><http://www.ituring.com.cn/article/504>
><http://ibruce.info/2013/11/26/markdown/>
>个人感觉上面的介绍已经比较详细了，博主就不在这多啰嗦了，不过关于图片的引入方法
>如果是网络图片，参照上面的教程
>如果是本地图片，则需要在`source`目录下新建`images`文件夹，将图片放入该目录中
>例如`![](/images/example.jpg)`

![](/images/20150814/myfirstblog3.jpg)
写完之后保存，本地启动服务器`hexo server`
在浏览器中打开`http://localhost:4000` 即可看到我们刚刚写完的文章已经发表了,页面右侧还会自动加入新的目录和标签等，是不是非常简单！！
![](/images/20150814/myfirstblog2.jpg)

## 发布到github
### 静态化处理
执行命令
```bash
$ hexo generate
```
在此说明一下静态化处理的目的，由于我们用hexo所搭建的这个博客，是静态网站，即只有html,css和javascript，无法动态更新。静态化处理即生成只有html、css和javascript的网站。

### 部署到github
- 注册github帐号
- 建立一个仓库，名为[your_user_name.github.io]
>已有github帐号的请忽略第一步

编辑配置文件_config.yml，在deploy部分，设置github的项目地址
```
deploy:
	type: git
	repository: git@github.com:example/example.github.io.git
```

然后执行命令
```bash
$ hexo deploy
```
若没问题的话会提示你输入帐号密码，之后就部署成功了，可以在github查看，
点击右侧的`setting`
![](/images/20150814/github-setting.jpg)
在此页面中你会看到
![](/images/20150814/github-publish.jpg)
这就是你网站部署的地址了，打开此网址，即可看到你发布的网站
>在执行`hexo deploy`命令时，可能会提示找不到git
解决方法：
在Hexo 3.0版本后`deploy git` 被分开的，所以需要安装，安装命令如下:
`npm install hexo-deployer-git --save` ,安装好后再尝试一下就ok

## 绑定域名
如果对于github默认分配的二级域名`example.github.io`满意的话，就用这个也是可以的。
如果不太满意，可以购买一个域名，博主是从[阿里云](http://www.aliyun.com/)购买的.com
域名。从网上大家的评论来看.com和.me的域名评价比较高，而且.com是国际域名,
比.cn的总归要方便一些（你懂的~）
购买域名都是按年付费的,.com和.me的一年50-60人民币，一般第一年都有优惠。

设置域名有两种方式
- 主域名绑定: 如example.com
- 子域名绑定：如blog.example.com

### 主域名绑定
在`source`根目录下新建文件 `CNAME`，无后缀，纯文本文件，内容为要绑定的域名
`example.com`,如果要使用`www.example.com`的形式，文件内容改为`www.example.com`

DNS设置
主机记录`@`，类型`A`，记录值`192.30.252.153`
主机记录`www`，类型`A`，记录值`192.30.252.153`
参考<https://help.github.com/articles/tips-for-configuring-an-a-record-with-your-dns-provider>

### 子域名绑定
比如使用域名`example.com`的子域名`blog.example.com`
`CNAME`文件内容为`blog.example.com`

DNS设置
主机记录`blog`，类型`CNAME`，记录值`example.github.io`
参考<https://help.github.com/articles/tips-for-configuring-a-cname-record-with-your-dns-provider>

## 更换主题
<https://github.com/hexojs/hexo/wiki/Themes>，可以从这上面挑选一个自己喜欢的主题
比如我觉得[next](https://github.com/iissnan/hexo-theme-next)还不错，
进入github的项目地址后
![](/images/20150814/git-theme.jpg)
复制如图所示的右侧地址
在`theme`目录下，执行命令
```bash
$ git clone https://github.com/iissnan/hexo-theme-next.git
```
完成后会在`theme`目录下生成`hexo-theme-next`主题文件夹
打开`_config.yml`配置文件，找到`theme`选项
将默认的`theme: landscape`更换为
`theme: hexo-theme-next`
此时，启动服务器`hexo server`可查看效果，之后便可静态化然后部署到github上即可。
><http://theme-next.iissnan.com/>这是next主题的说明，可参照此修改默认配置。
其他主题也可借鉴说明

## 配置插件
### 评论系统
由于 hexo默认集成的是[Disqus](http://disqus.com/)评论系统，所以你懂的~
国内推荐使用[多说](http://duoshuo.com/)
直接用你的qq/微博/豆瓣/人人/百度/开心网帐号登录多说，做一下基本设置。如果使用Next主题，在网站配置_config.yml（非主题的_config.yml配置文件！） 中配置`duoshuo_shortname`为多说的 基本设置->域名 中的`shortname`即可。你也可以在多说后台自定义一下多说评论框的格式，比如评论框的位置
这样，评论系统就加好了
![](/images/20150814/comment-duoshuo.jpg)

### RSS订阅
这个功能已经有人写好了插件，直接安装即可
```bash
$ npm install hexo-generator-feed
```
然后在站点配置文件`_config.yml` 的`plugins`选项中添加
![](/images/20150814/plugin.jpg)
启动服务器，打开`http://localhost:4000/atom.xml`，就可以看到RSS已经生效了。

### Sitemap站长地图
同RSS一样，直接安装
```bash
$ npm install hexo-generator-sitemap
```
然后在`plugins`中添加
`- hexo-generator-sitemap`
启动服务器，打开`http://localhost:4000/sitemap.xml`，就可以看到Sitemap已经生效了。

### 自定义界面
执行`new page`命令
```bash
$ hexo new page "about"
```
在 `source`下会生成`about`目录，里面有个`index.md`，直接编辑就可以了，然后在主题的 _config.yml 中将其配置显示出来。页面的其他部分会自动补全，只添加内容即可
>如果不想在about页面显示默认的评论框，添加`comments: false`，就可以了。

### 统计
由于 Google Analytics 偶尔会被墙，可以用百度统计，以Next主题为例，
去[百度统计](http://tongji.baidu.com)申请一个帐号，添加你的博客地址
在代码获取界面
![](/images/20150814/baidutongji.jpg)
?后面的这串字母加数字即是你的百度分析ID，复制下来，
在站点配置文件中添加如下所示
![](/images/20150814/baidutongji2.jpg)
添加完后过一段时间才能在百度统计看到结果

### 网站图标
将你的`favicon.ico`放到`source`目录下即可
图标的制作可以在 [比特虫](http://www.bitbug.net/)网站制作

## 后记
因为我的博客<http://blog.waydrow.com>基本是免费做出来的，所以写下这篇博文，希望可以帮助到其他人，也感谢此开源项目的贡献！
**参考资料**
<http://blog.fens.me/hexo-blog-github/>
<http://www.tuicool.com/articles/AfQnQjy>
<http://blog.lmintlcx.com/post/blog-with-hexo.html#绑定顶级域名>

>如有问题，欢迎留言或者发送邮件到<Waydrow@163.com>
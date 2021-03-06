---
title: 如何用正则表达式匹配中文
date: 2016-04-26 15:51:01
toc: true
categories: coding
tags:
- php
- 正则
---

还是没办法不去在意这个博客, 毕竟付出了自己将近一年的心血, 这是几个周前写的一篇文章, markdown格式写的不是很规范, 望见谅! 分享在此。
前几天因为在做学校教务处的爬虫，用php抓取的成绩和课程表竟然返回的是html格式的数据，也是很醉。没办法，干脆用正则匹配吧。因为之前并没有学过正则表达式，只好恶补了一下。在匹配的过程中遇到了一些问题，特别是在匹配中文的时候，很是蛋疼。下面说一下我的学习成果。
<!-- more -->
使用php在匹配中文的时候不能使用 `\w` 来匹配，可以使用元字符 . 来粗略匹配中文
精确匹配中文时需要考虑编码环境，`gb2312`和 `utf-8`。这两种编码有什么区别呢 ？ 最主要的就是gb2312编码的汉字占两个字节，而utf-8编码的汉字占3个字节。
一、好了，下面进入正题，如果你想匹配中文的话，可以采用下面的表达式：
utf-8编码：

```
[\x{4e00}-\x{9fa5}]
```

例如：匹配5个汉字，便可以这么写：
```
/[\x{4e00}-\x{9fa5}]{5}/u
```
千万注意，这个最后面的u一定要加上（如果是使用php的话），否则是无法正常匹配的。
二、通过上面的表达式我们可以匹配一段模糊的中文，那如果我们想要匹配精准的某个字或者词语呢 ？例如，我在做教务处爬虫时，抓取到的成绩不仅仅只是数字，还有优秀、通过、良好等。这种我们总不能漏掉吧？ 可以使用下面的方式来匹配：
1. 先将汉字转换成为16进制Unicode编码，可以在这个网站方便的转换：Unicode与中文互转 16进制Unicode编码转换、还原
   例如我们将 优秀 两个字转换成了该编码，为 :  \u4f18\u79c0
2. 匹配 优秀 两个汉字的正则表达式如下：
   /\x{4f18}\x{79c0}/u
   想必大家应该已经明白了，拿到16进制编码后，有这么几步，将u改为x, 再将具体的16进制编码加上{ }，最后不要忘记加上u
   三、包含换行段落的匹配
   先给出一段需匹配的代码：

```html
<span style="white-space:pre">	</span><tr class="H">
    <td class="td0" style='width:5%;padding:0px;' colspan='2'></td>
    <td class='td0' style="width:13%;height:20px;">
        星期一
    </td>
    <td class='td0' style="width:13%;height:20px;">
        星期二
    </td>
    <td class='td0' style="width:13%;height:20px;">
        星期三
    </td>
    <td class='td0' style="width:13%;height:20px;">
        星期四
    </td>
    <td class='td0' style="width:13%;height:20px;">
        星期五
    </td>
    <td class='td0' style="width:13%;height:20px;">
        星期六
    </td>
    <td class='td0' style="width:13%;height:20px;">
        星期日
    </td>
<span style="white-space:pre">	</span></tr>
```
我们的目标是从这段html代码中抓取星期一   —  星期日，有人可能会说，直接匹配td标签，来个for循环就好了吗，但现在我只是给出一个例子，很多时候我们拿到的数据并不像这样有规律，所以成段匹配还是很有必要的。
我一开始尝试的方法是从`<tr>`匹配到`</tr>`, 将其中的汉字全部抓出来 ，但很不幸，失败了。原因就是在于其中的换行，那我们怎样才能匹配包含换行的文本呢 ？其实方法很简单，只要使用这个表达式：`/[.\s\S]*/`
我曾经试过使用` /[.\n]/  `来匹配，但是并不可以。上面的表达式完美的解决了问题。

如有问题, 欢迎留言, 或者可直接联系我, 这是我的邮箱: <waydrow@163.com>
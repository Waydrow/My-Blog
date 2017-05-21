---
title: c++输入隐藏密码的实现
date: 2016-12-10 22:03:41
toc: true
categories: 编程
tags:
- c++
---

最近在用`C++`编写一个图书管理系统, 其中需要用户的登录, 原来没有想太多, 就用了普通的`cin`输入, 但是前两天组里的同学说, 要是输入密码的时候能将其隐藏就好了。以前做网站的时候, 有各种标签属性可以很方便的实现这个功能, 但是现在是控制台...不知道怎么搞了。最后百度谷歌后发现了一个很神奇的函数
<!-- more -->

## 实现
简而言之, 就是使用`C++`的`getch()`函数, **注意**不是`getchar`, 这个函数可以使用户的输入不显示在屏幕上, 其包含在`conio.h`头文件中, 下面看代码

```C++
static void inputPassword(string &str, int size) {
	char c;
	int count = 0;
	char *password = new char[size]; // 动态申请空间
	while ((c = getch()) != '\r') {

		if (c == 8) { // 退格
			if (count == 0) {
				continue;
			}
			putchar('\b'); // 回退一格
			putchar(' '); // 输出一个空格将原来的*隐藏
			putchar('\b'); // 再回退一格等待输入
			count--;
		}
		if (count == size - 1) { // 最大长度为size-1
			continue;
		}
		if ((c >= 'a' && c <= 'z') || (c >= 'A' && c <= 'Z') || (c >= '0' && c <= '9')) {  // 密码只可包含数字和字母
			putchar('*');  // 接收到一个字符后, 打印一个*
			password[count] = c;
			count++;
		}
	}
	password[count] = '\0';
	str = password;
	delete[] password; // 释放空间
	cout << endl;
}
```
为方便对其操作, 我使用了`string`, 其中需要进行`char*`和`string`的转换
## 项目地址

[https://github.com/Waydrow/Library](https://github.com/Waydrow/Library)
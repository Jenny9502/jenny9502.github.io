---
title: 使用hexo的基本操作
date: 2017-12-28 16:26:26
tags:
header-img: f.png
---
## hexo 基本用法和git代码管理 ##

	hexo server :启动本地浏览;

	hexo new post "新建文件名"：新建一篇文章;

	hexo clean &hexo g :更改了配置文件后，需要更新文件并且重置；
	
	hexo generate  

	hexo deploy

	git status //查看有什么改动的代码，这里运营了git来管理代码

	git add . //确认添加修改的代码

	git commit //commit修改的代码

	git push代码到网上的博客(我这里是push 到 origin source);

-----------

## 记录常用Markdown 基本语法的使用

### 段落 

 直接写

### 标题

在首行插入1到6个‘#’，对应到标题1到6阶

### 代码块

代码行缩进四个空格

### 分割线

在一行中用三个或以上的星号，减号，底线来建立一个分割线，行内不能有其他东西

### 链接

Markdown  支持两种形式的链接语法：  行内式和参考式， 但是不管哪一种， 链接文字都是用 “[方括号]”来标记  

>插入超链接语法`[链接名](URL)`  [百度](http://www.baidu.com)  

#### 行内式

只要在方块括号后面紧接着圆括号并插入网址链接即可，1是有“Title” 的。

>1.This is `[an example](http://www.baidu.com/ "Title")` inline link 显示如下,注意加title前面**需要空格**隔开  

This is [an example](http://www.baidu.com/ "Title") inline link 

>2.`[This link](http://www.baidu.com/)`has no title attribute 显示如下  

[This link](http://www.baidu.com/)has no title attribute

#### 参考式

链接是在链接文字的括号后面在接一个方括号，而在第二个方括号里面填入用以辨识链接的标记，我不常用，不写了

### 插入图片

>插入图片语法 ```![图片的替代文字](图片URL  "title")```   

`![somi](http://p1p8cvl3a.bkt.clouddn.com/f.png "somi")`

>Tips:创建了七牛CDN的网上存储器 -- 管理控制台-对象存储-内容管理-上传文件-选择文件后即可使用链接打开图片

<!-- !["bg1"](http://p1p8cvl3a.bkt.clouddn.com/bg1.jpg) -->
<img src="http://p1p8cvl3a.bkt.clouddn.com/bg1.jpg" width="200" height="200">


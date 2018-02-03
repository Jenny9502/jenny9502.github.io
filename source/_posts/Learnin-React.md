---
title: Learning-React
date: 2018-01-16 16:59:05
tags:
<!-- header_image: http://p1p8cvl3a.bkt.clouddn.com/homeBg.jpeg -->
---
## 起步

起步步骤网上有很多[教程](http://www.ruanyifeng.com/blog/2015/03/react.html)，这里就不多说了。我下面主要是记录在学习react过程中遇到的问题以及解决方法。

个人觉得用react开发非常适合多人开发，便于组件与组件之间的维护，提高开发效率。

## 组件间传递函数

>在父组件定义的函数，在子组件调用

父组件：

```bash
//定义需要传递的函数
parentTest = (msg) => {
	console.log("msg:",msg);
}
//传递函数都子组件
<child TabClick={(msg) => {this.parentTest(msg)}}></child>

```
子组件:

```bash
//在子组件调用TabClick函数
<Test onClick={(e)=>this.props.TabClick(e)}></Test>

```
## State赋值

```bash
state={
	name: 'zyn',
	List: [2,4,7]
}

```
>只能设置this.setState({'name','hhh'})，不能设置this.setState({'list[2]',3})

## 组件基类

> Component 和 PureComponent 的区别

一般我们是通过控制组件的state，props的变化刷新组件。

```bash
//同一个state
state={
	name: 'zyn',
	List: [2,4,7]
}

```
>当设置了this.setState({'name','hhh'})，在Component和PureComponent基类下，组件都会刷新。
>当更改了list[2]的值，重新给this.setState({'list',list})赋值后，在Component下组件会刷新，而在PureComponent下组件不会刷新。
所以，在PureComponent基类下，只有state的根值发生变化才会引发组件的变化。



# 更新ing

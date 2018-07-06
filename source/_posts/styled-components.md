---
title: The Learning styled-components
date: 2018-01-16 17:04:22
tags:
---
## 安装

```bash
$ npm install --save styled-components
```

## 引入

```bash
import React from 'react';
import styled from 'styled-components';
```

## 使用

### 定义

```bash
const Div = styled.div`
	height:30px;
    如同平时写的CSS一样写属性和属性值
`;
```
### 使用

	<Div>..</Div>

### Tips

#### 1.在定义名字时：const 名字 的名字首字母必须是 **大写** 才能起作用

#### 2.使用a标签做引用时，需要结合react-router

	import {Link} from 'react-router'

	const ALink = styled(Link)`
	    如同平时写的CSS一样写属性和属性值
	`;

	<ALink to=''></ALink>
其中的 **to** 要必须存在,就是HTML a标签里的 href

#### 3.hover,action...等样式的编写

	const Div = styled.div`
		width:100px;
		&:hover,&focus,&active{
			border：1px solid #000;
		}
	`;

#### 4.编辑下级样式

	const Div = styled.div`
		width:100px;
		.active{
			border：1px solid #000;
		}
	`;

多用在引入了其他组件库时的对组件产生的盒子进行样式改动

#### 5.实现有活动状态的高亮 

	const Div = styled.div`
		color:${props => props.active ? '#000' : '#fff'}
	`;

	<Div active = {true}>黑色字体</Div>
	<Div active = {false}>白色字体</Div>

中括号里的值可以通过组件里的参数进行判断返回值，控制该Div的字体颜色

#### 6.继承其他已定义样式

    const Div = styled.div`
        background: red
    `;
    const SpanDiv = styled(Div)`
        color: #ffffff;
    `;
    这样的 SpanDiv 实际是 背景颜色为红色, 字体颜色为白色 的 div

#### 7.继承多个其他已定义样式

    const Div = styled.div`
        background: red
    `;
    const Div2 = styled.div`
           color: #ffffff
       `;
    const BigDiv = styled(Div,Div2)`
        font-size: 20px
    `;
    这样的 BigDiv 实际是 背景颜色为红色,字体颜色为白色,字体大小是20px 的 div

# 暂时学习到的只有这些，后面学到更多会继续更新
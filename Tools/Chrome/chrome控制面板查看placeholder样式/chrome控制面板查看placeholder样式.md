---
title: chrome控制面板查看placeholder样式
date: 2018-11-05 15:41:33
tags: chrome
---

在开发过程中，有时候需要查看input或者textarea的placeholder属性，chrom浏览器只需要进行如下设置即可：

1、进入需要调试的页面，按F12打开控制面板；

2、按F1打开设置，找到Elements，勾选 show user agent shadow DOM即可

![](showagentusershadowdom.png)

效果如图：  

![](showagentusershadowdom-result.png)
---
layout:     post
title:      "获取另外iframe中的元素"
subtitle:   "get element inner orther iframe"
date:       2017-02-24 16:47:00
author:     "yang"
header-img: "img/post-bg-03.jpg"
---


# get element inner orther iframe

现在有两个iframe


```
  <frameset cols="180, 10, *" framespacing="0" border="0" id="frame-body">
    <frame src="index.php?act=menu" id="header-frame" name="header-frame" frameborder="no" scrolling="yes">
    <frame src="index.php?act=main" id="main-frame" name="-frame" frameborder="no" scrolling="no">
  </frameset>
 
```
其中id为header-frame的标题栏，id为main-frame的为主操作页面，有个功能需要在main-frame中进行操作后更新header-frame的一些元素，但是直接在main-frame里使用

```
document.getElementById('id')

```
无法获取到header-frame 中的元素，最后在stackoverflow上找到了解决办法，使用




```
var hf=top.document.getElementById('header-frame');
var innerDoc = hf.contentDocument || hf.contentWindow.document;
var ele = innerDoc.getElementById('id');
 
```
就可以获取到想要的元素了。


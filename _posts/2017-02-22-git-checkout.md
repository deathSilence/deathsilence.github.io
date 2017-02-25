---
layout:     post
title:      " git checkout"
subtitle:   "git checkout 命令的作用，以及错误使用git checkout 导致HEAD分离情况的处理"
date:       2017-02-22 16:04:00
author:     "yang"
header-img: "img/post-bg-06.jpg"
---

# git checkout 命令的作用，以及错误使用git checkout 导致HEAD分离情况的处理

## 切换分支

在提交层面的git checkout 非常简单，当传入分支名的时候，就可以切换到那个分支

比如：
```
 git checkout dev
 
```
上面这个命令做的不过是将HEAD移到一个新的分支，然后更新工作目录。因为这可能会覆盖本地的修改，Git强制你提交或者缓存工作目录中的所有更改，不然在checkout的时候这些更改都会丢失。
![image](https://camo.githubusercontent.com/5d7183ad484d57e357ae45ea400ae565a533fe9a/68747470733a2f2f7777772e61746c61737369616e2e636f6d2f6769742f696d616765732f7475746f7269616c732f616476616e6365642f726573657474696e672d636865636b696e672d6f75742d616e642d726576657274696e672f30342e737667)

## 切换到任意提交


```
 commit 9fa9c5fb42053d97276ac50c1935d61c0e19ebdc
Author: fanyu1986 <18396605592@163.com>
Date:   Fri Feb 17 15:15:37 2017 +0800

    add style of account message


```
其中 9fa9c5fb42053d97276ac50c1935d61c0e19ebdc 即为提交的HEAD

使用git checkout 9fa9c5fb42053d97276ac50c1935d61c0e19ebdc 就可以将HEAD移动到这个提交

比如如果想返会当前HEAD前两次的提交 可以使用

```
git checkout HEAD~2

```
![image](https://camo.githubusercontent.com/b6b326af6c5f485ea326120dd2f1f78e741d1748/68747470733a2f2f7777772e61746c61737369616e2e636f6d2f6769742f696d616765732f7475746f7269616c732f616476616e6365642f726573657474696e672d636865636b696e672d6f75742d616e642d726576657274696e672f30352e737667)

## HEAD分离

这对于快速查看项目旧版本来说非常有用。但如果你当前的HEAD没有任何分支引用（我现在理解是没有一个分支的最后一次提交的HEAD和当前HEAD一样），那么这会造成HEAD分离。

```
头指针分离于 9fa9c5f

```


这是非常危险的，如果你接着添加新的提交，



```
[分离头指针 a1444ee] 测试HEAD分离
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 test_head.php

```


然后切换到别的分支之时git会提醒你

```
警告：您正丢下 1 个提交，未和任何分支关联：

  9fa9c5f commit message 分离

如果您想要通过创建新分支保存它，这可能是一个好时候。
如下操作：

 git branch <新分支名> 9fa9c5f

切换到分支 'master'
您的分支与上游分支 'origin/master' 一致。

```

如果使用 git branch <新分支名> 9fa9c5f  会创建一个新的分支来保存本次提交，如果没有建分支的话，HEAD分离提交后的内容就会永远消失。



### 本文参考

[5.2 代码回滚：Reset、Checkout、Revert的选择](https://github.com/geeeeeeeeek/git-recipes/wiki/5.2-%E4%BB%A3%E7%A0%81%E5%9B%9E%E6%BB%9A%EF%BC%9AReset%E3%80%81Checkout%E3%80%81Revert%E7%9A%84%E9%80%89%E6%8B%A9)

[危险！分离头指针](http://www.jianshu.com/p/91a0f8feb45d) 





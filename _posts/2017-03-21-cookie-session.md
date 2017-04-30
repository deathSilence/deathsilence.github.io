---
layout:     post
title:      "laravel cookie and session"
subtitle:   "laravel 一次登录过程中cookie和session的变化"
date:       2017-03-21 09:11:00
author:     "杨"
header-img: "img/post-bg-06.jpg"
---

# laravel cookie and session

a cookie is a bit data stored by the brower and sent to server with every request.

a session is a collection of data stored in server and associated with a given user 

(usually via a cookie containing an id code)

### 一次登录过程中的session and cookie 分析

登录后可以在浏览器COOKIE和网站数据总看到本次新增的COOKIE，

```
2 个 Cookie, 数据库存储
XSRF-TOKEN laravel_session 索引型数据库
名字：	laravel_session
内容：	eyJpdiI6IkRwdSt5YU54a2JpZW9CZEtaUDVlRkE9PSIsInZhbHVlIjoiMGhmSEV1dWtEa0hTbmRwdld6ZGtGeTdvRnA1c3JQQUtNbHp3UzlnbnBONEZKNW4wYlZzVTdBQmFvZUw2N3RBSmxBdmRHSFVza0ZVQ0c3NzVSZUVpWHc9PSIsIm1hYyI6IjBlYTllNzY1YWE4ZjQ3NDFlMmYxMTE1Y2JhNWUwNzYwNGRhYTg1YzFiZWQ5MmM3OTkyMmMzNTE5ZjE1MmM3NzAifQ%3D%3D
域：	jindongjiaju.dev
路径：	/
发送用途：	各种连接
脚本可访问：	否（仅 Http）
创建时间：	2017年3月21日星期二 上午8:15:14
过期时间：	浏览会话结束时
```
在项目storage/framework/sessions目录下页会新增一个文件

```
 a:5:{s:6:"_token";s:40:"avHuCtCRLM5Xzo6F1s2jNWYxbRsnwuEF4wTbkGkG";s:3:"url";a:0:{}s:9:"_previous";a:1:{s:3:"url";s:28:"http://jindongjiaju.dev/test";}s:6:"_flash";a:2:{s:3:"old";a:0:{}s:3:"new";a:0:{}}s:50:"login_web_59ba36addc2b2f9401580f014c7f58ea4e30989d";i:41;}
 
```

我们来看下cookie中都存了哪些数据，使用var_dump($request->cookie()),结果如下


```
 array(2) { ["XSRF-TOKEN"]=> string(40) "avHuCtCRLM5Xzo6F1s2jNWYxbRsnwuEF4wTbkGkG" ["laravel_session"]=> string(40) "2DQPGRjmUYnlRl3fA5akENKLqxyk0Q22b0Mxuwv8" }
 
```
cookie中存了XSRF-TOKEN,和一个session字符串

使用var_dump($request->session())看下session中的数据


```
 object(Illuminate\Session\Store)#382 (5) { ["id":protected]=> string(40) "2DQPGRjmUYnlRl3fA5akENKLqxyk0Q22b0Mxuwv8" ["name":protected]=> string(15) "laravel_session" ["attributes":protected]=> array(5) { ["_token"]=> string(40) "avHuCtCRLM5Xzo6F1s2jNWYxbRsnwuEF4wTbkGkG" ["url"]=> array(0) { } ["_previous"]=> array(1) { ["url"]=> string(28) "http://jindongjiaju.dev/test" } ["_flash"]=> array(2) { ["old"]=> array(0) { } ["new"]=> array(0) { } } ["login_web_59ba36addc2b2f9401580f014c7f58ea4e30989d"]=> int(41) } ["handler":protected]=> object(Illuminate\Session\FileSessionHandler)#383 (3) { ["files":protected]=> object(Illuminate\Filesystem\Filesystem)#138 (0) { } ["path":protected]=> string(56) "/home/yang/Sites/jindongjiaju/storage/framework/sessions" ["minutes":protected]=> int(120) } ["started":protected]=> bool(true) }
```
我们可以看到session中的CSRF-TOKEN和Cookie中的是一样的，并且session中的id也等于cooki中的session字符串,laravel 应该是通过cookie和session中的数据关联起来来实现CSRF-TOKEN认证，和保持会话状态的。







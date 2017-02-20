---
layout:     post
title:      "ngrok快速使用"
date:       2017-02-18 12:00:00
author:     "yang"
header-img: "img/post-bg-05.jpg"
---

### ngrok的原理不是特别清楚，网上有很多使用的教程感觉太麻烦了。这里分享下ngrok文件和简单的配置

### 下载链接： [ngrok下载地址](http://lucksevenl7.github.io/share/ngrok.tar.gz)

### 下载解压后，首先要确保ngrok有可以执行的权限，然后执行./ngrok -config=ngrok.cfg -subdomain=example 80,其中example为公网可以访问到的域名比如baidu，建议填写自己的或者自己公司的域名。 80 为要转发的端口，可以根据自己的需求改变。
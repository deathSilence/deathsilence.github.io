---
layout:     post
title:      "ubuntu 14.04 升级到 16.04"
subtitle:   "在 ubuntu 14 各种软件源比较旧，升级软件还需要手动添加源，比较麻烦，直接升级系统。"
date:       2017-03-03 16:00:00
author:     "学长"
---

# 一、升级当前系统 
- 1、我的系统版本是 14.04.2，升级会报错，升级到 14.04.4 以上

```
cat /etc/lsb-release			#查看系统版本
sudo apt-get update			#升级软件列表
sudo apt-get upgrade			#升级软件
sudo apt-get dist-upgrade		#升级当前系统版本
```

# 二、开始升级

 - 1、执行系统升级命令

```
sudo do-release-upgrade -d
```

# 三、升级过程中的问题

- 1、提示选择软件配置，一般选择 "Yes" 升级版本

- 2、mysql 配置

 - 在 ubuntu14 中 mysql 版本是5.5，开启了慢查询日志，结果升级到 5.7时，无法读取配置文件，注释掉慢查询的两条语句

```
#log_slow_queries       = /var/log/mysql/mysql-slow.log
#long_query_time = 2
```

- 3、php-fpm 配置

 - php5.5 升级到 php7.0之后将 nginx里的 php5-fpm 改为 php7.0-fpm 的配置

```
fastcgi_pass unix:/run/php/php7.0-fpm.sock;
``` 

- 4、nginx 配置报错(unknown log format "access")

 - 在 include 之前添加

```
log_format access ‘$remote_addr – $remote_user [$time_local] “$request” ‘‘$status $body_bytes_sent “$http_referer” ‘‘”$http_user_agent” $http_x_forwarded_for’;
```

- 5、升级软件报错

 - 错误信息: (N: Ignoring file '50unattended-upgrades.ucf-dist' in directory '/etc/apt/apt.conf.d/' as it has an invalid filename extension)

```
sudo rm /etc/apt/apt.conf.d/50unattended-upgrades.ucf-dist
```

- 6、只安装了php7.0-mysql 和 php7.0-fpm ,不造漏掉几个... 

```
sudo apt-get install php7.0-fpm php7.0-mysql php7.0-common php7.0-curl ...
```

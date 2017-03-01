---
layout:     post
title:      "mysql主从复制实现数据库双向同步"
subtitle:   "两个数据库分别作为 master 和 slave 实现两个数据库相互复制，并忽略不需要同步的表"
date:       2017-03-01 20:00:00
author:     "学长"
---

# Mysql 主从数据库搭建 

## 条件
主服务器（master a ip: 192.168.1.2）、从服务器（slave b ip: 192.168.1.3）、相同版本的数据库（因 master 数据库版本不能高于 slave 服务器，以5.7为例）

## 步骤 
- 修改配置文件
 - 修改主数据库配置文件
```
vim /etc/mysql/mysql.conf.d/mysqld.cnf
server-id = 1
log_bin = /var/log/mysql/mysql-bin.log
binlog_do_db = databasedo
binlog_ignore_db = databaseignore
replicate_wild_ignore_table = database1.table1
slave-skip-errors=all
```  
 - 修改从数据库配置文件
```
vim /etc/mysql/mysql.conf.d/mysqld.cnf
server-id = 2
log_bin = /var/log/mysql/mysql-bin.log
binlog_do_db = databasedo
binlog_ignore_db = databaseignore
replicate_wild_ignore_table = database1.table1
slave-skip-errors=all
```
 - 修改完配置后重启数据库
 - 在主数据库里添加用于同步的账号(slave)，并查看主数据库(192.168.1.2)的 master 信息,
```
mysql -u root -p
grant replication slave on *.* to 'slave'@'192.168.1.3' identified by 'password';
show master status\G;
```
 - 记下主数据库的 File 和 Position 值(假设值为 File:mysql-bin.000001,Position:110)，连接从数据库(192.168.1.3)
```
mysql -uroot -p
change master to master_host='192.168.1.2',master_user='slave',master_password='password',master_log_file='mysql-bin.000001',master_log_pos=110;
start slave;
```
 - 如果主数据库端口修改过，需要加一条 (master_port=3306),查看从数据库的 slave 状态
```
show slave status\G;
```
 - 若 Slave_IO_Running 和 Slave_SQL_Running 均显示 Yes ，则说明 主-->从 复制设置成功。从--->主 设置步骤同样的方法。

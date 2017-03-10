---
layout:     post
title:      "laravel-schedule"
subtitle:   "laravel中使用定时任务的几点注意事项"
date:       2017-03-10 16:38:00
author:     "杨"
header-img: "img/post-bg-04.jpg"
---

# laravel中使用定时任务的几点注意事项

### 在App\Console\Kernel;文件下注册自己的任务
    
    比如：
    
```
 	protected function schedule(Schedule $schedule) {
		$schedule->command('backup:clean')->daily()->at('16:30');
		$schedule->command('backup:run')->daily()->at('17:00');
	}
```
有一点需要注意的是添加到schedule里的命令要在php aritsan list里可以找到才行，如果找不到的话需要在下面的数组里注册


```
	protected $commands = [

	];
	
```

### 在cron里配置定时任务

    

```
cron -l 查看当前的任务

cron -e 编辑新的任务 

```

使用cron -e，将下面的命令添加上


```
 * * * * * php /your project full path /artisan  schedule:run
```
比如我的



 
```
* * * * * php /home/yang/Sites/jindongjiaju/artisan  schedule:run

```
我的php是添加到环境变量里面的，可以直接使用，如果没有添加到全局变量，需要添加php的路径



```
which is php

php -i

这两条命令都可以查看php的路径

```
添加完后，可以在命令行里执行下，看看配置的是否有错误，如果执行没有报错，说明配置成功了。

### 修改项目的timezone
    
需要将项目的timezone 修改成本地的timezone,这一点很重要

在config/app.php 找到timezone 字段

默认是

 
```
 'timezone' => 'UTC',
```
修改成


```
'timezone' => 'Asia/Shanghai',

```

### 然后就测试下吧  哈哈

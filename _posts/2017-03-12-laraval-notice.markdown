---
layout:     post
title:      "laravel-notice"
subtitle:   "laravel useful method"
date:       2017-03-12 17:04:00
author:     "杨"
header-img: "img/post-bg-05.jpg"
---

# laravel笔记

1. 检测table中是否含有某字段

    
```
use Schema;
Schema::hasColumn($this->model->getTable(), 'DateTime');

```

2. 在html标签内部使用blade 标签


```
    style="{!! $entry->StorkType==1? '':'display:none' !!}" 
 
```



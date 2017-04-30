---
layout:     post
title:      "php mysql rank list"
subtitle:   "使用php和mysql生成带排名的数据"
date:       2017-04-25 09:15:31
author:     "杨"
header-img: "img/post-bg-06.jpg"
---

# php mysql 生成排名

## 方法一：直接使用mysql中的函数
    
```
select id,count，FIND_IN_SET( count, (SELECT GROUP_CONCAT( count ORDER BY count DESC ) 
FROM table_name )) AS rank
from table_name  order by rank;

```
这种方法比较省代码，但是不易读，查询效率也较低

## 方法二：php函数

```
     public  function actor_rank_list(){
          $result= DB::select('select id,count from table_name  order by count DESC ');
          $same=0;
          $rank=1;
          $length=count($result);
          for ($i=0; $i < $length; $i++) { 
            if ($i==0) {
              $result[$i]->rank=$rank;
              continue;
            }

            if ($result[$i]->count==$result[$i-1]->count) {
              $same=$same+1;
              $result[$i]->rank=$rank;
            }else{
              $rank=$rank+$same+1;
              $same=0;
              $result[$i]->rank=$rank;
            }

          }
          return $result;



     }
```



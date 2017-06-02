---
layout:     post
title:      "微信分享"
subtitle:   "微信分享解决特殊符号转换成html 实体的问题"
date:       2017-06-02 08:26:00
author:     "yang"
header-img: "img/post-bg-06.jpg"
---

## 在微信分享时，如果分享的标题里面含有下划线，空格等特殊符号，会转化成&mdash等特殊符号

微信分享时应该是为了安全先把特殊符号先转换成html实体，在php里htmlentities()可以做到

```
<?php
$str = "A 'quote' is <b>bold</b>";

// Outputs: A 'quote' is &lt;b&gt;bold&lt;/b&gt;
echo htmlentities($str);

// Outputs: A &#039;quote&#039; is &lt;b&gt;bold&lt;/b&gt;
echo htmlentities($str, ENT_QUOTES);
?>
```
这样我认为可以使用html_entity_decode()来转换回来


```
<?php
$orig = "I'll \"walk\" the <b>dog</b> now";

$a = htmlentities($orig);

$b = html_entity_decode($a);

echo $a; // I'll &quot;walk&quot; the &lt;b&gt;dog&lt;/b&gt; now

echo $b; // I'll "walk" the <b>dog</b> now
?>
```

结果在微信分享时没有起作用，可能是我打开姿势不对，然后有在stackoverflow上找到了一个黑科技


```
    function decodeHtml(html) {
        var txt = document.createElement("textarea");
        txt.innerHTML = html;
        return txt.value;
    }

```
这个方法太机智了
然后这个问题就解决了。。。




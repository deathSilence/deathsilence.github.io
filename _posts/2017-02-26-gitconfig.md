---
layout:     post
title:      "gitconfig配置"
subtitle:   ".gitconfig 有用的配置"
date:       2017-02-24 16:47:00
author:     "yang"
header-img: "img/post-bg-06.jpg"
---




# .gitconfig配置

看到大神同事的.gitconfig配置，很有用，分享一下



 
```
[alias]
    	lg1 = log --graph --all --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%cr)%C(reset) %C(white)%s%C(reset) %C(bold white)— %cn%C(reset)%C(bold yellow)%d%C(reset)' --abbrev-commit --date=relative
   	lg2 = log --graph --all --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%cD%C(reset) %C(bold green)(%cr)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(bold white)— %cn%C(reset)' --abbrev-commit
    	lg = !"git lg1" 

```

alias 是假名的意思.也就是说，执行git lg1时其实是执行后面一长串的命令，下面看效果

![image](http://outcage.xyz/img/git_lg2.png)

[.gitconfig配置参考](https://gist.github.com/pksunkara/988716)

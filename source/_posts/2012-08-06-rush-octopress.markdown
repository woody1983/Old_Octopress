---
layout: post
title: "Rush OctoPress"
date: 2012-08-05 19:37
comments: true
categories:  [octopress]
---

`Git`+`Octopress`+`Ruby`

>终于装好了 有些细节还需要继续调整 右边没有分类边框太特么反人类了.下次继续完善安装过程和步骤。
<!-- more -->

```
$ curl -L https://get.rvm.io | bash -s stable --ruby

#给 ~/.bashrc (Linux) 或 ~/.bash_profile (Mac 环境) 加上脚本引用，执行下面这行（注意修改文件名）:

$ echo '[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # This loads RVM into a shell session.' >> ~/.bashrc

# 如果是 Linux 
$ source ~/.bashrc

#first install openssl
rvm pkg install openssl

rvm install 1.9.3 -C --with-openssl-dir=$HOME/.rvm/usr
rvm use 1.9.3
rvm rubygems latest
rvm install rbenv
```

###Depoly三部曲
>建立一个source的分支用来保存博客源文件

#最重要的一点 在Github上配置项目地址
rake setup_github_pages
`//键入你的git源 git://github.com/username/username.github.com.git`

```
rake generate #生成静态文件
rake preview #本地预览
rake deploy #发布到github 只是第一次上传会比较慢一些 后面就快很多

git remote add octopress_github git@github.com:username/username.github.com.git
git add .
git commit -m "blog source md or something"
git push octopress_github source
```

``` c Address.c Source Code|测试一下贴代码功能 爽
#include <stdio.h>
void main(void)
{
    int x;
    int y;
    x=0x76543210;
    y=0xfedcba98;

    printf("\n%x",&y);
    printf("\n%d %d",x,y);
}
/*演示变量地址和变量的值*/
```


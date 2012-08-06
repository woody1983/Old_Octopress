---
layout: post
title: "Rush OctoPress"
date: 2012-08-05 19:37
comments: true
categories:  [OctoPress]
---

`"Fortune favors the brave" Vergil`

>终于装好了 有些细节还需要继续调整 右边没有分类边框太特么反人类了.下次继续完善安装过程和步骤。

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

``` c Address.c Source Code
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


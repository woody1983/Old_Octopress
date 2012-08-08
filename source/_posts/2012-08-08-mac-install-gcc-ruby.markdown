---
layout: post
title: "mac安装gcc和ruby"
date: 2012-08-08 20:27
comments: true
categories: git,mac,ruby,octopress
---

#### gcc
先说gcc 我这个mac安装过Xcode4.4 但安装过程中还是提示少c编译环境 查了一下需要安装xcode的插件

安装command line tools。具体路径为：`Xcode –> Preferences –> Downloads 的Components`下，选择安装Command Line Tools 即可。

#### wget
我下载的是source code编译安装的

```
wget http://ftp.gnu.org/gnu/wget/wget-1.14.tar.gz
tar zxvf wget-1.14.tar.gz
cd wget-1.14
./configure --with-ssl=openssl
make
make install
```

#### Yaml
```
wget http://pyyaml.org/download/libyaml/yaml-0.1.4.tar.gz`
解tar 编译安装 不装这个东西 rvm安装ruby总是报错
```

#### rvm 
``` 
bash -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)
rvm --version

xjmatoMacBook-Air:ruby-1.9.3-rc1 xj$ rvm install 1.9.3
Fetching yaml-0.1.4.tar.gz to /Users/xj/.rvm/archives
Extracting yaml-0.1.4.tar.gz to /Users/xj/.rvm/src
Configuring yaml in /Users/xj/.rvm/src/yaml-0.1.4.
Compiling yaml in /Users/xj/.rvm/src/yaml-0.1.4.
Installing yaml to /Users/xj/.rvm/usr
Building 'ruby-1.9.3-p194' using clang - but it's not (fully) supported, expect errors.
Installing Ruby from source to: /Users/xj/.rvm/rubies/ruby-1.9.3-p194, this may take a while depending on your cpu(s)...
中间需要download文件 
Install of ruby-1.9.3-p194 - #complete
Ruby 'ruby-1.9.3-p194' was built using clang - but it's not (fully) supported, expect errors.
xjmatoMacBook-Air:ruby-1.9.3-rc1 xj$ ruby -v
ruby 1.9.3p194 (2012-04-20 revision 35410) [x86_64-darwin11.4.0]
xjmatoMacBook-Air:ruby-1.9.3-rc1 xj$ gem -v
1.8.24
安装完以后看一下
```

#### bundle 
`bundle install`

这个是最关键的一步 进入项目文件里面 bundle所有需要的gem包


#### rake for octopress

日常维护octopress常用的命令大概就这么几个 分别为git和rake的

```
rake generate 声称静态页面
rake preview 预览
rake deploy 发布
```

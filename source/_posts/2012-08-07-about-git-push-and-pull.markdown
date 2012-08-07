---
layout: post
title: "关于Git的push & pull"
date: 2012-08-07 19:39
comments: true
categories: git
---

### 首先是两端同时维护的问题

同一个git的`Repo` 我如果需要在`office`和`home`里分开维护的话 数据的一致性是最主要的。
比如我在家里写的blog，早上到公司以后就需要先pull下来最新版本的Repo 然后在一天中任何一个时间点或时间段都会随意的维护工作记录或笔记什么的。

现在的做法是把没写完的blog先`push`到一个`TODO的branch`里 然后push到远端一个分支 也叫todo

```
woodyxu@woodyxu-ThinkPad-R61-neo:~/woody1983.github.com$ git checkout -b todo
Switched to a new branch 'todo'
woodyxu@woodyxu-ThinkPad-R61-neo:~/woody1983.github.com$ git branch
  master
  source
* todo
创建一个todo的分支 

woodyxu@woodyxu-ThinkPad-R61-neo:~/woody1983.github.com$ git add .
woodyxu@woodyxu-ThinkPad-R61-neo:~/woody1983.github.com$ git commit -m "todo branch : save temp file"
[todo 92e67d5] todo branch : save temp file
 1 file changed, 14 insertions(+)
 create mode 100644 source/_posts/2012-08-07-about-git-push-and-pull.markdown
woodyxu@woodyxu-ThinkPad-R61-neo:~/woody1983.github.com$ git push origin todo
Counting objects: 8, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (5/5), 883 bytes, done.
Total 5 (delta 2), reused 0 (delta 0)
To git@github.com:woody1983/woody1983.github.com.git
 * [new branch]      todo -> todo
```


>先merge一下 看能不能合并和发布出去。


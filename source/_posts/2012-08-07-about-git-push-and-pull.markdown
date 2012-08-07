---
layout: post
title: "关于Git的push & pull"
date: 2012-08-07 19:39
comments: true
categories: git
---

### 首先是两端同时维护的问题

同一个git的Repo 我如果需要在office和home里分开维护的话 数据的一致性是最主要的。
比如我在家里写的blog，早上到公司以后就需要先pull下来最新版本的Repo 然后在一天中任何一个时间点或时间段都会随意的维护工作记录或笔记什么的。

现在的做法是把没写完的blog先push到一个TODO的branch里

Github
======

About git

```
git checkout -b new_branch || git branch new_office
git checkout branch/master
git diff new_branch master
git merge new_branch {makesure you save master*}

git add -A 
git commit -m "make clean"
git push
```

>cool! I Love Git.

`2012-08-06 Commit`

`2012-08-07 Update source.branch something`

`2012-08-07 19:33 Back Home`

add about `git pull` something

```
  392  git checkout origin/master -b master #把master的分支也checkout出来
  402  git pull origin master
  403  git pull origin source
```

``` shell 
  570  git clone git@github.com:woody1983/woody1983.github.com.git
  571  cd woody1983.github.com/
  572  ls -lah
  573  git remote add octopress_github git@github.com:woody1983/woody1983.github.com.git
  574  git branch
  575  git branch -r
  577  git checkout origin/source -b source
  578  git branch
  579  ls -lah
  580  vi README.markdown
  581  git add .
  582  git commit -m "update source.branch"
  583  git push octopress_github source
  584  history
```

![Alt text](http://img1.douban.com/view/group_topic/large/public/27626318-1.jpg)

###初始化clone这个Project

clone过来以后默认的远端分支都在会里面 分别checkout出来
很奇怪的是 如果我先checkout `todo`这个branch 剩下的`source`就checkout不出来了。
今天测试一下 

先在todo里面 commit 然后切换到source里`merge` 最后push到两个分支

``` shell
woodyxu@woodyxu-ThinkPad-R61-neo:/tmp/woody1983.github.com$ git branch -r
  origin/HEAD -> origin/master
  origin/master
  origin/source
  origin/todo

woodyxu@woodyxu-ThinkPad-R61-neo:/tmp/woody1983.github.com$ git branch
* master
woodyxu@woodyxu-ThinkPad-R61-neo:/tmp/woody1983.github.com$ git checkout source
Branch source set up to track remote branch source from origin.
Switched to a new branch 'source'
woodyxu@woodyxu-ThinkPad-R61-neo:/tmp/woody1983.github.com$ git checkout todo
Branch todo set up to track remote branch todo from origin.
Switched to a new branch 'todo'
```

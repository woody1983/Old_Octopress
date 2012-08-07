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

`2012-08-06 Update source.branch something`

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

This is my Objective-C Project

``` shell
[root@mysql-test-woody just_c]# git add -A
[root@mysql-test-woody just_c]# git commit -m "make clean"
[master 8db2d64] make clean
 31 files changed, 0 insertions(+), 0 deletions(-)
 rename Address.c => C_Source/Address.c (100%)
 rename FreeTest.c => C_Source/FreeTest.c (100%)
 rename FreeTest_Copy.c => C_Source/FreeTest_Copy.c (100%)
 rename MallocAndFree.c => C_Source/MallocAndFree.c (100%)
 rename MallocAndFree_Copy.c => C_Source/MallocAndFree_Copy.c (100%)
 rename Pint_array.c => C_Source/Pint_array.c (100%)
 rename array.c => C_Source/array.c (100%)
 rename def.c => C_Source/def.c (100%)
 rename demo01.c => C_Source/demo01.c (100%)
 rename demo02.c => C_Source/demo02.c (100%)
 rename demo3.c => C_Source/demo3.c (100%)
 rename demo4.c => C_Source/demo4.c (100%)
 rename demo5.c => C_Source/demo5.c (100%)
 rename demo6.c => C_Source/demo6.c (100%)
 rename demo_string.c => C_Source/demo_string.c (100%)
 rename dim_array.c => C_Source/dim_array.c (100%)
 rename diy_main.c => C_Source/diy_main.c (100%)
 rename double_test.c => C_Source/double_test.c (100%)
 rename hello.c => C_Source/hello.c (100%)
 rename if_demo1.c => C_Source/if_demo1.c (100%)
 rename if_demo2.c => C_Source/if_demo2.c (100%)
 rename main.c => C_Source/main.c (100%)
 rename pint.c => C_Source/pint.c (100%)
 rename ptest.c => C_Source/ptest.c (100%)
 rename puts.c => C_Source/puts.c (100%)
 rename scan_demo.c => C_Source/scan_demo.c (100%)
 rename scan_demo01.c => C_Source/scan_demo01.c (100%)
 rename swap2.c => C_Source/swap2.c (100%)
 rename swap2_copy.c => C_Source/swap2_copy.c (100%)
 rename switch.c => C_Source/switch.c (100%)
 rename while_demo.c => C_Source/while_demo.c (100%)
[root@mysql-test-woody just_c]# git push
Counting objects: 4, done.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 1.14 KiB, done.
Total 3 (delta 1), reused 0 (delta 0)
To git@github.com:woody1983/just_c.git
   fc7f53d..8db2d64  master -> master
```

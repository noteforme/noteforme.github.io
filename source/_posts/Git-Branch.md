---
title: Git_Branch
comments: true
date: 2017-09-16 14:52:41
tags:
<<<<<<< HEAD
categories:  Git
=======
categories: Git
>>>>>>> 2912127c99b6d2d8ff21b897405bf30acee204f3
---
#svn项目修改为Git 

 在 .idea/vcs.xml 修改<mapping directory="" vcs="Git" />  

# github上传项目 

<<<<<<< HEAD
* 分支创建
  `$ git branch testing `
=======
```
git init
git remote add origin git@github.com:BlogForMe/fff.git
git add .
git commit -m "initialization"
git push -u origin master
 
```

# Git分支操作


* 分支创建

  `$ git branch testing`


* 添加另一个远程仓库

  `$ git fetch origin `

* Git分支切换

   `$ git checkout testing`

* 分支提交到远程仓库

   `$ git push origin testing `

* [显示远程仓库](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E7%9A%84%E4%BD%BF%E7%94%A8)

 `$ git remote show origin `
  

  
* Git分支切换
    ` $ git checkout testing`
  

    
 [git分支简介](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%AE%80%E4%BB%8B)
http://blog.csdn.net/hyr83960944/article/details/36185231
---
title: Git_Branch
comments: true
date: 2017-09-16 14:52:41
tags:
categories:  GIT

---
#svn项目修改为Git 

 在 .idea/vcs.xml 修改<mapping directory="" vcs="Git" />  

# github上传项目 

* 分支创建
  `$ git branch testing `
```
git init
git remote add origin git@github.com:BlogForMe/fff.git
git add .
git commit -m "initialization"
git push -u origin master

```

##  Git分支操作


* 分支创建

  `$ git branch testing`


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

## 远程仓库更新到本地

```
$ git fetch origin
$ git merge origin/hexo
```

或者合并

```
$ git pull origin hexo 
```


http://www.ruanyifeng.com/blog/2014/06/git_remote.html
    

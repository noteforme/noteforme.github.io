---
title: GIT_BRANCH(git和分支操作)
comments: true
date: 2017-10-19 09:24:09
tags:
categories: GIT

---

# Git基本操作

##  svn项目修改为Git 

 在 .idea/vcs.xml 修改<mapping directory="" vcs="Git" />  

##  github上传项目 

*  分支创建
`$ git branch testing `
*  下面是对已有项目的提交

```
mkdir EffectiveJava
cd EffectiveJava
git init
touch README.md
git add README.md
git commit -m "first commit"
git remote add origin git@gitee.com:huaiyi/EffectiveJava.git
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

*  合并分支
```
$ git checkout master
$ git merge hotfix
```
[合并分支](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6) 


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


#  fork后的项目处理
  https://gaohaoyang.github.io/2015/04/12/Syncing-a-fork/

## 忽略文件已提交的文件

 
  假如要忽略 .idea/misc.xml文件，.gitignore可以添加 `/.idea/* `把.idea文件过滤

 主要是　`git rm --cached　.idea/misc.xml ` 然后提交修改,每次手输入才有用?
  

#  Git仓库迁移

##  迁移git仓库

* 原来托管于github,clone一份裸版本库
 
      `git clone --bare git@gitee.com:huaiyi/CQJ.git`

* 在新的版本库(gitlab)里面创建一个新的项目，例如 cqianjia

* 推送刚才clone的镜像到gitlab服务器

``` 
cd CQJ.git/
git push --mirror git@45.77.22.97:root/cqianjia.git
```

 mirror 克隆出来的裸版本对上游版本库进行了注册，这样可以在裸版本库中使用git fetch命令和上游版本库进行持续同步

*   删除本地代码

```
cd ..
rm -rf project.git
```


 *  到新的服务器clone到本地就Ok了
 
  `  git clone git@45.77.22.97:root/cqianjia.git` 
 
  参考: https://my.oschina.net/kind790/blog/510601
  
  *   git clone 所有分支
  这种方式有一个弊端，切换分支后　工程名没了， 还是慢慢摸索吧　!
  
```
git branch -r | grep -v '\->' | while read remote; do git branch --track "${remote#origin/}" "$remote"; done
git fetch --all
git pull --all
```


参考: http://blog.csdn.net/allangold/article/details/78028709
  
  
    
 
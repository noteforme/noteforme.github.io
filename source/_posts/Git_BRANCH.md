---
title: GIT_BRANCH(分支操作)
comments: true
date: 2017-10-19 09:24:09
tags: 
categories: GIT

---

官方文档
https://git-scm.com/book/zh/v2
http://iissnan.com/progit/

#### Git基本操作

#####  github上传项目 

*  分支创建

```
 git branch Dev1.10 
 git push origin Dev1.10 　　　// 提交该分支到远程仓库
 git pull origin dev                         //从远程获取分支
       
```

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

#####  分支操作

分支提交到远程仓库
`$ git push origin v1.0.0 `

* commit

  Git撤销git commit 但是未git push的修改

  1. 找到上次git commit的 id

       git log 

       找到你想撤销的commit_id

  2. git reset --hard commit_id

         完成撤销,同时将代码恢复到前一commit_id 对应的版本。

  3. git reset commit_id 

       完成Commit命令的撤销，但是不对代码修改进行撤销，可以直接通过git commit 重新提交对本地代码的修改。

  

删除本地分支
` git branch -d v1.0.0 `

删除远程分支
`git push origin :foo`

  [git分支简介](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%AE%80%E4%BB%8B)
http://blog.csdn.net/hyr83960944/article/details/36185231

合并分支
合并hotfix到dev

```
$ git checkout            // 先切换到dev 
$ git merge hotfix
```
[合并分支](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6) 

#####　  远程分支

* 获取本地没有的远程分支

   ```
   git fetch origin step1 命令来把远程分支拉到本地
   
   git checkout -b step1 origin/step1在本地创建分支step1并切换到该分支
   
   git pull origin step1就可以把某个分支上的内容都拉取到本地了   //这个方法有空再实践下
   ```

   [官方](https://git-scm.com/book/zh/v1/Git-%E5%88%86%E6%94%AF-%E8%BF%9C%E7%A8%8B%E5%88%86%E6%94%AF "官方参考")

* [显示远程仓库](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E7%9A%84%E4%BD%BF%E7%94%A8)
   `$ git remote show origin `

   


##### 修复已发布版本bug

修复bug主要以下几步:

- 使用git reset --hard <commit id>命令退回到发布标签对应的版本

- 使用git checkout -b BugFix新建一个BugFix的分支，原分支前进到最新提交版本

- 使用git checkout BugFix切换到BugFix分支，修改bug，重新发布并使用git tag打标签

- git reaset --hard <head commit id> 切换主干最新的分支

- 使用git merge合并BugFix分支到主分支

  参考:<https://blog.masterliu.net/git-retag/>

  

  标签是一个文件快照，并不是真拉出一份代码放在了那里

```
D:\Project\ASProjects\cqianjia>git tag
v1.0.2
```

参考: http://gepeiyu.com/2017/06/28/git-tag-oldversion-debug/

还有一种情况是在最新提交版本修复bug

* git stash

  <https://www.jianshu.com/p/54b8ea4317cc>

  

  https://qiita.com/hudichao/items/d665cd769ed1d2ce832a

##### 远程仓库更新到本地

```
$ git fetch origin
$ git merge origin/hexo
```

或者合并

```
$ git pull origin hexo 
```
http://www.ruanyifeng.com/blog/2014/06/git_remote.html


######  fork后的项目处理
  https://gaohaoyang.github.io/2015/04/12/Syncing-a-fork/

##### 忽略文件已提交的文件

 对于单个文件处理
  假如要忽略 .idea/misc.xml文件，.gitignore可以添加 `/.idea/* `把.idea文件过滤
 主要是　`git rm --cached　.idea/misc.xml ` 然后提交修改,每次手输入才有用?

 对文件夹的处理,比如common-bankcard

```
git rm -r --cached common-bankcard
git rm --cached  *idea/*
```


####  Git仓库迁移

 迁移git仓库

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


##### 修改远程仓库地址 

* github创建 New项目，项目地址是 git@github.com:BlogForMe/News.git
* 修改远程服务器地址 ：`git remote set-url origin git@github.com:BlogForMe/News.git`
* 推送到远程服务器 : `git push origin `


http://jcpplus.github.io/2015/07/23/modify-remote-url/



##### GIT标签

  查看所有的版本 
​    `git tag`

- 查看远程分支  `git ls-remote --tags`
- 创建标签 `git tag -a v1.0.2 -m "my version 1.0.2"  `
  -m 选项指定了一条将会存储在标签中的信息


* 创建Tag

```
   git tag -a v1.0.0 -m "my version 1.0.0"
   git push origin v1.0.0    //推送到远程分支
   git push origin --tags       //推送所有的标签
```

* 删除tag 

``` 
   git tag -d <tagname> 
   git push origin :refs/tags/<tagname>

  例如
   git push origin :refs/tags/v1.0.2
   git tag -d  v1.0.2

```
[检出标签](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%89%93%E6%A0%87%E7%AD%BE) 检出标签
在 Git 中你并不能真的检出一个标签，因为它们并不能像分支一样来回移动。 如果你想要工作目录与仓库中特定的标签版本完全一样，可以使用 git checkout -b [branchname] [tagname] 在特定的标签上创建一个新分支：

		$ git checkout -b version2 v2.0.0
Switched to a new branch 'version2'

当然，如果在这之后又进行了一次提交，version2 分支会因为改动向前移动了，那么 version2 分支就会和 v2.0.0 标签稍微有些不同，这时就应该当心了。

修复已发布版本Bug
http://gepeiyu.com/2017/06/28/git-tag-oldversion-debug/

* SVN不能添加文件:https://blog.csdn.net/yujiayinshi/article/details/51381942

  ##### 如何在 GitHub 下载某个程序的特定版本

  git clone 下载源码

  git tag　列出所有版本号

  git checkout　+某版本号　

  git checkout 1.1 (如果提示需要回退那么输入 git checkout -b 1.1

  * 问题

    版本重复的话问题:error: dst refspec v1.0.0 matches more than one
    然后`git push origin :refs/heads/v1.0.0`就可以了
    [参考](https://git-scm.com/book/zh/v1/Git-%E5%88%86%E6%94%AF-%E8%BF%9C%E7%A8%8B%E5%88%86%E6%94%AF) 

  * Git  clone 非 master 分支的代码
    https://gaohaoyang.github.io/2016/07/07/git-clone-not-master-branch/

  * svn项目修改为Git 
    在 .idea/vcs.xml 修改<mapping directory="" vcs="Git" />  

  

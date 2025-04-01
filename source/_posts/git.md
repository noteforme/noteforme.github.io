---
id: 132
title: GIT
date: '2024-05-21T09:30:32+00:00'
author: Jon
layout: post
guid: 'https://jonblog.site/?p=132'
permalink: /2024/05/21/git/
categories:
    - TOOL
---

官方文档
https://git-scm.com/book/en/v2
http://iissnan.com/progit/

## Git基本操作

### github上传项目

* 分支创建

```
 git branch Dev1.10 
 git push origin Dev1.10 　　　// 提交该分支到远程仓库
 git pull origin dev          //从远程获取分支
```

* 下面是对已有项目的提交

```
git remote add origin git@gitee.com:huaiyi/EffectiveJava.git
git push -u origin master
```

分支提交到远程仓库
`$ git push origin v1.0.0 `

### commit

  Git撤销git commit 但是未git push的修改

1. 找到上次git commit的 id
   
     git log 
   
     找到你想撤销的commit_id

2. git reset --hard commit_id
   
       完成撤销,同时将代码恢复到前一commit_id 对应的版本。

3. git reset commit_id 
   
     完成Commit命令的撤销，但是不对代码修改进行撤销，可以直接通过git commit 重新提交对本地代码的修改。
   
   [git分支简介](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%AE%80%E4%BB%8B)
   http://blog.csdn.net/hyr83960944/article/details/36185231

合并分支
合并hotfix到dev

```
$ git checkout            // 先切换到dev 
$ git merge hotfix
```

[合并分支](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6)

## 远程分支

### 获取本地没有的远程分支

```
git branch -r #查看远程分支  或 git branch -a #查看所有分支
```

会显示

```
origin/HEAD -> origin/master
origin/daily/1.2.2
origin/master
```

然后直接 

```
git checkout -b <local-branch-name> origin/<remote-branch-name>
```

* [显示远程仓库](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E7%9A%84%E4%BD%BF%E7%94%A8)
   `$ git remote show origin `
  
   **1. 本地分支重命名(还没有推送到远程)**
  
  ```
  git branch -m oldName newName
  ```
  
   **2. 远程分支重命名 (已经推送远程-假设本地分支和远程对应分支名称相同)**
    a. 重命名远程分支对应的本地分支
  
  ```
  git branch -m oldName newName
  ```
  
   c. 删除本地分支
  
         `    git branch -d v1.0.0    `

### 删除远程分支

```
git push --delete origin oldName
```

### 远程仓库更新到本地

```
$ git fetch origin
$ git merge origin/hexo
```

或者合并

```
$ git pull origin hexo 
```

http://www.ruanyifeng.com/blog/2014/06/git_remote.html

### 修改远程仓库地址

* github创建 New项目，项目地址是 git@github.com:BlogForMe/News.git
* 修改远程服务器地址 ：`git remote set-url origin git@github.com:BlogForMe/News.git`
* 推送到远程服务器 : `git push origin `

http://jcpplus.github.io/2015/07/23/modify-remote-url/

## fork后的项目处理

### get fork后分支的tag

```
git remote add upstream git@github.com:square/okhttp.git
git fetch upstream --tags

//Verify that the tags are fetched:
git tag
//will show  [new tag]             parent-4.9.0                                          -> parent-4.9.0
git push origin --tags

git checkout tags/parent-4.12.0
```

  https://gaohaoyang.github.io/2015/04/12/Syncing-a-fork/
  https://blog.csdn.net/sdujava2011/article/details/138312278

### merge the src directory

Assume you want to merge the android-test-app directory from master into  parent-4.12.0-study.

```bash
okhttp_dump % git branch
  main
  master
* parent-4.12.0-study
okhttp_dump % git checkout parent-4.12.0-study
okhttp_dump % git checkout master android-test-app
okhttp_dump % git add android-test-app
okhttp_dump % git commit -m "Merged src directory from feature-branch"
```

### 忽略文件已提交的文件

 对于单个文件处理
  假如要忽略 .idea/misc.xml文件，.gitignore可以添加 `/.idea/* `把.idea文件过滤
 主要是　`git rm --cached　.idea/misc.xml ` 然后提交修改,每次手输入才有用?

 对文件夹的处理,比如common-bankcard

```
git rm -r --cached common-bankcard
git rm --cached  *idea/*
```

## 修复已发布版本bug

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

## Git仓库迁移

 迁移git仓库

* 原来托管于github,clone一份裸版本库
  
      git clone --bare git@gitee.com:huaiyi/CQJ.git

* 在新的版本库(gitlab)里面创建一个新的项目，例如 cqianjia

* 推送刚才clone的镜像到gitlab服务器

```
cd CQJ.git/
git push --mirror git@45.77.22.97:root/cqianjia.git
```

 mirror 克隆出来的裸版本对上游版本库进行了注册，这样可以在裸版本库中使用git fetch命令和上游版本库进行持续同步

* 删除本地代码

```
cd ..
rm -rf project.git
```

* 到新的服务器clone到本地就Ok了
  
  `  git clone git@45.77.22.97:root/cqianjia.git` 
  
  参考: https://my.oschina.net/kind790/blog/510601
  
  * git clone 所有分支
    
    这种方式有一个弊端，切换分支后　工程名没了， 还是慢慢摸索吧　!

```
git branch -r | grep -v '\->' | while read remote; do git branch --track "${remote#origin/}" "$remote"; done
git fetch --all
git pull --all
```

参考: http://blog.csdn.net/allangold/article/details/78028709

## GIT标签

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

### Checkout Git Tags

https://devconnected.com/how-to-checkout-git-tags/

> ```
>  git fetch --all --tags
>  git checkout tags/v1.0 -b v1.0-branch    //git checkout tags/v2.1.1 -b v2.1.1-branch  v1.0代表tag名称
> 
>  git log --oneline --graph        //You can inspect the state of your branch by using the “git log” command. Make sure that the HEAD pointer (the latest commit) is pointing to your annotated tag.
> ```

### checkout remote tag

```
git fetch --all --tags

git ls-remote --tags
  refs/tags/v2.5.9
  refs/tags/v2.5.9^{}

git checkout  refs/tags/v2.5.9 -b v2.5.9-branch                    //-b 后面是自定义的分支名称
                                                                                 这个自定义
```

在 Git 中你并不能真的检出一个标签，因为它们并不能像分支一样来回移动。 如果你想要工作目录与仓库中特定的标签版本完全一样，可以使用 git checkout -b [branchname] [tagname] 在特定的标签上创建一个新分支：

        $ git checkout -b version2 v2.0.0

Switched to a new branch 'version2'

当然，如果在这之后又进行了一次提交，version2 分支会因为改动向前移动了，那么 version2 分支就会和 v2.0.0 标签稍微有些不同，这时就应该当心了。

修复已发布版本Bug
http://gepeiyu.com/2017/06/28/git-tag-oldversion-debug/

### git stash

经常有这样的事情发生，当你正在进行项目中某一部分的工作，里面的东西处于一个比较杂乱的状态，而你想转到其他分支上进行一些工作。问题是，你不想提交进行了一半的工作，否则以后你无法回到这个工作点。解决这个问题的办法就是`git stash`命令。

1. ` git stash  save  '暂存备注信息'  ` //进入暂存状态,此时执行 git status 已经没有要提交的了

2. git checkout  切换到要修改的分支上 ,修改完

3. 完成后回到原来的开发分支上，git stash apply 获取最近暂存内容

4. `git stash drop  stash@{1} `
   
   > apply 选项只尝试应用储藏的工作——储藏的内容仍然在栈上。要移除它，你可以运行 `git stash drop`，加上你希望移除的储藏的名字：
* git stash 可以进行多次暂存,多次存后 git stash list长下面这样,可以用 · git stash apply stash@{1}· 获取某次暂存的内容
  
  > stash@{0}: WIP on dev: 3d01a6c Patient Entity数据库删除
  > stash@{1}: WIP on dev: b6a688e 跑起来提交

* 取消储藏  `git stash show -p stash@{0} | git apply -R`  如果没指定具体的标签 取消最近的
  
  这个和`git stash drop`的区别是 取消的是文件内容，stash标签还在
  
  https://git-scm.com/docs/git-stash
  
  >  注意是 stash@{0}  不是  3d01a6c
  
  git branch 分支名 hash(历史版本)

* update forked project
  
   https://blog.csdn.net/qq1332479771/article/details/56087333 

* 获取暂存
  
  > git stash apply stash@{0}   // don‘t remove stash
  > 
  > git stash pop stash@{0}

### SVN

* SVN不能添加文件:https://blog.csdn.net/yujiayinshi/article/details/51381942

### github提速

[www.github.com](https://link.zhihu.com/?target=http%3A//www.github.com/) 替换为   www.github.com.cnpmjs.org

https://github.com/flutter/flutter.git

git clone https://github.com.cnpmjs.org/love-flutter/flutter-column.git

## git pull git fetch区别

git fetch更新本地仓库的两种用法：

```crmsh
# 方法一
$ git fetch origin master                #从远程的origin仓库的master分支下载代码到本地的origin maste
$ git log -p master.. origin/master      #比较本地的仓库和远程参考的区别
$ git merge origin/master                #把远程下载下来的代码合并到本地仓库，远程的和本地的合并
```

```powershell
# 方法二
$ git fetch origin master:temp           #从远程的origin仓库的master分支下载到本地并新建一个分支temp
$ git diff temp                          #比较master分支和temp分支的不同
$ git merge temp                         #合并temp分支到master分支
$ git branch -d temp                     #删除temp
```

这样理解

pull = fetch + merge

https://segmentfault.com/a/1190000017030384

https://www.jianshu.com/p/d265f7763a3a

https://blog.csdn.net/riddle1981/article/details/74938111

https://www.cpweb.top/455

### master revert

 今天将一段代码合入了master，上线的时候有问题，而不能很快解决掉，为了不影响其他同事合入master或将有问题的commit带上线，因此我将我的commit revert掉了。
  同时，当天晚上的时候有同事刚好问我git revert后自己代码消失的问题，当时思考不清晰，也比较少用revert，因此现在来复盘下。
  commit提交流程示意图如下：

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/GIT/20690451-b8fff26d34df812c.webp)

其中A是有问题的commit，R是revert的commit，M是master，序号N代表master的流向,

1. 从master checkout分支 F-A ,提交了TestA文件
   
   ![]( https://raw.githubusercontent.com/BlogForMe/ImageServer/main/GIT/20221109111230.jpg)

2. 从master checkout分支 UP-R,提交了TESTB, TestR
    ![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/GIT/20221109115616.jpg)

3. 分别吧F-A , UP-R分支合并到master.

4. 此时觉得F-A代码有问题，需要修改，master对F-A进行revert ,此时 master的F-A 分支TesA被删除了,并且有一条提交记录

5. 从master checkout得到分支verify-b,然后将之前需要会退的提交 , 再进行 git revert commit,然后push (这个方式没想明白)

6. 把master合并到verify-b就能解决这个问题.

https://blog.csdn.net/oYiMiYangGuang123/article/details/99437382

上面的步骤还是没想明白.

主要 git reset不能解决，有问题.除非强制push。

### 删除一条历史记录

git revert 纪录

Git force push

git rebase -i 7158b278b8f47f9b46f9af2207996bce783c0b57 这篇blog介绍的，但是没起作用。

https://linuxhint.com/remove-commit-from-history-git/

查看日志

git log --author zh --since=2022-12-10

## git ssh

#### 原理

http://skypegnu1.blog.51cto.com/8991766/1641064

##### 多平台配置SSH

正常情况会哟几个平台的配置情况,ssh操作方式

##### 用户信息设置

在config后加上 --global 即可全局设置用户名和邮箱，否则就是局部的。有时候用手输入才有用

>  $ git config --global user.name "John Doe"
>  $ git config --global user.email johndoe@example.com

　　检查配置信息　`git config --list`

#### 生成sshkey

如果视其他平台生成时就要修改名称了

> ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
> Enter file in which to save the key (/home/jon/.ssh/id_rsa): /home/jon/.ssh/id_rsa.Oschina
> cat ~/.ssh/id_rsa.li | clip

Adding your SSH key to the ssh-agent

start the ssh-agent in the background

      $ eval $(ssh-agent -s)
        Agent pid 59566
      $ ssh-add ~/.ssh/id_rsa.li

gitbash下把生成的id_rsa.pub 添加到sshkey

oschina 测试连接：`$ ssh -T git@git.oschina.net`　　

github 测试连接:    ` $ ssh -T git@github.com`
如果出现:

> Hi username! You've successfully authenticated, but GitHub does not
>  provide shell access.
>  配置成功

##### 不同平台　不同的rsa.key

　我前面的key重新命名，clone项目还是有问题

  解决方法: 在 .ssh目录下　新建config文件 添加

>   Host github
>     HostName github.com
>     User Jon
>     IdentityFile ~/.ssh/id_rsa.li

##### 不同的平台，相同的rsa.key (推荐)

复制id_rsa.pub填入

> clip < ~/.ssh/id_rsa.pub

参考：
github生成方式
https://help.github.com/articles/about-ssh/

gitlab生成方式
https://gitlab.com/help/ssh/README

oschina方式:
http://git.mydoc.io/?t=154712

gogs:参考github,配置sskey后还需要用账号登陆

##### 问题

1. 类似下面错误使用　git push -u origin master
   
   `git push -f origin master`

> ! [rejected]        master -> master (fetch first)
> error: failed to push some refs to 'https://github.com/aniruddhabarapatre/learn-rails.git'
> hint: Updates were rejected because the remote contains work that you do
> hint: not have locally. This is usually caused by another repository pushing

参考：https://stackoverflow.com/questions/20939648/issue-pushing-new-code-in-github

2. 生成key时，我改成了　id_rsaOschina
   android studio 最后push 时
   authentication id_rsaOschina　using key failed报错
   方法: AndroidStudio -> File -> Settings ->Git -> SSH executable : built-in 改成Native

参考:https://stackoverflow.com/questions/24688700/android-studio-push-failed-fatal-could-not-read-from-remote-repository

##### GitHub CLI  配置

 在windows平台 `git auth login` 授权    要用windows命令行工具。 

[Mac使用ssh密钥登录Linux - 简书](https://www.jianshu.com/p/7990ca55da69)





push issue big file 

```bash
git config --global http.postBuffer 524288000
```
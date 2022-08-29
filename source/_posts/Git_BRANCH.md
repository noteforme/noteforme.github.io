---
title: GIT_BRANCH
comments: true
date: 2017-10-19 09:24:09
tags: Git
categories:

---

官方文档
https://git-scm.com/book/en/v2
http://iissnan.com/progit/

#### Git基本操作

#####  github上传项目 

*  分支创建

```
 git branch Dev1.10 
 git push origin Dev1.10 　　　// 提交该分支到远程仓库
 git pull origin dev          //从远程获取分支
       
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

 [git分支简介](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%AE%80%E4%BB%8B)
 http://blog.csdn.net/hyr83960944/article/details/36185231

合并分支
合并hotfix到dev

```
$ git checkout            // 先切换到dev 
$ git merge hotfix
```
[合并分支](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6)

##### 获取本地没有的远程分支

```
git branch -r #查看远程分支  或 git branch -a #查看所有分支
```

会显示

```
origin/HEAD -> origin/master
origin/daily/1.2.2
origin/daily/1.3.0
origin/daily/1.4.1
origin/develop
origin/feature/daily-1.0.0
origin/master
```

然后直接 

```
git checkout origin/daily/1.4.1
```

https://gaohaoyang.github.io/2016/07/07/git-clone-not-master-branch/

[官方](https://git-scm.com/book/zh/v1/Git-%E5%88%86%E6%94%AF-%E8%BF%9C%E7%A8%8B%E5%88%86%E6%94%AF "官方参考")

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
         `	git branch -d v1.0.0	`
   
   



##### 删除远程分支     

```
git push --delete origin oldName
```





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

      git clone --bare git@gitee.com:huaiyi/CQJ.git

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



#### GIT标签

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

##### Checkout Git Tags 

https://devconnected.com/how-to-checkout-git-tags/

> ```
>  git fetch --all --tags
>  git checkout tags/v1.0 -b v1.0-branch	//git checkout tags/v2.1.1 -b v2.1.1-branch  v1.0代表tag名称
>  
>  git log --oneline --graph		//You can inspect the state of your branch by using the “git log” command. Make sure that the HEAD pointer (the latest commit) is pointing to your annotated tag.
> ```

##### checkout remote tag

```
git fetch --all --tags

git ls-remote --tags
 
  refs/tags/v2.5.8^{}
  refs/tags/v2.5.9
  refs/tags/v2.5.9^{}

git checkout  refs/tags/v2.5.9 -b v2.5.9-branch					//-b 后面是自定义的分支名称
 																				这个自定义
```



在 Git 中你并不能真的检出一个标签，因为它们并不能像分支一样来回移动。 如果你想要工作目录与仓库中特定的标签版本完全一样，可以使用 git checkout -b [branchname] [tagname] 在特定的标签上创建一个新分支：

		$ git checkout -b version2 v2.0.0
Switched to a new branch 'version2'

当然，如果在这之后又进行了一次提交，version2 分支会因为改动向前移动了，那么 version2 分支就会和 v2.0.0 标签稍微有些不同，这时就应该当心了。

修复已发布版本Bug
http://gepeiyu.com/2017/06/28/git-tag-oldversion-debug/

##### git stash

经常有这样的事情发生，当你正在进行项目中某一部分的工作，里面的东西处于一个比较杂乱的状态，而你想转到其他分支上进行一些工作。问题是，你不想提交进行了一半的工作，否则以后你无法回到这个工作点。解决这个问题的办法就是`git stash`命令。

1. ` git stash  save  '暂存备注信息'  ` //进入暂存状态,此时执行 git status 已经没有要提交的了

2. git checkout  切换到要修改的分支上 ,修改完

3.   完成后回到原来的开发分支上，git stash apply 获取最近暂存内容

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

##### SVN

* SVN不能添加文件:https://blog.csdn.net/yujiayinshi/article/details/51381942

  

##### github提速

[www.github.com](https://link.zhihu.com/?target=http%3A//www.github.com/) 替换为   www.github.com.cnpmjs.org

https://github.com/flutter/flutter.git

git clone https://github.com.cnpmjs.org/love-flutter/flutter-column.git



#### 远程仓库

##### 修改远程仓库地址 

* github创建 New项目，项目地址是 git@github.com:BlogForMe/News.git
* 修改远程服务器地址 ：`git remote set-url origin git@github.com:BlogForMe/News.git`
* 推送到远程服务器 : `git push origin `


http://jcpplus.github.io/2015/07/23/modify-remote-url/



##### git pull git fetch区别

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

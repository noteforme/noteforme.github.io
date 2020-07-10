---
title: Hexo NEXT
comments: true
date: 2017-08-01 14:03:52
tags: BLOG	
categories: 

---

# hexo next主题添加分类


只要在　博客头部加　categories: "BLOG"　　就会自动展示分类了，如果觉得每次添加麻烦的话，修改scaffolds/post.md　模板

    title: {{ title }}
    date: {{ date }}
    tags:
    categories:
    comments: true

 然后hexo n "yourblog",就会有这些了，接下里就是在首页分类进行关联了

# 生成 分类（默认已有分类）

       hexo new page "categories"

 1. 新建page: $ hexo new page “categories” ，在 hexo > source 文件夹中会出现一个categories文件夹
 2. 打开categories文件夹中的index.md页面，在头部添加 – type: “categories” （为了点击的时候链接生效）
 3. 在hexo > theme > next > _config.yml 中修改menu下的categorues,去掉前面井号注释。 然后就可以看到分类页面了


参考：https://lannly.github.io/2016/11/16/Hexo-Next-%E6%B7%BB%E5%8A%A0%E8%8F%9C%E5%8D%95%E5%88%86%E7%B1%BB/

#  next 添加头像

- 新建uploads文件夹，放入图片


-  找到themes/next 下的 -config.yml,修改如下图所示

       # Sidebar Avatar
       # in theme directory(source/images): /images/avatar.jpg
       # in site  directory(source/uploads): /uploads/avatar.jpg
        avatar: /uploads/author.png
参考：https://github.com/iissnan/hexo-theme-next/wiki/%E8%AE%BE%E7%BD%AE%E4%BE%A7%E8%BE%B9%E6%A0%8F%E5%A4%B4%E5%83%8F

# 添加浏览数

https://notes.wanghao.work/2015-10-21-%E4%B8%BANexT%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0%E6%96%87%E7%AB%A0%E9%98%85%E8%AF%BB%E9%87%8F%E7%BB%9F%E8%AE%A1%E5%8A%9F%E8%83%BD.html#%E9%85%8D%E7%BD%AELeanCloud
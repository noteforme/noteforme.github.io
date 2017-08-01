---
title: HexoFunction
comments: true
date: 2017-08-01 14:03:52
tags:
categories: BLOG

---

一、hexo next主题添加分类
1、分类
只要在　博客头部加　categories: "BLOG"　　就会自动展示分类了，如果觉得每次添加麻烦的话，修改scaffolds/post.md　模板

    title: {{ title }}
    date: {{ date }}
    tags:
    categories:
    comments: true

 然后hexo n "yourblog",就会有这些了，接下里就是在首页分类进行关联了
 
 2、生成 分类（默认已有分类）
    
       hexo new page "categories"

 1. 新建page: $ hexo new page “categories” ，在 hexo > source 文件夹中会出现一个categories文件夹
 2. 打开categories文件夹中的index.md页面，在头部添加 – type: “categories” （为了点击的时候链接生效）
 3. 在hexo > theme > next > _config.yml 中修改menu下的categorues,去掉前面井号注释。 然后就可以看到分类页面了


参考：https://lannly.github.io/2016/11/16/Hexo-Next-%E6%B7%BB%E5%8A%A0%E8%8F%9C%E5%8D%95%E5%88%86%E7%B1%BB/

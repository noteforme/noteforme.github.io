---
title: github build blog 
date: 2017-07-17 15:05:03
tags:
---

一、搭建完博客后很重要的一点是图片的处理问题
1  .  我们生成的路径是以public目录的路径为相对路径，所以要看public下面有没有生成图片![图片](/2017/07/17/https-github-pages/img_generate20170717152203.png)

     所以我的写法是 
     “  ![描述](/2017/07/17/https-github-pages/img_generate20170717152203.png)  ”
     别忘了2017前面的 “/”，否则文章页面不显示图片



  参考： http://www.jianshu.com/p/950f8f13a36c
二、Cloudflare免费的ssl
　　１、创建账户
　　　　1)注册　　https://www.cloudflare.com/a/sign-up
       
　　　　２）登录后输入域名,点击扫描
　　　　　　如图
                   ![描述](/2017/07/17/https-github-pages/img.png)
  


　　　　3)　一直continue，到了Selet a Cloudflare Plan 选择Free Website
　　　　４）解析域名的地方，修改域名服务器,我这里是在godaddy修改，然后continue
       5)点击Preview on your site instantly－> 点击Overview(显示Ａctive即可)－>点击Crypto(选择Ｆlexible)


       
       参考：http://www.jianshu.com/p/92b6d4a6ecd5

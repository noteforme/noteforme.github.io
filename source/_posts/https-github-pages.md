---
title: github创建HEXO
date: 2017-07-17 15:05:03
tags: [多设备同步hexo,图片显示，https认证]
categories: "BLOG"

---
一、hexo多设备同步
搭建好hexo后，由于他是本地生成的，那么就要考虑同步的问题了，目前解决在github建一个分支hexo，然后把本地资源用git分支管理
１.在github创建hexo分支

2.在本地博客根目录上传资源文件，由于有些文件是hexo d后自动生成的不必上传，在　.gitignore，有下面这些不用上传，可以Google这些文件各自作用
    .DS_Store
    Thumbs.db
    db.json
    *.log
    node_modules/
    public/
    .deploy*/
接着就是上传了


        // git初始化
         git init
        // 添加仓库地址
        git remote add origin https://github.com/用户名/仓库名.git
        // 新建分支并切换到新建的分支
        git checkout -b 分支名
        // 添加所有本地文件到git
        git add .
        // git提交
        git commit -m ""
        // 文件推送到hexo分支
        git push origin hexo

３.博客同步：接着就是其他设备了，跨平台也是可以的，先clone　hexo分支到本地

    git clone -b hexo git@github.com:noteforme/noteforme.github.io.git
    // 安装hexo
    npm install hexo
    // 注意这里不需要hexo初始化：hexo init；否则之前的hexo配置参数会重置
    // 安装依赖库
    npm install
    // 安装部署相关配置
    npm install hexo-deployer-git

这要就完成同步了


问题:在ubuntu上，执行hexo d部署后每次都要输入github用户名和密码，在这里也找到了答案，就是根目录下的 _config.yml文件没有配置成ssh,之前是这样的     repository:https://github.com/noteforme/noteforme.github.io.git
    # Deployment
    ## Docs: https://hexo.io/docs/deployment.html
    deploy:
      type: git 
      repository: git@github.com:noteforme/noteforme.github.io.git
      branch: master


参考：http://www.jianshu.com/p/6fb0b287f950



二、搭建完博客后很重要的一点是图片的处理问题
1  .  我们生成的路径是以public目录的路径为相对路径，所以要看public下面有没有生成图片![图片](/2017/07/17/https-github-pages/img_generate20170717152203.png)

     所以我的写法是
     “  ![描述](/2017/07/17/https-github-pages/img_generate20170717152203.png)  ”
     别忘了2017前面的 “/”，否则文章页面不显示图片



  参考： http://www.jianshu.com/p/950f8f13a36c
三、https认证:Cloudflare免费的ssl
　　１、创建账户
　　　　1)注册　　https://www.cloudflare.com/a/sign-up
       
　　　　２）登录后输入域名,点击扫描
　　　　　　如图
                   ![描述](/2017/07/17/https-github-pages/img.png)
  


　　　　3)　一直continue，到了Selet a Cloudflare Plan 选择Free Website
　　　　４）解析域名的地方，修改域名服务器,我这里是在godaddy修改，然后continue
       5)点击Preview on your site instantly－> 点击Overview(显示Ａctive即可)－>点击Crypto(选择Ｆlexible)


   参考：http://www.jianshu.com/p/92b6d4a6ecd5
四、修改主题　分类和评论
１、主题两步就修改好了，在hexo s本地显示没问题，但是部署后网页显示错乱，问题是缓存，清下浏览器缓存就好了
　参考：http://jinyanhuan.github.io/2015/03/16/hexo-bulid-three/

2.评论　：

- 　修改themes下的_config.yml  disqus_shortname: your-disqus-shortname，your-disqus-shortname　参考：　http://www.jianshu.com/p/c4f65ebe23ad

-   formatter添加　comments: true
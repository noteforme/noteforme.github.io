---
title: HexoConfigure
comments: true
date: 2017-08-01 14:03:52
tags: BLOG
categories: 
---


####  基本配置

​    官方文档:https://hexo.io/zh-cn/docs/index.html

1. 下载 [Node.js ](https://nodejs.org/en/))

2. Git

   下载的是 node-v6.11.3-x64.msi ，一路安装下去自动配置好了环境变量
    输入 `node -v` 测试

3. npm  

   `npm install -g hexo-cli  `

   注意： 安装Node.js最佳方式使用nvm，使用master分支不起作用,需要github推荐的分支

   接着安装node.js

   搭建好hexo后，由于他是本地生成的，那么就要考虑同步的问题了，目前解决在github建一个分支 hexo，然后把本地资源用git分支管理

4. 安装 npm install hexo 



安装出问题 更换镜像源

[https://www.lemonneko.cn/win10%E6%90%AD%E5%BB%BAhexo%E5%8F%8A%E9%81%87%E5%88%B0%E7%9A%84%E5%9D%91/](https://www.lemonneko.cn/win10搭建hexo及遇到的坑/)


####  上传到分支

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



   其他设备安装好环境(支持跨平台)，先clone　hexo分支到本地

    git clone -b hexo git@github.com:noteforme/noteforme.github.io.git
    // 安装hexo , 下不下来就用privoxy
    npm install hexo
    // 注意这里不需要hexo初始化：hexo init；否则之前的hexo配置参数会重置
    // 安装依赖库
    npm install
    // 安装部署相关配置
    npm install hexo-deployer-git
    
    //如果出错执行下面的
    npm install -g hexo-cli

这要就完成同步了(上面的流程还不是很规范，不过执行上面几个命令基本都能解决)

如果有这些不用管了 

>npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@^1.0.0 (node_modules\chokidar\node_modules\fsevents):
>npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.1.2: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"

问题:在ubuntu上，执行hexo d部署后每次都要输入github用户名和密码，在这里也找到了答案，就是根目录下的 _config.yml文件没有配置成ssh,之前是这样的     repository:https://github.com/noteforme/noteforme.github.io.git
     Deployment
     Docs: https://hexo.io/docs/deployment.html
    deploy:
      type: git 
      repository: git@github.com:noteforme/noteforme.github.io.git
      branch: master


参考：http://www.jianshu.com/p/6fb0b287f950



###### https认证:Cloudflare免费的ssl

创建账户
注册　　https://www.cloudflare.com/a/sign-up

登录后输入域名,点击扫描
　　　　如图

一直continue，到了Selet a Cloudflare Plan 选择Free Website

解析域名的地方，修改域名服务器,我这里是在godaddy修改，然后continue

点击Preview on your site instantly－> 点击Overview(显示Ａctive即可)－>点击Crypto(选择Ｆlexible)

参考：http://www.jianshu.com/p/92b6d4a6ecd5



#### mac install hero

```
sudo npm install -g hero-cli 
```

theme didn't commit to GitHub ,so you could clone theme



######  hexo next theme

修改主题

`git clone git@github.com:ppoffice/hexo-theme-icarus.git themes/icarus`
里面都有介绍，主要石npm安装不了插件
就需要[这篇文章](http://blog.csdn.net/huaiyiheyuan/article/details/53026243) 的polipo工具，最有名的Privoxy反而不起作用

分类
只要在　博客头部加　categories: "BLOG"　　就会自动展示分类了，如果觉得每次添加麻烦的话，修改scaffolds/post.md　模板

  title: {{ title }}
  date: {{ date }}
  tags:
  categories:
  comments: true

 然后hexo n "yourblog",就会有这些了，接下里就是在首页分类进行关联了

生成 分类（默认已有分类）

```
hexo new page "categories"
```

 1. 新建page: $ hexo new page “categories” ，在 hexo > source 文件夹中会出现一个categories文件夹
 2. 打开categories文件夹中的index.md页面，在头部添加 – type: “categories” （为了点击的时候链接生效）
 3. 在hexo > theme > next > _config.yml 中修改menu下的categorues,去掉前面井号注释。 然后就可以看到分类页面了


参考：https://lannly.github.io/2016/11/16/Hexo-Next-%E6%B7%BB%E5%8A%A0%E8%8F%9C%E5%8D%95%E5%88%86%E7%B1%BB/

######  next 添加头像

新建uploads文件夹，放入图片

找到themes/next 下的 -config.yml,修改如下图所示

```
# Sidebar Avatar
# in theme directory(source/images): /images/avatar.jpg
# in site  directory(source/uploads): /uploads/avatar.jpg
 avatar: /uploads/author.png
```
https://github.com/iissnan/hexo-theme-next/wiki/%E8%AE%BE%E7%BD%AE%E4%BE%A7%E8%BE%B9%E6%A0%8F%E5%A4%B4%E5%83%8F



#### Mac  Hexo安装

command not found: hexo的解决方法   

$ npm root -g 
获取node_modules地址
/Users/guo/.npm-global/lib/node_modules

```
$export PATH=$PATH:/Users/m/.npm-global/lib/node_modules/hexo-cli/bin
```

也可以去修改~/.zshrc 或者~/.bashrc，在里面添加上述命令，然后 source ~/.zshrc



#### 图片不显示

打开/node_modules/hexo-asset-image/index.js，将内容更换为下面的代码

```js
'use strict';
var cheerio = require('cheerio');

// http://stackoverflow.com/questions/14480345/how-to-get-the-nth-occurrence-in-a-string
function getPosition(str, m, i) {
  return str.split(m, i).join(m).length;
}

var version = String(hexo.version).split('.');
hexo.extend.filter.register('after_post_render', function(data){
  var config = hexo.config;
  if(config.post_asset_folder){
    	var link = data.permalink;
	if(version.length > 0 && Number(version[0]) == 3)
	   var beginPos = getPosition(link, '/', 1) + 1;
	else
	   var beginPos = getPosition(link, '/', 3) + 1;
	// In hexo 3.1.1, the permalink of "about" page is like ".../about/index.html".
	var endPos = link.lastIndexOf('/') + 1;
    link = link.substring(beginPos, endPos);

    var toprocess = ['excerpt', 'more', 'content'];
    for(var i = 0; i < toprocess.length; i++){
      var key = toprocess[i];
 
      var $ = cheerio.load(data[key], {
        ignoreWhitespace: false,
        xmlMode: false,
        lowerCaseTags: false,
        decodeEntities: false
      });

      $('img').each(function(){
		if ($(this).attr('src')){
			// For windows style path, we replace '\' to '/'.
			var src = $(this).attr('src').replace('\\', '/');
			if(!/http[s]*.*|\/\/.*/.test(src) &&
			   !/^\s*\//.test(src)) {
			  // For "about" page, the first part of "src" can't be removed.
			  // In addition, to support multi-level local directory.
			  var linkArray = link.split('/').filter(function(elem){
				return elem != '';
			  });
			  var srcArray = src.split('/').filter(function(elem){
				return elem != '' && elem != '.';
			  });
			  if(srcArray.length > 1)
				srcArray.shift();
			  src = srcArray.join('/');
			  $(this).attr('src', config.root + link + src);
			  console.info&&console.info("update link as:-->"+config.root + link + src);
			}
		}else{
			console.info&&console.info("no src attr, skipped...");
			console.info&&console.info($(this));
		}
      });
      data[key] = $.html();
    }
  }
});

```



https://moeci.com/posts/hexo-typora/

https://juejin.cn/post/7006594302604214280

https://www.bilibili.com/video/BV1D7411U7Yk/





##### 卸载插件

```
npm list
npm uninstall <你的插件名>
```



安装插件

```
npm install https://github.com/fulsun/hexo-asset-image-master.git --save
```



https://carolinelh.github.io/2019/11/20/hexo%E6%96%87%E7%AB%A0%E5%9B%BE%E7%89%87%E4%B8%8D%E6%98%BE%E7%A4%BA%E7%9A%84%E5%9D%91/



_config.yml

```
## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
url: https://noteforme.github.io 			// 之前这个地址不对,多加了东西
```


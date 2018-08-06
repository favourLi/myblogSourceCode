---
title: hexo 与 github 搭建个人博客教程
date: 2018-05-15 11:27:49
tags:
    - hexo
    - github
    - 博客
---

### 前言

工作几年了，第一次写博客，看看自己能坚持多久吧。
今天就将hexo搭配github搭建个人博客教程作为第一篇文章。
<!-- more -->

### 准备工作
工欲善其事必先利其器。要想搭建个人博客，node和git是必须的工具。网上关于这两种工具的下载安装都有很成熟的介绍，在这里就不献丑了。
然后就是在github上注册自己的账号，成功后准备接下来的工作。
### github新建项目
>{% asset_img 1.png %}

<br/>
点击右边绿色的NEW按钮，会跳转到
<br/>

>{% asset_img 2.png %}

<br/>
输入对应的库名字以及描述，点击下方的绿色的Create repository按钮，就会生成自己的库了。如下图
<br/>

>{% asset_img 3.png %}

然后复制自己库地址的SSH，保存，稍后会用到

### hexo安装
在命令行执行下列代码即可全局安装hexo

>npm install -g hexo-cli

如果安装速度比较慢的话，也可以使用npm淘宝镜像来下载

>npm install -g hexo-cli --registry=https://registry.npm.taobao.org

### hexo使用
在命令行工具执行

>hexo init blog

经过稍长时间的等待，会在当前目录下生成一个新的文件夹，里面的文件就是我们项目的雏形了

>{% asset_img 4.png %}

cd进该目录，执行

>npm i

会下载项目需要的node模块

>{% asset_img 5.png %}

下载完成之后，执行

>hexo generate

会生成静态文件，也就是我们的页面了
*hexo generate 可以简写为 hexo g*

>{% asset_img 6.png %}

然后执行

>hexo server

会在本地搭建服务器，浏览器打开[http://localhost:4000/](http://localhost:4000/)就可以看到我们的博客了
*hexo server 可以简写为 hexo s*

>{% asset_img 7.png %}

如果hexo server出错，提示

>Port 4000 has been used. Try other port instead.

这说明电脑当前4000端口被占用，ctrl+c退出当前服务器，执行“hexo server -p 端口号”改变本地服务器端口就可以了

>{% asset_img 8.png %}

### 文件与github关联

本地可以访问我们的博客了，那么怎么才能保存到github里呢？这时候我们前面保存的新建的库的SSH就要用到了。
编辑器打开blog文件夹，找到根目录下面的_config.yml文件

>{% asset_img 9.png %}

找到deploy选项，修改并增加参数，repostory后面的值就是我们刚才保存的库的SSH
**这时候要注意，每一个key值的冒号后面都要有一个半角空格，不然会出错**
<br/>

因为我们github里面的库是在子目录下，所以还要找到url选项并修改
url是github库的url，root就是我们库的名字了

>{% asset_img 12.png %}

然后执行

>npm install hexo-deployer-git --save
>{% asset_img 15.png %}

这个模块是用来生成和部署文章的
执行

>hexo new post "first_blog"

>{% asset_img 14.png %}

打开source/_post目录，会发现已经生成一个.md文件
编辑器打开该文件，写入

>`### hello world`

之后，打开命令行工具，执行

>hexo clean
>{% asset_img 13.png %}

清除静态缓存文件，然后执行

>hexo g

生成静态文件，执行

>hexo d
>{% asset_img 16.png %}

即可部署到github里了，打开github里可以看到自己的commit

>{% asset_img 17.png %}

但是这个时候我们的博客还是没有地址的。点击settings，找到GitHub Pages选项的Source，选择master branch

>{% asset_img 18.png %}

然后点击save，等待页面刷新后就会出现

>{% asset_img 19.png %}

这时候点击链接就可以访问到我们的博客了

>{% asset_img 20.png %}

到此为止，基础的搭建博客完成，接下来就是对我们的博客进行个性化修改以及添加文章了。
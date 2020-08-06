---
layout:     post
title:      "静态网页托管 & git同时push"
subtitle:   "静态网页托管网站以及如何将一个本地仓库同时push到多个远程仓库"
date:       2020-07-31 12:00:00
author:     "MC"
header-img: "img/post-bg-tech.jpg"
header-mask: 20%
catalog: true
tags:
    - git
    - Tech
    - Blog
---

# 静态网页托管

静态网站托管是对静态网页(*.html)内容（包含音频和视频等文件）提供的托管服务，包括对静态网页的文件存储、访问控制、索引文档及错误文档的配置、网页重定向等托管服务。使用对象存储的静态网站托管服务，可快速托管基于静态内容的网站，并提供高可用与高可靠的服务保障，大幅简化建站的操作流程，同时大幅降低网站的日常运营与维护成本。

一般来说可以放自己的博客或个人主页等等，接下来我会列举几个常见的静态网页托管网站。

### 1. GitHub Pages

- 不支持服务端代码，比如 PHP、Ruby 或 Python

- 支持 HTTPS 访问
- 支持个性域名(无需备案)，自动部署

- 服务器在海外，而且国内有DNS污染，响应速度有点慢

- 网站仓库小于`1GB`，带宽限制 `100GB/每月`，构建限制`10次/每小时`

#### 搭建方法

在GitHub上新建一个repository，命名要以"用户名.github.io"的方式，像这样：

![img](/img/in-post/post-pages/1.png)

因为我已经建过了，所以会报错。

搭建完之后把你的代码传上去，GitHub Pages每次commit都会自动部署，直接访问"用户名.github.io"就行了，如MessiahChen.github.io

如果需要自定义域名的话，去仓库的settings/Github Pages里设置就行了

### 2. Gitee Pages 码云

- 支持HTTPS访问
- 国内访问速度极快
- 个性域名要付费且域名需备案
- 自动部署需付费，每次都要手动部署

#### 搭建方法

1.新建仓库或从已有仓库导入

注：如果选择从已有仓库导入，生成的仓库右边会有一个刷新图标，点击这个可以向源仓库强制同步

![img](/img/in-post/post-pages/2.png)

2.传完代码后点击“服务”里的Gitee Pages并开启服务

![img](/img/in-post/post-pages/3.png)

3.开启之后就可以访问“用户名.gitee.io/仓库名”来访问了，如MichaelChen1999.gitee.io/blog

如果觉得这个URL太长，也可以选择把该仓库地址改成和自己的用户名一样，如：

![img](/img/in-post/post-pages/4.png)

接下来就可以直接访问“用户名.gitee.io”来访问静态网页了，如MichaelChen1999.gitee.io

### 3. Coding Pages（原Gitcafe）

- 支持多个个性域名(无需备案)，自动部署

- 服务器在新加坡，国内访问速度时快时慢
- 自定义域名可以享有免费 SSL 证书，全站支持 HTTPS 协议
- 更新代码库就可以自动部署。服务器稳定，香港服务器国外支持也友好
- 新增动态页面部署

Coding的前身是Gitcafe，后来被腾讯云收购改名Coding。但是部署在它上面的网页国内访问速度很迷，时快时慢。有时候同一个网络同一个地点同一个时间~~同一个撤硕~~PC端访问奇快，而移动端访问像龟爬一样。

搭建方法跟前两个差不多，也可以从已有仓库导入，就不再赘述。

需要注意的时候创建仓库时要选择第三个DevOps模板，这里面才有静态托管服务

![img](/img/in-post/post-pages/5.png)

进去之后选择左侧菜单栏里的持续部署/静态网站就可以了。

### 4. Vercel（原Zeit/now）

据说国内访问速度很不错，我因为一些版本兼容问题还没部署上，大家可以去试一试。

传送门：[Vercel](https://vercel.com)

### 5. Netlify

- 可以使用 CLI 上传代码
- 支持自定义域名且自定义域名支持一键开启 https（证书来自 Let's Encrype）
- 支持强制让用户通过 https 访问网站（开启后此功能后，http 的访问一律会 **301** 跳转到 https）
- 支持自动构建
- 支持重定向（Redirects）和重写（Rewrites）功能
- 数据通过 HTTP2 协议传输
- 提供 webhooks 与 API

- 不支持后台逻辑运算能力的网页

- 如果要部署 Hexo 大体思路是，通过 CLI (命令行界面)将 md 渲染为静态网站，然后通过 git 部署到 Git 平台，然后使用 Netlify 的 webhook 自动抓取部署

- 具有全球CDN、持续部署、一键HTTPS等优势

- 能通过客户端 JS 与可重用 API 可以带来动态功能，炫酷。
- 服务器在国外，国内访问速度比GitHub Pages慢

传送门：[Netlify](https://www.netlify.com/)

### 7. GitLab Pages 

- 可以使用任何静态网站生成器，如 Jekyll、Middleman、Hexo、Hugo、Pelican等
- 可以配置自定义域名 HTTPS，需要的是上传证书
- 服务器在国外，国内访问速度比GitHub Pages慢

传送门：[GitLab](https://gitlab.com)

### 8. 其他

- [Firebase Hosting](https://www.firebase.com/docs/hosting/)
- [Bitbucket Cloud](https://confluence.atlassian.com/bitbucket/publishing-a-website-on-bitbucket-cloud-221449776.html)
- [Aerobatic](https://www.aerobatic.com/)
- [Surge.sh](https://surge.sh/)

# 本地仓库同时push到多个远程仓库

有些静态托管网站会根据源仓库的commit来自动更新，而有些则不会（比如gitee）。

有些时候我想只有一个本地repo，但是每次push的时候既能push到github的远程仓库，又能push到gitee的远程仓库，该怎么办呢？

#### 方式一

1. 打开本地仓库的.git文件夹
2. 打开config文件夹
3. 在remote origin一栏里添加你要push到的仓库HTTPS URL

![img](/img/in-post/post-pages/6.jpg)

完成。以后每次push这几个仓库都会push上（记得先把ssh key配置好，省的输密码）

#### 方式二

`$ git remote set-url --add origin 仓库URL`

## 根据访问地址选择不同的静态网站

因为有GFW的存在，Github受到DNS污染所以访问速度受到影响，我们有时希望根据访客IP所在的地区来将其导向不同的静态网页，来保证其访问速度。

比如国外的引向GitHub Pages，国内的引向Gitee Pages，该怎么做呢。

我接下来会写一篇文章来具体谈谈我是怎么实现的。


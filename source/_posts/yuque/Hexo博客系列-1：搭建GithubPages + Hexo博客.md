---
title: Hexo博客系列-1：搭建GithubPages + Hexo博客
urlname: gld1ge
date: '2019-11-26 00:15:20 +0800'
categories: 博客
tags:
  - Hexo系列
  - 教程
---

前言：Hexo 是一款基于 Node.js 的简单轻量静态博客框架，依赖少易于安装使用，可以方便的生成静态网页托管在 GitHub 和 Coding 上，是搭建博客的首选框架。

<!-- more -->

## Hexo 博客搭建

### 安装 Git

1. mac 安装

```shell
brew install git
```

2. windows 安装

[git 下载](https://gitforwindows.org/)

3. 安装完成后验证

```shell
git --version
```

### 安装 nodejs

1. mac 安装

```shell
brew install node
```

2. windows 安装

[nodejs 地址，选择 LTS 版本](https://nodejs.org/en/download/)

3. 安装完成后验证

```shell
node -v
npm -v
```

### 安装 hexo

1. 安装 hexo

```shell
npm install -g hexo-cli
```

2. 验证 hexo

```shell
hexo -v
```

3. 创建 Hexo 博客项目

```shell
hexo init myblog
cd myblog
npm install
```

完成后目录结构如下：

- node_modules: 依赖包
- public：存放生成的页面
- scaffolds：生成文章的一些模板
- source：用来存放你的文章
- themes：主题
- \_config.yml: 博客的配置文件

4. 运行 Hexo 项目

```shell
hexo g
hexo server
```

> hexo 的服务，在浏览器输入 localhost:4000 就可以看到你生成的博客了

##

## 配置 GithubPages 主页

### 注册 github 账户

首先，你先要有一个 GitHub 账户，去注册一个吧。
注册完登录后，在 GitHub.com 中看到一个 New repository，新建仓库[yourname].github.io
点击 create repository。

```shell
git config --global user.name "yourname"
git config --global user.email "youremail"

# 检查
git config user.name
git config user.email
```

### 生成 SSH

执行如下命令，一直回车即可

```shell
ssh-keygen -t rsa -C "youremail"
```

> ssh，简单来讲，就是一个秘钥，其中，id_rsa 是你这台电脑的私人秘钥，不能给别人看的，id_rsa.pub 是公共秘钥，可以随便给别人看。把这个公钥放在 GitHub 上，这样当你链接 GitHub 自己的账户时，它就会根据公钥匹配你的私钥，当能够相互匹配时，才能够顺利的通过 git 上传你的文件到 GitHub 上。

### 上传公钥到 GitHub

1. 在 mac 中找到`~/.ssh/id_rsa.pub`
1. 在 GitHub 的 Setting > SSH and GPG keys 中，找到 SSH keys 的设置选项，点击 New SSH key
1. 把你的 id_rsa.pub 里面的信息复制进去。

![](https://cdn.nlark.com/yuque/0/2019/jpeg/264294/1574749300215-4951a6bb-34e4-4b83-803d-b0e9c19a75b0.jpeg#align=left&display=inline&height=1080&originHeight=1080&originWidth=2411&size=0&status=done&width=2411)

### 验证 SSH

```shell
ssh -T git@github.com
```

## 部署 hexo 到 GitHub

### 修改配置

修改`_config.yml`配置如下

```shell
deploy:
  type: git
  repo: git@github.com:YourgithubName/YourgithubName.github.io.git
  branch: master
```

## 安装插件

```shell
npm install hexo-deployer-git --save
```

### 执行同步

```shell
hexo clean
hexo generate
hexo deploy
```

## 设置自己的域名

### 默认访问地址如下

[YourgithubName].github.io

### 配置 GITHUB 为私有域名

在如下配置页面设置私有域名为你自己购买的域名 如:blog.heidi.pub
![](https://cdn.nlark.com/yuque/0/2019/jpeg/264294/1574749319525-5c29c261-635f-425e-983e-4c2f03690432.jpeg#align=left&display=inline&height=1080&originHeight=1080&originWidth=2105&size=0&status=done&width=2105)

### 设置 CNAME

在域名后端将 blog.heidi.pub 解析为  [YourgithubName].github.io

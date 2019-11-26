
---

title: Hexo博客系列-1：搭建GithubPages + Hexo博客

urlname: gld1ge

date: 2019-11-26 00:15:20 +0800

categories: 博客

tags: [Hexo系列,教程]

---


前言：Hexo是一款基于Node.js的简单轻量静态博客框架，依赖少易于安装使用，可以方便的生成静态网页托管在GitHub和Coding上，是搭建博客的首选框架。<br /><!-- more -->

<a name="iIHXe"></a>
## Hexo博客搭建
<a name="qaJqT"></a>
### 安装Git

1. mac安装
```shell
brew install git
```

2. windows安装

 [git下载](https://gitforwindows.org/)   

3. 安装完成后验证

```shell
git --version
```

<a name="Qf59C"></a>
### 安装nodejs

1. mac安装
```shell
brew install node
```

2. windows安装

[nodejs地址，选择LTS版本](https://nodejs.org/en/download/)

3. 安装完成后验证
```shell
node -v 
npm -v
```

<a name="gPwbL"></a>
### 安装hexo

1. 安装hexo
```shell
npm install -g hexo-cli
```

2. 验证hexo
```shell
hexo -v
```

3. 创建Hexo博客项目

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
- _config.yml: 博客的配置文件

4. 运行Hexo项目

```shell
hexo g
hexo server
```

> hexo的服务，在浏览器输入localhost:4000就可以看到你生成的博客了

<a name="piu63"></a>
## 
<a name="maK7x"></a>
## 配置GithubPages主页

<a name="pcG9W"></a>
### 注册github账户
首先，你先要有一个GitHub账户，去注册一个吧。<br />注册完登录后，在GitHub.com中看到一个New repository，新建仓库[yourname].github.io<br />点击create repository。


```shell
git config --global user.name "yourname"
git config --global user.email "youremail"

# 检查
git config user.name
git config user.email
```

<a name="BQqPJ"></a>
### 生成SSH
执行如下命令，一直回车即可
```shell
ssh-keygen -t rsa -C "youremail"
```

> ssh，简单来讲，就是一个秘钥，其中，id_rsa是你这台电脑的私人秘钥，不能给别人看的，id_rsa.pub是公共秘钥，可以随便给别人看。把这个公钥放在GitHub上，这样当你链接GitHub自己的账户时，它就会根据公钥匹配你的私钥，当能够相互匹配时，才能够顺利的通过git上传你的文件到GitHub上。

<a name="BYKw8"></a>
### 上传公钥到GitHub

1. 在mac中找到`~/.ssh/id_rsa.pub`
1. 在GitHub的 Setting > SSH and GPG keys中，找到SSH keys的设置选项，点击New SSH key
1. 把你的id_rsa.pub里面的信息复制进去。

![image.png](https://cdn.nlark.com/yuque/0/2019/png/264294/1574696451035-e2ec703a-ba0f-4f42-ac17-68f0e356206b.png#align=left&display=inline&height=797&name=image.png&ocrLocations=%5Bobject%20Object%5D&ocrLocations=%5Bobject%20Object%5D&ocrLocations=%5Bobject%20Object%5D&ocrLocations=%5Bobject%20Object%5D&originHeight=1594&originWidth=3558&search=Sc3rchcrjump1o-%20sSHkeys%2FAddnew%20RHpaMllories%20MddSSHkay&size=247202&status=done&width=1779)If
<a name="aNZs8"></a>
### 验证SSH

```shell
ssh -T git@github.com
```

<a name="eRl7S"></a>
## 部署hexo到GitHub
<a name="wKjIr"></a>
### 修改配置
修改`_config.yml`配置如下

```shell
deploy:
  type: git
  repo: git@github.com:YourgithubName/YourgithubName.github.io.git
  branch: master
```

<a name="dyp2W"></a>
## 安装插件

```shell
npm install hexo-deployer-git --save
```

<a name="QpBoP"></a>
### 执行同步

```shell
hexo clean
hexo generate
hexo deploy
```

<a name="whXd4"></a>
## 设置自己的域名
<a name="BxlEM"></a>
### 默认访问地址如下
[YourgithubName].github.io

<a name="omhwc"></a>
### 配置GITHUB为私有域名
在如下配置页面设置私有域名为你自己购买的域名 如:blog.heidi.pub<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/264294/1574696907153-45637200-bada-4e41-b546-2167de93bc37.png#align=left&display=inline&height=935&name=image.png&ocrLocations=%5Bobject%20Object%5D&ocrLocations=%5Bobject%20Object%5D&ocrLocations=%5Bobject%20Object%5D&originHeight=1870&originWidth=3644&search=GitHubPages%20YrgHRaCiaLihna.%20DangerZone&size=466709&status=done&width=1822)

<a name="nsw6l"></a>
### 设置CNAME
在域名后端将blog.heidi.pub 解析为 [YourgithubName].github.io


---
title: Hexo博客系列-2：增加coding发布源，绑定自己的域名，增加https访问
urlname: bfihqo
date: '2019-11-26 00:15:20 +0800'
categories: 博客
tags:
  - Hexo系列
  - 教程
---

前言：Hexo 是一款基于 Node.js 的简单轻量静态博客框架，依赖少易于安装使用，可以方便的生成静态网页托管在 GitHub 和 Coding 上，是搭建博客的首选框架。

<!-- more -->

## 增加 coding 发布源

首先，去官网登陆你的 coding 账号，没有就注册一个，然后完善个人信息，。然后新建一个 repository，项目名称就填你的用户名，选择私有，然后创建项目。

### 创建项目

项目地址格式是 你的域名.coding.me

### 选择静态页面服务

企业版界面如下
![](https://cdn.nlark.com/yuque/0/2019/jpeg/264294/1574901761377-029a06dd-f6c8-4a0e-88c7-1a4353d32509.jpeg#align=left&display=inline&height=1080&originHeight=1080&originWidth=2134&size=0&status=done&width=2134)

### 配置 SSH 公钥

同 Github
首先点击右上角的头像-个人设置-SSH 公钥-新增公匙-输入你自己的 SSH 公匙-永久有效(推荐)-确定即可。
如果你之前部署过 github 用同一个公匙是没问题的。

### 配置 hexo 的\_config.yml

实例如下

```shell
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo:
      github: git@github.com:DevinShi2019/DevinShi2019.github.io.git,master
      coding: git@e.coding.net:sdf-system/blog-hexo.git,master

```

### 配置`.travis/ssh_config`

```shell
Host github.com
  HostName github.com
  User git
  StrictHostKeyChecking no
  IdentityFile ~/.ssh/id_rsa
  IdentitiesOnly yes

# 我这里使用的企业版，所以域名是e.coding.net
Host e.coding.net
  HostName e.coding.net
  User git
  StrictHostKeyChecking no
  IdentityFile ~/.ssh/id_rsa
  IdentitiesOnly yes
```

### 发布

配置完之后，提交代码到 master 分支即可

## 绑定自己的域名并申请 Https

### 域名 CNAME 配置

配置自由域名如

- blog.sdf.heidi.pub CNAME 境外 devinshi2019.github.io
- blog.sdf.heidi.pub CNAME 默认  zfr5mu.coding-pages.com

### Github 配置

![](https://cdn.nlark.com/yuque/0/2019/jpeg/264294/1574865641700-d173e1bc-f6ab-4044-aee1-06aeae9612af.jpeg#align=left&display=inline&height=1080&originHeight=1080&originWidth=1561&size=0&status=done&width=1561)

> 如需要 https，需要勾上此选项，等待大概 1 天左右

### Coding 配置

此处配置，并点击右侧操作的申请证书，完成之后如下
![](https://cdn.nlark.com/yuque/0/2019/jpeg/264294/1574840265197-53a2c333-0ebf-414d-af4d-40033dbc899d.jpeg#align=left&display=inline&height=604&originHeight=604&originWidth=3244&size=0&status=done&width=3244)

> 如需要 https，需要先禁用掉 blog.sdf.heidi.pub 指向 github 的解析，等待 1 个小时左右生效后，再启用

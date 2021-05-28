---
title: Hexo博客系列-3：使用主题pure
urlname: ors15g
date: '2019-11-29 11:20:00 +0800'
categories: 博客
tags:
  - Hexo系列
  - 教程
---

## 特色

- 多语言
- 第三方评论框（友言、来必力、gitment、gitalk）
- 可展示个人豆瓣书单
- 可展示个人 github 托管项目
- 可设置支付宝、微信打赏

## 配置说明

在 Hexo 中有两份主要的配置文件，其名称都是 \_config.yml。 其中，一份位于站点根目录下，主要包含 Hexo 本身的配置；另一份位于主题目录下，这份配置由主题作者提供，主要用于配置主题相关的选项。为了描述方便，在以下说明中，将前者称为站点配置文件， 后者称为主题配置文件

## 安装主题

```shell
git clone https://github.com/cofess/hexo-theme-pure.git themes/pure
cd themes/pure
rm -rf .git
```

##

## 启用 pure 主题

打开站点配置文件，找到 theme 字段，将其值更改为 pure

```shell
theme: pure
```

到此，主题安装完成。然后启动 Hexo 服务验证主题是否正确启用。

```shell
hexo s
```

## 安装插件

### [hexo-wordcount](https://github.com/willin/hexo-wordcount)

```
npm install hexo-wordcount --save
```

### [hexo-generator-json-content](https://github.com/alexbruno/hexo-generator-json-content)

```
npm install hexo-generator-json-content --save
```

### [hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed)

```
npm install hexo-generator-feed --save
```

### [hexo-generator-sitemap](https://github.com/hexojs/hexo-generator-sitemap)

```
npm install hexo-generator-sitemap --save
```

### [hexo-generator-baidu-sitemap](https://github.com/coneycode/hexo-generator-baidu-sitemap)

```
npm install hexo-generator-baidu-sitemap --save
```

## 主题配置

### 设置语言

打开站点配置文件, 将 language 设置成你所需要的语言。建议明确设置你所需要的语言,可选值对应 themes\pure\languages 目录下语言文件，简体中文配置如下：

```shell
language: zh-CN
```

### 主题颜色

定制了五套颜色，默认白，黑:theme-black，蓝:theme-blue，绿:theme-green，紫:theme-purple

```shell
# config
config:
  skin: # 主题颜色 theme-black theme-blue theme-green theme-purple
```

### 导航菜单

```shell
# 导航菜单
menu:
  Home: .
  Archives: archives # 归档
  Categories: categories # 分类
  Tags: tags # 标签
  Repository: repository # github repositories
  Books: books     # 书单
  Links: links # 友链
  About: about # 关于

# 导航菜单图标（font awesome）
menu_icons:
  enable: true # 是否启用菜单图标
  home: icon-home-fill
  archives: icon-archives-fill
  categories: icon-folder
  tags: icon-tags
  repository: icon-project
  books: icon-book-fill
  links: icon-friendship
  about: icon-cup-fill
```

### 设置个人信息

> 头像在 themes\pure\source\images 目录下替换图片即可，捐献的二维码同理。
>
> 个人信息大部分都在 主题配置文件 中设置

### 搜索

主题内置三种站内搜索方式：insight、swiftype、baidu

```shell
# Search
search:
  insight: true # you need to install `hexo-generator-json-content` before using Insight Search
  swiftype: # enter swiftype install key here
  baidu: false # you need to disable other search engines to use Baidu search
```

### 分享

支持`weibo,qq,qzone,wechat,tencent,douban,diandian,facebook,twitter,google,linkedin`

```
# Share
# weibo,qq,qzone,wechat,tencent,douban,diandian,facebook,twitter,google,linkedin
share:
  enable: true  # 是否启用分享
  sites: weibo,qq,wechat,facebook,twitter  # PC端显示的分享图标
  mobile_sites: weibo,qq,qzone  # 移动端显示的分享图标
```

### 评论

主题集成了[disqus](https://disqus.com/)、[友言](http://www.uyan.cc/)、[来必力](https://livere.com/)、[gitment](https://github.com/imsun/gitment)、[gitalk](https://github.com/gitalk/gitalk)、[valine](https://valine.js.org)评论系统，选择其中一种即可
本文中选择 valine 主题，简洁大方

```shell
comment:
  type: valine  # 启用哪种评论系统
  valine: # Valine. https://valine.js.org
    appid:   # your leancloud application appid
    appkey:  # your leancloud application appkey
    notify: false # mail notifier , https://github.com/xCss/Valine/wiki
    verify: false # Verification code
    placeholder: 每一份发言都值得被记录 # comment box placeholder
    avatar: mm # gravatar style
    meta: nick,mail,link # custom comment header
    pageSize: 10 # pagination size
    visitor: false # Article reading statistic https://valine.js.org/visitor.html

```

### 文章浏览量统计

主题内置了不蒜子和 leancloud 来统计文章浏览量，启用其中之一即可，注意 leancloud 需要到其官网申请 APP ID 和 APP Key。

比如，启用不蒜子来统计文章浏览量，在主题配置文件中把 busuanzi 设置为 true 即可：

```shell
pv:
  busuanzi:
    enable: true  # 不蒜子统计
```

如果不需要第三方来统计浏览量，只需将相应设置改为 false 即可（设置为 false 后不会加载第三方 JS 脚本）

### Github respostory

复制`theme/pure/_source/` 目录下`repository`文件夹到`blog path/source/` 目录下

```
# Github
github:
  username: ***  # github username
```

### 豆瓣书单

复制`theme/pure/_source/` 目录下`books`文件夹到`blog path/source/` 目录下

```
# douban 豆瓣书单
douban:
  user: *** # 豆瓣用户名
  start: 0 # 从哪一条记录开始
  count: 100 # 获取豆瓣书单数据条数
```

### 友情链接

复制`theme/pure/_source/` 目录下`links`文件夹到`blog path/source/` 目录下

在 hexo 目录下的 source 文件夹内创建一个名为 \_data（禁止改名）的文件夹。

然后在文件内创建一个名为 links.yml 的文件,在其中添加相关数据即可。

单个友情链接的格式为：

```
Name:
    link: http://example.com
    avatar: http://example.com/avatar.png
    desc: "这是一个描述"
```

添加多个友情链接，我们只需要根据上面的格式重复填写即可。

. 将 Name 改为友情链接的名字，例如 Cofess。

. [http://example.com](http://example.com) 为友情链接的地址。

. [http://example.com/avatar.png](http://example.com/avatar.png) 为友情链接的头像。

. 这是一个描述 为友情链接描述。

### 文章索引目录

```
title: 文章标题
categories:
  - 文章分类
tags:
  - 文章标签
toc: true # 是否启用内容索引
```

### sidebar 侧边栏

文章侧边栏默认为开启状态，如果某篇文章不想开启侧边栏，在文章开头配置加入“sidebar: none”即可：

```
title: 文章标题
categories:
  - 文章分类
tags:
  - 文章标签
sidebar: none # 是否启用sidebar侧边栏，none：不启用
```

## 博客优化

### [hexo-neat](https://github.com/rozbo/hexo-neat)

> auto Minify html、js、css and make it neat

```
npm install hexo-neat --save
```

在博客配置文件`_config.yml`中添加

```
# hexo-neat
neat_enable: true
neat_html:
  enable: true
  exclude:
neat_css:
  enable: true
  exclude:
    - '*.min.css'
neat_js:
  enable: true
  mangle: true
  output:
  compress:
  exclude:
    - '*.min.js'
```

### [hexo-baidu-url-submit](https://github.com/huiwang/hexo-baidu-url-submit)

```
npm install hexo-baidu-url-submit --save
```

### [hexo-translate-title](https://github.com/cometlj/hexo-translate-title)

> 使用 Google 翻译，百度翻译和有道翻译将 Hexo 中的汉字标题转成英文标题

安装

```
npm install hexo-translate-title --save
```

在博客配置文件`_config.yml`中添加

```yml
translate_title:
  translate_way: google #google | baidu | youdao
  youdao_api_key: XXX
  youdao_keyfrom: XXX
  is_need_proxy: true #true | false
  proxy_url: http://localhost:8123
```

**注意**：判断是否需要配置 google 本地代理，因为我在本地是开启时才能访问 google 翻译的，如果没有被墙，请将`_config.yml` 下的`is_need_proxy: true`改为 false。如果设置为 true,请设置本地代理地址

## 数学公式

> Hexo 默认使用"hexo-renderer-marked"引擎渲染网页，该引擎会把一些特殊的 markdown 符号转换为相应的 html 标签

### 解决方案

解决方案有很多，可以网上搜下，为了节省大家的时间，这里只提供亲身测试过的方法。

更换 Hexo 的 markdown 渲染引擎，[hexo-renderer-markdown-it-plus](https://github.com/CHENXCHEN/hexo-renderer-markdown-it-plus)引擎替换默认的渲染引擎[hexo-renderer-marked](https://github.com/hexojs/hexo-renderer-marked)即可。

### 安装 hexo-renderer-markdown-it-plus 插件

```
npm un hexo-renderer-marked --save
npm i hexo-renderer-markdown-it-plus --save
```

### 配置

安装插件后，如果未正常渲染 LaTeX 数学公式，在博客配置文件`_config.yml`中添加

```
markdown_it_plus:
  highlight: true
  html: true
  xhtmlOut: true
  breaks: true
  langPrefix:
  linkify: true
  typographer:
  quotes: “”‘’
  plugins:
    - plugin:
        name: markdown-it-katex
        enable: true
    - plugin:
        name: markdown-it-mark
        enable: false
```

### 文章启用 mathjax

```
title: Hello World
mathjax: true
```

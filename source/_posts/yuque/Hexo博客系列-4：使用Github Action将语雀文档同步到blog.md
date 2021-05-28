---
title: Hexo博客系列-4：使用Github Action将语雀文档同步到blog
urlname: fe9yff
date: '2021-05-27 22:01:00 +0800'
categories: 博客
tags:
  - Hexo系列
  - 教程
---

## 具体流程

- Hexo 博客搭建好，将源码上传到 Github
- 在 Github 中开启 Github Actions 服务，配置 yaml 格式的 deploy 脚本
- 编写 serverless 函数 ，测试可以触发 Github Actions，获取触发地址
- 在语雀中创建知识库并发布一篇文章，必须发布过文章，因为 yuque-hexo 会删除 \_posts 文件夹
- 配置语雀的 webhook，使其可调用 serverless 函数
- 触发 Github Actions 实现 Hexo 的自动部署，生成静态文件并发布

## 使用 Github + Hexo

[Hexo 博客系列-1：搭建 GithubPages + Hexo 博客](https://blog.shiyutian.com/2019/11/26/yuque/Hexo%E5%8D%9A%E5%AE%A2%E7%B3%BB%E5%88%97-1%EF%BC%9A%E6%90%AD%E5%BB%BAGithubPages%20+%20Hexo%E5%8D%9A%E5%AE%A2/)

##

## 在 Github 中使用 Github Actions

1. 在 Hexo 源码仓库的页面选择`Actions` -> `Set up a workflow yourself`，进行网页编写
1. 在本地环境根目录创建`.github/workflows`文件夹，里面放写好的 `yaml` 脚本

具体脚本如下:

```javascript
# workflow name
name: Deploy To Github Pages

# 当有 push 到仓库和外部触发的时候就运行
on: [push, workflow_dispatch]

# YUQUE_TOKEN
# GITHUB_PERSONAL_API_TOKEN
jobs:
  deploy:
    name: Deploy Hexo Public To Pages
    runs-on: ubuntu-latest
    env:
      TZ: Asia/Shanghai

    steps:
    # check it to your workflow can access it
    # from: https://github.com/actions/checkout
    - name: Checkout Repository master branch
      uses: actions/checkout@master

    # from: https://github.com/actions/setup-node
    - name: Setup Node.js 10.x
      uses: actions/setup-node@master
      with:
        node-version: "10.x"

    # from https://github.com/x-cold/yuque-hexo
    - name: Setup Hexo Dependencies and Generate Public Files
      env:
        YUQUE_TOKEN: ${{ secrets.YUQUE_TOKEN }}
      run: |
        npm install hexo-cli -g
        npm install yuque-hexo -g
        npm install
        npm run start

    # from https://github.com/peaceiris/actions-gh-pages
    - name: Deploy
      uses: peaceiris/actions-gh-pages@v3
      with:
        personal_token: ${{ secrets.GITHUB_PERSONAL_API_TOKEN }}
        external_repository: 用户名/仓库
        publish_branch: master
        publish_dir: ./public
        commit_message: ${{ github.event.head_commit.message }}
```

​

### YUQUE_TOKEN 配置

在语雀中新建 Token，并添加此 Token 到 Hexo 源码仓库`Setting` -> `Secrets`中添加变量 key 为 YUQUE_TOKEN ，值为实际的 token 值
![image.png](https://cdn.nlark.com/yuque/0/2021/png/264294/1622125094633-dd8763b2-0ce2-4dd7-ab4a-fda525df4490.png#clientId=u19d8ecd1-3c6d-4&from=paste&height=1035&id=u46b27b52&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1035&originWidth=1907&originalType=binary&size=113842&status=done&style=none&taskId=u3f98407c-62cc-401e-b331-12cb4289d3b&width=1907)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/264294/1622125180151-963a2640-c4ad-4de7-bd58-09bee6abcb74.png#clientId=u3bc1a531-732f-4&from=paste&height=813&id=u21ce58c5&margin=%5Bobject%20Object%5D&name=image.png&originHeight=813&originWidth=1403&originalType=binary&size=123966&status=done&style=none&taskId=u11477364-41c6-4eb0-a906-86cbe3aab66&width=1403)

### GITHUB_PERSONAL_API_TOKEN 配置

Github 同理，点击 GitHub 用户设置页面 最下方的 Developer setting ，然后选择 Personal access tokens 来生成一个 token。然后添加到`Secrets`中即可
![image.png](https://cdn.nlark.com/yuque/0/2021/png/264294/1622125359200-f21c50d1-8ee7-4949-ab0b-734cc61f016b.png#clientId=u3bc1a531-732f-4&from=paste&height=566&id=u57031de8&margin=%5Bobject%20Object%5D&name=image.png&originHeight=566&originWidth=1200&originalType=binary&size=302931&status=done&style=none&taskId=u005a8200-d0fb-4619-a517-2de4959676b&width=1200)

​

## 配置腾讯云函数

1. 配置腾讯云函数登录腾讯云，搜索云函数，创建此处以 Python2.7 为例

![image.png](https://cdn.nlark.com/yuque/0/2021/png/264294/1622125492149-dfafba12-dc73-40f1-bccc-b8fe57fcca0e.png#clientId=u3bc1a531-732f-4&from=paste&height=482&id=ud8fc2900&margin=%5Bobject%20Object%5D&name=image.png&originHeight=482&originWidth=889&originalType=binary&size=31720&status=done&style=none&taskId=u5cb44a9d-c445-4986-8b0d-d6bcb4fc74b&width=889)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/264294/1622125554271-4cb247af-ea99-42f2-85e5-9c243453e126.png#clientId=u3bc1a531-732f-4&from=paste&height=873&id=u9a55ea0d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=873&originWidth=1462&originalType=binary&size=117834&status=done&style=none&taskId=uba62086e-90ae-4220-9ba0-4b9a869d865&width=1462)
详细代码如下

```python
# -*- coding: utf8 -*-
import requests

def main_handler(event, context):
    r = requests.post("https://api.github.com/repos/${{GithubUserName}}/${{GithubRepo}}/actions/workflows/${{workflowId}}/dispatches",
    json = {"ref":"master"},
    headers = {"User-Agent":'curl/7.52.1',
              'Content-Type': 'application/json',
              'Accept': 'application/vnd.github.v3+json',
              'Authorization': 'token ${{替换为你的githubToken}}'})

    if r.status_code == 204:
        return "This's OK!"
    else:
        return r.status_code

```

> 如果不知道具体的 workflowId，可以在调用此接口获取

```python
url:   https://api.github.com/repos/${{GithubUserName}}/${{GithubRepo}}/actions/workflows
method:  get
headers:  {"User-Agent":'curl/7.52.1',
   'Content-Type': 'application/json',
              'Accept': 'application/vnd.github.v3+json',
              'Authorization': 'token ${{替换为你的githubToken}}'}
```

### 创建云函数触发器

选择 API 网关触发即可，生成触发器之后，通过触发器 URL 直接访问，即可调用云函数
​

![image.png](https://cdn.nlark.com/yuque/0/2021/png/264294/1622126679142-0142e6cc-fb25-4d65-b4ee-ddd93c9abf77.png#clientId=u3bc1a531-732f-4&from=paste&height=661&id=u9891ed0a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=661&originWidth=903&originalType=binary&size=38481&status=done&style=none&taskId=u7f60d949-d807-44e7-8b40-95de0cd2f7d&width=903)

## 语雀配置

注册，登录创建知识库–>文档知识库–>可见范围为互联网可见
工作台–>知识库–>找到新创建的知识库，管理–>设置–>路径进行自定义，后面有用
![image.png](https://cdn.nlark.com/yuque/0/2021/png/264294/1622126805571-c3811d6e-1e77-4e97-af80-111af8d8c4d1.png#clientId=u3bc1a531-732f-4&from=paste&height=492&id=jIpuX&margin=%5Bobject%20Object%5D&name=image.png&originHeight=492&originWidth=895&originalType=binary&size=24910&status=done&style=none&taskId=u6f9d471c-97db-484e-9f85-213b5dac859&width=895)
工作台–>知识库–>找到新创建的知识库，管理–>设置–>开发者–>名称任意。URL 为云函数的地址，即上面获取到的访问路径
![image.png](https://cdn.nlark.com/yuque/0/2021/png/264294/1622126927418-6dc17af1-0339-4d18-990f-c69e73673572.png#clientId=u3bc1a531-732f-4&from=paste&height=789&id=ude09aeb0&margin=%5Bobject%20Object%5D&name=image.png&originHeight=789&originWidth=1352&originalType=binary&size=82518&status=done&style=none&taskId=u99bf6f88-192f-4086-9280-cdd7c354828&width=1352)

### 安装 yuque-hexo

```json
npm install yuque-hexo --save
```

编辑 package.json 文件，添加以下内容
​

Npm Scripts “start” 和 yuqueConfig，将操作命令合并为一个脚本，只需要执行 npm run start 即可完成语雀的同步和 Hexo 的清理和静态文件生成，yueque 记得要用最新版本

```json
{

 ...

  "scripts": {
    "start": "yuque-hexo clean && yuque-hexo sync && hexo clean && hexo generate",
    "build": "hexo generate",
    "clean": "hexo clean",
    "deploy": "hexo deploy",
    "server": "hexo server"
  },

 ...

  "yuqueConfig": {
    "baseUrl": "https://www.yuque.com/api/v2",
    "login": "语雀个人路径",
    "repo": "知识库名称",
    "mdNameFormat": "title",
    "postPath": "source/_posts",
    "onlyPublished": false
  }
}
```

[
](https://blog.csdn.net/z_johnny/article/details/104629805/)

PS: 本文参考[https://www.zhwei.cn/hexo-github-actions-yuque/](https://www.zhwei.cn/hexo-github-actions-yuque/)，对最新的配置做了些优化

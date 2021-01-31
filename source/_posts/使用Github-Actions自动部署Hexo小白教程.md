---
title: 使用Github Actions自动部署Hexo小白教程
toc: true
tags:
  - Hexo
  - 代码
  - CI
categories:
  - 代码
  - 博客相关
sticky: false
mermaid: false
math: false
date: 2021-01-31 18:45:13
---

## 概述

使用Github page + Hexo可以很方便的搭建博客，写博客。一般将hexo源文件存储在一个仓库的一个分支，而hexo产生的静态页面放在另一个分支，例如master分支，在Github page上选择以master分支为源展示页面。不过这产生了一个问题，一个完整的流程是<!-- more -->先`hexo clean && hexo g && hexo d`部署到github仓库的master分支，再将hexo源文件使用如下代码：

```git
git add .
git commit -m "备注信息……"
git push origin fluid(源文件所在分支名，我这里是fluid分支)
```

推送到仓库，这就显得过程比较繁琐复杂。`Github actions `提供了一种方便的方法，我们仅仅需要执行上述第二步，将源文件推送到Github Page的fluid分支上，当Github Actions 识别到我们有推送行为，就会将源文件自动部署到master分支上，减少了一半的工作量，让我们更专注于博客写作而不是调试博客系统。下面叙述一下Github Actions实现这一功能的配置步骤。

## 获取访问秘钥

如果之前能正常使用`hexo d`推送，这步应该已经完成，可以视情况跳过。

在本地库的hexo文件夹下使用git bash打开命令窗口，输入

```git
git config --global user.name "你的github名字"
git config --global user.email "你的github邮箱"
```

之后输入指令生成SSH的公钥和私钥

```git
ssh-keygen -t rsa -C "你的github邮箱"
```

输入以下代码后一直按回车确认，直到代码执行完毕，在`/c/Users/你当前的windows用户名/.ssh文件夹`可以找到生成的公钥和私钥(有时Win系统Users文件夹会显示为中文的“用户”)，然后里面的两个文件分别为`id_rsa(私钥) 和 id_rsa.pub(公钥)`，同时在本地库的`.gitignore`文件中把公钥和私钥的**文件名**加进去，防止误操作将秘钥泄露到github远程库。

打开github，在头像下面点击`settings`，再点击`SSH and GPG keys`，新建一个SSH，名字随意，将**公钥文件**内所有内容粘贴到框内。在git bash中输入`ssh -T git@github.com`，下面出现你的用户名说明已经核验成功。(p.s.：SSH keys是最高权限的用户公钥，仅仅管理Github的Hexo仓库的话，`Setting -> Deploy keys`单独设置deploy keys也可以达到同样的效果。)然后在Hexo文件所在Github仓库中`Settings -> Secrets -> Add a new secret`页面上创建一个新的**私钥文件**，名字随意但是要记下来，等下配置actions时有用，默认`Name`为`ACTIONS_DEPLOY_KEY`，将`id_rsa(私钥)`文件中的所有内容粘贴进去。

## 配置Github Actions

### 新建目录

在本地库中新建文件，目录结构为`.github/workflows/CI.yml`,总共三个文件。

### 配置CI的YAML文件

```yaml
name: CI
on:
  push:
    branches:
      - fluid
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: 查询分支
        uses: actions/checkout@v2

      - name: 安装node.js
        uses: actions/setup-node@v1
        with:
          node-version: '10.x'
    
      - name: 安装Hexo依赖
        run: |
          npm install hexo-cli -g
          npm install

      - name: 清除Hexo
        uses: heowc/action-hexo@main
        with:
          args: clean

      - name: 生成Hexo
        uses: heowc/action-hexo@main
        with:
          args: generate

      - name: 部署到master分支
        uses: peaceiris/actions-gh-pages@v3
        with:
          deploy_token: ${{ secrets.ACTIONS_DEPLOY_KEY }}
          external_repository: 你的github名称/你的github名称.github.io
          publish_branch: master
          publish_dir: ./public
```

### 相关参数简述

CI会在fluid分支中出现push时执行以下jobs。

环境运行在Ubuntu最新版本系统下，`actions/checkout@v2`组件会查询所在库的下拉菜单，识别到各个分支名称。

然后安装版本10.x的node.js，并安装hexo的依赖以及必备组件，这样github服务器上的完整运行环境就搭建起来了。

最后一步就是服务器代为我们完成`clean`，`generate`等操作，最后使用`peaceiris/actions-gh-pages@v3`组件帮我们`deploy`到`master`分支。当然，这个组件还有两种其他配置方法，可以在github上查询到更详细更灵活的用法。

## 尾声

本地库配置完毕后，就在hexo文件夹的根目录下的git bash中执行

```git
git add .
git commit -m "备注信息……"
git push origin fluid(源文件所在分支名，我这里是fluid分支)
```

同步至github远程库中，github会自动识别`.github/workflows/CI.yml`里的YAML配置文件，然后建立Actions。正常情况下博客源文件push后，几分钟Actions建立(Build)完毕后，就会看到Github服务器将源文件部署到master分支上了。之后写博客也仅仅需要执行上述三行代码就能将`Hexo源文件`和`Hexo生成的静态页面`同时部署到github上了。
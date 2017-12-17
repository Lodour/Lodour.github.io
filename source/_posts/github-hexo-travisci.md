---
title: Hexo + Travis CI 自动部署博客
date: 2017-12-17 08:25:47
tags: 技术
categories: 技术
---

利用 GitHub + Hexo 搭建个人博客，并通过 Travis CI 自动部署。

<!--more-->

# 安装环境
## Homebrew
> macOS 软件包管理工具。

### 替换 brew 源
* [brew](https://mirrors.shuosc.org/help/homebrew.html)
* [homebrew-bottles](https://mirrors.shuosc.org/help/homebrew-bottles.html)
* [homebrew-cask](https://mirrors.shuosc.org/help/homebrew-cask.html)
* [homebrew-core](https://mirrors.shuosc.org/help/homebrew-core.html)

### 更新 brew
```shell
brew update && brew upgrade
```

## Node.js
> JavaScript 运行环境，npm 是其自带的软件包管理工具。

```shell
brew install node
node -v
# v8.9.2
npm -v
# 5.5.1
```

### 替换 npm 源
```shell
npm config set registry https://registry.npm.taobao.org
npm config get registry
# https://registry.npm.taobao.org/
```

### 备注
npm 权限问题参考[链接](https://docs.npmjs.com/getting-started/fixing-npm-permissions)

## Hexo
> 可以直接参考[官网](https://hexo.io/)
```shell
npm install -g hexo-cli
hexo -v
# hexo-cli: 1.0.4
# ...
```

# 准备博客

## 初始化

```shell
hexo init <folder>
cd <folder>
npm install
```

此时就能使用 `hexo s` 预览博客了。

## 发布到 src 分支
首先在 GitHub 上创建仓库 `<repository>`
若创建个人博客，要求仓库名为 `<username>.github.io`

此时可以根据需要添加相关文件，如 `README.md`，`LICENSE`。

```shell
git init
git checkout -b src
git add .
git commit -m "Initial Commit"
git remote add origin git@github.com:<username>/<repository>.git
git push -u origin src
```

## 自动部署
实现将源码 push 到 src 分支后，自动将页面部署到 master 分支。

### 获取 Github Token
该 token 将用于使 Travis CI 具有操作仓库的权限。

* GitHub / Settings / Developer settings
* Personal access tokens / Generate new token
* 编辑 Token description 如 `Travis CI for <repository>`
* 选中 **repo - Full control of private repositories**
* Generate Token

### 准备 Travis CI
* 通过 GitHub 登录面向 [Public](https://travis-ci.org/) / [Private](https://travis-ci.com/) 仓库的Travis CI
* 左侧 **My Repositories** 旁 **+** 号 添加仓库
* 右上角 **Sync account** 同步仓库数据
* 找到对应仓库，**开启**后进入**设置**
  * 开启 **Build only if .travis.yml is present**
  * 在 **Environment Variables** 中添加刚才生成的 **token**
    *  **Name:** `token`
    *  **Value:** `<token>`
* 点击仓库名旁徽章，可获取其 Markdown 代码

### 配置 Travis CI
相关配置文件可参考[这里](https://github.com/Lodour/Lodour.github.io)。
* 在 GitHub 仓库中添加配置文件 `.travis.yml`
* 在 `README.md` 中添加构建状态徽章
* 提交到仓库
  ```shell
  git add .
  git commit -m "Enable Travis CI"
  git push
  ```

正常情况下，此时已经触发了自动部署。

### 关闭 GitHub 依赖安全检测
在 Settings / Data services 中关闭 Vulnerability alerts。

## 定制博客
### 开启 RSS Feed
* `npm install hexo-generator-feed --save`
* 在 `_config.yml` 添加
  ```yml
  ## RSS
  feed:
      type: atom
      path: atom.xml
      limit: 20
  ```

### 开启 Mathjax
* `npm install hexo-renderer-mathjax --save`
* 在 `_config.yml` 添加
  ```yml
  ## Mathjax
  mathjax: true
  ```

### 更换主题
更换主题后，对于主题内的 `node_modules`，有以下两种解决方案。

1. 上传，则修改 `.gitignore` 为 `/node_modules`
2. 编译，对 `.travis.yml` 进行修改

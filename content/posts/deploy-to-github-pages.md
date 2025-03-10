---
title: "如何将Hugo博客部署到GitHub Pages"
date: 2025-03-11T00:10:00+08:00
draft: false
tags:
  - GitHub
  - 部署指南
categories:
  - 网站建设
---

# 将Hugo博客部署到GitHub Pages

GitHub Pages是一个免费的静态网站托管服务，非常适合用来部署Hugo生成的静态博客。本文将指导你如何将Hugo博客部署到GitHub Pages上。

## 准备工作

在开始之前，确保你已经：

1. 安装了[Git](https://git-scm.com/)
2. 有一个[GitHub](https://github.com/)账号
3. 已经在本地创建并配置好了Hugo博客

## 创建GitHub仓库

首先，你需要在GitHub上创建两个仓库：

1. `<username>.github.io` - 用于存放生成的静态网站文件
2. `hugo-blog` (或任何你喜欢的名称) - 用于存放Hugo博客的源代码

## 配置Hugo

修改你的`hugo.toml`（或`config.toml`）文件，设置正确的`baseURL`：

```toml
baseURL = "https://<username>.github.io/"
```

## 创建部署脚本

在博客根目录创建一个名为`deploy.sh`的脚本文件：

```bash
#!/bin/bash

echo -e "\033[0;32m开始部署到GitHub Pages...\033[0m"

# 构建静态页面
hugo

# 进入public目录
cd public

# 初始化git仓库
git init
git add .
git commit -m "Deploy to GitHub Pages"

# 推送到你的GitHub Pages仓库
git push -f git@github.com:<username>/<username>.github.io.git master

# 返回上级目录
cd ..

echo -e "\033[0;32m部署完成！\033[0m"
```

记得将`<username>`替换为你的GitHub用户名，并给脚本添加执行权限：

```bash
chmod +x deploy.sh
```

## 使用GitHub Actions自动部署

更现代的方法是使用GitHub Actions进行自动部署。在你的Hugo源码仓库中创建`.github/workflows/hugo.yml`文件：

```yaml
name: Deploy Hugo site to Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

defaults:
  run:
    shell: bash

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: recursive
      
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: true

      - name: Build
        run: hugo --minify
      
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

## 部署流程

### 手动部署

1. 在本地编辑你的博客内容
2. 运行`./deploy.sh`脚本进行部署

### 使用GitHub Actions自动部署

1. 将你的Hugo博客源码推送到GitHub仓库
2. GitHub Actions会自动构建并部署你的网站

```bash
git add .
git commit -m "更新博客内容"
git push origin main
```

## 自定义域名

如果你想使用自定义域名，可以在GitHub Pages仓库的Settings中设置，或者在`static`目录下创建一个名为`CNAME`的文件，内容为你的域名：

```
yourdomain.com
```

## 常见问题

### 部署后网站样式丢失

检查`hugo.toml`中的`baseURL`是否正确设置。如果使用自定义域名，`baseURL`应该是你的域名。

### 主题子模块问题

如果你的主题是作为Git子模块添加的，确保在克隆仓库时使用`--recursive`选项：

```bash
git clone --recursive https://github.com/<username>/hugo-blog.git
```

或者在已克隆的仓库中初始化子模块：

```bash
git submodule update --init --recursive
```

## 总结

通过GitHub Pages部署Hugo博客是一种免费且高效的方式，可以让你专注于内容创作而不必担心服务器维护问题。结合GitHub Actions，你可以实现完全自动化的部署流程，每次推送更改后自动更新你的网站。

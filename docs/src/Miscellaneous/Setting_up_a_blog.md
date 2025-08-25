# 搭建 blog

## 作用

写博客，无非是存一些零散知识什么的，方便回顾。

[示例](https://tenshi0x0.github.io/)

## 搭建流程

本文以 Github 博客 + Hexo 为例讲解。

1. 首先准备：
- Node.js
- Git

2. 创建 GitHub 仓库，名字必须为 `[user_name].github.io`，设为 Public 且不要初始化 README。

3. 搭建 Hexo 博客：

   ```sh
   # 安装 Hexo
   npm install -g hexo-cli
   
   # 创建博客
   cd D:\MyBlog
   hexo init blog
   cd blog
   npm install
   
   # 安装 Fluid 主题
   npm install --save hexo-theme-fluid
   
   # 安装部署插件
   npm install --save hexo-deployer-git
   
   # 复制主题配置文件
   Copy-Item node_modules/hexo-theme-fluid/_config.yml _config.fluid.yml
   ```

   然后编辑根目录（记为 blog，下列操作默认在根目录下执行）下的 `_config.yml`：

   ```sh
   # 站点信息
   title: 你的博客标题
   subtitle: '副标题'
   description: '博客描述'
   keywords: '关键词1,关键词2'
   author: 你的名字
   language: zh-CN
   # timezone: 'Asia/Shanghai'
   
   # URL
   url: https://你的用户名.github.io
   permalink: :year/:month/:day/:title/
   
   # 主题
   theme: fluid
   
   # 部署配置
   deploy:
     type: git
     repo: https://github.com/你的用户名/你的用户名.github.io.git
     branch: main
   ```

   创建必要页面（我忘记是否一定要执行这一步了，如果发现自己博客已经有下列页面（或者 `blog/source/` 下已经有 `about, tags, categories`）可以不做）：

   ```sh
   hexo new page about
   hexo new page tags
   hexo new page categories
   ```

   4. 源码管理

      ```sh
      # 初始化 Git
      git init
      
      # 添加所有文件
      git add .
      git commit -m "Initial blog source"
      
      # 添加远程仓库
      git remote add origin https://github.com/你的用户名/你的用户名.github.io.git
      
      # 创建并切换到 source 分支
      git checkout -b source
      
      # 推送源码
      git push -u origin source
      ```

   5. 创建部署脚本 `deploy.ps1`，执行时使用指令 `.\deploy.ps1 -Verbose` 即可。

      ```sh
      # 保存源码
      git add .
      git commit -m "Update blog source"
      git push
      
      # 部署网站
      hexo clean
      hexo generate
      hexo deploy
      
      Write-Host "Blog deployed successfully!" -ForegroundColor Green
      ```

      

   

## 后续维护

### 本地预览

```sh
# 通过 http://localhost:4000 预览
hexo server

# 刷新缓存并开启本地预览
hexo clean; hexo g; hexo s
```

### 新建文章

我认为直接在 `source/_post/` 下存放所有的文章不便维护（因为多起来就不知道分类了），后试了一下发现可以在 `_post` 下随便开文件夹也会被检测到，所以可以放心在 `source/_post/` 下建文件夹并维护文章。



### 在其他电脑上继续写博客

```sh
# 克隆源码
git clone -b source https://github.com/你的用户名/你的用户名.github.io.git blog
cd blog

# 安装依赖
npm install

# 下面即可正常使用
```



### 编写博客

md 文件顶部需要加上类似于下列格式：

```md
---
title: CF934 题解
date: 2025-08-24 20:00:00
categories:
  - CP
  - CF 题解
tags:
  - DP
  - 计算几何
mathjax: true
---
```

**支持数学公式**：

1. 修改 `_config.fluid.yml`：

   ```yaml
   math:
       enable: true    # 改这里
       specific: false
       engine: mathjax
   ```

2. 更换 Markdown 渲染器

   ```sh
   # 卸载默认渲染器
   npm uninstall hexo-renderer-marked --save
   
   # 安装支持数学公式的渲染器
   npm install hexo-renderer-kramed --save
   ```

3. 安装 MathJax 插件：

   ```sh
   npm install hexo-filter-mathjax --save
   ```

   

**修改代码高亮**：

```yaml
# 代码高亮
# Code highlight
highlight:
  # ...
  highlightjs:
    style: "monokai"
    style_dark: "monokai"
```





## 可能遇到的问题

暂无。

## 进阶操作



## 美化

*待更新
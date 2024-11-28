---
date: 2024-11-28
title: 用vitepress搭建一个简单博客
category: 教程
tags:
- vitepress
- markdown
description: 最近在搭建博客，因为我原来用的docsify对SEO的支持不好，遂换源码
---
# 前言
最近在搭建博客，因为我原来用的docsify对SEO的支持不好，遂换源码，然后找到了vitepress这个项目，然后我在github找到了一个很好的主题，没错，就是你正在看的这个博客的主题

[这个主题的github项目地址](https://github.com/airene/vitepress-blog-pure)

## 安装

需要的程序：nodejs v18及以上最好，git命令

```bash
git clone https://github.com/airene/vitepress-blog-pure
cd vitepress-blog-pure

# 该项目有点依赖问题，所以加上--force忽略依赖问题
npm install --force

# 开发模式
npm run dev

# 构建成静态文件，会生成在.vitepress/dist里面
npm run build
```

主题怎么配置？见[https://github.com/airene/vitepress-blog-pure](https://github.com/airene/vitepress-blog-pure)

vitepress还是很不错的，访问也快，还能部署到各种pages上

好啊，很好啊（赞赏）

<Comment />



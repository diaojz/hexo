---
title: 如何发布文章
date: 2019-07-29 10:59:53
tags: 教程
categories: 教程
---
## 新建一篇文章
> 在hexo博客目录下，输入命令行

``` shell
hexo new "博客文章名字，最好是英文，方便生成链接路径"
```

## 本地运行服务，看效果
``` shell
hexo server
# 或者
hexo s 
```

## 一键发布到网站
```shell
# 清理 && 生成静态文件 && 部署远程网站
hexo clean && hexo generate && hexo deploy
# 或者
hexo clean && hexo g && hexo d
```
官网地址: [官网中文](https://hexo.io/zh-cn/docs/server.html)

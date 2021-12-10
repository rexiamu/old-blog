---
title: git使用
date: 2021-11-22 15:52:27
tags:
  - git
  - 工具
categories:
  - 工具使用
---

![git](https://gitee.com/Rexiamu/image-hosting/raw/master/img/20211122160438.png)

Git 是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。Git 与常用的版本控制工具 CVS, Subversion 等不同，它采用了分布式版本库的方式，不必服务器端软件支持。

<!-- more -->

## git主要工作流程

![git工作流程](https://gitee.com/Rexiamu/image-hosting/raw/master/img/20211122160702.png)

图例说明：

- workspace：工作区
- staging area：暂存区/缓存区
- local repository：版本库或本地仓库
- remote repository：远程仓库

## git基础命令

Git 常用的是以下 6 个命令：git clone、git push、git add 、git commit、git checkout、git pull。

```Shell
git add .         # 添加所有文件到暂存区
git commit        # 提交暂存区到本地仓库
git push          # 上传代码至远程仓库并合并
git pull          # 拉取远程仓库代码并合并
git checket       # 检出，创建分支、切换分支
git clone         # 克隆远程仓库代码
```
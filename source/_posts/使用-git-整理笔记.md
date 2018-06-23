---
title: '使用 git 整理笔记 '
date: 2018-03-10 17:06:49
tags:
---

# 记录常用 git 操作

## 克隆远端代码

        git clone '远端代码地址: Use an SSH key'

## 分支管理

查看当前所有分支

        git branch

创建分支 work 为分支名可自定义

        git branch work

切换分支 切换到work分支

        git checkout work

创建并切换到敢创建的分支

        git checkout -b work

删除分支 work

        git branch -d work

## 缓存

缓存当前未提交的代码

        git stash

缓存当前未提交代码并命名为  stashOne

        git stash save "stashOne"

查看缓存区列表

        git stash list

取出缓存区最新的一条缓存代码 (取出的同时在缓存区移除)

        git stash pop

取出指定列表代码 代码编号(从list查看)

        git stash apply "代码编号"

# git 合作开发的基本操作

## 拉取远端代码 拉取最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突

        git pull

## 推送代码到远程

        git push

## 添加修改的文件到版本库

        git add

添加所有的修改

        git add .

添加指定文件的修改

        git add "指定路径"

## 撤销修改

            git checkout

撤销所有的修改

            git checkout .

撤销指定文件的修改

            git checkout "指定路径"

## 提交上一步 add 的文件

        git commit -m "此次修改或增加的功能简要说明"

## 切换分支  切换到work分支

        git checkout work

## 查看提交日志

        git log

## 查看使用命令日志

        git reflog

## 查看工作区文件修改的内容

        git diff

## 版本回退

HEAD:代表当前版本,HEAD^代表上个版本,HEAD^^上上个版本......HEAD~100......

        git reset --hard 版本号 (或者HEAD)

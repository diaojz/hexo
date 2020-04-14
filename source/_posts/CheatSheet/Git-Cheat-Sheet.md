---
title: Git常用命令
date: 2019-08-23 15:39:54
tags: Git
categories: CheatSheet
---

工作中使用最多的就是Git了，除了用了管理代码，发布博客。我还用来管理文档，可以使用`git show`或`git diff`查看更改。
整理了一下Git常用命令，方便随时查阅。

# 速查表
![Git常用命令速查表](/Git-Cheat-Sheet/git_command_pic.png)


<!--more-->


# Git初始化
## 初始化用户信息
```bash
git config --global user.name "xxx"
git config --global user.email "xxx@gmail.com"
```

## 初始化项目
### 1.在网站用模板创建的项目
```bash
# 1.克隆远程版本库
git clone <url>
# 2.进入项目文件夹
cd xxx
# 3.创建README.md文件
touch README.md
# 4.创建LICENSE文件
touch LICENSE
# 5.创建.gitignore文件,根据项目语言选择和修改
touch .gitignore
# 以上3~5最好用GitHub或gitlab网站生成

# 6.提交文件，填提交日志，推送
git add .
git commit -m "commit message"
git push -u origin master
```
### 2.本地已有项目，和网站关联
```bash
cd existing_folder
git init
git remote add origin git@xx.xx.xx.xx:diaojz/xxx.git

# 也需要添加.gitignore、README.md、LICENSE等文件
git add .
git commit -m "Initial commit"
git push -u origin master
```

### 3.本地已经有Git仓库了,关联仓库，推送所有分支和tags
```bash
cd existing_folder
git remote add origin git@xx.xx.xx.xx:diaojz/xxx.git
git push -u origin --all
git push -u origin --tags
```

## 推送代码到指定仓库
```bash
# 推送到主仓库
git push origin master
# 推送分支
git push origin sit:sit   
git push 仓库名  本地分支名:远程分支名

# 推送到gitee仓库(不同仓库)
git push gitee master:master
```

## 查看远程仓库
```bash
# 查看远程仓库详情(神器，可以显示所有分支和tags)
git remote show origin

# 查看本地代码关联的远程仓库
git remote -v

# 添加远程仓库（针对已经有.git文件的项目）
git remote add origin git@xx.xx.xx.xx:diaojz/xxx.git
git remote add gitee git@yy.yy.yy.yy:diaojz/xxx.git
以上可以关联2个仓库地址

# 解除远程git和本地代码绑定
git remote rm origin
git remote rm gitee
```

# 增删改查(add/rm/mv)
## 查看
```bash
# 查看修改文件
git status
# 查看日志详情
git show
# 查看修改具体内容(工作区和暂存区差异)
git diff
# 查看日志
git log
# 查看指定文件的提交历史记录
git log -p <file>
# 以列表的方式查看指定文件的提交历史
git blame <file>
```

## 搜索
```bash
git grep xxx 
```

## 增删改
```bash
# 增加
git add .
git add <file>
```
```bash
# 删除
git rm <file>
# 删除文件追踪
git rm --cached <file>
# 忽略追踪文件
git rm -r --cached .
```
```bash
# 修改文件
git mv <old> <new>
# 撤销指定未提交的文件
git checkout HEAD <file>
git checkout xxx.xib  xxx.h  xxx.m
```

## 提交 commit
```bash
git commit -m "commit message"
```

# 回滚(reset/revert)
```bash
# 修改最后一次提交（修改提交日志）
git commit --amend 

# 撤销所有未提交的文件
git reset --hard HEAD

# 撤销指定提交，是个相反的操作
git revert <commit号>
git revert 分支名 <commit号>
git revert HEAD^
```
[git revert 和 git reset区别](https://juejin.im/post/5b0e5adc6fb9a009d82e4f20)

# 合并(merge/rebase)
2个仓库之间的合并，合并其他人分支开发，发布版本等
```bash
# 合并指定分支到当前分支
git merge <branch即指定分支名>
# 衍合
git rebase <branch>
```

# 分支 branch
```bash
# 查看分支（`-a`包含本地和远程所有分支）
git branch -a
# 切换分支/标签
git checkout <branch/tag>
# 创建分支（`-b`会自动切换到当前分支）
git checkout -b dev 
# 创建指定分支(自动切换)
git checkout -b 分支名 origin/远程分支名
# 创建指定分支(不切换)
git fetch origin 远程分支名x:本地分支名x
# 删除分支(删除分支时,该分支必须完全和它的上游分支merge完成)
git branch -d dev
# 强制删除分支(这样写可以在不检查merge状态的情况下删除分支)
git branch -D dev
# 强制删除分支(将当前branch重置到初始点(startpoint),如果不使用--force的话,git分支无法修改一个已经存在的分支)
git branch -d -f dev
```
# 标签 tag
```bash
# 查看标签：
git tag
# 删除标签 
git tag -d v1.0.0
# 打标签：
git tag -a v1.0.0 -m 'version 1.0.0 打版'
# 显示标签详情：
git show v1.0.0
# 推送标签 
git push origin v1.0.0
# 推送全部标签 
git push origin --tags
```
[打标签官网](https://git-scm.com/book/zh/v1/Git-%E5%9F%BA%E7%A1%80-%E6%89%93%E6%A0%87%E7%AD%BE)

	
# 暂存 stash 
## 普通用法
```bash
git stash
git stash pop
git stash list
```
## 高级用法
```bash
git stash pop [–index] [stash_id]
git stash pop 恢复最新的进度到工作区。git默认会把工作区和暂存区的改动都恢复到工作区。
git stash pop --index 恢复最新的进度到工作区和暂存区。（尝试将原来暂存区的改动还恢复到暂存区）
git stash pop stash@{1}恢复指定的进度到工作区。stash_id是通过git stash list命令得到的 
通过git stash pop命令恢复进度后，会删除当前进度。

git stash apply [–index] [stash_id]
除了不删除恢复的进度之外，其余和git stash pop 命令一样。

git stash drop [stash_id]
删除一个存储的进度。如果不指定stash_id，则默认删除最新的存储进度。

git stash clear
删除所有存储的进度。
```

# 解决冲突 conflict
多人代码提交，版本合并等操作可能产生冲突
```bash
# 通过工具，解决合并冲突
git mergetool -t opendiff
# 合并项目
--allow-unrelated-histories
```

# 别名 alias 
## 打开配置文件
```bash
vim ~/.bash_profile
source ~/.bash_profile
```
## 配置别名
```bash
git config --global alias.last log -l
git config --global alias.gst git status
git config --global alias.gc  git checkout
git config --global alias.gcam git commit -a -m
git config --global alias.br branch
```
## 丧心病狂的log
```bash
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

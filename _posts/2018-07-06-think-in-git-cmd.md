---
layout: bootstrap
title: 'Git命令理解'
description: 'Think in git cmd'
date: '20118-07-06 15:58:00'
author: Jast
---
## {{ page.title }} 
<i class="far fa-clock"></i>{{ page.date | date: "%Y-%m-%d"}}  <i class="far fa-user">{{ page.author }}  

## 1.基础
### 1.1 git init
在现有目录中初始化仓库，执行该命令后会生成一个`.git`的目录，目录中会有如下文件结构:
```
|--.git
	|--hooks
	|--info
	|--objects
	|--refs
	`--config
	`--description
	`--HEAD
```

### 1.2 git add
添加内容到下一次提交中。这是个多功能命令：可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并是把冲突的文件标记为已解决状态。  
实例：
- 添加单个文件：`git add 文件名（包括后缀）`
- 添加一类文件：`git add *.java （后缀名）`
- 添加所有文件：`git add --all|-a`
- 添加目录：`git add .(当前目录)`

### 1.3 git clone
克隆现有的仓库到一个新目录（新目录可以指定也可以不用）。  
实例：
- 克隆仓库到当前目录：`$ git clone https://github.com/libgit2/libgit2`
- 克隆仓库到新目录：`$ git clone https://github.com/libgit2/libgit2 mylibgit2`

### 1.4 git status
用于查看文件状态，工作目录下的文件状态：已跟踪（tracked）和未跟踪（tracked） ,已跟踪包括三种类型：未修改，已修改， 已暂存（staged）。该命令可以带选项`-s|--short`即：`git status -s|--short`使输出变得简洁。

实例： 
```
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   test.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   _posts/2018-07-06-version-control-system.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        _posts/2018-07-06-think-in-git-cmd.md

```
状态描述：
- Changes to be committed: 已暂存
- Changes not staged for commit:未暂存（修改了的文件也是未暂存的，通过git add .暂存当前目录下的所有文件）
- Untracked files：未跟踪
#### 带`-s`选项
实例：  
```
$ git status -s
 M _posts/2018-07-06-version-control-system.md
A  test.txt
?? _posts/2018-07-06-think-in-git-cmd.md
```
A :新添加到暂存区中的文件  
??:新添加的未跟踪文件  
 M:靠右的M,文件被修改了但是还没放入暂存区  
M :靠左的M文件被修改了并放入了暂存区  
MM:工作区被修改并提交到暂存区后又在工作区中被修改了  

### 1.5 git diff

### 1.6 git commit

### 1.7 git commit

### 1.8 git rm

### 1.9 git mv

### 1.10 git log

### 1.11 git reset HEAD

### 1.12 git checkout -- 

### 1.13 git remote

### 1.14 git fetch

### 1.15 git push

### 1.16 git tag

### 1.17 git别名

### 参考
- [Git 基础 - 获取 Git 仓库](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E8%8E%B7%E5%8F%96-Git-%E4%BB%93%E5%BA%93)

## 

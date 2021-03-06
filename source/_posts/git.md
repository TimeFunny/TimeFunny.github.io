---
title: git
date: 2019-11-18 12:02:53
tags:
 - git
---

<!-- toc -->

1. [git docs](https://git-scm.com/docs)
2. [git tutorials](https://www.atlassian.com/git/tutorials/)
3. [github help]( https://help.github.com/en/github )

# basic

## 1. create a first repo

```shell
# create a dir
> mkdir dir && cd dir
# initialize a repo
> git init

# check repo status
> git status
# add new files and changes to stage area
> git add -A ./   # 当前目录下所有的文件与子目录，包含删除文件
# commit stage area data to local repo
> git commit -m "some change record"
```



## 2. branch

```shell
# check branch info
> git branch
# check branch detailed information
> git branch -vv

# create a repo branch
> git branch fixed
# switch to branch
> git checkout fixed

# one command to create and switch to a new branch
> git checkout -b fixed

# merge fixed branch to master branch,and delete fixed branch
> git checkout master
> git merge fixed
> git branch -d fixed
```



## 3. reset

 `HEAD`表示当前版本(最新的提交),上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`,往上100个版本`HEAD~100` 

### 1. 分支操作
```shell
# 文件会被还原
> git reset --hard HEAD^

> git reset --hard <commit id>
```
### 2. 文件操作
不修改文件内容,改变文件提交状态，可以取消`git add`操作

```shell
> git reset HEAD -- readme.txt<filename>

> git reset -- filename
```
- You can unstage files by using the `git reset <target>`command
- Files can be changed back to how they were at the last commit by using the command: `git checkout -- <target>`
- `git reset <target>` 取消`git add`的操作效果，`git checkout -- <target>` 把文件恢复到上一次commit的状态

修改文件内容

```shell
> git checkout -- readme.txt<filename>
```



## 4. patch

### 1. create branch

```shell
# 创建新的branch
> git checkout -b  fix-bug
# 修复,提交,测试后,check
> git log --pretty=oneline -3
# Extract all commits which are in the current branch but not in the master branch
> git format-patch maste --stdout > bug_fixed.patch
```

### 3. apply patch

```shell
# Apply patch, switch to master branch
> git checkout master
## look at what changes are in the patch
> git apply --stat bug_fixed.patch
##  test the patch
> git apply --check bug_fixed.patch

# apply patch
## applies the patch but does not create a commit
> git apply bug_fixed.patch  # 适合于引用外部开源软件(submodule)，保留其开源版权
## create commits from patches generated by git-format-patch
> git am --signoff < bug_fixed.patch
```



## 5. submodule

```shell
# create a new submodule
> git submodule add https://github.com/chaconinc/DbConnector
##  specify a path
> git submodule add https://github.com/chaconinc/DbConnector path

# update the submodules of a repo
> git submodule init
> git submodule update --init --recursive

# clone repo, and update submodule
git clone --recurse-submodules https://github.com/chaconinc/MainProject
```

## 6. remote

**url format**:

```shell
# public repo:
https://github.com/username/repository.git

# authorize, private repo
https://username:password@github.com/username/repository.git
```

### 1. clone

```shell
> git clone url
```

### 2. add/delete/rename remote

```shell
# verigy remote
> git remote -v

# add a new remote 
> git remote add remote_name url

# Remove remote
> git remote rm remote_name

# rename remote
> git remote rename remote_name other
```

### 3. pull

```shell
#
> git pull origin master

## 
# downloads the data to your local repository
> git fetch upstream
# 选择一个branch进行合并
> git merge upstream/master
```

 `git clone`command automatically sets up your local master branch to track the remote master branch (or whatever the default branch is called) on the server you cloned from. Running `git pull` generally **fetches** data from the server you originally cloned from and automatically tries to **merge** it into the code you're currently working on. 

```shell
# it runs git rebase instead of git merge
> git pull --rebase origin(remote_name)  master(remote_branch)
```

### 4. push

```shell
# Pushing  Remotely
# The -u tells Git to remember the parameters, so that next time we can simply run git push and Git will know what to do. Go ahead and push it!  
> git push -u origin(remote_name) master(local_branch):remote_branch
```

### 5. Inspecting a Remote

```shell
# git remote show <remote>
> git remote show origin
```


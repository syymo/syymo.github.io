---
title: Github基本用法
date: 2017-09-25 13:32:33
layout: post
subtitle: ""
author: "syymo"
header-img: ""
cdn: 'header-on'
tags:
    - GitHub
    - Linux
---
### linux安装：
```
sudo apt-get install git 
```

> 创建：$ mkdir SignIn
> 进入：$ cd SignIn
> 显示所有文件：$ pwd
> 显示隐藏的文件夹：ls -ah
> 查看操作记录：git log
> 查看历史命令：git reflog
> 退回上个操作：git reset --hard HEAD^

### window安装：
点击下载：[git](https://git-for-windows.github.io/)

### 绑定用户名和邮箱：
```
$ git config --global user.name "your name"  //绑定用户名
$ git config --global user.email "your_email@youremail.com" //绑定邮箱
```
### 生成ssh key：
$ ssh-keygen -t rsa -C "your_email@youremail.com"


### 创建本地仓库：
> git init 生成仓库
> git add 添加到工作区
> git commit -m "描述记录"    提交至仓库

### 远程文件同步到本地
解决git error: failed to push some refs to 'git@github.com:
```
远程同步到本地：
$ git pull origin master 
```
```
将远程的readme下载至本地：
$ git pull --rebase origin master
```
### 从远端克隆到本地
```
从远端下载到本地：
$ git clone git@github.com:michaelliao/gitskills.git
```
### 从本地上传至远程仓库：
```
添加远程地址：
$ git remote add origin git@github.com:yourName/yourRepo.git
```

```
第一次推送master分支的所有内容：
$ git push -u origin master
```
```
推送最新修改：
$ git push origin master
```
```
强制推送
$ git push -f origin master 
```
```
fatal: remote origin already exists.
$ git remote rm origin //删除远程主机
```

```
新建分支
$ git branch 分支名
$ git checkout 分支名
```
```
删除分支
$ git push origin --delete 分支名
```

### 利用git托管静态html页面：
> - #### 第一步
/dist 目录需要被 git 记录，于是后面我们才可以用它作为子树（subtree），因此 /dist 不能被 .gitignore 规则排除

> - #### 第二步
git subtree push --prefix dist origin gh-pages  //创建gp-pages分支并且把dist文件上传
其中：
dist 代表子树所在的目录名
origin 是 remote name
gh-pages 是目标分支名称

### 一些错误解决方法：
sh出错 sign_and_send_pubkey: signing failed: agent refused operation
Posted on2016-05-26
在服务器添加完公钥之后，ssh服务器然后报了这个错误

sign_and_send_pubkey: signing failed: agent refused operation
然后执行了以下命令才好。。
```
eval "$(ssh-agent -s)"
ssh-add
```
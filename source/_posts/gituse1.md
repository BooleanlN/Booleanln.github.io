---
title: Git的使用
url: 37.html
id: 37
categories:
  - 编程收录
date: 2018-04-14 21:46:26
tags:
---

**git的使用** 在大佬的要求下，强行学会了使用Git，故记一下 之前参加比赛有使用过Git，所有电脑安装好了Git，不过之前不知道干了什么操作，右键并没有Git Bash,所以转向网上求助！知乎大法好！ 通过在注册表修改HKEY\_CLASSES\_ROOT\\Directory\\Background\\shell\ 添加open git ，OK了！ 那么接下来开始Git大法！参考连接 **1.Git 初始化** **Git 配置** $ git config --global user.name "John Doe" $ git config --global user.email johndoe@example.com **在相应目录右键Git Bash** git init ——创建一个.git子目录，包含Git的骨干文件 git add xx ——向Git仓库添加文件 git status ——查看当前状态 三种状态：untracked、modified、committed git commit -m "描述语句"——提交并添加描述语句 git add 可将文件由untracked、modified状态转至committed git clone 下载远程仓库的文件 **2.连接远程仓库** 首先生成自己的sshkey ssh-keygen -t rsa -C "用户名" 会让填写保存地址，若直接回车会保存在默认文件夹 用户/.ssh 接着两次回车 会生成两个文件，一个rsa，一个rsa.pub cat xxx.rsa.pub 复制内容，粘贴在github或coding的ssh设置 git remote add-name -url git remote -v 查看当前添加的remote主机 **3.新建分支** 两种方式： 远程新建分支，本地推送 git checkout -b feature-branch origin/feature-branch //检出远程的feature-branch分支到本地 本地创建新分支，推送 $ git checkout -b feature-branch //创建并切换到分支feature-branch $ git push origin feature-branch:feature-branch //推送本地的feature-branch(冒号前面的)分支到远程origin的feature-branch(冒号后面的)分支(没有会自动创建)
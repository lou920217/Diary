---
layout: post
title: Git 常用命令
category: interview
tags: [git]
---
1. 配置Git

    ```
    git config --global user.name 'yourname'
    git config --global user.email 'youremail@mail.com'
    ```
    该命令是你在系统上的用户全局用户配置，只要运行一次，那么你以后在该系统上做的任何事情，git会直接使用这些信息，如想对特定项目使用不同的用户信息，只需在那个项目下运行该命令，去掉 `--global` 选项即可。
2. 获取Git仓库

    ```
    git init
    ```
    在现有项目或目录下运行该命令，即可创建一个初始化的仓库，并在项目根目录下创建一个.git的子目录，这个子目录中包含初始化仓库的必须文件。
    ```
    git clone [url] [name]  // @url  一个已经存在的Git仓库  @name 可选
    ```
    运行该命令会直接从url所在的仓库中克隆一份几乎所有的项目文件跟数据，`git clone url`命令会在当前目录下创建一个与远程仓库命名一样的仓库目录，而`git clone url name`则会自定义本地仓库的名字，创建一个name命名的仓库文件。
3. 查看当前文件状态

    ```
    git status
    ```
4. 将变更的文件放到暂存区

    ```
    git add file/./-A/--all...  // @file 某个文件  ./-A/--all 所有变动的文件
    ```
5. 提交文件

    ```
    git commit -m info    // @info 提交信息
    ```
    运行该命令会将修改过并已经添加到暂存区的文件提交到本地的Git仓库。
6. 添加远程仓库

    ```
    git remote add name url  // @name 远程仓库的简写名称  @url 远程仓库的地址，可以是http或者ssh协议的地址 
    ```
    给本地项目添加一个远程仓库。
    
7. 查看远程仓库

    ```
    git remote -v
    ```
    返回远程仓库的简写和fetch，push地址
8. 修改远程仓库地址

    ```
    git remote set-url name newurl oldurl  // @name 远程仓库的简写名称  @newurl 远程仓库的旧地址  @oldurl  远程仓库的新地址 
    ```
9. 拉取远程仓库

    ```
    git pull name branch  //  @name 远程仓库的简写名称   @branch  远程仓库的需要拉取的分支
    ```
    `git pull` 命令会拉取数据并自动尝试合并到当前所在的分支
10. 推送到远程仓库

    ```
    git push remote-name branch-name  // @remote-name 远程仓库的简写名称  @branch-name 分支名称
    ```
11. 查看所有分支

    ```
    git branch
    ```
12. 新建分支

    ```
    git branch branch-name   //  @branch-name  分支名称
    ```
13. 切换分支

    ```
    git checkout branch-name   // @branch-name 分支名称
    ```
14. 查看所有提交

    ```
    git log
    ```
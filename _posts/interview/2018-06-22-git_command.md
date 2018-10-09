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
    git remote set-url name newurl oldurl 
    ```
    `@name` 远程仓库的简写名称  `@newurl` 远程仓库的旧地址  `@oldurl` 远程仓库的新地址
9. 拉取远程仓库

    ```
    git pull name branch  //  @name 远程仓库的简写名称   @branch  远程仓库的需要拉取的分支
    ```
    `git pull` 命令会拉取数据并自动尝试合并到当前所在的分支
10. 推送到远程仓库

    ```
    git push remote-name branch-name  // @remote-name 远程仓库的简写名称  @branch-name 分支名称
    ```
11. 查看当前分支

    ```
    git branch
    ```
12. 新建分支

    ```
    git branch branch-name   //  @branch-name  分支名称
    ```
13. 切换分支

    ```
    git checkout -b branch-name   // @branch-name 分支名称   `-b`参数表示创建并切换
    ```
15. 合并分支
    
    ```
    git merge branch-name   // @branch-name 分支名称  表示将branch-name合并到当前分支
    ```
16. 删除分支
    
    ```
    git branch -d branch-name // @branch-name 分支名称  
    ```
17. 版本回退
    
    ```
    git reset --soft/--hard/--mixed(default) hash/HEAD/HEAD^   
    ```
    `@hash` 表示版本号，是有SHA1计算出来的一个非常大的数字，用十六进制表示，定位版本号只需前六位即可;   

    `@HEAD` 表示当前版本;  

    `@HEAD^` 表示上一个版本，以此类推，往上100个版本就用`HEAD~100`;    

    `--soft/--hard/--mixed` 参数表示三个回复等级，`--soft` 仅仅是将HEAD指针改变，已经add的缓存以及工作区的所有东西都不变；`--mixed` 则是将HEAD指针改变，已经add到暂存区的缓存也会丢失掉，但是工作区的东西却不会改变；`--hard` 则一切都会恢复，所有东西都会恢复到指定的版本状态。
18. 查看命令历史记录
    
    ```
    git reflog  // 查看命令历史记录
    ```
19. 查看提交历史记录

    ```
    git log
    ```
20. 查看版本改动，文件对比
    
    ```
    git diff
    ```
    不加参数即默认比较工作区与暂存区
    ```
    git diff --cached/HEAD/commit-id [<path>]
    ```
    `--cached` 比较暂存区与最新本地版本库

    `HEAD` 比较工作区与最新本地版本库

    `commit-id` 比较工作区与指定commit-id
    ```
    git diff --cached [<commit-id>] [<path>]
    ```
    比较暂存区与指定commit-id
    ```
    git diff [<commit-id>] [<commit-id>] [<path>]
    ```
    比较两个commit-id
    ```
    git diff --cached/--HEAD/[<path>] > patch 
    ```
    patch的命名是随意的，不加其他参数时作用是当我们希望将我们本仓库工作区的修改拷贝一份到其他机器上使用，但是修改的文件比较多，拷贝量比较大，此时我们可以将修改的代码做成补丁，之后在其他机器上对应目录下使用 `git apply patch` 将补丁打上即可

    `--cached` 暂存区与版本库的差异做成补丁

    `--HEAD` 工作区与版本库的差异做成补丁

    `[<path>] `将单个文件做成一个单独的补丁

    `git apply patch` 应用补丁

    `git apply --check patch` 检测补丁是否可用，没有输出表示可用

    `git apply --reject patch` 根据一定的融合策略融合补丁，有冲突则生成冲突文件

**附录：** [Git文档传送门](https://git-scm.com/book/zh/v2)
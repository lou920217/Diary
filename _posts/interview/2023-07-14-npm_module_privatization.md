---
layout: post
title: 库包开发之后怎么管理？
category: interview
tags: [npm, git, module]
---
库包开发之后，应该怎么管理？  

1. 可以使用 `npm link`链接项目与库包

    当库包文件与项目在同一目录下时，进入项目根目录，执行 `npm link`，该命令会在项目的node_modules目录下创建一个module_name(库包名称)的超链接，链接到库包。生成的虚拟包名会根据库包里的package.json进行指定。

   当库包文件与项目不在同一目录下时，先进入库包根目录，执行 `npm link`，把库包链接到全局，然后再进入项目根目录，执行 `npm link module_name`，把全局的超链接链接到项目里。
    
   `npm unlink module_name`解除链接。
   
2. 通过git+仓库地址来获取库包
3. verdaccio 搭建私有的库包服务器
   verdaccio 是forked于sinopia@1.4.0并且100%向后兼容。

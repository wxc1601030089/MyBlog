---
title: 使用HEXO搭建个人博客
date: 2020-05-09 16:26:35
tags:
 - HEXO
categories: 搭建博客
---

搭建博客过程!

---
## 2020/5/9 16:27 Write

关键词：GitPage

```bash
下载安装git//下面的终端操作都使用Git Bush Here
npm install hexo-cli -g //安装hexo
hexo init *****.github.io (****为自己的github名)//初始化博客
cd *****.github.io //进入目录
hexo g//生成静态页面
hexo s//启动服务器，在localhost:4000上查看刚生成的博客页
ssh-keygen -t rsa -C "**********" (****为个人邮箱,注意引号)//配置SSH
根据路径找到id_rsa.pub，复制里面的内容
在Github项目当中Setting中找到SSH and GPG keys黏贴进去
ssh -T git@github.com (输入yes)//回到Git Bash中检查
需要在主文件夹中的_config.yml文件配置deploy // 配置属性的时候需要在冒号后空一格，不然会报错
deploy:
    type: git
    repo: **********//你的项目地址 类：https://github.com/yourname/yourname.github.io.git
    branch: master
npm install hexo-deployer-git --save //安装插件
hexo clean //清理public文件夹
hexo g//同上
hexo d//部署到github的项目中
许多坑，希望找到方法解决。
```
---
title: webstorm配置vue+element开发环境
date: 2022-09-01 14:47:53
tags: 
    - vue
    - 编辑器
categories: vue
keywords: '编辑器,vue'
aplayer: true
---
## 1. 设置启动环境
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/f9f9aeb1d85380bd72d4f31142efecb5.png)
新建npm,Name可以自定义一个名字，script选择启动项目的指令
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/95c08a2d7613926e0e99ac37d8b328b4.png)


## 2. 处理element-ui标签提示unknown的问题
File --> settings --> Languages & Frameworks --> JavaScript --> Libraries
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/f1c5f5afa22cbc702d810997366bbd78.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/a56e4fedd353b546f30ec2a8a3d52d0c.png)



## 3. 处理webstorm不识别@路径别名的问题
在 Languages & Frameworks --> Webpack中配置webpck.base.conf.js
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5c503886d9b688ed9bdef6caea25fb59.png)
## 4. 处理webstorm使用scss变量提示Element '--color-primary' is resolved only by name without use of explicit imports 的问题
File --> settings --> Editor --> Inspections --> Sass/SCSS --> 取消Missing import的勾选
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/68bc20bf158feec9b6531f9a4f0c0d8c.png)

---
title: 使用 vue-element-admin 开发一个后台管理——登录
date: 2018-03-15 10:37:23
tags: 
- vue
- admin
- element
categories: 
- 教程
---

在开发的阶段，要一直在服务器环境下进行开发，所以要先执行`npm run dev`启动本地虚拟服务，接着就可以愉快的敲代码了。

<!-- more -->

## 登录

在登录这一块使用到token验证，获取token及用户信息流程如下：

> 
（前端）进入登录页面 —— \src\views\login\login.vue
|
（前端）输入账号密码，发起登录请求 —— \src\views\login\login.vue
|
（后端）校验账号密码，返回token —— \src\mock\login.js
|
（前端）获取token，存入cookie，发起获取用户信息请求 —— \src\permissiom.js
|
（后端）校验token，是否有效或者过期，正常则返回用户权限等信息 —— \src\mock\login.js
|
（前端）获取用户信息，存入state，跳转首页 —— \src\permissiom.js
> 

以上流程只列出主要文件，各个文件所引用到的其他模块没有一一列出，详情请看源码

> 
特别说明：
本地开发时使用mock来进行数据模拟，所以后端的两次操作在开发阶段都在`\src\mock\login.js`中进行处理
token存储在浏览器cookie，页面刷新不清除
用户信息存储在应用state中，页面刷新会重新获取
> 

另外，在页面间跳转时路由都会进行拦截，判断是否已存在token及用户信息，如果没有则会跳转登录页面，具体处理在`\src\permissiom.js`文件中。

> 
登录页的相关文件目录如下：
视图 `\src\views\login\`
接口 `\src\api\login.js`
路由拦截 `\src\permissiom.js`
模拟数据 `\src\mock\login.js`
> 

## 系列文章

- [使用-vue-element-admin-开发一个后台管理——初始化](/2018/03/12/使用-vue-element-admin-开发一个后台管理——初始化/)
- [使用-vue-element-admin-开发一个后台管理——接口约定](/2018/03/15/使用-vue-element-admin-开发一个后台管理——接口约定/)
- [使用-vue-element-admin-开发一个后台管理——mock数据](/2018/03/15/使用-vue-element-admin-开发一个后台管理——mock数据/)
- [使用-vue-element-admin-开发一个后台管理——国际化](/2018/03/15/使用-vue-element-admin-开发一个后台管理——国际化/)
- [使用-vue-element-admin-开发一个后台管理——登录](/2018/03/15/使用-vue-element-admin-开发一个后台管理——登录/)
- [使用-vue-element-admin-开发一个后台管理——首页](/2018/03/15/使用-vue-element-admin-开发一个后台管理——首页/)
- [使用-vue-element-admin-开发一个后台管理——新建栏目](/2018/03/15/使用-vue-element-admin-开发一个后台管理——新建栏目/)
- [使用-vue-element-admin-开发一个后台管理——接口联调](/2018/03/15/使用-vue-element-admin-开发一个后台管理——接口联调/)
- [使用-vue-element-admin-开发一个后台管理——发布上线](/2018/03/15/使用-vue-element-admin-开发一个后台管理——发布上线/)

## 相关文档

- [vue](https://vuefe.cn/v2/guide/)
- [mockjs](http://mockjs.com/)
- [element-ui](http://element-cn.eleme.io/)
- [vue-element-admin](https://panjiachen.github.io/vue-element-admin-site/)
- [vue-el-admin-tpl](https://github.com/fredwei/vue-el-admin-tpl)
---
title: 使用 vue-element-admin 开发一个后台管理——接口联调
date: 2018-03-15 10:41:02
tags: 
- vue
- admin
- element
categories: 
- 教程
---

在本地开发环境也是可以与后台服务器接口进行调试的，需要解决的问题就是跨域问题。如果是线上的跨越问题，解决办法有很多，常用的有`CORS`，全称为 Cross Origin Resource Sharing（跨域资源共享），这种方案工作量基本都在后端那里，在此不多介绍。

<!-- more -->

## 接口联调

在联调之前，首选需要去掉`mock`数据，否则`mock`会一直拦截请求接口。去掉`mock`很简单，直接注释`\src\main.js`中的引用即可：

```javascript
// import './mock'
```

接着还需要修改一下开发环境的配置，在本项目中，打开`\config\index.js`文件，修改`dev`的`proxyTable`配置，如下：

```javascript
module.exports = {
  dev: {
    // Paths
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    // dev开发模式跨域处理
    proxyTable: {
      '/api': 'http://test.api.com'
    },
    ...
  }
}
```

这样子就可以完成简单的跨域处理，例如请求到`/api/users`现在会被代理到请求`http://test.api.com/api/users`。
但是在浏览器开发工具的network中并不会体现出来，可以通过查看返回值或者在浏览器直接访问接口地址如`http://localhost:9527/api/users`，检查代理是否设置正确。

修改完毕之后，需要关闭本地服务，然后再启动，否则配置文件不能生效。（ctrl+c无法关掉本地服务的，直接在进程里边关掉）

全部调整完毕，就可以跟和后端愉快的调试了，本地测试通过，那么接下来就应该上传到服务器，在外网环境测试了。

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
---
title: 使用 vue-element-admin 开发一个后台管理——国际化
date: 2018-03-15 10:33:29
tags: 
- vue
- admin
- element
categories: 
- 教程
---

[vue-element-admin](http://panjiachen.github.io/vue-element-admin)的默认语言是英文，在这里我已经把默认语言改成中文了，需要修改的地方比较分散，在这里不展开来讲。

<!-- more -->

## 国际化

国际化方案有很多，在该项目中是以js文件作为配置文件，并且在全局使用；语言包在`\src\lang\`文件中，默认使用中文，即调用`\src\lang\zh.js`，语言文件如下所示：

```javascript
export default {
  route: {
    home: '首页',
    news: '新闻',
    ...
  },
  navbar: {
    logOut: '退出登录',
    ...
  },
  login: {
    title: '系统登录',
    ...
  }
  ...
}
```

在使用后台管理系统过程中，可以随时切换，增加新的字段时，记得在`两个语言包`中同时添加，以免进行语言切换后找不到对应的文字。

具体使用方法如下：

在视图文件的`template`中使用时，直接调用`$t('键名')`，例如:

```html
<template>
  <h3 class="title">{{$t('login.title')}}</h3>
</template>
```

可以在视图文件的`script`中调用，例如:

```html
<script>
  const _titleText = this.$t('login.title')
  console.log('login.title对应语言包中的文字是：' + _titleText)
</script>
```

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
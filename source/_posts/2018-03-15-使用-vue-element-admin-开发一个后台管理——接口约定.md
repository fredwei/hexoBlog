---
title: 使用 vue-element-admin 开发一个后台管理——接口约定
date: 2018-03-15 10:27:01
tags: 
- vue
- admin
- element
categories: 
- 教程
---

约定接口往往是项目开始之前要做的事情，当然实际是很多时候都是边写边确定，只有数据格式约定好，那么后续的开发就可以做到两翼齐飞了~

<!-- more -->

## 接口约定

前后端接口要约定好请求及返回格式，以便双方可以并行开发，提高开发效率。

请求及返回的拦截都在`\src\utils\request.js`中进行处理

请求时，每个请求头都带上token

```javascript
// \src\utils\request.js

// 请求拦截器
service.interceptors.request.use(config => {
  // 请求拦截操作
  if (store.getters.token) {
    // 让每个请求携带token-- ['X-Token']为自定义key 请根据实际情况自行修改
    config.headers['X-Token'] = getToken()
  }
  return config
}, error => {
  // 请求拦截报错
  console.log(error)
  Promise.reject(error)
})
```

请求的数据格式为json格式，如：

```javascript
getSomeDate({
  id: '123123',
  status: 1
}).then(response => {
  // 请求成功，处理成功结果
}).catch(() => {
  // 请求失败，处理失败结果
})
```

返回的格式也是json格式，如下：

```javascript
{
  status: 200,
  message: '数据获取成功',
  data: {
    total: 100,
    items: [1,2,..., 100]
  }
}
```

`status`是状态码，`vue-el-admin-tpl`中约定200是正常返回，也可以与后台约定好，在`\src\utils\request.js`修改状态码的封装处理即可

```javascript
// \src\utils\request.js

// 50008:非法的token
// 50012:其他客户端登录了
// 50014:Token 过期了
if (res.status === 50008 || res.status === 50012 || res.status === 50014) {
  this.$msgbox.confirm('你已被登出，可以取消继续留在该页面，或者重新登录', '确定登出', {
    confirmButtonText: '重新登录',
    cancelButtonText: '取消',
    type: 'warning'
  }).then(() => {
    store.dispatch('FedLogOut').then(() => {
      // 为了重新实例化vue-router对象 避免bug
      location.reload()
    })
  })
}
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
---
title: 使用 vue-element-admin 开发一个后台管理——mock数据
date: 2018-03-15 10:28:26
tags: 
- vue
- admin
- element
categories: 
- 教程
---

自从有了前后端分离的概念，前端模拟数据这一块就出了很多方案，mock无疑是一个很好的选择。

<!-- more -->

## mock数据

接口约定好了之后，前端可以使用mock来模拟后端接口返回的数据，做到前后分离并行开发。

假设登录页面需要一个登录接口`/login/login`，接口的处理以及生成mock数据流程如下：

(1) 在`\src\api\`目录新建一个`login.js`文件，用于处理所有登录相关的接口，文件示例如下：
```javascript
// 引入request组件，处理所有ajax请求
import request from '@/utils/request'

// 设置一个方法名为loginByUsername，传入的参数是账号及密码
export function loginByUsername(query) {
  // 获取token
  return request({
    url: '/login/login',
    method: 'post',
    params: query
  })
}
```

(2) 在`\src\mock\`目录新建一个`login.js`文件，用于模拟所有登录相关的数据，文件示例如下：
```javascript
// 如果需要使用Mock生成随机结果，则需要引入Mock模块，文档见 http://mockjs.com/
// import Mock from 'mockjs'
// 引入param2Obj模块用于处理传入的参数
import { param2Obj } from '@/utils'

export default {
  // 登录请求处理
  loginByUsername: config => {
    const { username } = param2Obj(config.url)

    if (username) {
      return {
        status: 200,
        message: '登录成功',
        data: {
          roles: ['admin'],
          token: 'admin_token_12345',
          introduction: '我是超级管理员',
          avatar: 'https://wpimg.wallstcn.com/f778738c-e4f8-4870-b634-56703b4acafe.gif',
          name: 'Super Admin'
        }
      }
    } else {
      return {
        status: 2002,
        message: '登录失败',
        data: null
      }
    }
  }
}
```

(3) 在`\src\mock\`目录建好`login.js`文件，还需要在`\src\mock\index.js`中将该文件注册，否则无法访问，文件示例如下：
```javascript
// 引入Mock模块
import Mock from 'mockjs'
// 引入login文件
import loginAPI from './login'


// 模拟接口延迟响应
// Mock.setup({
//   timeout: '350-600'
// })

// 登录相关（接口地址与模拟数据的方法名称一一对应）
Mock.mock(/\/login\/login/, 'post', loginAPI.loginByUsername)
Mock.mock(/\/login\/logout/, 'post', loginAPI.logout)
Mock.mock(/\/user\/info\.*/, 'get', loginAPI.getUserInfo)
// 等等...

```

(4) 写好接口文件及mock数据，那么接下来就可以在视图文件中使用了，示例如下：

```html
<template>
...
</template>

<script>
// 引入该页面需要调用的接口方法名称
import { loginByUsername } from '@/api/login'

export default {
  name: 'login',
  components: {
    ...
  },
  data() {
    return {
      ...
    }
  },
  methods: {
    // 登录
    handleLogin() {
      loginByUsername({
        username: 'admin',
        password: '12345678'
      }).then(response => {
      console.log('处理登录请求成功返回结果')
    }).catch((error) => {
      console.log('处理登录请求失败返回结果')
    })
    }
    ...
  }
}
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
---
title: 使用 vue-element-admin 开发一个后台管理——发布上线
date: 2018-03-15 10:41:41
tags: 
- vue
- admin
- element
categories: 
- 教程
---

本地开发完成，接着就需要上传到外网环境，让测试的童鞋来做测试了。

<!-- more -->

## 发布上线

发布上线之前，首先是要打包构建，生成最终代码。然后在打包构建之前，还需要对配置文件进行修改。

打包构建需要修改的配置文件主要在`\config\`目录中，`dev.env.js`、`sit.env.js`和`prop.env.js`分别对应开发、测试、生产环境，文件的配置项基本一致，如下：

```javascript
module.exports = {
	NODE_ENV: '"production"',
	ENV_CONFIG: '"sit"',
	BASE_API: '"https://api-sit"'
}
```

这里都是全局变量，可以在开发过程中直接引用，如在`\src\utils\request.js`请求封装的文件中：

```javascript
// create an axios instance
const service = axios.create({
  baseURL: process.env.BASE_API, // api的base_url
  timeout: 5000 // request timeout
})
```

通过`process.env.BASE_API`直接调用`\config\`中配置文件里的`BASE_API`。

如果需要修改构建打包输出的配置，可以打开`\config\index.js`文件，找到`build`配置项：

```javascript
module.exports = {
  dev: {
    ...
  },

  build: {
    // 资源构建打包输出的入口文件
    index: path.resolve(__dirname, '../dist/index.php'),
    // 资源构建打包输出的本地路径
    assetsRoot: path.resolve(__dirname, '../dist'),

    // 这是将静态资源打包到指定的文件夹下
    assetsSubDirectory: 'static',
    // 这是静态资源的路径
    assetsPublicPath: './',

    ...
  }
}
```

主要是修改`assetsSubDirectory`和`assetsPublicPath`选项，如果你的资源服务器和web服务器不在同一台机器，那么正好修改这里，例如：

```javascript
module.exports = {
  dev: {
    ...
  },

  build: {
    // 资源构建打包输出的入口文件
    index: path.resolve(__dirname, '../dist/index.php'),
    // 资源构建打包输出的本地路径
    assetsRoot: path.resolve(__dirname, '../dist'),

    // 这是将静态资源打包到指定的文件夹下
    assetsSubDirectory: 'static',
    // 这是静态资源的路径
    assetsPublicPath: '//static.demo.com/',

    ...
  }
}
```

配置文件修改完毕之后，在命令行输入：

```bash
npm run build:sit
```

以上构建命令是打包到测试环境，等待命令窗口显示打包构建完成，在`\dist\index.php`文件中会发现，所有静态资源的链接都是`//static.demo.com/static/`开头。

假如web服务器是php环境，地址是`http://www.demo.com/`，那么把`\dist\index.php`这个入口文件上传到web服务器的根目录；
再把`\dist\static\`整个目录上传到`http://static.demo.com/`的根目录。

完成以上操作，就可以通过地址`http://www.demo.com/`访问到你发布完成的网站了。

> 
补充：
除了修改`\config\`目录中的配置，还可以按需修改`\build\`目录中的配置，`webpack.dev.conf.js`和`webpack.prop.conf.js`，其中测试和生产环境共用`webpack.prop.conf.js`的配置。
> 

## 结语

教程中还有许多不足之处，有一些地方没有讲述清楚，有任何问题或者建议，欢迎联系我进行友好的沟通交流~

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
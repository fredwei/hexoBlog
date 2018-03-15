---
title: 使用 vue-element-admin 开发一个后台管理——初始化
date: 2018-03-12 11:41:12
tags: 
- vue
- admin
- element
categories: 
- 教程
---

在学习和使用`vue-element-admin`过程中遇到蛮多的问题的，主要是模板跟业务相关性有所出入，熟悉及调整的时候花费了不少时间。基于`vue-element-admin`进行二次开发，产出`vue-el-admin-tpl`。

这一系列的教程只针对有一些`vue`开发基础的童鞋，篇幅有限，教程有很多细节的不会展开来说。

<!-- more -->

## 初始化

### 1、下载到本地

```bash
git clone git@github.com:fredwei/vue-el-admin-tpl.git
```

### 2、进入目录

```bash
cd vue-el-admin-tpl
```

### 3、安装项目需要的依赖

```bash
npm install

// 推荐使用国内淘宝镜像
npm install --registry=https://registry.npm.taobao.org
```

### 4、开发及发布

```bash
# 构建本地环境
npm run dev

# 构建测试环境
npm run build:sit

# 构建生产环境
npm run build:prod
```

还有一个补充点就是，项目使用了eslint作为代码规范约束，但是开发过程中有时候一些空格或者分号等小问题很多，一个个手动修改实在太麻烦，那么有一个命令可以帮你解决额这个问题，在命令窗口下输入：

```bash
# 进行代码规范校验并修复
npm run lint --fix
```

如果有一些重要的错误无法自动修复，那么就需要手动修改了，错误提示会详细的指出是哪个文件，哪一行，是什么规则的校验没有通过，还有链接可以点开前往查看该规则的详细介绍，示例如下：

```bash
  http://eslint.org/docs/rules/semi  Extra semicolon
  src\main.js:29:32
    Vue.filter(key, filters[key]);
                                  ^


  1 problem (1 error, 0 warnings)


Errors:
  1  http://eslint.org/docs/rules/semi

You may use special comments to disable some warnings.
Use // eslint-disable-next-line to ignore the next line.
Use /* eslint-disable */ to ignore all warnings in a file.

```

表示在`\src\main.js`这个文件的第29行第32个字符，多了一个分号，校验的规则地址是`http://eslint.org/docs/rules/semi`。

以上只是简单的讲述了一下从下载模板到打包构建完成的一个流程，下边会细讲开发及打包发布的事项。

> 注意事项：
> 
> - 在最后联调的时候修改`\config`目录下对应的配置文件，对应各个环境的接口，记得去掉mock模拟数据（在`\src\main.js`中，去掉`import './mock'`）
> - 新增、删除、修改文件时，应用会自动构建并刷新浏览器，如无法正常访问，请在命令窗口输入`ctrl+c`停止本地服务器，然后输入`npm run dev`重新启动
> - 发现这个项目在windows下`ctrl+c`不能完全停止本地虚拟服务器，不知道是不是个例；如果遇到同样的情况，直接在任务管理器，结束进程`node.exe`。（不能停止表现为，`npm run dev`启动服务时，端口与配置文件中不一致，进程中会有多个node进程）
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
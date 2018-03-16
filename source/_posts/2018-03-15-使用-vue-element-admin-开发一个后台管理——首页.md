---
title: 使用 vue-element-admin 开发一个后台管理——首页
date: 2018-03-15 10:39:03
tags: 
- vue
- admin
- element
categories: 
- 教程
---

首页只保留了几个统计数据跟一个线形图表，这一块可以参考[vue-element-admin](http://panjiachen.github.io/vue-element-admin)，根据业务需要自行定义页面元素。

<!-- more -->

## 首页

首页的视图目录有一个`components`目录，凭着就近原则，每个栏目独立的组件都应该放到当前栏目下，以便进行管理。对组件的应用如下：

```html
<template>
  <div class="dashboard-editor-container">
    <!-- 调用PanelGroup组件 -->
    <panel-group :totalData="totalData" :activeType="activeType" @handleSetLineChartData="handleSetLineChartData"></panel-group>

    <el-row style="background:#fff;padding:16px 16px 0;margin-bottom:32px;">
      <!-- 调用LineChart组件 -->
      <line-chart :chart-data="lineChartData"></line-chart>
    </el-row>
  </div>
</template>

<script>
// 引用PanelGroup组件
import PanelGroup from './components/PanelGroup'
// 引用LineChart组件
import LineChart from './components/LineChart'

export default {
  name: 'home',
  // 将引入的组件进行注册，才可以在template中应用
  components: {
    PanelGroup,
    LineChart
  },
  data() {
    return {
      activeType: '',
      totalData: [],
      lineChartData: {
        expectedData: [],
        actualData: []
      }
    }
  }
  ...
}
</script>
```

对组件的应用时，建议将子组件需要操作的数据及对该数据处理放在父组件来进行。

> 
首页的相关文件目录如下：
视图 `\src\views\home\`
接口 `\src\api\home.js`
模拟数据 `\src\mock\statistics.js`
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
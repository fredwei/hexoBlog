---
title: 使用 vue-element-admin 开发一个后台管理——新建栏目
date: 2018-03-15 10:40:00
tags: 
- vue
- admin
- element
categories: 
- 教程
---

`vue-el-admin-tpl`中默认一个栏目中有三个页面，分别是列表、新增、编辑，其中新增跟编辑使用同一个模版。如有其他页面，可以自行添加。

<!-- more -->

## 新建栏目

以下是从无到有创建一个栏目的过程

### 新建页面文件

(1) 在`\src\views\`目录下新建一个`product`目录，并新建文件`index.vue`、`add.vue`、`edit.vue`三个页面
(2) 在`\src\router\index.js`文件中加上product栏目列表页的路由信息，如下：
```javascript
export const asyncRouterMap = [
  // 产品
  {
    path: '/product',
    component: Layout,
    redirect: '/product/index',
    meta: {
      title: 'product',
      icon: 'component'
    },
    children: [{
      // 首页
      path: 'index',
      component: _import('product/index'),
      name: 'product',
      hidden: true,
      meta: {
        title: 'product',
        icon: 'component'
      }
    }, {
      // 新增
      path: 'add',
      component: _import('product/add'),
      name: 'productAdd',
      hidden: true,
      meta: {
        title: 'productAdd',
        icon: 'form',
        roles: ['admin', 'editor']
      }
    }, {
      // 编辑
      path: 'edit/:id',
      component: _import('product/edit'),
      name: 'productEdit',
      hidden: true,
      meta: {
        title: 'productEdit',
        icon: 'form',
        roles: ['admin']
      }
    }]
  },
  // 404页面拦截
  { path: '*', redirect: '/404', hidden: true }
]
```

> 
因为后台的页面（除了登录）都使用同一个页面框架，封装在`\src\views\layout\Layout.vue`，所以栏目路由的`component`设置为`Layout`，并且重定向到`/product/index`。
不想在左侧菜单显示列表、新增跟编辑才到，所以`hidden`设置为`true`，左侧菜单这块有做过特别的处理
> 

路由写好之后，要记得在语言包中写上对应的文字，打开`\src\lang\zh.js`和`\src\lang\en.js`，根据在路由表中设置的`title`值，一一对应在语言包文件中的`route`对象中加对应字段的文字，如下：

```javascript
// zh.js
export default {
  route: {
    home: '首页',
    news: '新闻',
    product: '产品',
    productAdd: '添加产品',
    productEdit: '编辑产品',
    ...
   }
}

// en.js
export default {
  route: {
    home: 'Home',
    news: 'News',
    product: 'Product',
    productAdd: 'Product Add',
    productEdit: 'Product Edit',
    ...
  }
}
```

以上处理好了之后，就可以通过`/product/index`、`/product/add`和`/product/edit/xxx`访问对应的页面了

### 列表页

列表页主要包含三块区域：列表操作区、列表展示区、列表页码区，页面结构如下：

```html
<!-- \src\views\product\index.vue -->

<template>
  <div class="app-container calendar-list-container">
    <!-- 头部操作区 -->
    <div class="filter-container">
    ...
    </div>

    <!-- 表格 -->
    <el-table>
    ...
    </el-table>

    <!-- 页码 -->
    <div class="pagination-container">
    ...
    </div>
  </div>
</template>
```

具体的内容不详细列举，可以参考源码，页面中的ui组件都可以在[element-ui](http://element-cn.eleme.io/)和[vue-element-admin](https://panjiachen.github.io/vue-element-admin-site/)中找到。

页面操作及数据处理部分如下：
```html
<script>
// 数据接口
import { fetchList, fetchPv, updateArticle } from '@/api/article'
// 按钮动画特效 - 水波纹指令
import waves from '@/directive/waves'
// 工具接口
import { parseTime } from '@/utils'

export default {
  name: 'product',
  directives: {
    waves
  },
  data() {
    return {
      // 表格的key，改变后表格会重新渲染
      tableKey: 0,
      // 列表数据集
      list: null,
      ...
    }
  },
  filters: {
    // 一些过滤器方法
    ...
  },
  created() {
    this.getList()
  },
  methods: {
    // 获取列表数据
    getList() {
      ...
    }
    ...
  }
}
</script>
```

页面中使用到的数据接口，可以参考mock数据的处理来完成。

### 新增及编辑页

新增及编辑页面使用同样的页面模板，所以需要封装起来，在`\src\views\product\`目录下新建一个`components`组件目录，新建`ArticleDetail.vue`作为共同模板

新增及编辑页面结构如下：

```html
<!-- \src\views\product\add.vue -->
<template>
  <article-detail :is-edit='false'></article-detail>
</template>

<script>
import ArticleDetail from './components/ArticleDetail'

export default {
  name: 'productAdd',
  components: { ArticleDetail }
}
</script>

<!-- \src\views\product\edit.vue -->
<template>
  <article-detail :is-edit='true' :detailId="this.$route.params.id"></article-detail>
</template>

<script>
import ArticleDetail from './components/ArticleDetail'

export default {
  name: 'productEdit',
  components: { ArticleDetail }
}
</script>
```

在调用`ArticleDetail`组件的时候传入两个值，一个是`is-edit`，一个是`detailId`，新增页`detailId`为空。

在`ArticleDetail`组件中判断`is-edit`是否为`true`并且`detailId`的值不会空时，会根据`detailId`的值获取详细信息，在表单中显示。

#### 表单校验

表单校验是每个表单都需要处理的部分，[element-ui](http://element-cn.eleme.io/)对这一块做了集成处理，使用方式如下：

(1) 先定义校验规则
```javascript
export default {
  name: 'productDetail',
  components: {
    ...
  },
  props: {
    ...
  },
  data() {
    // 字段校验 - 必填
    const validateRequire = (rule, value, callback) => {
      if (!value.length) {
        callback(new Error(rule.field + '为必填项'))
      } else {
        callback()
      }
    }
    // 字段校验 - URL
    const validateSourceUri = (rule, value, callback) => {
      if (value) {
        if (validateURL(value)) {
          callback()
        } else {
          callback(new Error(rule.field + '链接填写不正确'))
        }
      } else {
        callback(new Error(rule.field + '为必填项'))
      }
    }
    return {
      ...
      // 校验规则 - 注意：被校验的字段必须加上prop属性
      rules: {
        image_uri: [{ validator: validateRequire }],
        pic_uri: [{ validator: validateRequire }],
        title: [
          { validator: validateRequire, trigger: 'blur' },
          { min: 2, max: 100, message: '长度在 2 到 100 个字符', trigger: 'blur' }
        ],
        content_short: [
          { validator: validateRequire },
          { min: 5, max: 200, message: '长度在 5 到 100 个字符', trigger: 'blur' }
        ],
        content: [{ validator: validateRequire }],
        source_uri: [{ validator: validateSourceUri, trigger: 'blur' }]
      }
      ...
    }
  },
  ...
}
```

校验规则`rules`在`data`中去设定，有默认约定的校验格式（[element-ui 表单校验](http://element-cn.eleme.io/#/zh-CN/component/form#biao-dan-yan-zheng)），也可以自定义函数来进行处理，如上所示。

(2) 应用表单校验

```html
<template>
  <div class="createPost-container">
    <el-form class="form-container" :model="postForm" :rules="rules" ref="postForm" label-width="65px">
      <el-form-item style="margin-bottom: 40px;" label="标题:" prop="title">
          <el-input placeholder="请输入标题，不超过100个字符" v-model="postForm.title">
          </el-input>

          <span v-show="postForm.title.length>=26" class='title-prompt'>标题长度超过26个字符时提示</span>
        </el-form-item>
      </el-form>
  </div>
</template>

<script>
// ... 省略各种引用

export default {
  name: 'productDetail',
  components: {
    ...
  },
  props: {
    ...
  },
  data() {
      ...
    }
  },
  methods: {
  	// 提交数据-发布上线
    submitForm() {
      console.log(this.postForm)
      // 表单校验
      this.$refs.postForm.validate(valid => {
        if (valid) {
          console.log('表单校验通过!')
        } else {
          console.log('表单校验不通过!')
          return false
        }
      })
    },
    ...
  },
  ...
}
<script>
```

> 
注意：
在`el-form`组件中，设定校验规则`:rules="rules"`
在`el-form-item`组件中，设定校验规则`prop="title"`，否则在执行表单校验时，不会对该字段进行校验
> 

#### 表单常见字段

表单中经常出现的几种字段类型：

(1) 文本输入框
```html
<template>
  <el-form-item style="margin-bottom: 40px;" label="标题:" prop="title">
    <el-input placeholder="请输入标题，不超过100个字符" v-model="postForm.title"></el-input>

    <span v-show="postForm.title.length>=26" class='title-prompt'>标题长度超过26个字符时提示</span>
  </el-form-item>
</template>
```

(2) 单选多选
```html
<template>
<el-radio-group style="padding: 10px;" v-model="postForm.comment_disabled">
  <el-radio :label="true">关闭评论</el-radio>
  <el-radio :label="false">打开评论</el-radio>
</el-radio-group>

<el-checkbox-group v-model="postForm.platforms" style="padding: 5px 15px;">
  <el-checkbox label="多选1">多选1</el-checkbox>
  <el-checkbox label="多选2">多选2</el-checkbox>
</el-checkbox-group>
</template>
```

(3) 时间选择
```html
<template>
  <el-date-picker v-model="postForm.display_time" type="datetime" format="yyyy-MM-dd HH:mm:ss" placeholder="选择日期时间"></el-date-picker>
</template>
```

> 
注意：
设置时间选择器默认值时，默认值必须转成Date类型，否则时间选择器会报错，导致无法正常使用！
> 


(4) 搜索选择
```html
<template>
  <el-form-item label="作者:" class="postInfo-container-item" prop="author">
    <multiselect v-model="postForm.author" :options="userLIstOptions" @search-change="getRemoteUserList" placeholder="搜索用户" selectLabel="选择"
      deselectLabel="删除" track-by="key" :internalSearch="false" label="key">
      <span slot='noResult'>无结果</span>
    </multiselect>
  </el-form-item>
</template>
```

> 
注意：
`vue-element-admin`在这里使用的是`vue-multiselect`一个外部的多选组件，可以参考文档[vue-multiselect](https://www.npmjs.com/package/vue-multiselect)
搜索选择功能需要定义一个方法，用来处理输入框改变时发起请求，实时获取根据关键词检索出来的结果。
> 

```html
<template>
  <el-form-item label="作者:" class="postInfo-container-item" prop="author">
    <el-select
      v-model="postForm.author"
      multiple
      filterable
      remote
      reserve-keyword
      placeholder="请输入关键词"
      :remote-method="getRemoteUserList"
      :loading="loading">
      <el-option
        v-for="item in userLIstOptions"
        :key="item.value"
        :label="item.label"
        :value="item.value">
      </el-option>
    </el-select>
  </el-form-item>
</template>
```

> 
注意：
这里使用的是`element-ui`提供的集成方案，可以参考文档[element-ui 选择组件](http://element-cn.eleme.io/#/zh-CN/component/select)
> 


(5) 图片上传
```html
<template>
  <el-form-item style="margin-bottom: 40px;" label="封面图:" prop="image_uri">
    <Upload :fileList="postForm.image_uri"></Upload>
  </el-form-item>
  
  <el-form-item style="margin-bottom: 40px;" label="产品图:" prop="pic_uri">
    <Upload :fileList="postForm.pic_uri"></Upload>
  </el-form-item>
</template>
```

> 
注意：
这是我基于`element-ui`的文件上传再次封装的，只需传入一个图片数组即可，默认为空数组，数组格式如下：
```javascript
{
  ...
  // 文章封面图
  image_uri: [{
  	name: 'food.jpeg',
  	url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100'
  }],
  // 文章图片
  pic_uri: [{
  	name: 'food.jpeg',
    url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100'
  }, {
  	name: 'food2.jpeg',
    url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100'
  }],
  ...
}
```
> 其余的默认值可以查看`\src\components\Upload\singleImage4.vue`，选择性的传入
> 

(6) 多行文本
```html
<template>
<el-form-item style="margin-bottom: 40px;" label="摘要:" prop="content_short">
  <el-input
    type="textarea"
    :autosize="{ minRows: 2, maxRows: 4}"
    placeholder="请输入摘要，不超过200字"
    v-model="postForm.content_short">
  </el-input>
  <span class="word-counter" v-show="contentShortLength">{{contentShortLength}}字</span>
</el-form-item>
</template>
```

(7) 富文本编辑器
```html
<template>
<el-form-item style="margin-bottom: 40px;" label="内容:" prop="content">
  <div class="editor-container">
    <tinymce :height=500 ref="editor" v-model="postForm.content"></tinymce>
  </div>
</el-form-item>
</template>
```

表单中其余用到的字段类型，可以参考文档[element-ui](http://element-cn.eleme.io/)

把提交的接口写好，那么这个栏目就算是开发完成了，在本地调试没问题后，就可以与后端的接口进行一下联调了。

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
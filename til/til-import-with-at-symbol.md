# TIL:`import`声明中的@符号（译）

原文地址：[The @ symbol in JavaScript import statement](https://www.jerriepelser.com/blog/til-at-symbol-javascript-import/)

作者：Jerrie Pelser
  
---

TIL: 今天我学习了JavaScript`import`声明中@符号的用法

本人最近正在忙于一个新的项目，前端使用Vue.js开发。项目使用[Vue Webpack template](https://github.com/vuejs-templates/webpack)初始化，同时我注意到好几处`import`声明使用了@符号。例如在`router/index.js`文件中：

```js
import Vue from 'vue'
import Router from 'vue-router'
import Home from '@/components/Home'
import AuthCallback from '@/components/AuthCallback'
import Map from '@/components/Map'
```

不太确定@具体是啥意思，于是Google了一番，发现了StackOverflow上的[这个回答](https://stackoverflow.com/questions/42749973/es6-import-using-at-sign-in-path-in-a-vue-js-project-using-webpack)。原来是webpack`build/webpack.base.conf.js`中进行了如下的配置：

```
resolve: {
  extensions: ['.js', '.vue', '.json'],
  alias: {
    'vue$': 'vue/dist/vue.esm.js',
    '@': resolve('src'),
  }
},
```

换句话说，这是webpack配置的一部分，使用`resolve.alias`来设置一些路径的别名。而遇到别名符号时，Webpack会调用`resolve`方法来获得最终路径：

```js 
function resolve (dir) {
  return path.join(__dirname, '..', dir)
}
```

所以，对于上面`router/index.js`中`import Home from '@/components/Home'`的导入声明，模块路径会被解析为`build/../src/components/Home`。这是一种十分便捷的避免使用相对路径（而在绝对路径中使用别名）的方式。

（以下略）
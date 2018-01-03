# JavaScript的模块

原文地址：[https://qiqihaobenben.github.io/Front-End-Basics/JavaScript/utility/module](https://qiqihaobenben.github.io/Front-End-Basics/JavaScript/utility/module)

作者：qiqihaobenben

---

## TOC

* **介绍**
  * 模块的优点
* **CommonJS**
  * Node.js
    * 加载方式
    * 加载缓存
    * 模块载入过程
    * require函数
    * 模块循环依赖
* **AMD**
  * RequireJS
* **CMD**
  * SeaJS
* **UMD**
* **ES6 module**
  * 严格模式
  * 模块 module
  * 导出 export
  * 导入 import
  * 默认导出 export default
  * export与import的复合写法
  * ES6中的循环引用
  * 实例

---

## CommonJS

* CommonJS 规范的主要适用场景是**服务器端编程**，所以采用**同步加载**模块的策略。如果我们依赖3个模块，代码会一个一个依次加载它们
* 该模块实现方案主要包含 **require** 与 **module** 这两个关键字，其允许某个模块对外暴露部分接口并且由其他模块导入使用
* CommonJS 需要一个兼容的脚本加载器作为前提条件，该脚本加载器必须支持名为 **require** 和 **module.exports** 的函数，它们将模块相互导入导出

```js
// gogogo.js
function Gogogo() {
    this.hello = function() {
        console.log('hello');
    };

    this.goodbye = function() {
        console.log('goodbye');
    };
}

module.exports = Gogogo;

// main.js 引入 gogogo.js
var Go = require('./gogogo.js');
var g = new Go();
g.hello(); // hello
```

## Node.js

* Node.js基于CommonJS实现了模块化，其形式（由于在服务端的流行而不正确地）被称为 CommonJS
* **核心模块（原生模块）**
  * 官方提供的标准API中提供的模块，如 fs、http、net等
  * 被预先编译成了二进制代码，可直接通过require获取，如 `require('fs')` 
  * 具有最高的加载优先级
* **文件模块**
  * 存储为单独的文件/文件夹的模块，可以是JS、JSON或编译好的C/C++代码
  * 不显式指定扩展名时，Node.js会试图依次加上.js、.json、.node（编译好的C/C++代码）
    * `.js` - 通过fs模块同步读取js文件并编译执行
    * `.json` - 读取文件，调用JSON.parse解析加载
    * `.node` - 通过C/C++进行编写的Addon，通过dlopen方法进行加载
* "./"、"../" 为按路径加载，否则，若非核心模块，则会在node\_modules中查找并加载
  * require的文件查找策略：文件模块缓存 --&gt; 原生模块（先查找原生模块缓存） --&gt; 文件模块

          ![](/assets/image1.jpg)

* Node.js通过实际文件名缓存所有加载过的文件模块，避免重复加载
















































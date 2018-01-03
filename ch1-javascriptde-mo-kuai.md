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
var Go = require('./Gogogo.js');
var g = new Go();
g.hello(); // hello
```














































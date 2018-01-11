# JavaScript的模块

原文地址：[https://qiqihaobenben.github.io/Front-End-Basics/JavaScript/utility/module](https://qiqihaobenben.github.io/Front-End-Basics/JavaScript/utility/module)

作者：qiqihaobenben

#### TOC

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
var say = new Go();
say.hello(); // hello
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
* 模块发生循环依赖时，我们只能从exports对象中得到发生在循环依赖之前的部分

## AMD

* Asynchronous Module Definition（异步模块定义）
* 诞生于CommonJS的讨论中，服务于浏览器的模块加载场景，采用异步加载/回调的方式
* AMD和CommonJS一样需要**脚本加载器**的支持（主要是对define方法的支持）
* define既是引用模块的方式，也是定义模块的方式
* define方法的参数有三：
  * Module ID 模块名称
  * Dependency Array 依赖数组
  * Factory Function 所有依赖都可用之后执行的工厂函数 （必须）

```js
// lib/gogogo.js (独立模块，无依赖，直接定义)
define(function() {
    return {
        hello: function () {
            console.log('hello');
        }
    };
});

// main.js 引入 gogogo.js (非独立模块，对其他模块有依赖)
define(['./lib/gogogo'], function(say) {
    say.hello(); // hello
})
```

## RequireJS

* 前端模块化管理工具，遵循AMD规范
* 要求每个模块均放在独立的文件之中
* `require` 语法糖：AMD加载器会将require函数解析为Function.prototype.toString\(\)，然后对define的参数做一些转换

```js
define(function(require) {
    var d1 = require('dep1');
    var d2 = require('dep2');

    return function() {};
});

// internally
define(['require', 'dep1', 'dep2'], function(require) {
    var d1 = require('dep1');
    var d2 = require('dep2');

    return function() {};
});
```

## CMD

* Common Module Definition（通用模块定义），更贴近CommonJS和node.js的模块化标准
* CMD也使用define关键字定义模块，与AMD相比区别在于：
  * 二者模块加载虽均为异步，但AMD中会立即执行加载完毕的模块，而CMD则是按照代码中引用模块的顺序执行的（就近依赖，即用即加载，as lazy as possible）
  * 相较于AMD中的require语法糖，CMD中的require是在真正的加载并执行代码
* CMD中factory可以为函数，也可以为对象或字符串

## SeaJS

* 遵循CMD规范
* 有以下三种载入模块的方式
  * seajs.use - 主要用于载入入口模块，类似AMD模块加载模式
  * require - 正常加载
  * require.async - 用到时再加载（懒加载）

## UMD

* Universal Module Definition（统一模块定义），一种试图将AMD和CommonJS结合的尝试
* 常见做法是将CommonJS语法包裹在兼容AMD的代码中
* 其核心思想在于对IIFE（Imediately Invoked Function Expression，立即执行的函数表达式）的应用

## ES6

* 一个模块，就是一个对其他模块暴露自己的属性或者方法的文件
* 设计思想是尽量静态化，从而在编译时就能确定模块的依赖关系，以及输入和输出的变量
* **编译时加载**，而非运行时加载
  * 与CommonJS不同，ES6模块不是对象
  * 通过export命令显式的制定输出的代码，输入时也采用静态命令的形式
  * 在编译时即可完成模块加载，效率更高，但无法引用模块本身
  * 使静态分析成为可能，可以进一步拓宽JS的语法，引入宏和类型检验等依赖静态分析实现的功能
* **导出 export**
  * export命令
* **默认导出 default export**
* **导入 import**


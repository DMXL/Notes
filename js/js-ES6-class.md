# ES6 Class

曾经的JavaScript教科书上都会这样写道：

> Javascript是一种基于对象（object-based）的语言，所有东西几乎都是对象。然而它与C++或Java这种传统的面向对象（OOP）语言不同，因为它的语法中没有类（class）的概念。
> 
> ——《JavaScript面向对象编程指南》

ES6标准引入了类（class）的概念，然并卵。现在ES6的教科书里都会这样写道：

> 基本上，ES6的类（class）可以看作只是一个**语法糖**，它的绝大部分功能，ES5都可以做到，新的`class`写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。
>
> ——《ES6标准入门》

有种鸡肋的感觉。。我们来自己研究一番。

## Class基本语法

ES5定义一个叫形状（Shape）的类：

```js
// 定义Shape构造函数
function Shape(id, x, y) {
    this.id = id;
    this.setPos(x, y);
}
// setPos和getPos公共方法
Shape.prototype.setPos(x, y) {
    this.x = x;
    this.y = y;
}
Shape.prototype.getPos() {
    return {
        x: this.x,
        y: thix.y
    };
}
```

生成一个Shape类的实例：

```js
var x = new Shape('x', 0, 1);
x.getPos(); // {x:0,y:1}
```

按ES6的语法来写一遍：

```js
// 定义Shape类
class Shape {
    // 构造函数
    construnctor(id, x, y) {
        this.id = id;
        this.setPos(x, y);
    }
    // 类的公共方法
    setPos(x, y) {
        this.x = x;
        this.y = y;
    }
    getPos() {
        return {
            x: this.x,
            y: this.y
        };
    }
}
// 生成实例
const y = new Shape('y', -1, 0);
y.getPos(); // {x:-1,y:0}
```

看起来好像差不多。。我们用[Babel](http://babeljs.io/repl)翻译一下ES6的代码：

```js
"use strict";

var _createClass = (function() {
  function defineProperties(target, props) {
    for (var i = 0; i < props.length; i++) {
      var descriptor = props[i];
      descriptor.enumerable = descriptor.enumerable || false;
      descriptor.configurable = true;
      if ("value" in descriptor) descriptor.writable = true;
      Object.defineProperty(target, descriptor.key, descriptor);
    }
  }
  return function(Constructor, protoProps, staticProps) {
    if (protoProps) defineProperties(Constructor.prototype, protoProps);
    if (staticProps) defineProperties(Constructor, staticProps);
    return Constructor;
  };
})();

function _classCallCheck(instance, Constructor) {
  if (!(instance instanceof Constructor)) {
    throw new TypeError("Cannot call a class as a function");
  }
}

var Shape = (function() {
  function Shape() {
    _classCallCheck(this, Shape);
  }

  _createClass(Shape, [
    {
      key: "construnctor",
      value: function construnctor(id, x, y) {
        this.id = id;
        this.setPos(x, y);
      }
    },
    {
      key: "setPos",
      value: function setPos(x, y) {
        this.x = x;
        this.y = y;
      }
    },
    {
      key: "getPos",
      value: function getPos() {
        return {
          x: this.x,
          y: this.y
        };
      }
    }
  ]);

  return Shape;
})();

var y = new Shape("y", -1, 0);
y.getPos();
```


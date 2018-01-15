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
// Shape构造函数
function Shape(id, x, y) {
    this.id = id;
    this.setPos(x, y);
}
// setPos和getPos公共方法
Shape.prototype.setPos(x, y) {
    this.x = x;
    this.y = y;
}
```
# ES6 函数参数的默认值

## 由一个问题引发的思考

```js
(function(a, f = () => a) {
  var a;
  var b = a;
  a = 2;
  return [a, b, f()];
})(1)
```
这段代码执行结果是：

- A [ 2, 1, 1 ]
- B [ 2, undefined, 1 ]
- C [ 2, 1, 2 ]
- D [ 2, undefined, 2 ]

---

### 答案

```js
(function(a, f = () => a) {
  var a;
  var b = a;
  a = 2;
  return [a, b, f()];
})(1)
// 正确答案是：
// A [ 2, 1, 1 ]
```

### 把问题延伸一下

```js
(function(a, b, f = () => [a, b]) {
  var a;
  var c = a;
  a = 2;
  b = 2;
  return [a, b, c, f()];
})(1, 1) // [2, 2, 1, [1, 2]]
```
- 为什么c为1而不是undefined？
- 为什么f执行时取到的a和b值不同？

## 知识点

- 参数默认值 (ES6)
- 作用域

### ES5实现参数默认值

```js
function print(x, y) {
  y = y || 'World';
  console.log(x, y);
}

print('Hello') // Hello World
print('Hello', '') // Hello World
```

### 更严格一点

```js
function print(x, y) {
  if (typeof y === 'undefined') {
    y = 'World';
  }
  console.log(x, y);
}

print('Hello') // Hello World
print('Hello', '') // Hello
print('Hello', undefined) // Hello World
```

### ES6写法更简洁自然

```js
function print(x, y = 'World') {
  console.log(x, y);
}

print('Hello') // Hello World
print('Hello', '') // Hello
print('Hello', undefined) // Hello World
print('Hello', null) // Hello `null`
```

---

### 惰性求值

参数默认值不是传值的，而是每次都重新计算默认值表达式的值

```js
function foo(p = x + 1) {
  let x = 98;
  console.log(p);
}
let x = 99;
foo() // 100

x = 100;
foo() // 101
```

### 参数作用域

未设置默认值时，参数与函数主体在同一级作用域

![scope-bubbles](/assets/es6-functions/scope-bubbles-1.jpg)


一旦设置了参数的默认值，函数进行声明初始化时，参数会形成一个单独的作用域

![scope-bubbles](/assets/es6-functions/scope-bubbles-2.jpg)

### ECMA-262

9.2.12 Function Declaration Instantiation

![scope-bubbles](/assets/es6-functions/ecma262-1.jpg)

### 软一峰的栗子

```js
let a = 1;
function foo(b = a) {
  let a = 2;
  console.log(b);
}
foo() // 1

function bar(c = d) {
  let d = 2;
  console.log(c);
}
bar() // ReferenceError: d is not defined
```

## 回到题目中

```js
(function(a, f = () => a) {
  var a = 2; // 又声明了一个内部变量a
             // 与参数a不在同一个作用域
             // 不是同一个变量
  return f();
})(1) // 1
```

### 对于第二种情况

```js
(function(b, f = () => b) {
  b = 2; // 内部变量b指向第一个参数
         // 与匿名函数内部的b是一致的
  return f();
})(1) // 2
```

### 迎刃而解！~

ʕ•̫͡•ʔ-̫͡-ʕ•͓͡•ʔ-̫͡-ʔ

### 还剩下一个问题

```js
function foo(a) {
  var a; // 参数变量a已默认声明，此时相当于重复声明
  var b = a;
  return b;
}
foo(1) // 1
```

#### 使用`let`或`const`声明

```js
function foo(a) {
  let a;
  let b = a;
  return b;
}
foo(1) // SyntaxError: Identifier 'a' has already been declared
```

#### 对于参数有默认值的情况

9.2.12 Function Declaration Instantiation

![scope-bubbles](/assets/es6-functions/ecma262-2.jpg)

### 汇报一个bug...

![babel](/assets/es6-functions/babel-1.jpg)

#### 结果不同

```js
(function(x, y = () => x) {
  var x = 2;
  console.log(y());
})(1) // 1

// Babel转译后
(function(x) {
  var y = arguments.length > 1 && arguments[1] !== undefined
      ? arguments[1]
      : function() {
          return x;
        };
  var x = 2;
  console.log(y());
})(1) // 2，跟转译前结果不同
```

## 回顾一下

```js
(function(a, b, f = () => [a, b]) {
  var a;
  var c = a;
  a = 2;
  b = 2;
  return [a, b, c, f()];
})(1, 1) // [2, 2, 1, [1, 2]]

// 1. 为什么c为1而不是undefined？
// 2. 为什么f执行时取到的a和b值不同？
```
1. 因为a在函数内部作用域已默认声明且具有初始值
2. 因为参数及默认值的定义存在于独立作用域
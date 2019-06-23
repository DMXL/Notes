## 组件

### 使用组件

> 组件自定义标签的命名规则不强制遵循W3C规则（小写，并且包含一个短横），尽管这被认为是**最佳实践**。

#### 全局注册

```js
// 注册
Vue.component('my-component', {
  template: '<div>A custom component!</div>'
})

// 创建根实例
new Vue({
  el: '#example'
})
```

#### 局部注册

```js
var Child = {
  template: '<div>A custom component!</div>'
}

new Vue({
  // ...
  components: {
    // <my-component> 将只在父组件模板中可用
    'my-component': Child
  }
})
```

`data`属性**必须是函数**，因为这样可以最大限度保证组件内部状态的独立，实现组件之间的解耦。

在Vue中，父子组件的关系可以总结为**prop向下传递，事件向上传递**。父组件通过**prop**给子组件下发数据，子组件通过**事件**给父组件发送消息。

![Components](/Notes/assets/images/vue-comps/props-events.png)

### Prop

HTML特性是不区分大小写的。所以对于自定义标签的prop属性名称，camelCase（驼峰式命名）的prop需要转换为相对应的**kebab-case（短横线分隔式命名）**。

可使用`v-bind:{prop-name}`动态地将prop绑定到父组件的数据上。而使用不带任何参数的`v-bind`可将一个对象的所有属性作为prop传递给子组件。

**注意**：`v-bind`的值会被当做JavaScript表达式计算，而非字面量。

#### Prop是单向绑定的

当子组件想要使用或处理prop传入的值时（尤其当prop为对象或数组时），最好使用以下两种做法：

A. 定义一个内部变量，用prop的值初始化它

```js
props: ['initialCounter'],
data: function () {
  return { counter: this.initialCounter }
}
```

B. 定义一个计算属性来处理prop的值

```js
props: ['size'],
computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}
```

> 注意：在JavaScript中对象和数组是**引用类型**，指向同一个内存空间，如果prop是一个对象或数组，在子组件内部改变它会影响父组件的状态。

### 非Prop特性



### 自定义事件
### 使用插槽分发内容
### 动态组件
### 杂项

## 注册组件
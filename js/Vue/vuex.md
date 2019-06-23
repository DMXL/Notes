# 状态管理

## 导语

### **单向数据流**的痛点：

1. 多个view依赖于同一state

> 传参的方法对于多层嵌套的组件将会非常繁琐，并且对于兄弟组件间的状态传递无能为力

2. 来自不同view的action需要变更同一state

> 经常会采用父子组件直接引用或者通过事件来变更和同步状态的多份拷贝

以上模式非常脆弱，通常会导致无法维护的代码。

### Vuex的基本思想

1. 把组件的共享状态抽取出来，以一个全局单例模式管理
2. 通过定义和隔离状态管理中的各种概念并强制遵守一定的规则，使代码更健壮
3. 借鉴了Flux、Redux和The Elm Architecture的基本思想

![Vuex](/Notes/assets/images/vue-vuex/vuex.png)

### Store

1. **store**就是一个容器，包含应用中大部分state
2. 当组件从store中读取state的时候，state发生变化，相应的组件也会相应地得到高效更新
3. 不能直接改变store中的state。唯一改变的途径是commit一个mutation，可由此跟踪每一个状态的变化（devtool）

###
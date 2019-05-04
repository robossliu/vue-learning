## Vue.js 是什么？

Vue.js是一套用于构建用户界面的**渐进式框架**。

## 安裝
- CDN 直接导入
```html
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```
或者
```html
<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```
- NPM
```shell
# 最新稳定版
$ npm install vue
```
- CLI - 命令行工具

## 声明式渲染
Vue.js的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进DOM的系统：
```html
<div id='app'>
    {{ message }}
</div>
```
```js
var app = new Vue({
    el: '#app',
    data: {
        message: 'Hello Vue!'
    }
});
```
我们已经成功创建了第一个Vue应用！看起来这跟渲染一个字符模板非常相似，但是Vue在背后做了大量工作。现在数据和DOM已经被建立了关联，所有东西都是**响应式的**。我们要怎么确认呢？打开你的浏览器的JavaScript控制台（就在这个页面打开），并修改*app.message*的值，你将看到上例相应地更新。

除了文本插值，我们还可以像这样来绑定元素特性：
```html
<div id="app-2">
  <span v-bind:title="message">
    鼠标悬停几秒钟查看此处动态绑定的提示信息！
  </span>
</div>
```
```js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: '页面加载于 ' + new Date().toLocaleString()
  }
})
```

> 鼠标悬停几秒钟查看此处动态绑定的提示信息！

这里我们遇到了一点新东西。你看到的v-bind特性被称为指令。指令带有前缀v-，以表示是Vue提供的特殊特性。可能你已经猜到了，它们会在渲染的DOM上应用特殊的响应式行为。在这里，该指令的意思是：“将这个元素节点的title特性和Vue实例的message属性保持一致”。

*注意：JavaScript代码中的el为element的前两个字母，第二为l，而非数字1。有时容易将l错写成1，从而导致始终无法得到预期的输出。*

## 条件与循环
控制切换一个元素是否显示也相当简单：
```html
<div id="app-3">
  <p v-if="seen">现在你看到我了</p>
</div>
```
```js
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```
继续在控制台输入 app3.seen = false，你会发现之前显示的消息消失了。

这个例子演示了我们不仅可以把数据绑定到 DOM 文本或特性，还可以绑定到 DOM 结构。此外，Vue 也提供一个强大的过渡效果系统，可以在 Vue 插入/更新/移除元素时自动应用**过渡效果**。

还有其它很多指令，每个都有特殊的功能。例如，v-for 指令可以绑定数组的数据来渲染一个项目列表：
```html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```

```js
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: '学习 JavaScript' },
      { text: '学习 Vue' },
      { text: '整个牛项目' }
    ]
  }
})
```
在控制台里，输入 app4.todos.push({ text: '新项目' })，你会发现列表最后添加了一个新项目。
## 处理用户输入
为了让用户和你的应用进行交互，我们可以用v-on指令添加一个事件监听器，通过它调用在Vue实例中定义的方法：
```html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">逆转消息</button>
</div>
```

```js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```
注意在 reverseMessage 方法中，我们更新了应用的状态，但没有触碰 DOM——所有的 DOM 操作都由 Vue 来处理，你编写的代码只需要关注逻辑层面即可。

Vue还提供了v-model指令，它能轻松实现表单输入和应用状态之间的双向绑定。
```hmtl
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```

```js
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
```
## 组件化应用构建
组件系统是Vue的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。仔细想想，几乎任意类型的应用界面都可以抽象为一个组件树：

> 组件树图

在Vue里，一个组件本质上是一个拥有定义选项的一个Vue实例。在Vue中注册组件很简单：
```js
// 定义名为 todo-item的新组件
Vue.component('todo-item', {
    template: '<li>这是个待办项</li>'
})
```
*Vue.component用于全局组件定义与注册，如果需要进行局部Vue组件注册，需采用如下方式*
```js
var appName = new Vue({
    el: 'el-name',
    components: {
        "todo-item": {
            template: '<li>这是个待办项</li>'
        }
    }
});
```
*如果组件名采用驼峰命名规则，在使用组件时，需将组件名进行去驼峰规则转化，即在大写字母前增加一个-，同时将大写字母转化为小写字母。例如，如果定义的组件名为todoItem，则在使用时需转化为todo-item。*

现在你可以用它构建另一个组件模板：
```html
<ol>
  <!-- 创建一个 todo-item 组件的实例 -->
  <todo-item></todo-item>
</ol>
```
但是这样会为每个待办项渲染同样的文本，这看起来并不炫酷。我们应该能从父作用域将数据传到子组件才对。让我们来修改一下组件的定义，使之能够接受一个 prop：
```js
Vue.component('todo-item', {
    // todo-item組件現在接受一個
    // “prop”，類似於一個自定義特性。
    // 這個prop名為todo。
    props: ['todo'],
    template: '<li>{{ todo.text }}</li>'
});
```
## 与自定义元素的关系
你可能已经注意到Vue组件非常类似于**自定义元素**——它是Web组件规范的一部分，这是因为Vue组件语法部分参考了该规范。例如Vue组件实现了Slot API与is特性。但是，还是有几个关键差别：
1. Web Components 规范已经完成并通过，但未被所有浏览器原生实现。目前 Safari 10.1+、Chrome 54+ 和 Firefox 63+ 原生支持 Web Components。相比之下，Vue 组件不需要任何 polyfill，并且在所有支持的浏览器 (IE9 及更高版本) 之下表现一致。必要时，Vue 组件也可以包装于原生自定义元素之内。
2. Vue 组件提供了纯自定义元素所不具备的一些重要功能，最突出的是跨组件数据流、自定义事件通信以及构建工具集成。

虽然 Vue 内部没有使用自定义元素，不过在应用使用自定义元素、或以自定义元素形式发布时，依然有很好的互操作性。Vue CLI 也支持将 Vue 组件构建成为原生的自定义元素。

# 单页面应用程序

- Single Page Application (SPA)
- 一个网站就一个页面
  + 极致的用户体验，就像 native app 一样

- 优点：
  + 分离前后端关注点，前端负责界面显示，后端负责数据存储和计算

- 缺点：
  + 不利于 SEO，搜索引擎优化

## 简单模拟单页面应用程序的效果

-  `hash`
-  `window.onhashchange` 事件
- 当 hash 改变时，根据不同的 hash 做不同的处理

```html
<div class="aside">
    <ul>
        <!-- 
为了和普通的锚点作区分，所以这里的路径加了一个前缀
这个标识看起来就和我们的服务端中的路由路径很像
注意：思想是一样的，就是标识
/ -->
        <li><a href="#/">发现音乐</a></li>
        <li><a href="#/my-music">我的音乐</a></li>
        <li><a href="#/friend">朋友</a></li>
    </ul>
</div>
<script>
    // 这里我们通过注册 window.onhashchange 事件来监听 hahs（锚点）的改变
    // 我们的 url 地址发生改变了
    // 改变之后我们就解析 hash 中的路径标识
    // 然后根据不同的路径标识渲染不同的页面到单页面中的容器中
    window.onhashchange = function () {
        var hash = window.location.hash.substr(1)// #/ 
        if (hash === '/') {
            $.get('./find-music.html', function (data) {
                $('#container').html(data)
            })
        } else if (hash === '/my-music') {
            $.get('./my-music.html', function (data) {
                $('#container').html(data)
            })
        } 
    }
</script>
```

# MVVM

架构模式

核心思想：数据驱动视图

这种模式最大的好处是解耦，数据和视图不再是强耦合在一起

好处：

- 低耦合
- 可重用性
- 独立开发
- 可测试性

字面意义：

- M：Model 业务数据模型，和视图没有关系
- V：View 视图
- VM： ViewModel 视图数据模型，视图数据

数据绑定原理（VM原理）

- 设计模式：事件观察者（发布/订阅模式）

```javascript
function EventEmit() {
    this.callbacks = {};
}
EventEmit.prototype.on = function(eventName, eventFun) {
    if (!this.callbacks[eventName]) {
        this.callbacks[eventName] = [];
    }
    this.callbacks[eventName].push(eventFun);
};

EventEmit.prototype.emit = function(eventName) {
    if (!this.callbacks[eventName]) {
        return;
    }
    this.callbacks[eventName].forEach(fn => {
        fn();
    });
};
```

- Object.defineProperty() 高级属性定义方式

```javascript
// 允许精确添加或修改对象的属性，默认情况下，使用 Object.defineProperty() 添加的属性值是不可修改的。
var o = {b:''}; // 创建一个新对象
// 参数1：该对象 参数2：属性（要更改|添加属性） 参数3：方法 
Object.defineProperty(o, "b", {
    get : function(){// 当访问该属性时，该方法会被执行，方法执行时没有参数传入
        return bValue;
    },
    set : function(newValue){// 当属性值修改时，触发执行该方法。该方法将接受唯一参数，即该属性新的参数值。
        bValue = newValue;
    }
});
```

- 所有关心 message 改变的 DOM 都订阅了 message 事件
- 当 message 改变时，就发布 message 事件

# Vue

基础

## 安装：

```shell
npm i vue
```

Vue 实例:

- el
  + 表明被 Vue 管理的模板入口，网页中的 DOM 节点
  + 不能使用 body、html
  + 必须是一个普通的 HTML 标签节点，一般是 div

- data

  + 作用：**数据驱动图** 中的 **数据**

  + 不是什么数据都往里面初始化的

  + 只需要在 data 中初始化一个数据成员，然后在模板中绑定使用这个成员

  + 核心就是把 DOM 操作的思想转变到 **数据驱动视图** 的思想

- methods

  + 作用：一般是用来定义事件处理函数

  + 虽然我们可以把方法写到 data 中，但是在 Vue 中更推荐把所有方法都写到 methods 属性中
  + 不允许具有和 data 中重名的成员

- computed

  + 称作  计算属性，在模板中绑定多次，当相关的数据发生变化，只会执行一次，缓存计算结果

  + 从代码上看是一个方法，但是只能当作属性来使用

计算属性与方法的区别（返回结果用于模板绑定）：

- 都可以达到相同的效果
- 如果是方法，则一旦方法所在的视图发生变化，则方法一定会重复执行
- 方法没有缓存，也就是是说多次使用该方法，则会重复执行
- 计算属性是整正依赖与内部  data 中的数据，如果数据没变，则计算属性不会重复执行
- 所以相比来说，计算属性要比方法更为高效


```html
<div id="app">{{ message }}</div>
<div id="app">{{ say() }}</div> <!--{{ say() }}中也可使用 methods 中的方法，获得的是调用的方法的返回值，data中的成员发生改变，方法就会自动执行一次-->
<div id="app">{{ num }}</div><!--不能加括号-->
<script src="node_modules/vue/dist/vue.js"></script>
<script>
    const app = new Vue({
        // 规定不能作用到 body 和 html 节点上
        el: '#app',
        data: {
            message: 'Hello Vue.js!'
        },
        methods:{
            say(){ 
                return 'hello'
            }
        },
        computed:{
            num(){
                return 1+2
            }
        }
    })
</script>
```

简单总结：

 jQuery 提高了 DOM 操作的效率
 Vue 极大的解放了 DOM 操作（Vue 全部把 DOM 操作都给你屏蔽了）
 Vue 的核心思想就是：数据驱动视图（MVVM）

还有一点就是 Vue 其实是一个更高级的模板引擎

- 在 Vue 中，通过模板绑定的数据都是响应式的
  + 数据如果一旦变化，则绑定该数据的视图元素也会变化

## 数据绑定渲染

- 文本绑定：`{{ data 中的数据成员名 }}`

- 属性绑定：`v-bind:属性名称="JavaScript 表达式"`
  + 简写： `:属性名称="表达式"`
  + v-bind:class="{类名: 布尔值}"
    * 当布尔值为 true 时，则作用这个类名
    * 当布尔值为 false 时，则去除这个类名

- 数据单向绑定

  + 数据变，视图变

  + 视图变，数据不会变

使用 JavaScript 表达式：

```html
{{ number +1 }}
{{ ok ? 'yes' : 'no' }}
{{ message.split('').reverse().join('') }}
<div v-bind:id="'list-' + id"></div>
```

## 模板语法

## 计算属性和观察者

- 计算属性用于需要在模板中绑定输出值
- 而 watch 观察者则用于需要根据数据的改变，从而定制特殊功能业务
  + watch 只是监视，不返回数据，而且只能观察自己的 data 成员
  + 不是用于模板绑定的

## Class 与 Style

## 条件渲染

-  `v-if` 
-  `v-else` 
  +  `v-else` 元素必须紧跟在带 `v-if` 或 `v-else-if` 的元素之后
-  `v-if` 和 `<template>` 
- `v-else-if`
  + 类似与 `v-else` ，`v-else-if` 也必须紧跟在带 `v-if` 或 `v-else-if` 的元素之后
-  `v-show` 
  + `v-show` 只是简单的切换元素的 CSS 属性 `display`
  +  `v-show` 不支持 `<template>` 元素，也不支持 `v-else`

-  `v-if` 和 `v-show` 的区别
  +  `v-if` 是“真正”的条件渲染，条件为真则渲染，条件为假则移除 DOM 元素
  +  `v-show` 不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换
  +  `v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销
  + 因此，如果需要非常频繁的切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好

## 列表循环渲染

-  `v-for="item in items"`
-  `v-for="(item,index) in items"`
-  `v-for="item of items"`
   + 可以用 `of` 替代 `in` 作为分隔符，因为他是最接近 JavaScript 迭代器的语法
-  对象迭代
   +  `v-for="value in object"`
   +  `v-for="(value,key) in object"
   +  `v-for="(value,key,index) in object`
-  数组更新检测
-  对象更新检测

```html
<ul>
    <li v-for="item in fruits">{{ item }}</li>
</ul>
<script>
    const app = new Vue({
        el: '#app',
        data: {
            fruits:['apple','orange','banana']
        }
    })
</script>
```

## 事件处理

- 定义事件处理函数

```html
<!-- 如果函数语句简单可以直接内联写 -->
<!-- <button v-on:click="number++">点击+1</button> -->
<!-- 如果函数语句比较多，建议把处理写到 JavaScript 中 -->
<div id="app">
    <div>{{ number }}</div>
    <button v-on:click="handleIncrement">点击+1</button>
</div>
<script src="./node_modules/vue/dist/vue.js"></script>
<script>
    const app = new Vue({
        el: '#app',
        data: {
            number: 1
        },
        // 注意：方法写到 data 中可以，但是函数内部的 this 是 Window
        // 所以我们都建议把方法写到 methods 选项中
        methods: {
            handleIncrement: function () {
                // 在通过 v-on 注册的方法中我们可以直接通过 this 来访问 data 中的数据成员 this === app 这个对象
                this.number++
            }
        }
    })
</script>
```

- 绑定事件处理函数
  +  `v-on:事件名称="事件 处理函数"`
  + 简写： `@事件名称="事件 处理函数`

- 访问原声 DOM 事件，可以使用特殊变量 $event 把它传入方法

```html
<button @click="clickHandler('hello',$event)">Submit</button>
```

```javascript
// ...
method:{
    clickHandler (message,event){
        ...
    }
}
```


## 表单输入绑定

## **表单** 数据双向绑定

- `v-model=" data 中的数据成员名"`

  > v-model 是 Vue 提供的一个特殊的属性，在 Vue 中被称之为指令
  > 它的作用就是：双向绑定表单控件
  > 什么是叫双向数据绑定？
  > 当数据发生改变， DOM 会自动更新
  > 当表单控件的值发生改变，数据也会自动得到更新

- 双向数据绑定

  + 数据变化 <==> 视图变化  相互影响

- 文本

- 多行文本

- 复选框

- 单选按钮

- 选择列表

## 组件

### 组件概念

### 使用组件

> 在模板使用时基于驼峰命名法，转为小写，用 `-` 拼接
> component 时，可以使用驼峰命名法

#### 全局组件（通用组件）

> 我们一般把网页中特殊的反而公共部分注册为全局组件：轮播图、tab选项卡、分页、通用导航

创建组件构造器（直接注册，可省略）

```javascript
let Profile = Vue.extend({
    // 模板选项
    template:`<div><p>hello</p></div>`
})
```

注册

```javascript
// 全局 参数1：组件名称 参数2：构造器名
Vue.component('my-component',Profile)
// 简写方式 将 Profile 替换成 { template:`...`}
Vue.component('my-component',{
    template:`<div><p>hello</p></div>`
})
```

使用：挂载作用域下实例化

```html
<div id="app">
    <my-component></my-component>
</div>
```

#### 局部组件（子组件）

> 局部组件注册一般是注册一些非通用，只适用于当前项目的

注册：

```javascript
var Child = {
    template:`<div><p>hello</p></div>`
}
new Vue({
    // ...
    components:{
        // <my-component> 将只在父组件模板中可用
        'my-component':Child,
    }
})
```

使用：

```html
<div id="app">
    <my-component></my-component>
</div>
```

#### 组件与组件之间是相互独立的

- 默认情况下，组件与组件之间无法进行跨组件数据访问，父子组件也不行
- 组件就是一种特殊的 Vue 实例，不需实例化，它管理自己的 `template` 
- 在组件中，也可以配置自己的类似于 Vue 实例的一些选项资源：data、methods、computed
- 思想：组件管理自己，不影响别人

#### 组件的 `data` 选项必须是函数

- 组件的 data 必须是函数（手动 new 出来的 Vue 实例才是普通对象）
- 函数内部返回一个对象

```javascript
Vue.component('my-div', {
    template: '#my-template',
    data:function(){
        return {counter:0}
    }
})
// 原因：由于自定义组件可以使用多次，data返回的函数则说明函数内部变量是局部变量（组件之间不会相互影响）
```

##### template标签

```html
<template id="my-template">
    <!-- template 标签只能返回一个根元素，要想输出多个，需要用一个大标签包裹-->
    <div>
        <div>hello</div>
        <p>world</p>
    </div>
</template>

<div id="app">
    <my-div></my-div>
</div>

<script>
    Vue.component('my-div', {
        template: '#my-template'
    })
    new Vue({
        el: '#app'
    })
</script>
```

##### script 标签

```html
<!--用法与template标签一样-->
<script type="text/template" id="my-template">
    <div>
        <div>hello</div>
        <p>world</p>
    </div>
</script>
```

#### 使用组件注意事项

> 组件可以理解为特殊的 Vue 实例，管理自己的 `template` 模板

- 组件的 `template` 必须有且只有一个根节点
- 组件与组件之间是相互独立的，可以有自己的 data、methods、computed。。。
- 组件的 `data` 必须是函数

### 父子组件

```javascript
// 子组件构造器
let Child1 = Vue.extend({
    template:`<div><p>i am child1</p></div>`
})
let Child2 = Vue.extend({
    template:`<div><p>i am child2</p></div>`
})
// 父组件构造器
Vue.component('parent',{
    components:{
        'child1':Child1,
        'child2':Child2,
    },
    template:`<div><child1></child1><child2></child2></div>`
    // 在模板中不能使用 <child1></child1> ，它只在父组件构造器中有用，要在外部使用需注册
})
```


### 父子组件间的通信：props down

- 父组件要给子组件传递数据，子组件需要将它内部发生的事件告知给父组件
- (props down,events up ) 父组件通过 `props` 向下传递数据给子组件，子组件通过 events 给父组件发送消息

父组件给子组件传递数据通过 props 来传递：

1. 先在父组件中往子组件标签上添加动态属性（v-bind）来传递数据
2. 然后在子组件中声明 props 来接收父组件传递的数据
3. 然后在子组件中就可以直接通过 this 来访问 props 中的数据了

```html
<div id="app">
    <my-parent :imgtitle="title"></my-parent>
</div>

<template id="my_parent">
    <div>
        <!--多层传递需用 v-bind 绑定-->
        <child1 :title="imgtitle"></child1>
    </div>
</template>

<script>
    Vue.component('my-parent', {
        template: '#my_parent',
        props: ['imgtitle'],
        // 注册子组件
        components: {
            'child1': {
                template: '<h2>{{title}}</h2>',
                // 使用 props 获取父组件传递的数据
                props: ['title']
            },
        }
    })

    new Vue({
        el: '#app',
        data: {
            title: 'hello',
        },
    })
</script>
```

#### 单向数据流

props 是单向绑定的，父组件的属性变化时，将传导给子组件，但是反过来不会。这是为了防止子组件无意间修改父组件的状态，来避免应用的数据流变得难以理解

另外，每次父组件更新时，子组件的所以 props 都会更新为最新值

如果只想拿到一次父组件给的数据，而不想受父组件影响

```javascript
props: ['initialCounter'],
    data: function () {
        return {
            counter: this.initialCounter
        }
    }
```

#### Prop 验证

建议，声明 props 都使用验证的方式

```javascript
Vue.component('my-component', {
    props: {
        // 带有默认值的数字
        propD: {
            type: Number,
            default: 100
        },
    }
}
```

### 父子组件间的通信：Events Up

1. 子组件在内部通过 `this.$emit(自定义事件名称[,可选参数])` 发布事件
2. 父组件在使用子组件的标签上通过 `@自定义事件名称="事件处理函数"` 的方式来订阅子组件内部发布的事件
3. 这样的话，当子组件内部 `this.$emit` 时，则父组件订阅的自定义事件就会执行

```html
<div id="app">
    <num @counter="allCount"></num>
    <num @counter="allCount"></num>
    <num @counter="allCount"></num>
    <p>一共点击了{{total}}</p>
</div>

<script>
    Vue.component('num', {
        template: '<button @click="counter">{{ count }}</button>',
        data: function () {
            return {
                count: 0,
            }
        },
        methods: {
            counter() {
                this.count += 1
                return this.$emit('counter')
                // 通过给父组件传递 已触发 counter自定义事件 来传递数据
            }
        }
    })

    new Vue({
        el: '#app',
        data: {
            total: 0,
        },
        methods: {
            allCount() {
                this.total += 1
            }
        }
    })
</script>
```

### 使用插槽分发组件内容

### 动态组件

### 组件其他

## 进入/离开 & 列表过渡

组件的切换使用

- component 标签的 is 属性来完成组件动态绑定
- 按钮的@click来修改 data 属性从而切换组件
- keep-alive 标签来保留组件的状态值

```html
<div id="app">
    <button @click="changeComp(1)">第一项</button>
    <button @click="changeComp(2)">第二项</button>
    <button @click="changeComp(3)">第三项</button>
    <!--要想保持状态 使用keep-alive标签包裹-->
    <keep-alive>
        <component :is="nowHeader"></component>
    </keep-alive>
</div>
<script>
    var Vue = new Vue({
        el:'#app',
        data:{
            nowHeader:'header-1'
        }
        components:{
        'header-1':{template:`<div>组件1</div>`},
        'header-2':{template:`<div>组件2</div>`},
        'header-3':{template:`<div>组件3</div>`}               
    },
        methods:{
            changeComp(index){
                this.nowHeader='header-'+index
            }
        }
    })
</script>
```

## Vue指令

### 系统内置指令

- [v-text](https://cn.vuejs.org/v2/api/#v-text) 
  + 和 `{{}}` 一样，唯一的区别是 `{{}}` 会闪烁，`v-text` 不会有闪烁
  + 如果想用 `{{}}` 又不想闪烁，可以使用 `v-cloak`
- [v-html](https://cn.vuejs.org/v2/api/#v-html) 
  + 更新元素的 innerHTMl，即 可插入带html标签的字符串
- [v-show](https://cn.vuejs.org/v2/api/#v-show) 
  + 通过控制 `display: none|block;` ，条件显示和隐藏
  + 如果需要频繁的切换显示和隐藏，则使用 `v-show`
- [v-if](https://cn.vuejs.org/v2/api/#v-if) 
  + 真正的条件渲染
  + 条件为真，则渲染；条件为假，则移除不渲染
  + 如果只是一次显示和隐藏，则建议 `v-if`
- [v-else](https://cn.vuejs.org/v2/api/#v-else)
- [v-else-if](https://cn.vuejs.org/v2/api/#v-else-if)
- [v-for](https://cn.vuejs.org/v2/api/#v-for)
- [v-on](https://cn.vuejs.org/v2/api/#v-on)
- [v-bind](https://cn.vuejs.org/v2/api/#v-bind)
- [v-model](https://cn.vuejs.org/v2/api/#v-model)
- [v-slot](https://cn.vuejs.org/v2/api/#v-slot)
- [v-pre](https://cn.vuejs.org/v2/api/#v-pre) 
  + Vue 不处理 带有 `v-pre` 指令的 DOM 元素
  + 用于提高性能

```html
<div v-pre><h1>{{ message }}</h1></div>
<!-- 输出 {{message}} -->
```

- [v-cloak](https://cn.vuejs.org/v2/api/#v-cloak)
  + 在头部加载一个特殊样式：`[v-cloak] {display: none;}` 
  + 然后在被 Vue 管理的模板入口节点上作用 ： `v-cloak` 指令
  + 原理：默认一开始被 Vue 管理的模板是隐藏的，当 Vue 解析处理完 Vue 模板之后，会自动把这个样式去除，然后就显示出来
- [v-once](https://cn.vuejs.org/v2/api/#v-once)

  + 只绑定渲染一次，以后数据再变，也不会影响该内容

```html
<div v-once><h1>{{ message }}</h1></div>
```

### 自定义指令

在需要对普通 DOM 元素进行底层操作时，就会用到自定义指令。

- 注册绑定方式
  + 指令的名字
    * 名字中不要加 `v-` 前缀（只有使用时才加）
  + 如果有多个单词，则建议使用驼峰命名法
  + 在使用时，首先要加 `v-` 前缀，然后基于驼峰命名法，转为小写，用 `-` 拼接
  + 全局注册
    * `Vue.directive('指令名称',{配置参数})`  
    * 如果是全局指令，则一定要在实例化 Vue 之前注册
    * 全局指令在所有组件(实例)中都可以使用
    * 如果需要在任何组件中可能使用的指令，把其注册成全局指令
  + 局部注册
    * 通过实例选项 `directives` 来注册
    * 指令的名字作为 `directives` 对象的成员
    * 局部自定义指令只能在这个组件（实例）中使用
    * 如果只是在某个组件中使用，这个时候注册为组件内局部指令

举个聚焦输入框的例子，如下：

```javascript
// 注册一个全局自定义指令 `v-focus`
// 第一个参数 指令名称，使用时需加上 v- 前缀
Vue.directive('focus', {
    // 当被绑定的元素插入到 DOM 中时……
    inserted: function (el) {
        // 聚焦元素 el为 在模板中使用 v-focus 指令的 DOM 元素
        el.focus()
    }
})
// 简写方式，自动应用到 binding 和 update 两钩子函数
Vue.directive('focus',  function (el) {
    el.focus()
})
```

在模板中使用

```html
<input v-focus>
```

#### 指令的生命钩子函数

- `bind`：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。
- `inserted`：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
- `update`：所在组件的 VNode 更新时调用，**但是可能发生在其子 VNode 更新之前**。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新
- `componentUpdated`：指令所在组件的 VNode **及其子 VNode** 全部更新后调用。
- `unbind`：只调用一次，指令与元素解绑时调用。

#### 钩子函数参数

- el
  + 作用该指令的 DOM 元素
- binding
  + value 指令的值

## 服务端通信

- Vue  没有内置任何 ajax 请求方法
- 在 Vue 1.0 时代，大家都使用一个插件： `vue resource` 来发送请求
  + 支持 Promise
- 在 Vue 2.0 时代，大家都选择社区中第三方库：`axios`
  + 支持 Peomise
- 在 HTML5 时代，后来浏览器增加了一个特殊的异步请求方法 ：`fetch` （原生支持 Promise）
  + 由于兼容性问题，一般只在移动端使用
- 甚至你可能会在一些别人的项目看到 Vue 和 jQuery 混用的情况
- 结合生命钩子获取数据，渲染数据，一般在 `created` 阶段使用

## 生命周期钩子

- [beforeCreate](https://cn.vuejs.org/v2/api/#beforeCreate) 
  + Vue 实例化之后调用；数据观测（data observer）和 event/watcher 事件配置之前
  + `$data` 、`$el` 不可见
- [created](https://cn.vuejs.org/v2/api/#created) 
  + 在实例创建完成之后调用，已完成：数据观测（data observer），属性和方法的运算，watch/event 事件回调，
  + `$data` 可见（但是模板还没得到更新），`$el` 不可见
  + 由于模板还未得到更新，所以无法通过 DOM 操作得到更新之后的结果
  + 适用于 发送请求，修改 data 数据
- [beforeMount](https://cn.vuejs.org/v2/api/#beforeMount) 
  + 在挂载元素之前被调用：相关的 render 函数首次被调用，还没有渲染 DOM
  +  `$el` 可见，模板还是未更新
  + 如果在实例化时 没有 `el:'#app'` 则调用 `new Vue({...}).$mount('#app')` 两个方式等价
- [mounted](https://cn.vuejs.org/v2/api/#mounted) 
  + 挂载渲染已完成，DOM 数据已更新
  + 如果想 DOM 操作，可在这里进行
- [beforeUpdate](https://cn.vuejs.org/v2/api/#beforeUpdate) 
  + 数据更新前调用， DOM 还没得到更新
- [updated](https://cn.vuejs.org/v2/api/#updated) 
  + 数据更新之后调用， DOM 已得到更新
- [activated](https://cn.vuejs.org/v2/api/#activated) 
  +  `keep-alive` 组件激活时调用
- [deactivated](https://cn.vuejs.org/v2/api/#deactivated) 
  +  `keep-alive` 组件停用时调用
- [beforeDestroy](https://cn.vuejs.org/v2/api/#beforeDestroy) 
  + Vue 实例销毁之前
- [destroyed](https://cn.vuejs.org/v2/api/#destroyed) 
  + Vue 实例销毁之后，销毁不是删除实例，而是不再管理
  + `app.$destroy()` 完全销毁一个实例，清理它与实例的连接，解绑指令及事件监听器
- [errorCaptured](https://cn.vuejs.org/v2/api/#errorCaptured) 
  + 捕获组件中发生的错误

## 使用 `json-server` 来模拟数据接口

> 参考文档：[json-server](https://github.com/typicode/json-server)
>
> 该工具默认启动的服务自带跨域能力
> 这个工具已经在服务端处理了跨域问题，你直接来请求即可

安装：

```shell
npm i -g json-server 
```

使用：

创建一个 `db.json` ，写入以下内容：

```json
{
    "list":[{
        "id":1,
        "name":"jack",
        "age":18
    },{
        "id":2,
        "name":"jack",
        "age":18
    },{
        "id":3,
        "name":"jack",
        "age":18
    }]
}
```

启动接口服务(该服务默认占用 3000 端口)：

```shell
json-server --watch db.json
```

增删改查：

- GET /list 查询所有
- GET /list/id 查询单个
- POST /list 添加
- DELETE /list/id 根据 id 删除
- PATCH /list/id 根据 id 修改

## 接口测试工具： `postman`

## http-server

安装：

```shell
npm i http-server
```

查看使用帮助：

```shell
hs -h
```

基本使用：

```shell
# 默认占用 8080 端口启动一个服务器，直接打开浏览器
hs -o
# 指定端口
hs -p 3000 -o
# 不启用缓存开启
hs -c-1 -p 3000 -o
```

## lodash

> lodash 第三方 JavaScript 工具函数库：提供 函数节流、函数防抖 等辅助函数
>
> 官网： [loadsh](https://www.lodashjs.com/) 

安装：

```shell
npm i --save lodash
```

使用：

```html
<script scr="../node_modules/lodash/lodash.js"></script>
<script>
    // 自动添加对象 _ ，使用时 直接 _.函数名
	console.log(_)
</script>
```

## element ui

>  官网 [elelment ui](http://element-cn.eleme.io/#/zh-CN/component/installation)

安装：

````shell
npm i element-ui -S
````

配置：

```html
<!-- 引入样式 -->
<link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css">
<!-- 引入组件库 -->
<script src="https://unpkg.com/element-ui/lib/index.js"></script>
```

## 图片懒加载

主要用于一屏页面比较多时，没滚到时，图片不显示，当滚到特定页面自动加载

安装

```shel
npm i vue-lazyload
```

配置：

```javascript
// main.js
import VueLazyload from 'vue-lazyload'
Vue.use(VueLazyload,{
    loading:".static/loading-svg/loading-bars.svg"
    //图片在static文件夹，就可以直接写路径了
    loading: require('../public/loading-svg/loading-bars.svg')
	//图片在assets文件夹，就需要使用require（）进行引入
})
// vue-lazyload是在 main.js 文件中引入，不会被 webpack 进行编译，src 中的文件会被 webpack 编译，包括 assets ，assets 文件夹中的图片地址，会在编译过程中改变。因此 vue-lazyload 无法正确获得图片地址，就不能显示图片了。
```

使用：

```html
<img v-lazy="'./img/1.jpg'"/>
```

## crumbs

awesome 技术名称 方便在 github 中查找别人收集好的资源

例如 vue awesome 就有别人收集好的

v-for 多层数据 再套用 v-for

 字符串转html

```html
<div class="item" v-for="item in socialArray"> 
 <dl v-html="item.content"> 
  {{item.content}} 
 </dl> 
</div>
```

methods 中一个方法调用另一个方法

```javascript
methods: {
    test1:function(){
        alert(this.test)
    },
        test2:function(){
            alert("this is test2")
            alert(this.test) //test3调用时弹出undefined
        },
            test3:function(){
                this.$options.methods.test2();//在test3中调用test2的方法
            }
}
```



8-1
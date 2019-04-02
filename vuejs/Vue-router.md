## 起步

### 安装：

```shell
npm install vue-router
```

### 配置：

非模块化工程 直接引入

```html
<script src="..."></script>
```

在模块化工程使用时 使用 `Vue.use()` 

```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'
Vue.use(VueRouter)
```

### 实例

在页面中通过 `<router-view></router-view>` 声明路由的出口

new VueRouter({})

 通过 routers 选项配置路由表，当请求 xxx 时，渲染指定组件

在 Vue 实例中，通过选项 router 配置路由实例，从而让整个应用具有路由能力

```html
<div id="app">
    <h1>Hello App!</h1>
    <p>
        <!-- 使用 router-link 组件来导航. -->
        <!-- 通过传入 `to` 属性指定链接. -->
		<!--<a href="#/foo">Go to Foo</a> 等效下面的-->
        <router-link to="/foo">Go to Foo</router-link>
        <router-link to="/bar">Go to Bar</router-link>
    </p>
    <!-- 路由出口 -->
    <!-- 路由匹配到的组件将渲染在这里 -->
    <router-view></router-view>
</div>

<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
<script>
    // 1. 定义 (路由) 组件。
    // 可以从其他文件 import 进来
    const Foo = { template: '<div>foo</div>' }
    const Bar = { template: '<div>bar</div>' }

    // 2. 定义路由
    // 每个路由应该映射一个组件。 其中"component" 可以是
    // 通过 Vue.extend() 创建的组件构造器，
    // 或者，只是一个组件配置对象。
    const routes = [
        { path: '/foo', component: Foo },
        { path: '/bar', component: Bar }
    ]

    // 3. 创建 router 实例，然后传 `routes` 配置
    const router = new VueRouter({
        routes // (缩写) 相当于 routes: routes
    })

    // 4. 创建和挂载根实例。
    // 记得要通过 router 配置参数注入路由，
    // 从而让整个应用都有路由功能
    const app = new Vue({
        router
    }).$mount('#app')
</script>
```

## 配置链接激活时使用的 CSS 类名

```javascript
const router = new VueRouter({
    linkActiveClass: 'active',// 当链接激活时，获得的 CSS 属性是 active
    routes: [
        { 
            path: '/',
            component: Home // 没有 's' ！
        }
    ]
})
```

```html
<!--设置 router-link 渲染的标签类型，默认 a 签-->
<router-link to="/" exact tag="li">
    <!-- exact 指严格匹配（不匹配 /user 这种也是以 / 开头的）
	tag="li" 渲染成 li 标签
	-->
    <a>Home</a>
</router-link>
```

## 导航守卫 `beforeRouteUpdate` 

主要用来通过跳转或取消的方式守卫导航，自动在导航到该组件之前调用

- **to: Route**: 即将要进入的目标 路由对象
- **from: Route**: 当前导航正要离开的路由
- **next: Function**: 一定要调用该方法来 **resolve** 这个钩子。执行效果依赖 `next` 方法的调用参数。
  - **next()**: 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 **confirmed** (确认的)。
  - **next(false)**: 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 `from` 路由对应的地址。
  - **next('/') 或者 next({ path: '/' })**: 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 `next` 传递任意位置对象，且允许设置诸如 `replace: true`、`name: 'home'` 之类的选项以及任何用在 `router-link` 的 `to` prop 或 `router.push`中的选项。
  - **next(error)**: (2.4.0+) 如果传入 `next` 的参数是一个 `Error` 实例，则导航会被终止且该错误会被传递给 `router.onError()`注册过的回调。

**确保要调用 next 方法，否则钩子就不会被 resolved。** 没有 next() 路由过不来

```javascript
export default {
    data() {
        return {
            list: []
        };
    },
    async beforeRouteUpdate(to, from, next)  {
        // ...
        next(vm => {
            // 通过 `vm` 访问组件实例
            // 注意：不！能！获取组件实例 `this`,也不能获得 methods 的函数
            // 所以不能调用 methods 的函数
        })
    },
};
```

## 路由传参

```html
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>

<script>
router.push({ name: 'user', params: { userId: 123 }})
</script>
```

## 默认子路由

```javascript
// router.js
export default new Router({
    model: 'history',
    routes: [{
        path: '/task_manage',
        name: 'home',
        component: Home,
        redirect: '/task_manage/index',// 路由重定向
        children: [{
            path: '/task_manage/index',
            name: 'index',
            component: BoxMiddle,
        }]
    }]
})
```

## crumbs

在配置好 vue.router 之后，会自动在 vue 组件添加 $router 和 $route

this.$router 访问得到的是 路由实例 对象

this.$route 访问到的是 路由参数 对象

this.$router.query 获取查询字符串参数
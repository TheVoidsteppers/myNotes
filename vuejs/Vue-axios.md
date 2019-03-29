## 起步

安装：

```shell
# vue-axios 依赖 axios
npm install --save axios vue-axios
```

vuecli3 配置 vue-axios

提示：使用插件的时候，一般都要在入口文件main.js中引入，因为main.js项目运行首先运行的文件。

```javascript
// main.js
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'

Vue.use(VueAxios, axios)
```

疑问：为什么使用Vue.use(VueAxios, axios)
解惑：通过全局方法 Vue.use() 使用插件，就相当于调用install方法，vue官网举例如下：

```javascript
// 调用 'MyPlugin.install(Vue)'
Vue.use(MyPlugin)
```

使用：

```javascript
Vue.axios.get(api).then((response) => {
    console.log(response.data)
})

this.axios.get(api).then((response) => {
    console.log(response.data)
})

this.$http.get(api).then((response) => {
    console.log(response.data)
})
```

## vue-axios学习网址

<https://blog.csdn.net/qq_41115965/article/details/80780264>

网址1： <https://github.com/imcvampire/vue-axios>
网址2： <https://www.npmjs.com/packge/axios>

## 本地跨域

```javascript
// vue.config.js
module.exports = {
    devServer: {
        proxy: {
            '/api': {
                target: 'https://mufeng.me',
                changeOrigin: true
            }
        }
    }
}
```


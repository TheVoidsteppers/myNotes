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
// 使用 axios 不是 vue-axios
axios.post(
    "/task_manage/task",
    {
        sddsdf: "sdfd"
    },
    {
     headers: {
         "Content-type": "application/x-www-form-urlencoded"
     }//没有 headers 不是 post 请求？？？？
    }
).then(res => {
    this.taskInfo = res.data.data;
    console.log(res.data.data);
});
```

## vue-axios学习网址

<https://blog.csdn.net/qq_41115965/article/details/80780264>

网址1： <https://github.com/imcvampire/vue-axios>
网址2： <https://www.npmjs.com/packge/axios>

## 本地跨域

```javascript
devServer: {
    port: 8080,
        proxy: {
            '/task_manage': {
                target: 'http://localhost:80/ztong/manual.php/task_manage/task', 
                // target host 接口地址    80 是接口端口
                ws: true, 
                // proxy websockets 
                changeOrigin: true,
                // needed for virtual hosted sites
                pathRewrite: {
                    '^/task_manage': '' // rewrite path
                }
            }
        }
}
```

## axios 发 post 请求，后端接收不到参数的解决方案

> axios会帮我们 **转换请求数据和响应数据** 以及 **自动转换 JSON 数据**

就是说，我们的 Content-Type 变成了 application/json;charset=utf-8
然后，因为我们的参数是 JSON 对象，axios 帮我们做了一个 stringify 的处理。
而且查阅 axios 文档可以知道：axios 使用 post 发送数据时，默认是直接把 json 放到请求体中提交到后端的。

那么，这就与我们服务端要求的 'Content-Type': 'application/x-www-form-urlencoded' 以及 @RequestParam 不符合。

解决方案：

```javascript
//用 URLSearchParams 传递参数】
let param = new URLSearchParams()
param.append('username', 'admin')
param.append('pwd', 'admin')
axios({
	method: 'post',
	url: '/api/lockServer/search',
	data: param
})
```

原文：<https://blog.csdn.net/csdn_yudong/article/details/79668655>

`jQuery.ajax`的`post`提交默认的请求头的`Content-Type: application/x-www-form-urlencoded`
而`axios.post`提交的请求头是`Content-Type: application/json`。

`application/json`是一个趋势，但是如果改一个旧项目，把`jQuery.ajax`全部换成`axios.post`时，需要对请求做一些配置

```javascript
// 原来的代码 没有指定请求头的content-type
var data = {age: 18};
$.ajax({
    url: '',
    type: 'POST',
    data: data
    dataType: 'json',
    success: function(result) {
        // do something
    }
})
// 使用 axios 的代码
import axios from 'axios';
import qs from 'qs';

var data = {age: 18};
var url = '';

axios.post(
    url, 
    qs.stringify(data), 
    {headers: {'Content-Type': 'application/x-www-form-urlencoded'}}
).then(result => {
    // do something
})
```


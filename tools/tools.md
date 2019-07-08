awesome 技术名称 方便在 github 中查找别人收集好的资源

例如 vue awesome 就有别人收集好的


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


## 图片滚动自动加载

vue-infinite-scroll 

## 轮播图插件

swiper

## 前端开发技术

awesomes
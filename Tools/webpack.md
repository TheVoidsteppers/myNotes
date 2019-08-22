## 介绍

- 是什么
  + 打包工具
  + JavaScript 模块打包之后就可以运行在浏览器
- 能做什么
  + JavaScript 资源打包
  + css 打包
  + 图片打包
  + less
  + sass
  + babel EcmaScript 6 转 EcmaScript 5
  + 开发工具：http 服务器
  + 代码改变，自动刷新浏览器
  + 压缩代码
- [官方指南](https://webpack.js.org/)

## 起步

### 安装

全局安装：

```shell
npm install --global webpack
```

把 webpack 安装到本地项目中，这样项目到哪，webpack 就跟到哪（打包工具随项目走）

开发依赖(--save-dev)，webpack 只是一个打包工具，项目如果上线，上线的是打包结果，而不是这个工具，所以为了区分核心包依赖和开发工具依赖，通过 `--save` 和 `--save-dev` 区分

本地安装（推荐）：

```shell
npm install --save-dev webpack
```

对于安装到项目中的 webpack 需要配置 package.json 中的 script

```json
{
    "name": "blog",
    "version": "1.0.0",
    "description": "",
    "main": "app.js",
    "scripts": {
        "a": "node ./src/a.js",
        "start": "node ./src/main.js",
        "build": "webpack",
        // 监视编译，使用该命令时，源代码一改，自动编译
        "watch-build": "webpack --watch"
        // 相当于给命令起别名
    },
    "keywords": [],
    "author": "",
    "license": "ISC",
    "dependencies": {
        "art-template": "^4.13.2",
    }
}
```

然后通过 `npm run` 命令使用 webpack

```shell
npm run a
# start 比较特殊，可以不加 run
npm start
# 打包构建
# 这里使用的 webpack 就是项目中安装的 webpack
npm run build
```

### hello world

全局安装：

```shell
npm install --global webpack
```

准备目录结构：

```
.
├── index.html
├── main.js
└── foo.js
```

打包：

```shell
webpack 模块入口文件 模块出口文件路径
```

最后记得把 index.html 文件中的脚本应用改为打包后的结果文件路径

划分 src 和 dist 目录：

- 把源码存储到 src 目录
- 把打包的结果存储到 dist 目录

### 配置文件 `webpack.config.js` 

基本配置：在项目根目录下创建 `webpack.config.js`

```javascript
// 该文件最终是在 Node 环境下执行的
const path = require('path')
// 导出一个具有特殊属性配置的对象
moudle.exports = {
    entry: './src/main.js', // 入口文件模块路径
    output: {
        path: path.join(__dirname, './dist/'), 
        // 出口文件模块所属目录，path 必须是绝对路径
        filename: 'bundle.js' // 打包的结果文件名称
    }
}
```

打包：

```shell
# webpack 会自动读取 webpack.config.js 文件作为默认的配置文件
# 也可以通过 --config 参数手动指定配置文件
webpack
```

## 打包 JavaScript 模块

### JavaScript 模块化

- AMD
  + Require.js
- CMD
  + Sea.js
- CommonJS
- 以上都是民间搞出来的，所以在 2015 年的EcmaScript 6 中官方就发布了官方的模块规范：EcmaScript 6 Module 模块规范
  + 推荐在项目中使用 EcmaScript 6 模块规范
  + 以后的趋势统一
- webpack
  + AMD
  + CMD
  + CommonJS
  + EcmaScript 6 Module

### EcmaScript 6 模块规范

导入 import（require）

导出 export（module.exports）

导出默认成员：

```javascript
// 默认成员只能有一个
export default 成员
```

加载默认成员：

```javascript
// 如果没有 default 成员，则加载到 undefined
import xxx from '模块标识'
```

导出多个成员：

```javascript
// export 必须引用到内部的一个成员
export const a = 123
export const b = 'hello'
export function fn () {
    console.log('hi')
}
// 也可以 注意：这里不是对象的简写方式，这是导出的特殊方法
export {
	a,
    b,
    fn
}
```

按需加载指定的多个成员：

```javascript
import {a, b} from '模块标识'
```

一次性加载所有的导出成员：

```javascript
// 包含 default
import * as xxx from '模块标识'
```

## 资源管理

webpack 不仅可以打包 JavaScript 模块，设置可以把网页的一切资源都当作模块处理

需要结合插件来实现，这些插件被称为 loader（加载器）

### Loading CSS

安装依赖：

```shell
# css-loader 的作用是把 css 文件转为 JavaScript 模块
# style-loader 的作用是动态创建 style 节点插入到 head 中
npm install --save-dev style-loader css-loader
```

配置 webpack.config.js 文件

```javascript
module.exports = {
    +   module: {
    +     rules: [
    +       {
    +         test: /\.css$/,
    +         use: [
    +           'style-loader',
    +           'css-loader'
    +         ]
        +       }
            +     ]
                +   }
};
```

打包：

```shell
npm run build
```

打包 css 也是 把 CSS 文件内容转换成一个 JavaScript 模块，然后再运行 JavaScript 的时候，会动态的创建一个 style 节点插入到 head 头部

### Loading Images

安装依赖：

```shell
npm install --save-dev file-loader
```

配置 webpack.config.js 文件：

```javascript
module.exports = {
    module: {
        rules: [{
            +   test: /\.(png|svg|jpg|gif)$/,
            +         use: [
            +           'file-loader'
            +         ]
    }]
}
};
```

### Loading Fonts

### Loading Data

### Loading Less

安装依赖：

```shell
# 需前置 css-loader style-loader
npm install --save-dev less-loader less
```

配置 webpack.config.js 文件：

```javascript
// webpack.config.js
module.exports = {
    ...
    module: {
        rules: [{
            test: /\.less$/,
            use: [
                "style-loader", // creates style nodes from JS strings
                "css-loader", // translates CSS into CommonJS
                "less-loader", // compiles Less to CSS
            ]
        }]
    }
};
```

打包：

```shell

```

### Loading Sass

### transpiling JavaScript

#### babel

+ https://babeljs.io/
+ babel 是一个 JavaScript 编译器，可以把 EcmaScript 6 编译成 EcmaScript 5
+ babel 可以独立使用，但是独立使用没有意义，一般是和 webpack 结合使用


安装依赖：

```shell
npm install --save-dev babel-loader babel-core babel-preset-env webpack
```

配置 webpack.config.js 文件：

```javascript
module: {
    rules: [
        {
            test: /\.js$/,
            exclude: /(node_modules|bower_components)/,// 不转换 node_modules 中的文件模块
            use: {
                loader: 'babel-loader',
                options: {
                    // babel-preset-env 的最后一个词，转码规则
                    presets: ['env']
                }
            }
        }
    ]
}
```

#### babel-polyfill

+ 默认 babel 只转换语法
+ 可以使用 babel-polyfill 来转换 EcmaScript 6 中的 API

安装依赖：

```shell
npm install --save @babel/polyfill
```

配置 webpack.config.js ：

```javascript
module.exports = {
    // js 入口主文件 
    entry: ["@babel/polyfill", "./src/main.js"],
};
```

这样的话，就会在打包的结果中提供一个垫胶片用以兼容低版本浏览器中不支持的 API

#### transform-runtime 

用以解决代码重复问题

在打包过程中，babel 会在某些包提供一些的工具函数，而这些工具函数可能出现重复的出现在多个模块，这样的话就会导致打包体积过大

安装依赖：

```shell
npm install babel-plugin-transform-runtime --save-dev
npm install babel-runtime --save
```

配置 webpack.config.js：

```javascript
rules: [
    // 'transform-runtime' 插件告诉 babel 要引用 runtime 来代替注入。
    {
        test: /\.js$/,
        exclude: /(node_modules|bower_components)/,
        use: {
            loader: 'babel-loader',
            options: {
                presets: ['@babel/preset-env'],
            +   plugins: ['@babel/transform-runtime']
            }
        }
    }
]
```

#### 加入缓存节省编译时间

babel 编译是非常耗时的，我们可以通过开启对编译结果的缓存来提高打包速度：

```javascript
module: {
    rules: [
        {
            test: /\.js$/,
            exclude: /(node_modules|bower_components)/,
            use: {
                loader: 'babel-loader',
                options: {
                    // 默认把打包的结果缓存到 node_modules/.cache 文件
             +      cacheDirectory: true,
                    presets: ['env'],
                    plugins: ['transform-runtime']
                }
            }
        }
    ]
}
```

## 输出管理

### HtmlWebpackPlugin

打包结束后，如果 index.html 在根目录直接运行的话，那么图片资源这些路径就无法访问到。

解决方案就是把 index.html 一起打包放到 dist 目录中

可以使用插件 `html-webpack-plugin`

而且可以自动添加打包 js 等的文件 `bundle.js` 路径到打包后的 index.html 文件中


安装依赖：

```shell
npm install --save-dev html-webpack-plugin
```

配置 webpack.config.js 文件：

```javascript
const path = require('path');
+ const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    entry: {
        app: './src/index.js',
        print: './src/print.js'
    },
	// 自动在 index.html 中引入 script 链接
    // 而且引用的资源名称，取决于你的 bundle叫什么
    +   plugins: [
    +     new HtmlWebpackPlugin({
    +       template: 'index.html',// 模板文件
    +       title: 'Output Management'
    +     })
    +   ],
            output: {
                filename: 'bundle.js',
                path: path.resolve(__dirname, 'dist')
            }
};
```

## Development 开发

### Source maps

### webpack-dev-server

功能：监视代码改变，自动打包，打包完毕后自动刷新浏览器

安装依赖：

```shell
npm install --save-dev webpack-dev-server
```

配置 webpack.config.js 文件：

```javascript
module.exports = {
    +   devServer: {
    	// 配置 webpack-dev-server 的 www 目录
    	// webpack-dev-server 为了提高打包效率，它把文件存储在了内存中
    	// 所以打包结果运行在项目根目录的 虚拟目录 中
    +   contentBase: './'
    +   }
};
```

配置 `package.json`  的script

```json
{
    "name": "development",
    "version": "1.0.0",
    "description": "",
    "main": "webpack.config.js",
    "scripts": {
        "build": "webpack",
        "watch-build": "webpack --watch",
    +   "dev": "webpack-dev-server --open",
    }
}
```

启动开发模式：

```shell
npm run dev
```

该工具会自动帮你打包，打包完毕会自动开启一个服务器，默认监听 8080 端口号，同时自动打开浏览器，接下来就会监视代码的改变，然后自动编译，编译完毕，自动刷新浏览器

## Hot Module Replacement 热更新

配置 webpack.config.js 文件：

```javascript
+ const webpack = require('webpack');

  module.exports = {
    entry: {
+      app: './src/index.js'
    },
    devtool: 'inline-source-map',
    devServer: {
      contentBase: './dist',
+     hot: true
    },
    plugins: [
      new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
        title: 'Hot Module Replacement'
      }),
+     new webpack.HotModuleReplacementPlugin()
    ],
  };
```

## Vue Loader

> 打包 .vue 单文件组件的

- vue-loader
- vue-template-compiler

安装依赖：

```shell
npm install --save-dev vue-loader vue-template-compiler
```

配置 webpack.config.js 文件：

```javascript
// webpack.config.js
const VueLoaderPlugin = require('vue-loader/lib/plugin')
module.exports = {
    ...
    module: {
        rules: [{
            test: /\.vue$/,
            use: [
                'vue-loader'
            ]
        }]
    },
     plugins: [
        new VueLoaderPlugin()
     ]
};
```

## externals

> 配置 webpack 不打包第三方包

通常情况下我们不打包第三方包，因为第三方包太大，和 bundle 打包到一起会造成资源体积过大，所以还是通过 script 标签的方式把第三方资源引入到页面中

1. 下载第三方包

```shell
npm install jquery
```

2. 在页面中引入资源

```html
<!-- 注意：这里的路径以最终打包后的 index.html 文件位置为准 -->
<script src="../node_modules/jquery/dist/jquery.js"></script>
```

3. 配置 webpack.config.js

```javascript
module.exports = {
    ...
    externals:{
        // key 是第三方包名，value 是全局中的 jQuery 对象
        // 这里配置的含义是：当你在代码中 import jquery 时，不会把 jquery 打包到 bundle 中，而是使用用户指定的全局中的 jQuery 对象
        jquery: "jQuery"
    }
}
```

4. 加载使用

```javascript
import $ from 'jquery'
$('#app',{
    width:200
})
```

5. 打包测试

```shell
npm run build
```

## `--save` 和 `--save-dev` 的区别

我们把开发工具相关的依赖信息保存到 `devDependencies` 选项中，把核心依赖（例如 vue）的依赖信息保存到 `dependencies` 选项中

这样做的话，就是把开发依赖和核心依赖分开了，因为开发依赖在打包结束之后上线的话就不需要了

最后项目上线，我们真正需要安装的是 `dependencies` 依赖项的包，可以通过以下命令安装

```shell
npm install --production
```

## vue-webpack-starter

在模块化环境中使用 vue-router

1. 下载

```shell
npm i vue-router
```

2. 引用资源

```html
<script src="node_modules/vue-router/dist/vue-router.js"></script>
```

3. 配置 webpack.config.js

```javascript
externals:{
    'vue-router': 'VueRouter'
}
```

4. 在 `router.js` 文件中加载使用

```javascript
import VueRouter from 'vue-router'
import Foo from './components/Foo.vue'
import Bar from './components/Bar.vue'

// 这里直接默认导出 new 出来的 router 实例
export default new VueRouter({
    routes: [
        {
            path: '/foo',
            component: Foo
        },
        {
            path: '/bar',
            component: Bar
        }
    ]
}) 
```

5. 在 `main.js` 文件中配置使用路由对象

```javascript
import Vue from 'vue'
import App from './App.vue'
+ import router from './router'

new Vue({
    components: { App },
    // 自动将 id 为 app 的标签替换成 <app></app>
    template: '<App />',
    router
}).$mount('#app')
```

6. 在 `App.vue` 中设置路由出口

```vue
<template>
<div id="app">
    <h1>根组件</h1>
    <ul>
        <li><a href="#/foo">Foo</a></li>
        <li><a href="#/bar">Bar</a></li>
    </ul>
+   <router-view></router-view>
</div>
</template>
```

### 引入 bootstrap 样式库

安装

```shell
npm i bootstrap
```

引入到 index.html

```html
<link rel="stylesheet" href="./node_modules/bootstrap/dist/css/bootstrap.css">
```

## 浏览器报错显示 源代码行号

更改 webpack.config.js

```javascript
// 默认显示 bundle.js 的行号
// 加上 
devtool: 'inline-source-map',
```

## VueCLi

Vue 作者考虑到新手使用 webpack 带来的复杂度，所以官方开发了一个工具： `vue-cli` 。它可以帮你快速生成一个已经配置好了的 webpack 项目
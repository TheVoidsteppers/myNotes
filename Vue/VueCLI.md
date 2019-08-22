# Vue VLI

> Vue CommandLineInterface
>
> 一个脚手架工具，可以快速生成基本的项目骨架，自带了 webpack 配置
>
> https://github.com/vuejs/vue-cli
>
> https://cli.vuejs.org/

## 起步

安装：

```shell
npm install -g @vue/cli
```

初始化项目：

```shell
vue create hello-world
# 以下是旧版本
vue init webpack-simple domo5 # 简单模板
vue init webpack domo6
```

## 引入 bootstrap 和 jQuery

安装：

```shell
npm install bootstrap --save
npm install jquery --save
```

配置：

```javascript
// vue.config.js
const webpack = require('webpack')
configureWebpack: {
    plugins: [
        new webpack.ProvidePlugin({
            $: "jquery",
            jQuery: "jquery",
            "windows.jQuery": "jquery"
        })
    ]
}
// main.js
import '../node_modules/_bootstrap@3.3.6@bootstrap/dist/css/bootstrap.min.css'
import '../node_modules/_jquery@2.1.4@jquery/dist/jquery.js'
import '../node_modules/_bootstrap@3.3.6@bootstrap/dist/js/bootstrap.js'
```
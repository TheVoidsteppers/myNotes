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

# bugs

- form
  - validate-on-rule-change 只会校验 rules 的变化，不会监听 form-item 写的 rule 的变化
  - validateField 执行后不会清空报错状态
- table
  - 自定义表头
    - 如果没有写 slot-scope="scope"，表头里数据变化，不会重新渲染

# Html

~~~html
<html>
	<head>
		<tittle></tittle>
	</head>
	<body>
	</body>
</html>
~~~

| <h1></h1>~<h6></h6> | <p></p>  |
| :-----------------: | :------: |
|      <hr />水平线      | <br />换行 |

|   字体    |                   |      |
| :-----: | :---------------: | :--: |
| <b></b> | <strong></strong> |  加粗  |
| <i></i> |     <em></em>     |  斜体  |
| <s></s> |    <del></del>    | 删除线  |
| <u></u> |    <ins></ins>    | 下划线  |

|      <img />      |         |
| :---------------: | :-----: |
|   src="url或路径"    |  图片地址   |
|     alt="文字"      | 图片不存在显示 |
|    tittle="文字"    | 鼠标悬停时显示 |
| width/heigth="数值" |  图片像素   |
|    border="数字"    |  边框大小   |

|      <a></a>      |             anchor锚，链接             |
| :---------------: | :--------------------------------: |
|    href="url"     | \|外部：http：//\|本地：文件名.html \|临时地址：# |
| target="目标窗口弹出方式" |        _self\|__blank(新窗口)         |

|         锚点定位          | 页内跳转 |
| :-------------------: | ---- |
| <a href="#id名">文本</a> |      |
| <h1 id="id名">文本</h1>  |      |

```html
<!--在html5中，如果input标签内type="email"，在客户端会校验输入的内容是否是邮箱（是否含@符），如果想要关掉就在父标签form添加 novalidate取消浏览器自带的校验功能-->
<!--autocomplete="off" 关闭客户端的自动填充历史输入功能-->
<form autocomplete="off" novalidate>
    <label for="email" class="sr-only">邮箱</label>
    <input id="email" name="email" type="email" class="form-control" placeholder="邮箱" autofocus value="">
</form>
```

```html
<!--link:链入一个文档，通过rel属性声明链入的文档与当前文档之间的关系
rel='stylesheet'时，表示链入的是样式表文档，会自动下载该文件作为样式表-->
<link rel="stylesheet" href="/static/assets/vendors/bootstrap/css/bootstrap.css">
```

## 表单

```html
<!-- 
以前表单是如何提交的？
表单中需要提交的表单控件元素必须具有 name 属性
表单提交分为：
1. 默认的提交行为
2. 表单异步提交

action 就是表单提交的地址，说白了就是请求的 url 地址
method 请求方法
get
post
-->
<form action="/pinglun" method="get"></form>
```




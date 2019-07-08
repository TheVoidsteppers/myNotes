## 栅格系统

```
<!--h5文档申明-->
<!DOCTYPE html>
<!--文档语言申明  en zh-CN zh-tw -->
<html lang="zh-CN">
<head>
    <!--文档编码申明-->
    <meta charset="utf-8">
    <!--要求当前网页使用浏览器最高版本的内核来渲染-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!--视口的设置：视口的宽度和设备一致，默认的缩放比例和PC端一致，用户不能自行缩放-->
    <meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=0">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <!-- 优先加载和浏览器解释 -->

    <title>title</title>

    <!-- Bootstrap 核心样式-->
    <link href="../lib/bootstrap/css/bootstrap.css" rel="stylesheet">
    <!-- html5shiv 和  respond 分别用来解决IE8版本浏览器不支持 H5标签和媒体查询的  不兼容问题-->
    <!-- HTML5 shiv and Respond.js for IE8 support of HTML5 elements and media queries -->
    <!-- 警告：不能以file形式打开，本地打开。最好http://打开 -->
    <!-- WARNING: Respond.js doesn't work if you view the page via file:// -->
    <!-- 在 IE 9 一下引入-->
    <!--[if lt IE 9]>
    <script src="../lib/html5shiv/html5shiv.min.js"></script>
    <script src="../lib/respond/respond.min.js"></script>
    <![endif]-->
    <style>
        .container{height: 100px;background: pink;}
        .container > .row{height: 50px;background: green;}
        .container > .row > div{height: 25px;border: 1px solid #ccc;}
    </style>
</head>
<body>
<!--响应式布局容器-->
<div class="container">
    <!--栅格系统：其实就是行和列的布局，网格状布局-->
    <!--行：row-->
    <!-- .container容器默认有15px的左右内间距  .row 填充父容器的15px的左右内间距   margin-left,margin-right -15px拉伸 -->
    <div class="row">
        <!--列：col-*-*  *不确定（参数） -->
        <!--
            col：列样式
            第一个*：屏幕的大小
            大屏设备     lg   大屏设备以上生效包含本身
            中屏设备     md   中屏设备以上生效包含本身
            小屏设备     sm   小屏设备以上生效包含本身
            超小屏设备   xs   超小屏设备以上生效包含本身
            第二个*：每一行的分等份，默认分成12等份 ，数字代表的是占多少份
        -->
        <div class="col-xs-4"></div>
        <div class="col-xs-4"></div>
        <div class="col-xs-4"></div>
    </div>
</div>
<!-- bootstrap依赖jquery-->
<!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
<script src="../lib/jquery/jquery.min.js"></script>
<!-- bootstrap js 核心文件-->
<!-- Include all compiled plugins (below), or include individual files as needed -->
<script src="../lib/bootstrap/js/bootstrap.min.js"></script>
</body>
</html>
```

### 栅格系统的拓展功能

```html
<div class="row">
    <!--栅格嵌套-->
    <div class="col-xs-4">
        <div class="row">
            <div class="col-xs-6"></div>
            <div class="col-xs-6"></div>
        </div>
    </div>
    <!--列的偏移-->
    <div class="col-xs-4">
        <div class="row">
            <div class="col-xs-3"></div>
            <div class="col-xs-6 col-xs-offset-1"></div>
        </div>
    </div>
    <!--列的排序-->
    <div class="col-xs-4">
        <div class="row">
            <!--
                push 往后推
                pull 往前拉
            -->
            <div class="col-xs-3 col-xs-push-9">col-xs-3</div>
            <div class="col-xs-9 col-xs-pull-3">col-xs-9</div>
        </div>
    </div>
```

### 响应式工具

```html
<!--
大屏设备    显示
中屏设备    隐藏
小屏设备    显示
超小屏设备  隐藏
visible-lg     大屏显示其他隐藏
visible-md
visible-sm
visible-xs
3.2版本以后  建议使用hidden
hidden-lg
hidden-md
hidden-sm
hidden-xs
-->
```

## 组件

### collapse 组件 折叠


```html
<button data-toggle="collapse" data-target=".box">切换</button>
<!-- href 也可以选择目标 相当于 data-target-->
<a href=".box" data-toggle="collapse" >切换</a>
<div class="box">
    内容<br>
    内容<br>
    内容<br>
    内容<br>
    内容<br>
</div>

<!-- 导航的内容容器 -->
<div class="container">
    <!-- 包含 商标区域 和 切换按钮（在移动端显示） -->
    <div class="navbar-header">
        <!--切换按钮-->
        <!--
            类名：collapsed  样式
            属性：
            data-toggle="collapse"  申明是什么组件=折叠组件
            data-target="#bs-example-navbar-collapse-1" 控制的目标元素=选择器
            其他：
            aria-expanded="false"  aria-* 代表提供给屏幕阅读器使用的（盲人阅读器）
            class="sr-only" screen read only  代表提供给屏幕阅读器使用的（盲人阅读器）
        -->
        <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
        </button>
        <!--商标区域-->
        <a class="navbar-brand" href="#">Brand</a>
    </div>
    <!-- 导航连接  表单  其他内容  被切换 -->
    <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
        <!--默认的导航-->
        <ul class="nav navbar-nav">
            <li class="active"><a href="#">Link</a></li>
            <li><a href="#">Link</a></li>
            <li><a href="#">Link</a></li>
        </ul>
        <!--右对齐的导航-->
        <ul class="nav navbar-nav navbar-right">
            <li><a href="#">Link</a></li>
        </ul>
    </div>
</div>
```

### carousel 组件 轮播图

```html
<!-- carousel 轮播图的模块  slide是否加上滑动效果 -->
<!-- data-ride="carousel" 初始化轮播图属性-->
<div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
    <!-- 点盒子 -->
    <ol class="carousel-indicators">
        <!--
            data-target="#carousel-example-generic" 控制目标轮播图
            data-slide-to="0" 控制的是轮播图当中的第几张 （索引）
            class="active" 当前选中的点
        -->
        <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
        <li data-target="#carousel-example-generic" data-slide-to="1"></li>
    </ol>
    <!-- 图片盒子 -->
    <!-- role="listbox" 提供给屏幕阅读器使用 -->
    <div class="carousel-inner">
        <!--需要轮播的容器-->
        <div class="item active">
            <!--图片-->
            <img src="..." alt="...">
            <!--说明-->
            <div class="carousel-caption">
                ...
            </div>
        </div>
        <div class="item">
            <img src="..." alt="...">
            <div class="carousel-caption">
                ...
            </div>
        </div>
    </div>

    <!-- 上一张下一张按钮 -->
    <!--
     data-slide="prev"
     data-slide="next"
     href="#carousel-example-generic"   控制目标轮播图
    -->
    <a class="left carousel-control" href="#carousel-example-generic"  data-slide="prev">
        <span class="glyphicon glyphicon-chevron-left"></span>
    </a>
    <a class="right carousel-control" href="#carousel-example-generic" data-slide="next">
        <span class="glyphicon glyphicon-chevron-right"></span>
    </a>
</div>
```


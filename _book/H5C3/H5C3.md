# 购物车宣传页

`原理：window.onmousewheel=function(){}`

`运用：使用JQuery全屏滚动插件fullPage.js`

## 动画

### js动画与css动画的区别

> 使用动画（js实现动画、css3显示的动画）
>
> ​	一个是帧动画	一个是补间动画
> ​	帧动画：使用定时器 每隔一段时间 更改当前元素的状态
> ​	补间动画：过渡（加过渡 只要状态发生改变产出动画） 动画（多个节点来控制动画） 性能会更好
> ​	在支持H5C3的浏览器尽可能使用css3动画（移动端开发）
>
> ​	transition animation
> ​	transition 组合写法(transition:all 1s linear 1s)
> ​			拆分写法(transition-property transition-duration transition-timing-function transition-delay)



`animation: more01 0.3s linear infinite alternate forwards;/* 循环 逆播放 保留最后一帧的样式值（继承原有属性，默认backwords）*/`

### transform转换属性

`/*1.组合写法  多个转换属性之间空格  transform:rotate() translateX() skew() scale()*/
​    /*2.如果你先旋转  那么坐标轴也会旋转*/
​    /*3. 最好是先位移在旋转*/`

`  /*jquery插件初始的时候封装这个方法*/
​            /*1.回想jquery插件的封装 $.fn.fullpage = function(){} */
​            /*2.jquery本身没有的方法通过$.fn的方式追加方法  认为是插件方法*/
​            /*3.例如：$.fn.src = function(){ return this.attr('src') } this 你选择谁this（jquery对象）执行谁 */`

`/*1.掐时间  做延时  完成动画的衔接*/
/*2.jquery的动画  $('dom').animate(property,duration,speed,callback) */
/*speed  (swing linear) */
/*3.能不能监听到动画或者过度的结束*/
/*4.使用js监听css3当中 过渡 transitionend  动画 animationend*/`

```javascript
/*当第四屏的购物车动画结束之后执行收货地址的动画*/
$('.screen04 .cart').on('transitionend',function () {
    /* :last :first :visible :hidden :checked :selected jquery扩展选择器*/
    $('.screen04 .address').show().find('img:last').fadeIn(1000);
    $('.screen04 .text').addClass('show');
});
```

`left: 50%;
 /*css3的居中 基于本身*/
 transform: translateX(-50%);`

css动画库：animate.css

### 3d转换 translate3d rotate3d

2d转换和3d转换区别：
1.除了多了一个参数表示3d
2.在移动端使用3d转换可以优化性能（如果设备有3d加速引擎 GPU 可以提高性能 ,是2dz转换是无法调用GPU）

### 背景图百分比

```css
.box{
    width: 1000px;
    height: 500px;
    border: 1px solid #ccc;
    background: url("../cart/images/06-bg.png") no-repeat 25% bottom / 1200px 300px;
    /*1.背景图的百分比不是按照容器的大小去换算的  注意：：：*/
    /*2.百分比的背景定位：基于  容器的宽度-背景的宽度  */
    /*3.计算公式  背景的x的定位 = （容器的宽度-背景的宽度 ）* 百分比 (1000-1200)/25% */
}
```

对于相同元素最好不要同时使用animation transition ，两者都是做动画，会互相干扰

元素使用transform属性 转换之后会提高层级

### 多个相同元素 延时显示动画

```html
<div class="star">
    <img src="./images/07-star.png" alt="">
    <img src="./images/07-star.png" alt="">
    <img src="./images/07-star.png" alt="">
    <img src="./images/07-star.png" alt="">
    <img src="./images/07-star.png" alt="">
</div>
<style>
    .screen07 .star {
        position: absolute;
        top: 142px;
        left: 158px;
    }
    .screen07 .star img {
        float: left;
        margin-right: 6px;
        /* display: none; */
        opacity: 0;
    }
    .screen07 .star.show img {
        opacity: 1;
        transition: all 0.3s linear 2s;
    }
</style>
<script>
    $('.container').fullpage({
        onLeave: function (index, nextIndex, direction) {
            if (index == 6 && nextIndex == 7) {//从第六页到第七页
                $('.screen07 .star img').each(function (i, index) {
                    $('.screen07 .star').addClass('show');
                    // 法1：$(this).delay(i * 0.5 * 1000).fadeIn();
                    $(this).css('transition-delay',i*0.5+'s');
                })
            }
        },
    })
</script>
```

### 获取鼠标位置   重置动画

```javascript
$('.screen08').on('mousemove',function (e) {
    /*鼠标的坐标设置给手*/
    $(this).find('.hand').css({
        left:e.clientX -190,
        top:e.clientY - 20
    });
}).find('.again').on('click',function () {
    /*2.点击再来一次重置动画跳回第一页*/
    /*动画怎么怎么进行的？*/
    /*2.1 加now  类*/
    /*2.2 加leaved  类*/
    /*2.3 加show 类*/
    $('.now,.leaved,.show').removeClass('now').removeClass('leaved').removeClass('show');
    /*2.4 加css属性  后果：加一个style属性*/
    /*2.5 用jquery方法  show() fadeIn() 后果：加一个style属性*/
    $('.content [style]').removeAttr('style');
```

### 边框图片

```css
/*浏览器兼容问题 没有广泛使用*/
.box:first-child{
    /*border-image: url("images/border01.jpg") 167/20px round;*/
    /*1.在内容变化的容器可以使用，边框自动填充*/
    /*2.用法：*/
    /*2.1 图片资源*/
    border-image-source: url("images/border01.jpg");
    /*2.2 切割的尺寸 默认的单位px 而且不能使用小数*/
    /*2.2.1 切割的位置距离对应的边的距离  切了四刀   4个切割的尺寸*/
    border-image-slice: 167;/*167 167 167 167 相对于图片的上右下左边框的距离*/
    /*2.3 边框的宽度*/
    border-image-width: 20px;
    /*2.4 平铺的方式  三种平铺方式 round stretch repeat*/
    /*round 环绕 完整的自适应（等比缩放）平铺在边框内*/
    /*stretch 拉伸 拉伸显示在边框内容 变形的*/
    /*repeat 平铺 从边框的中间向两侧平铺 自适应平铺但是不是完整的*/
    border-image-repeat: round;
}
```

# QQTIM

## 视差效果

在滚动的时候，内容和多层次的背景实现或不同速度，或不同方向的运动
原理：利用background-attachment
实际运用时使用插件  stellar.js

> 1、引入 html中加 data-stellar-background-ratio="0.3"
> 2、结构 css中加 background-attachment: fixed;
> 3、js初始化插件 $.stellar({
> ​	horizontalScrolling: false,
> ​	responsive: true
> });

## 语义化标签

```html
<head>
    <!--[if lte IE 8]>
<script src="html5shiv.min.js"></script>
<![endif]-->
<!--引入的js文件一定要在head处引入-->    
</head>
<body>
    <!--1.语义化标签的作用-->
    <!--1.1 从开发角度考虑是提高代码的可读性可维护性-->
    <!--1.2 网站的发布者：seo 搜索引擎优化 -->

    <!--2.语义化标签的兼容问题-->
    <!--2.1 IE9以下不支持H5标签（大部分css3属性，一些H5的api）-->
    <!--2.2 IE9以下不认识，把这些标签当做行内元素去看待-->
    <!--2.3 语义化标签需要动态创建 document.createElement('header') 同时设置块级元素-->
    <!--2.4 有一款插件能帮你完成这件事件  html5shiv.js   -->
    <!--2.5 必须引入在头部，在页面结构之前，提前解析h5标签-->
    <!--2.6 优化处理：支持H5标签的不需要加载，IE9以下不支持H5加载-->
    <!--2.7 js判断客户的浏览器版本可以做到，但是不能做到js提前加载-->
    <!--2.8 条件注释：网页的任何地方  根据浏览器版本去加载内容（html标签）-->
    <!--2.9 固定语法  lt 小于  gt 大于  lte 小于等于  gte 大于等于 -->
<!--[if lt IE 9]>
<h1>您的版本浏览器太低，请升级</h1>
<![endif]-->
</body>
```

# H5C3

## H5获取DOM元素的方式

```html
<!--H5 api
1. querySelector()       获取符合选择器的第一个元素
2. querySelectorAll()    获取符合选择器的所有元素
-->
<!--<script>
console.log($('li:eq(2)').html());
</script>-->
<script>
    /*'Document': 'li:eq(2)' is not a valid selector. 是无效选择器 */
    /* H5 api有效选择器:在css当中能使用的都是有效选择器*/
    /* jquery的扩展选择器  :eq :last :first ....  */
    console.log(document.querySelector('li:last-child').innerHTML);
</script>
```

## 类操作

```html
<button>切换类</button>
<!--
1. jquery操作类的方法：addClass() removeClass() toggleClass() hasClass()
2. h5也有类似的api 基于一个对象classList的方法 add() remove() toggle() contains()
-->
<script>
var dom = document.querySelector('li:nth-child(2)');
/*1.获取类*/
console.log(dom.classList);
/* DOM.classList  获取的是这个DOM元素上的所有class */
/*2.操作类*/
dom.classList.add('blue');
dom.classList.remove('red');

document.querySelector('button').onclick = function () {
    dom.classList.toggle('toggle');
    console.log(dom.classList.contains('red'));
}
</script>
<!--js原生 事件委托方法-->
<script>
    window.onload = function () {
        /*切换当前样式的效果*/
        document.querySelector('ul').onclick = function (e) {
            /*当前的LI*/
            var currentLi = e.target;
            /*如果已经选中 程序停止*/
            if(currentLi.classList.contains('active')) return false;
            /*如果没有选中*/
            /*之前的去除*/
           document.querySelector('li.active').classList.remove('active');
            /*在给当前的加上*/
            currentLi.classList.add('active');
        }
    }
</script>
```

## 自定义属性

```html
<!--
1.自定义属性  data-* 自定义属性
2.获取元素的自定义属性  jquery获取方式  $('div').data('自定义属性的名称')
3.自定义属性的名称是什么？ data-user==>user data-user-name==>userName
4.命名规则 遵循驼峰命名法
5.获取元素的自定义属性 h5的方式  dataset  自定义属性的集合
-->
<div data-user="wl" data-user-age="18"></div>
<script>
    var div = document.querySelector('div');
    /*获取所有  $('div').data() */
    var set = div.dataset;
    /*获取单个自定义属性  $('div').data('user')*/
    /*var user = set.user;
    var user = set['user'];*/

    /*设置单个属性  $('div').data('user','gg')*/
    set.user = 'gg';

    /*jquery的data() 操作内存*/
    /*H5的dataset     操作dom*/
</script>

<!-- css序号选择器伪类选择器  E:first-child-->
<!-- 查找顺序：
通过E确定父元素
通过父元素找到所有的子元素
再去找第一个子元素
找到第一个子元素之后再去匹配类型是不是E  不是：无效选择器
ul:last-child
-->
<ul></ul>
<ul></ul>
<script></script>
<style>
	ul:last-child{}/*找不到，因为最后一个是script*/
</style> 
```

## 全屏方法

```javascript
document.querySelector('.btn1').onclick = function () {
    /*全屏操作*/
    /*每一个元素都有全屏方法*/
    /*H5的api存在兼容问题*/
    /*在IE9以下不支持  加也没有用*/
    /*但是在高级浏览器新版本支持，但是需要加上私有前缀*/
    /*私有前缀：css (-webkit-,-moz-,-ms-,-o-) */
    /*在js当中也有私有前缀  在方法属性之前加上就可以  首字母大小*/
    //document.querySelector('video').webkitRequestFullScreen();

    /*页面文档全屏*/
    //document.querySelector('body').webkitRequestFullScreen();
    document.documentElement.webkitRequestFullScreen();
    //document.querySelector('.box').webkitRequestFullScreen();
}
document.querySelector('.btn2').onclick = function () {
    /*取消全屏   跟元素没有关系 统一使用一下方法*/
    document.webkitCancelFullScreen();
}
```

详细转H5C3_Notes.md

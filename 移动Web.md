## 流式布局：

就是百分比布局，非固定像素，内容向两侧填充，理解成流动的布局，称为流式布局

## 视觉窗口：

viewport,是移动端特有。这是一个虚拟的区域，承载网页的。
  ​        承载关系：浏览器---->viewport---->网页

## 适配设置：

```text
如果任何设置都没有，默认走的就是viewport的默认设置
              去设置新的viewport设置,达到适配要求。
              <meta name="viewport"> 设置视口的标签  在head里面并且应该紧接着编码设置
viewport的功能：
    1. width    可以设置宽度   (device-width 当前设备的宽度)
    2. height   可以设置高度
    3. initial-scale  可以设置默认的缩放比例
    4. user-scalable  可以设置是否允许用户自行缩放
    5. maximum-scale  可以设置最大缩放比例
    6. minimum-scale  可以设置最小缩放比例
在<meta name="viewport" content="" >  content="" 使用以上参数
    1. width=device-width   宽度一致比例是1.0
    2. initial-scale=1.0    宽度一致比例是1.0
    3. user-scalable=no     不允许用户自行缩放  （yes，no  1,0）
标准适配方案：
	<meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=0">
	meta:vp + tab  快捷方式     
```

## js库

> 1.可以使用jquery,但是不建议
> 2.jquery  做了很多桌面浏览器的兼容问题 特别是IE，但是移动端没有IE浏览器
> 3.主流的浏览器：谷歌 火狐（2016年停止了维护和更新） safari浏览器  百度  360 qq ...
> 4.特点：内核基本上都是  webkit  或者 blink  兼容  -webkit-
> 5.使用H5的api 或者说使用一个 叫做： zepto.js 的库（基于高版本浏览器开发）

## base.css

```css
/*=======reset css========*/
*,
*::before,
*::after{
    /*所有的标签，和伪元素都选中*/
    margin: 0;
    padding: 0;
    /*移动端常用布局是非固定像素*/
    box-sizing: border-box;
    -webkit-box-sizing: border-box;
    /*点击高亮效果的清除*/
    tap-highlight-color: transparent;
    -webkit-tap-highlight-color: transparent;
}
body{
    font-size: 14px;
    font-family: "Microsoft YaHei",sans-serif;
    color: #333;
}
ul,ol{
    list-style: none;
}
a{
    text-decoration: none;
    color: #333;
}
input,textarea{
    border: none;
    outline: none;
    /*不允许改变尺寸*/
    resize: none;
    /*元素的外观  none没有任何样式*/
    -webkit-appearance: none;
}
/*=======common css========*/
.f_left{
    float: left;
}
.f_right{
    float: right;
}
.clearFix::before,
.clearFix::after{
    content: "";
    display: block;
    visibility: hidden;
    height: 0;
    line-height: 0;
    clear: both;
}
```

## 触摸事件 touch

```text
解释touch:
1. touch是移动端的触摸事件 而且是一组事件
2. touchstart   当手指触摸屏幕的时候触发
3. touchmove    当手指在屏幕来回的滑动时候触发
4. touchend     当手指离开屏幕的时候触发
5. touchcancel  当被迫终止滑动的时候触发（来电，弹消息）
6. 利用touch相关事件实现移动端常见滑动效果和移动端常见的手势事件

使用touch:
1.绑定事件：box.addEventListener('touchstart',function () { });
2.事件对象：
名字：TouchList------触摸点（一个手指触摸就是一个触发点，和屏幕的接触点的个数）的集合
changedTouches    改变后的触摸点集合
targetTouches     当前元素的触发点集合
touches           页面上所有触发点集合
3.触摸点集合在每个事件触发的时候会不会去记录触摸
changedTouches 每个事件都会记录
targetTouches，touches 在离开屏幕的时候无法记录触摸点
4.分析滑动实现的原理：
4.1 就是让触摸的元素随着手指的滑动做位置的改变
4.2 位置的改变：需要当前手指的坐标
4.3 在每一个触摸点中会记录当前触摸点的坐标 e.touches[0] 第一个触摸点
4.4 clientX clientY      基于浏览器窗口（视口）
4.4 pageX   pageY        基于页面（视口）
4.4 screenX screenY      基于屏幕
```

### 手势事件 --- 基于 touch 事件

```javascript
/*1. 理解移动端的手势事件*/
/*2. swipe swipeLeft  swipeRight swipeUp swipeDown */
/*3. 左滑和右滑手势怎么实现*/
var bindSwipeEvent = function (dom,leftCallback,rightCallback) {
    /*手势的条件*/
    /*1.必须滑动过*/
    /*2.滑动的距离50px*/
    var isMove = false;
    var startX = 0;
    var distanceX = 0;
    dom.addEventListener('touchstart',function (e) {
        startX = e.touches[0].clientX;
    });
    dom.addEventListener('touchmove',function (e) {
        isMove = true;
        var moveX = e.touches[0].clientX;
        distanceX = moveX - startX;
    });
    dom.addEventListener('touchend',function (e) {
        /*滑动结束*/
        if(isMove && Math.abs(distanceX) > 50){
            if(distanceX > 0){
                rightCallback && rightCallback.call(this,e);
            }else{
                leftCallback && leftCallback.call(this,e);
            }
        }
        /*重置参数*/
        isMove = false;
        startX = 0;
        distanceX = 0;
    });
}
bindSwipeEvent(document.querySelector('.box'),function (e) {
    console.log(this);
    console.log(e);
    console.log('左滑手势');
},function (e) {
    console.log(this);
    console.log(e);
    console.log('右滑手势');
});
```

### tap 事件 --- 基于 touch 事件



```html
<!--
1. tap事件  轻击 轻触  （响应速度快）
2. 移动端也有click事件 （在移动为了区分是滑动还是点击，click点击延时300ms）
3. 影响用户体验 响应太慢了。
4. 解决方案：
4.1 使用tap事件（不是移动端原生事件，通过touch相关事件衍生过来） （zepto.js tap事件）了解其原理
4.2 使用一个叫：fastclick.js 提高移动端click响应速度的
4.2.1 下载：https://cdn.bootcss.com/fastclick/1.0.6/fastclick.min.js
4.2.2 使用：
-->
<script>
    /*当页面的dom元素加载完成*/
    document.addEventListener('DOMContentLoaded', function() {
        /*初始化方法*/
        FastClick.attach(document.body);
    }, false);
    /*正常使用click事件就可以了*/
</script>
<script>
    /*使用tap事件*/
    /*1. 响应的速度比click要快   150ms */
    /*2. 不能滑动*/
    var bindTapEvent = function (dom, callback) {
        /*事件的执行顺序*/
        /*在谷歌浏览器模拟看不到300ms的效果*/
        /*在真机上面才能看看到延时效果*/
        var startTime = 0;
        var isMove = false;
        dom.addEventListener('touchstart', function () {
            //console.log('touchstart');
            startTime = Date.now();
            /*Date.now();*/
        });
        dom.addEventListener('touchmove', function () {
            //console.log('touchmove');
            isMove = true;
        });
        dom.addEventListener('touchend', function (e) {
            //console.log('touchend');
            console.log((Date.now() - startTime));
            if ((Date.now() - startTime) < 150 && !isMove) {
                callback && callback.call(this, e);
            }

            startTime = 0;
            isMove = false;
        });
        /*dom.addEventListener('click',function () {
             //console.log('click');
             });*/
    }

    bindTapEvent(document.querySelector('.box'), function (e) {
        console.log(this);
        console.log(e);
        console.log('tap事件')
    });
</script>
```

iscroll  区域滚动插件

超小屏  <768px
小屏 768px-992px
中屏 992px-1200px
宽屏  >1200px

## normalize.css 与 reset.css 的区别

```html
<!--
共同点：都是重置样式库，增强浏览器的表现一致性
不同点：
举个例子：ul
reset.css   list-style:none ---因为需求
normalize.css 不会重置ul样式 ---本身已经在每个浏览器表现一致的元素

一句话：都是增强浏览器的表现一致性但是normalize不会重置已经一致的元素
-->
```








day 5-1

移动Web：6
vue2.0：5
reactjs：2
$$
6+5+2=13
$$


bootstrap 小程序 学习

webpack 从入门到

项目！！！

sass

typescript
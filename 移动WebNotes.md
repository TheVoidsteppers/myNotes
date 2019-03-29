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





day2

移动Web：6
vue2.0：5
reactjs：2
$$
6+5+2=13
$$
全局变量可以在不同的script标签中使用吗？可以
var 创建多个同名变量是覆盖，还是新创建？不是，值类型的存储在栈，对象才会同名多次创建
php的局部变量也是存储在数据段全局区吗；是

应看到--- 12/28 vuejs 6

bootstrap 小程序 学习

项目！！！
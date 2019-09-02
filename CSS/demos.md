CSS3的animation制作的动画效果的CSS集合：https://daneden.github.io/animate.css/

## 去掉图片下间隙

```css
/* 图片默认是文字，以基线对齐 */
body{
    /*font-size: 0px;*/
}
img{
    /*display: block;*/
    vertical-align: middle;
}
```

## 自动换行

```css
word-wrap:break-word;
word-break:break-all;
/* 两行 */
overflow : hidden;
text-overflow: ellipsis;
display: -webkit-box;
-webkit-line-clamp: 2;
-webkit-box-orient: vertical;
```

## 模仿 placeholder

```html
<div contenteditable="true" data-placeholder="(untitled)" class="title">
</div>
```

```css
.title:empty:not(:focus):before {
  content: attr(data-placeholder)
}
```

## 去掉已设置的样式

```javascript
initial | inherit 
```

## 超出文字 省略号

```css
overflow:hidden;
text-overflow:ellipsis;
white-space:nowrap;
```

## 文字渐变色

```css
background: linear-gradient(to right, #ff338e, #f83e3e);
-webkit-background-clip: text;
color: transparent;
```

## flex 布局

```html
<div class="box"></div>
```

```css
.box{
  display: flex;
  display: inline-flex; /* 行内元素 */
  display: -webkit-flex; /* Safari */
}
```

## 滚动条美化

```css
/*滚动条美化*/
::-webkit-scrollbar-track-piece {
    /*滚动条凹槽的颜色，还可以设置边框属性*/
    background-color:#f8f8f8;
}
::-webkit-scrollbar {
    /*滚动条的宽度*/
    width:9px;
    height:9px;
}
::-webkit-scrollbar-thumb {
    /*滚动条的设置*/
    background-color:#dddddd;
    background-clip:padding-box;
    min-height:28px;
}
::-webkit-scrollbar-thumb:hover {
    background-color:#bbb;
}
/*滚动条美化 end*/
```

## 背景图片 --- 显示位置、裁剪

```css
/*让背景从内容开始平铺*/
background-origin: content-box;
/*没有做背景裁剪 背景图默认就是从边框显示*/
/*默认的就是
border-box  边框以外被裁剪掉
padding-box 内边距以外被裁剪掉
content-box 内容以外被裁剪掉
*/
background-clip: content-box;
```

## 添加字体

```css
/*自定义字体图标*/
/*1.通过@font-face定义自己的字体*/
@font-face {
    /*2.申明自己的字体名称*/
    font-family: 'wjs';
    /*3.引入字体文件（约束某一段字符代码什么图案）*/
    src:
    url(../fonts/MiFie-Web-Font.svg) format('svg'),
    url(../fonts/MiFie-Web-Font.eot) format('embedded-opentype'),
    url(../fonts/MiFie-Web-Font.ttf) format('truetype'),
    url(../fonts/MiFie-Web-Font.woff) format('woff');
}
/*4.怎么使用维护性更好*/
.wjs_icon{
    font-family: wjs;
}
```

## 文本环绕

```html
<style>
    .box1{
        float: left;
        width: 100px;
        height: 100px;
        background: pink;
    }
    .box2{
        /*让这个元素绝对绝缘  bfc*/
        /*不让其他浮动元素影响自己*/
        /*不让自己的浮动去影响别的元素*/
        overflow: hidden;
    }
</style>
<body>
    <div class="box1"></div>
    <div class="box2">
    	内容内容内容内容内容内容内容内容内容内容内容内容
    </div>    
</body>
<!-- 
|￣￣|内容内容内容
|____|内容内容内容
内容内容内容内容内容内容
-->
```

## 媒体查询

```css
/*使用媒体查询能针对不同屏幕区间设置不同的布局和样式*/
/*怎么使用媒体查询：关于媒体查询 @media */
/*语法： @media screen and (max-width: 768px) and (min-width: 320px){属性样式}*/
@media screen and (max-width: 768px) {
    /*1. 在超小屏设备的时候 768px以下      当前容器的宽度100%     背景蓝色*/
    .container{
        width: 100%;
        background: blue;
    }
}
```

## 三角形

```css
div::before{
    width: 0;
    height: 0;
    content: "";
    position: absolute;
    left: 85px;
    top: -10px;
    border-left: solid 10px transparent;
    border-bottom: solid 10px rgba(255,255,255,.9);
    border-right: solid 10px transparent;
}
```

## 去掉鼠标选中文本效果

```css
-webkit-touch-callout: none; /* iOS Safari */
-webkit-user-select: none;/* Chrome/Safari/Opera */
-khtml-user-select: none; /* Konqueror */
-moz-user-select: none; /* Firefox */
-ms-user-select: none; /* Internet Explorer/Edge */
user-select: none; /* Non-prefixed version, currently not supported by any browser */
```


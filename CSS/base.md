## box-shadow

```
box-shadow: h-shadow v-shadow blur spread color inset;
参数详解：
h-shadow: 阴影的水平偏移量。
v-shadow: 阴影的垂直偏移量。
blur: 模糊距离(就是渐变的距离，设为0就没有渐变)。
spread: 投影的尺寸，通过这个控制“影分身”的大小。
color: 投影颜色，通过这个实现后方的乌云。
inset: 改为内阴影。这里用不到。
```

## 选择器

### id选择器

### 类名选择器

### 伪类选择器

```css
/*
	找到P元素，通过P找到父元素，通过父元素找子元素当中类型为P的，然再去找第几个。
    p:first-of-type
    p:last-of-type
    p:nth-of-type(n)
    p:nth-last-of-type(n)
    å¦‚æžœä½¿ç”¨çš„æ˜¯child;
    p:first-child
    找到P元素,通过P找到父元素，通过父元素找所有的子元素，找第一个元素，匹配判断类型(如果不是无效选择器)
    */
```

## clip-path

可以创建一个只有元素的部分区域可以显示的剪切区域。区域内的部分显示，区域外的隐藏

```css
/* Keyword values */
clip-path: none;

/* <clip-source> values */ 
clip-path: url(resources.svg#c1);

/* <geometry-box> values */
/* 如果同 <basic-shape> 一起声明，它将为基本形状提供相应的参考框盒。通过自定义，它将利用确定的盒子边缘包括任何形状边角（比如说，被 border-radius 定义的剪切路径）*/
clip-path: margin-box;
clip-path: border-box;
clip-path: padding-box;
clip-path: content-box;
clip-path: fill-box;
clip-path: stroke-box;
clip-path: view-box;

/* <basic-shape> values */
clip-path: inset(100px 50px);
/*矩形
( <length-percentage>{1,4} [ round <'border-radius'> ]? )
上 右 下 左 */
clip-path: circle(50px at 0 100px);
/*椭圆
( [ <shape-radius> ]? [ at <position> ]? )*/
clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
/*多边形
( <fill-rule>? , [ <length-percentage> <length-percentage> ]# )*/
clip-path: path('M0.5,1 C0.5,1,0,0.7,0,0.3 A0.25,0.25,1,1,1,0.5,0.3 A0.25,0.25,1,1,1,1,0.3 C1,0.7,0.5,1,0.5,1 Z');

/* Box and shape values combined */
clip-path: padding-box circle(50px at 0 100px);

/* Global values */
clip-path: inherit;
clip-path: initial;
clip-path: unset;
```

## filter

将模糊或颜色偏移等图形效果应用于元素。滤镜通常用于调整图像，背景和边框的渲染

```css
/* URL to SVG filter */
/*接受一个XML文件，该文件设置了 一个SVG滤镜，且可以包含一个锚点来指定一个具体的滤镜元素*/
filter: url("filters.svg#filter-id");
/*==============================================*/
/* <filter-function> values */
filter: blur(5px);/*( <length> )*/
/*给图像设置高斯模糊。“length”一值设定高斯函数的标准差，或者是屏幕上以多少像素融在一起，所以值越大越模糊；则默认是0；*/

filter: brightness(0.4);/*( <number-percentage> )*/
/*给图片应用一种线性乘法，使其看起来更亮或更暗。如果值是0%，图像会全黑。值是100%，则图像无变化。其他的值对应线性乘数效果。值超过100%也是可以的，图像会比原来更亮。默认是1。*/

filter: contrast(200%);/*( [ <number-percentage> ] )*/
/*调整图像的对比度。0%的话，图像会全黑。100%，图像不变。可以超过100%，意味着会运用更低的对比。默认是1。*/

filter: drop-shadow(16px 16px 20px blue);/*( <length>{2,3} <color>? )*/
/*给图像设置一个阴影效果。阴影是合成在图像下面，可以有模糊度的，可以以特定颜色画出的遮罩图的偏移版本。 函数接受<shadow>（在CSS3背景中定义）类型的值，除了“inset”关键字是不允许的。该函数与已有的 box-shadow 属性很相似；不同之处在于，通过滤镜，一些浏览器为了更好的性能会提供硬件加速。*/

filter: grayscale(50%);/*( <number-percentage> )*/
/*将图像转换为灰度图像。值定义转换的比例。值为100%则完全转为灰度图像，值为0%图像无变化。值在0%到100%之间，则是效果的线性乘子。值默认是0。*/

filter: hue-rotate(90deg);/*( <angle> )*/
/*给图像应用色相旋转。“angle”一值设定图像会被调整的色环角度值。值为0deg，则图像无变化。若值未设置，默认值是0deg。该值虽然没有最大值，超过360deg的值相当于又绕一圈。*/

filter: invert(75%);/*( <number-percentage> )*/
/*反转输入图像。值定义转换的比例。100%的价值是完全反转。值为0%则图像无变化。值在0%和100%之间，则是效果的线性乘子。 值默认是0。*/

filter: opacity(25%);/*( <number-percentage> )*/
/*转化图像的透明程度。值定义转换的比例。值为0%则是完全透明，值为100%则图像无变化。值在0%和100%之间，则是效果的线性乘子，也相当于图像样本乘以数量。 值默认是1。该函数与已有的opacity属性很相似，不同之处在于通过filter，一些浏览器为了提升性能会提供硬件加速。*/

filter: saturate(30%);/*( <number-percentage> )*/
/*转换图像饱和度。值定义转换的比例。值为0%则是完全不饱和，值为100%则图像无变化。其他值，则是效果的线性乘子。超过100%的值是允许的，则有更高的饱和度。值默认是1。*/

filter: sepia(60%);/*( <number-percentage> )*/
/*将图像转换为深褐色。值定义转换的比例。值为100%则完全是深褐色的，值为0%图像无变化。值在0%到100%之间，则是效果的线性乘子。值默认是0。*/
/*==============================================*/
/* Multiple filters */
filter: contrast(175%) brightness(3%);
/*==============================================*/
/* Global values */
filter: inherit;
filter: initial;
filter: unset;
```


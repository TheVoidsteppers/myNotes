CSS3的animation制作的动画效果的CSS集合：https://daneden.github.io/animate.css/

## 图片下间隙

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

## 设置滚动条的样式

```css

```

### box-shadow

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


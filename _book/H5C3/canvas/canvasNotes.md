# canvas

## 线

### 初识canvas

```html
<style>
    canvas{
        border: 1px solid #ccc;
        /*不建议在 样式设置尺寸,更改宽高会变成缩放画布*/
        /*width: 600px;
        height: 400px;*/
    }
</style>
<!--1.准备画布-->
<!--1.1 画布是白色的 而且默认300*150-->
<!--1.2 设置画布的大小  width="600" height="400" -->
<canvas width="600" height="400" ></canvas>
<!--2.准备绘制工具-->
<!--3.利用工具绘图-->
<script>
    /*1.获取元素*/
    var myCanvas = document.querySelector('canvas');
    /*2.获取上下文 绘制工具箱 */
    /*是否有3d 暂时没有*/
    var ctx = myCanvas.getContext('2d'); /*web gl 绘制3d效果的网页技术*/
    /*3.移动画笔*/
    ctx.moveTo(100,100.5);
    /*4.绘制直线 (轨迹，绘制路径)*/
    ctx.lineTo(200,100.5);
    /*5.描边*/
    ctx.stroke();
    /*关于线条的问题*/
    /*1.默认的宽度是多少   1px*/
    /*2.默认的颜色是什么   黑色*/
    /*产生原因  对齐的点是线的中心位置  会把线分成两个0.5px 显示的是会不饱和（灰色）浏览器最小显示单位1px（增加宽度）*/
    /*解决方案  上下（y轴）左右（x轴）移动0.5px */
</script>
```

### 样式冲突问题

```javascript
/*画平行线*/
ctx.beginPath();/*开启新路径*/
/*蓝色  10px*/
ctx.moveTo(100,100);
ctx.lineTo(300,100);
ctx.strokeStyle = 'blue';
ctx.lineWidth = 10;
/*描边*/
ctx.stroke();

ctx.beginPath();/*开启新路径*/
/*红色 20px*/
ctx.moveTo(100,200);
ctx.lineTo(300,200);
ctx.strokeStyle = 'red';
ctx.lineWidth = 20;
/*描边*/
ctx.stroke();

/*开启新路径可以解决样式覆盖问题*/
//注意每次开启新路径都得将上一个描边
```

### 填充

```javascript
/*描边 ctx.stroke();*/
//填充
ctx.fill();
```

### 关闭路径

```javascript
/*1.绘制一个三角形*/
ctx.moveTo(100,100);
ctx.lineTo(200,100);
ctx.lineTo(200,200);
/*起始点和lineTo的结束点无法完全闭合缺角*/
/*使用canvas的自动闭合 */
//ctx.lineTo(100,100);
/*关闭路径 自动闭合图形*/
ctx.closePath();//与beginPath无关系

ctx.lineWidth = 10;//大线宽下，由于线宽关于轨迹对称，出现缺角
ctx.stroke();
```

### 填充规则：非零环绕规则

```javascript
/*1.绘制两个正方形 一大200一小100 套在一起*/
ctx.moveTo(100,100);
ctx.lineTo(300,100);
ctx.lineTo(300,300);
ctx.lineTo(100,300);
ctx.closePath();

ctx.moveTo(150,150);
ctx.lineTo(150,250);
ctx.lineTo(250,250);
ctx.lineTo(250,150);
ctx.closePath();

/*2.去填充*/
ctx.fillStyle = 'red';//设置填充的颜色
ctx.fill();

/*在填充的时候回遵循非零环绕规则*/
/*非零环绕规则
判断一个区域是否填充
1、从这个区域拉一条直线
2、看这条直线相交的轨迹
3、轨迹为 顺时针+1,逆时针-1
4、计算所有的轨迹之和
5、得到0则不填充
*/
```

### 线条样式设置

```javascript
- lineWidth 线宽，默认1px
- lineCap 线末端类型：(butt默认)、round、square 
- lineJoin 相交线的拐点 miter(默认)、round、bevel
- strokeStyle 线的颜色
- fillStyle 填充颜色
- setLineDash() 设置虚线 
//setLineDash([5,10,15])实线5px，虚线10px，每15px重复实虚线 实虚相同为[5]
- getLineDash() 获取虚线宽度集合
- lineDashOffset 设置虚线偏移量（负值向右偏移）
```

### 图形绘制API

```javascript
- rect(x,y,w,h) 没有独立路径
- strokeRect(x,y,w,h) 有独立路径，不影响别的绘制
- fillRect(x,y,w,h) 有独立路径，不影响别的绘制
- clearRect(x,y,w,h) 擦除矩形区域
```

### 颜色渐变

```javascript
var myCanvas = document.querySelector('canvas');
var ctx = myCanvas.getContext('2d');

//法1、线是由点构成的
ctx.lineWidth = 30;
for (var i = 0; i < 255; i++) {
    ctx.beginPath();
    ctx.moveTo(100+i-1,100);
    ctx.lineTo(100+i,100);
    ctx.strokeStyle = 'rgb('+i+',0,0)';
    ctx.stroke();
}
//法2、API
/*fillStyle 'pink' '#000' 'rgb()' 'rgba()' */
/*也可以使用一个渐变的方案了填充矩形*/
/*创建一个渐变的方案*/
/*渐变是由长度的*/
/*x0y0 起始点 x1y1 结束点  确定长度和方向*/
var linearGradient = ctx.createLinearGradient(100,100,500,400);
linearGradient.addColorStop(0,'pink');
//linearGradient.addColorStop(0.5,'red');
linearGradient.addColorStop(1,'blue');

ctx.fillStyle = linearGradient;

ctx.fillRect(100,100,400,100);

/*pink---->blue*/
/*回想线性渐变---->要素 方向  起始颜色 结束颜色 */
/*通过两个点的坐标可以控制 渐变方向*/
```

## 弧

```javascript
var myCanvas = document.querySelector('canvas');
var ctx = myCanvas.getContext('2d');

/*绘制圆弧*/
/*确定圆心  坐标 x y*/
/*确定圆半径  r */
/*确定起始绘制的位置和结束绘制的位置  确定弧的长度和位置  startAngle endAngle   弧度*/
/*取得绘制的方向 direction 默认是顺时针 false 逆时针 true */

/*在中心位置画一个半径150px的圆弧左下角*/
var w = ctx.canvas.width;
var h = ctx.canvas.height;

/*把起点放到圆心位置，非必须，用于绘制扇形*/
ctx.moveTo(w/2,h/2);

ctx.arc(w/2,h/2,150,Math.PI/2,Math.PI,true);

/*闭合路径，需将起点放到圆心位置*/
ctx.closePath();

ctx.fill();
```

### 文字

```javascript
var myCanvas = document.querySelector('canvas');
var ctx = myCanvas.getContext('2d');

/*1.在画布的中心绘制一段文字*/
/*2.申明一段文字*/
var str = '您吃-,了吗';
/*3.确定画布的中心*/
var w = ctx.canvas.width;
var h = ctx.canvas.height;
/*4.画一个十字架在画布的中心*/
ctx.beginPath();
ctx.moveTo(0, h / 2 - 0.5);
ctx.lineTo(w, h / 2 - 0.5);
ctx.moveTo(w / 2 - 0.5, 0);
ctx.lineTo(w / 2 - 0.5, h);
ctx.strokeStyle = '#eee';
ctx.stroke();
/*5.绘制文本*/
ctx.beginPath();
ctx.strokeStyle = '#000';
var x0 = w/2;
var y0 = h/2;
/*注意：起点位置在文字的左下角*/
/*有文本的属性  尺寸 字体  左右对齐方式  垂直对齐的方式*/
ctx.font = '40px Microsoft YaHei';
/*左右对齐方式 (center left（默认） right start end) 基准起始坐标*/
ctx.textAlign = 'center';
/*垂直对齐的方式 基线 baseline(top,bottom,middle) 基准起始坐标*/
ctx.textBaseline = 'middle';
//ctx.direction = 'rtl';//了解
//ctx.strokeText(str,x0,y0);
ctx.fillText(str,x0,y0);
/*6.画一个下划线和文字一样长*/
ctx.beginPath();
/*获取文本的宽度*/
console.log(ctx.measureText(str));
var width = ctx.measureText(str).width;
ctx.moveTo(x0-width/2,y0 + 20);
ctx.lineTo(x0+width/2,y0 + 20);
ctx.stroke();
```

### 饼状图

```javascript
/*var myCanvas = document.querySelector('canvas');
var ctx = myCanvas.getContext('2d');*/

/*1.绘制饼状态图*/
/*1.1 根据数据绘制一个饼图*/
/*1.2 绘制标题 从扇形的弧中心伸出一条线在画一条横线在横线的上面写上文字标题*/
/*1.3 在画布的左上角 绘制说明 一个和扇形一样颜色的矩形 旁边就是文字说明*/

var PieChart = function (ctx) {
    /*绘制工具*/
    this.ctx = ctx || document.querySelector('canvas').getContext('2d');
    /*绘制饼图的中心*/
    this.w = this.ctx.canvas.width;
    this.h = this.ctx.canvas.height;
    /*圆心*/
    this.x0 = this.w / 2 + 60;
    this.y0 = this.h / 2;
    /*半径*/
    this.radius = 150;
    /*伸出去的线的长度*/
    this.outLine = 20;
    /*说明的矩形大小*/
    this.rectW = 30;
    this.rectH = 16;
    this.space = 20;
}
PieChart.prototype.init = function (data) {
    /*1.准备数据*/
    this.drawPie(data);
};
PieChart.prototype.drawPie = function (data) {
    var that = this;
    /*1.转化弧度*/
    var angleList = this.transformAngle(data);
    /*2.绘制饼图*/
    var startAngle = 0;
    angleList.forEach(function (item, i) {
        /*当前的结束弧度要等于下一次的起始弧度*/
        var endAngle = startAngle + item.angle;
        that.ctx.beginPath();
        that.ctx.moveTo(that.x0, that.y0);
        that.ctx.arc(that.x0, that.y0, that.radius, startAngle, endAngle);
        var color = that.ctx.fillStyle = that.getRandomColor();
        that.ctx.fill();
        /*下一次要使用当前的这一次的结束角度*/
        /*绘制标题*/
        that.drawTitle(startAngle, item.angle, color , item.title);
        /*绘制说明*/
        that.drawDesc(i,item.title);
        startAngle = endAngle;
    });
};
PieChart.prototype.drawTitle = function (startAngle, angle ,color , title) {
    /*1.确定伸出去的线 通过圆心点 通过伸出去的点  确定这个线*/
    /*2.确定伸出去的点 需要确定伸出去的线的长度*/
    /*3.固定伸出去的线的长度*/
    /*4.计算这个点的坐标*/
    /*5.需要根据角度和斜边的长度*/
    /*5.1 使用弧度  当前扇形的起始弧度 + 对应的弧度的一半 */
    /*5.2 半径+伸出去的长度 */
    /*5.3 outX = x0 + cos(angle) * ( r + outLine)*/
    /*5.3 outY = y0 + sin(angle) * ( r + outLine)*/
    /*斜边*/
    var edge = this.radius + this.outLine;
    /*x轴方向的直角边*/
    var edgeX = Math.cos(startAngle + angle / 2) * edge;
    /*y轴方向的直角边*/
    var edgeY = Math.sin(startAngle + angle / 2) * edge;
    /*计算出去的点坐标*/
    var outX = this.x0 + edgeX;
    var outY = this.y0 + edgeY;
    this.ctx.beginPath();
    this.ctx.moveTo(this.x0, this.y0);
    this.ctx.lineTo(outX, outY);
    this.ctx.strokeStyle = color;
    /*画文字和下划线*/
    /*线的方向怎么判断 伸出去的点在X0的左边 线的方向就是左边*/
    /*线的方向怎么判断 伸出去的点在X0的右边 线的方向就是右边*/
    /*结束的点坐标  和文字大小*/
    this.ctx.font = '14px Microsoft YaHei';
    var textWidth = this.ctx.measureText(title).width ;
    if(outX > this.x0){
        /*右*/
        this.ctx.lineTo(outX + textWidth,outY);
        this.ctx.textAlign = 'left';
    }else{
        /*左*/
        this.ctx.lineTo(outX - textWidth,outY);
        this.ctx.textAlign = 'right';
    }
    this.ctx.stroke();
    this.ctx.textBaseline = 'bottom';
    this.ctx.fillText(title,outX,outY);

};
PieChart.prototype.drawDesc = function (index,title) {
    /*绘制说明*/
    /*矩形的大小*/
    /*距离上和左边的间距*/
    /*矩形之间的间距*/
    this.ctx.fillRect(this.space,this.space + index * (this.rectH + 10),this.rectW,this.rectH);
    /*绘制文字*/
    this.ctx.beginPath();
    this.ctx.textAlign = 'left';
    this.ctx.textBaseline = 'top';
    this.ctx.font = '12px Microsoft YaHei';
    this.ctx.fillText(title,this.space + this.rectW + 10 , this.space + index * (this.rectH + 10));
};
PieChart.prototype.transformAngle = function (data) {
    /*返回的数据内容包含弧度的*/
    var total = 0;
    data.forEach(function (item, i) {
        total += item.num;
    });
    /*计算弧度 并且追加到当前的对象内容*/
    data.forEach(function (item, i) {
        var angle = item.num / total * Math.PI * 2;
        item.angle = angle;
    });
    return data;
};
PieChart.prototype.getRandomColor = function () {
    var r = Math.floor(Math.random() * 256);
    var g = Math.floor(Math.random() * 256);
    var b = Math.floor(Math.random() * 256);
    return 'rgb(' + r + ',' + g + ',' + b + ')';
};

var data = [
    {
        title: '15-20岁',
        num: 6
    },
    {
        title: '20-25岁',
        num: 30
    },
    {
        title: '25-30岁',
        num: 10
    },
    {
        title: '30以上',
        num: 8
    }
];

var pieChart = new PieChart();
pieChart.init(data);
```

## 做动画

### 绘制图片

```javascript
/*1.加载图片到内存即可*/
/*var img = document.createElement('img');
    img.src = 'image/01.jpg';*/
/*创建对象*/
var image = new Image();
/*绑定加载完成事件*/
image.onload = function () {
    /*实现图片绘制*/
    console.log(image);
    /*绘制图片的三种方式*/

    /*3参数*/
    /*图片对象*/
    /*绘制在画布上的坐标 x y*/
    //ctx.drawImage(image,100,100);

    /*5个参数*/
    /*图片对象*/
    /*绘制在画布上的坐标 x y*/
    /*是图片的大小  不是裁剪  是缩放*/
    //ctx.drawImage(image,100,100,100,100);

    /*9个参数*/
    /*图片对象*/
    /*图片上定位的坐标  x y */
    /*在图片上截取多大的区域  w h*/
    /*绘制在画布上的坐标 x y*/
    /*是图片的大小  不是裁剪  是缩放*/
    ctx.drawImage(image,400,400,400,400,200,200,100,100);

};
/*设置图片路径*/
image.src = 'image/02.jpg';
```

### 帧动画

```javascript
var Person = function (ctx) {
    /*绘制工具*/
    this.ctx = ctx || document.querySelector('canvas').getContext('2d');
    /*图片路径*/
    this.src = 'image/04.png';
    /*画布的大小*/
    this.canvasWidth = this.ctx.canvas.width;
    this.canvasHeight = this.ctx.canvas.height;

    /*行走相关参数*/
    this.stepSize = 10;
    /* 0 前  1 左  2 右  3 后  和图片的行数包含的图片对应上*/
    this.direction = 0;
    /*x轴方向的偏移步数*/
    this.stepX = 0;
    /*y轴方向的偏移步数*/
    this.stepY = 0;

    /*初始化方法*/
    this.init();
};
Person.prototype.init = function () {
    var that = this;
    /*1.加载图片*/
    this.loadImage(function (image) {
        /*图片的大小*/
        that.imageWidth = image.width;
        that.imageHeight = image.height;
        /*人物的大小*/
        that.personWidth = that.imageWidth / 4;
        that.personHeight = that.imageHeight / 4;
        /*绘制图片的起点*/
        that.x0 = that.canvasWidth / 2 - that.personWidth / 2;
        that.y0 = that.canvasHeight / 2 - that.personHeight / 2;
        /*2.默认绘制在中心位置正面朝外*/
        that.ctx.drawImage(image,
                           0, 0,
                           that.personWidth, that.personHeight,
                           that.x0, that.y0,
                           that.personWidth, that.personHeight);
        /*3.能通过方向键去控制人物行走*/
        that.index = 0;
        document.onkeydown = function (e) {
            if (e.keyCode == 40) {
                /*前*/
                that.direction = 0;
                that.stepY++;
                that.drawImage(image);
            } else if (e.keyCode == 37) {
                /*左*/
                that.direction = 1;
                that.stepX--;
                that.drawImage(image);
            } else if (e.keyCode == 39) {
                /*右*/
                that.direction = 2;
                that.stepX++;
                that.drawImage(image);
            } else if (e.keyCode == 38) {
                /*后*/
                that.direction = 3;
                that.stepY--;
                that.drawImage(image);
            }
        }
    });
}
/*加载图片*/
Person.prototype.loadImage = function (callback) {
    var image = new Image();
    image.onload = function () {
        callback && callback(image);
    };
    image.src = this.src;
};
/*绘制图片*/
Person.prototype.drawImage = function (image) {
    this.index++;
    /*清除画布*/
    this.ctx.clearRect(0, 0, this.canvasWidth, this.canvasHeight);
    /*绘图*/
    /*在精灵图上的定位 x  索引*/
    /*在精灵图上的定位 y  方向*/
    this.ctx.drawImage(image,
                       this.index * this.personWidth, this.direction * this.personHeight,
                       this.personWidth, this.personHeight,
                       this.x0 + this.stepX * this.stepSize, this.y0 + this.stepY * this.stepSize,
                       this.personWidth, this.personHeight);
    /*如果索引超出了 变成0*/
    if (this.index >= 3) {
        this.index = 0;
    }
};
new Person();
```

### 坐标变换

```javascript
var myCanvas = document.querySelector('canvas');
var ctx = myCanvas.getContext('2d');
//ctx.translate(100,100);移动画布的坐标原点
//ctx.scale(0.5,1);缩放画布的XY轴
//ctx.rotate(Math.PI/6);旋转画布的坐标轴
var startAngle = 0;
ctx.translate(150,150);
setInterval(function () {
    startAngle += Math.PI/180;
    ctx.rotate(startAngle);
    ctx.strokeRect(-50,-50,100,100);
},500);
```

详细方法看canvas.md
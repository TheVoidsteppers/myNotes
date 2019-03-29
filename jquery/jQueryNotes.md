## jQuery介绍

jQuery：就是JavaScript库中的一种，免费的

JavaScript库：把一些浏览器兼容性的代码或者是常用的函数封装在一个js文件中，这个文件就是一个JavaScript库也可以叫JavaScript函数库，如Prototype、YUI（网络反响一般）、Dojo、ExtJS、jQuery等

jQuery中的顶级对象：jQuery----可以用$符号来代替，为了方便，jQuery（jQuery的js文件中的所有东西都是jQuery或者$符号下的的）

## DOM对象与jQuery对象

通过DOM方式获取的-------DOM对象
通过jQuery或$方式获取的-----jQuery对象，不是同一个对象

DOM对象 转 jQuery对象：$(DOM对象);
jQuery对象 转 DOM对象：jQuery对象[0];  或者jQuery对象.get(0);

注意：
.css("属性","值");属性的写法可以是DOM操作中的js写法，也可以是css中的写法，建议js的写法
this是DOM对象，需$(this)转成jQuery

## 页面加载事件

```javascript
//DOM的写法，只能同时存在一个，多个只执行最后一个
window.onload = function() {
    console.log("hello");
  };
//下面的可以同时存在多个
//jQuery的第一种写法
$(window).load(
    function() {
        console.log("hello");
    }
);
//jQuery的第二种写法---比上面快---页面中基本的元素加载后就触发
$(document).ready(function(){
    console.log("hello");
});
//jQuery的第三种写法---跟上面的一样---页面中基本的元素加载后就触发
$(function(){
    console.log("hello");//要执行的代码，上面的function不能省略
});//或者$用jQuery代替
```

## jQuery中获取元素

```javascript
$("选择器");/*
$("#btn");id	
$(".dv");类样式	
$("p");标签名	
$("p.cls");交集选择器	
$("#btn,p,.cls");并集选择器
以下是后代（层次）选择器
$("#dv span");获取id为dv的所有子代span <==> $("#dv").find("span);
$("#dv>span");获取id为dv的直接子代span
$("#dv~span");获取id为dv后面的所有兄弟span
$("#dv+span");获取id为dv后面的直接（相邻）兄弟span
$("#dv").siblings("span");查找兄弟节点，不包括自身
$("#dv").next();找下一个兄弟;获取所有nextAll();会断链
$("#dv").prev();找上一个兄弟;获取所有prevAll();会断链
$("#dv").parent();查找父类
$("#dv").children("span");查找子类
$("#uu>li").eq(2);索引为2的li <==>$("li:eq(2)")
*/
```

## jQuery中获取/设置表单标签的value

```html
jQuery对象.val();//---------表示的是获取该元素的value属性值
jQuery对象.val("值");//---------表示的是设置该元素的value属性值
<select id="s1">
  <option value="1">hello</option>
</select>
<script>
$("s1").val(1);//默认下拉菜单value为1的选中，只有这个会，单选和复选是该value值
</script>
```

## 数组操作

```javascript
//html5新属性
//判断数组内是否含有某值
var arr = []
var id = '3'
arr.includes(id)
```



## jQuery获取/设置文本内容

```javascript
jQuery对象.text();//-------获取文本内容，等价与DOM中的innerText
jQuery对象.text("值");//--------设置该元素的文本内容
$("p").text("hello");//若p有多个，不需循环
//本身代码没有循环操作，jQuery中内部帮助我们循环操作的------>隐式迭代
```

## jQuery设置css样式

```javascript
jQuery对象.css("css的属性名","属性值");//-----------设置元素的样式属性值
//括号内也可以传入json格式的数据
//要获取某css样式，可以调用以下方法
jQuery对象.css("css的属性名");//获取的是字符串类型
//注意css()方法不能移除也不能添加类样式
```

## jQuery获取/设置标签中的html内容

```javascript
$("div").html("<p>this is a p</p>");//相当与DOM中的innerHTML,相当于赋值，会把原来的覆盖掉
```

## 选择奇数偶数子元素

```javascript
$("#dv>li:odd").css("width","100px");//奇数---odd
$("#dv>li:even").css("width","100px");//偶数---even
```

## 鼠标进入和离开

```javascript
$(".dv>ul>li").mouseenter(function (){
    $(this).children("ul").stop().show();//当前的li中所有的ul子元素显示，show里可以添加时间，show(1000),用1秒显示;添加stop(),执行结束才执行下一次。
});//相当与DOM的onmouseover，但比mouseover好
$(".dv>ul").mouseleave(function (){
    $(this).children("ul").hide();//当前的li中所有的ul子元素隐藏
});//相当与DOM的onmouseout，但比mouseout好
```

## 索引

```javascript
var index=$(this).index();//当前元素的索引
$("#dv>li:eq("+index+")")//:eq---选择器---获取对应索引的元素
```

## 链式编程

对象不停的调用方法

对象调用方法，如果返回值还是当前对象，那么就可以继续调用方法
经验：在jQuery中，如果方法是设置的意思，返回的一般都是当前对象，就可以继续链式编程，获取值则一般不能继续

断链：对象调用方法之后，返回的已经不是当前的这个对象了，此时再调用方法就出现断链
解决方法：在断链的方法后面加.end()方法，此时不推荐使用链式编程，影响效率

## 类样式

```javascript
$("#dv").addClass("cls");//为id为dv的元素添加cls类样式
$("#dv").addClass("cls1").addClass("cls2");//为id为dv的元素添加多个cls类样式
$("#dv").addClass("cls1 cls2");//与上面效果的相同
$("#dv").addClass();//返回的是该对象
//移除类样式
$("#dv").removeClass("cls");//移除多个的用法与上面的同
$("#dv").removeClass();//返回的是该对象
//是否应用了类样式
$("#dv").hasClass("cls cls2");
//切换类样式
$("body").toggleClass("cls");//切换，事件触发，自动为body添加或删除cls。有则删，无则加
```

## 显示与隐藏

```javascript
jQuery对象.show();//显示;参数1：毫秒;参数2：回调函数----动画执行完毕后执行
jQuery对象.hide();//隐藏;参数同上
//
jQuery对象.slideDown();//下滑，显示;参数同上，效果类似卷帘门
jQuery对象.slideUp();//上滑，隐藏;参数同上
jQuery对象.slideToggle();//切换上滑和下滑，上两个的合并;参数同上
//
jQuery对象.fadeIn();//淡入，显示;参数同上，大小不变，渐显
jQuery对象.fadeOut();//淡出，隐藏;参数同上
jQuery对象.fadeToggle();//切换淡入和淡出，上两个的合并;参数同上
jQuery对象.fadeTo();//更改透明度;参数1：时间毫秒(不可省)，参数2：透明度
```

## 动画

```javascript
jQuery对象.animate();/*
参数1：css属性和值，json格式数据{"width":"100px","height":"200px"}
参数2：时间（毫秒）
参数3：回调函数（可省）
*/
```

## 元素的创建及添加

```javascript
//jQuery父级元素.append(jQuery子级元素);
$("#dv").append($("<a href='www.baidu.com'>百度</a>"));
//创建一个子级元素直接加到父级元素
//jQuery子级元素.appendTo(jQuery父级元素);
$("<a href='www.baidu.com'>百度</a>").appendTo($("#dv"));
//注意：以上两种如果页面存在该元素，添加时相当于剪切，即原元素会消失
$("#dv>span").clone().appendTo($("#dv1"));
//prepend
$("#dv").prepend($("<span>this is span</span>"));//把创建的span标签这个子元素直接插入到id为dv的第一个子元素之前
$("<span>this is span</span>").prependTo($("#dv"));
//after与before
$("#dv").after($("<span>this is span</span>"));//创建一个兄弟元素，放在id为dv的后面
$("#dv").before($("<span>this is span</span>"));//创建一个兄弟元素，放在id为dv的前面
```

## 移除元素

```javascript
$("#dv").html("");//移除id为dv的所有子元素
$("#dv").empty();//清空id为dv的所有子元素
$("#dv").remove();//删除自身
```

## 自定义属性

```javascript
//html5 提倡自定义属性名为 data-xxx
$("#dv").attr("data-checked");//获取自定义属性为checked值
//当用上述命名法后 原生写法 this.dataset['checked']  省略data-
//如果是data-foo-id这种时  this.dataset['fooId'];自动转换为驼峰命名法
//JQuery写法 $("#dv").data("checked")
$("#dv").attr("data-checked",true);//设置自定义属性为checked值为true
$("#dv").removeAttr("data-checked");//移除自定义属性为checked
```

## 设置或者获取元素选中

```html
<!--.prop获取的是元素对应的DOM对象属性，.attr获取的是元素的属性(代码有写才有);.prop获取的是该元素所有的属性（无论代码有没定义）-->
.prop("属性",值);//值一般是布尔类型
.prop("属性");//u获取是否选中
<tbody id="j_tb">
<input type="checkbox" id="ck1"/>
<input type="checkbox"/> 
</tbody>
<script>
$("#ck1").prop("checked",true);//值一般是布尔类型,checked属性一般用这个设置
$("#j_tb :checked");//获取id为j_tb内(有空格)有checked（勾上）的元素
$("#uu>li[index=1]");//这是获取自定义属性为index=1的元素  
</script>
```

## 设置宽、高、左距、右距、卷曲

```javascript
.width();//获取宽，返回的是数字;括号里面可以是数字或带px的字符串
.height();//获取高
.offset();//返回一个对象{left:100,top:200},值是数字;也可以直接设置值;left包含left和margin-left
.scrollLeft();//获取元素向左卷曲出去的距离，返回数字
.scrollTop();//获取元素向上卷曲出去的距离
```

## 为元素绑定事件

### 为同一元素绑定不同事件

```javascript
//1、
$("#btn").mouseenter();
$("#btn").mouseleave();
$("#btn").click();
//链式编程
$("#btn").mouseenter().mouseleave().click();
//2、bind方法绑定
$("#btn").bind("mouseenter",function(){});
$("#btn").bind("mouseleave",function(){});
$("#btn").bind("click",function(){});
//bind链式编程
$("#btn").bind("mouseenter",function(){}).bind("mouseleave",function(){}).bind("click",function(){});
//bind 传入json
$("#btn").bind({"mouseenter":function(){},"mouseleave":function(){},"click":function(){}});
```

### 为元素绑定相同事件

```javascript
//1、
$("#btn").click().click();
//2、
$("#btn").bind("click",function(){}).bind("click",function(){});
//不能使用键值对，只会执行最后一个事件
```

### 为元素绑定事件的另一种方式

```javascript
//父级元素调用方法，为子元素绑定事件
$("#dv").delegate("p","click",function(){});//为id为dv的子元素p绑定点击事件
//on方法与delegate方法是一样的，参数顺序不同而已;delegate是调用on实现的
$("#dv").on("click","p",function(){});//为id为dv的子元素p绑定点击事件
//多用于子元素动态创建 ，因此作为委托事件使用
```

### tips:

第一种和bind方法不能为绑定事件后的元素绑定，而delegate方法和on方法可以为后面创建的元素绑定，建议以后直接用on方法

## 为元素解绑事件

解铃还需系铃人

### 解绑bind绑定的事件

```javascript
//解绑bind 或者解绑$("#dv").click()等绑定事件
$("#dv").unbind("click");//解绑所有的点击事件，可以解绑多个事件"click mouseenter"
$("#dv").unbind();//此时解绑所有事件
```

### 解绑delegate绑定的事件

```javascript
//解绑delegate绑定的事件
$("#dv").undelegate();//移除了id为dv的子元素所有的delegate方法绑定的事件
$("#dv").undelegate("p","click mouseenter");//移除子元素p的点击和鼠标进入事件
```

### 解绑on绑定的事件

```javascript
//解绑on绑定的事件
$("#dv").off();//把父级元素和子级元素的所有事件全部解绑
$("#dv").off("click mouseenter");//把父级元素和子级元素的所有点击、鼠标进入事件全部解绑
$("#dv").off("click mouseenter","p");//把子级元素p的所有点击、鼠标进入事件全部解绑
$("#dv").off("","p");//把子级元素p的所有事件全部解绑
$("#dv").off("click","**");//把所有子级元素的所有点击事件全部解绑
```

## 事件冒泡

多个标签嵌套，如果注册相同的事件，会产生事件冒泡

### 阻止事件冒泡

```javascript
$("#dv").click(function (){
    console.log("hello");
    return false;//在想要阻止的那一层添加此方法即可，可兼容其他浏览器
});
```

## 事件的触发 

```javascript
//1、
$("#btn").click(function (){
    $("#txt").focus();//点击id为btn的按钮，让id为txt的文本框获取焦点事件发生
});
//2、
$("#btn").click(function (){
    $("#txt").trigger("focus");//点击id为btn的按钮，让id为txt的文本框获取焦点事件发生
});
//3、
$("#btn").click(function (){
    $("#txt").triggerHandler("focus");//直接执行focus的处理函数;可以触发事件，但不能触发浏览器的默认行为
});
```

## 便历jQuery对象

```javascript
//使用each方法，遍历ul内所有的li
$("#uu>li").each(function (index,ele) {//是一个伪数组，可以通过索引获得其中的元素
        var opacity=(index+1)/10;
        $(ele).css("opacity",opacity);//注意ele是DOM对象，要先转化
      })
    });
```

## 多库共存

多个jQuery库引用，$会冲突

> 1、$被占用时，可以使用jQuery来代替
>
> 2、var xy=$.noConflict();//把$的权利给了xy，$就可以作为其他的用法出现在代码中

## 包装集

把很多的DOM的对象进行包装或者是封装------>jQuery对象
jQuery对象------->jQuery对象[0]----------->获取到这个对象

如果jQuery获取的是多个对象，可以改变索引

```javascript
$(function (){
    $("p")[1]innerText;//获取第二个p标签
});
```

### 判断对象是否存在

```javascript
$("#btn").length==0;//等于0则代表存在
if($("#btn")){};//这个方法不行，btnsdsa这种如果有，则会返回true
```

## 宽高属性

```javascript
.innerWidth();//返回元素的宽度（不包括边框）
.innerHeight();//返回元素的高度（不包括边框）
.outerWidth();//返回元素的宽度（包括边框）;q里面填true 或false 与不填结果一样
.outerHeight();//返回元素的高度（包括边框）
```

## 插件

就是一个功能，别人把功能写好了，直接那过来就可以用了

### 自己做插件

```javascript
//如果希望jQuery的对象能调用这个方法，就把这个方法加入到$.fn中
$.fn.functionName=function (){};
```

### 引用别人的插件

引入css文件
jQuery的js文件，插件的js文件
复制html代码到body中
复制实现基本功能的代码到页面加载的事件中 $.fn

## 事件参数对象下的几个属性

```javascript
//用于事件冒泡的调试
e.target;//这个属性得到的是触发该事件的目标对象，是DOM对象;点击的目标
e.currentTarget;//得到的是触发该事件的当前对象，是DOM对象;由于事件冒泡也发生点击事件的对象
e.delegateTarget;//得到的是代理的这个对象，是DOM对象
```

jQuery UI

jQuery的用户界面

引入方法类似上面的引用插件方法

## mouseenter和mouseover的区别

不论鼠标指针穿过被选元素或其子元素，都会触发mouseover事件

mouseover：会有事件冒泡，进入子元素也会触发一次
mouseenter：只有鼠标进入本元素才会触发

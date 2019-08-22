js的介绍

布兰登·艾奇 js发明人

### js是什么：

> js  是一门脚本语言；
> js  是一门解释性语言；
> js  是一门动态类型语言；
> js  是一门基于对象语言；

### js 的作用：

最初作用：解决用户和浏览器之间的交互问题
现在用于：1.网页特效 2.服务端开发（Node.js） 3.命令行工具（Node.js） 4. 桌面程序（Electron） 5.App（Cordova） 6. 控制硬件--物联网（Ruff）7.游戏开发（cocos2d-js）

### js和html、css的区别：

> Html：标记语言，展示数据，提供网页的结构，提供网页的内容
> Css：美化页面
> JS： 用户和浏览器交互 ，控制网页内容，给网页增加动态效果

### js的代码可以在哪三个地方写:

> 1、在html的文件中，script的标签写js代码
> 2、js代码可以在html的标签中写
> 3、在js文件中可以写js代码，但需要在html的页面中引入 ，script的标签中src="js的路径"

### js的注意问题：

> 1、在一对script的标签中有错误的js代码，那么该错误代码后面的js代码不会执行
> 2、如果第一对的script标签中有错误，不会影响到后面的script标签中的js代码执行
> 3、script的标签中可以写 type="text/javascript"这是标准写法 或者写language="Javascript" 都可以但是，目前在我们的html页面中，type和language都可以省略，html遵循h5的标准
> 4、有可能出现script标签中同时出现type和language的写法
> 5、script标签在页面中可以出现多对
> 6、script标签一般是放在body的标签的最后，有时候会在head标签中
> 7、如果script标签是引入外部js文件的作用，那么这对标签中不要写任何js代码，应重新写一对script标签

## 变量

操作的数据都是在内存中操作
js中存储数据使用变量的方式（名字，值--->数据）
js声明变量都用var-----> 存储数据，数据应该有对应的数据类型，字符串类型的值都用成对的双引号或单引号

### 变量的作用：

存储数据或操作数据

变量声明（有var 有变量名字， 没有赋值） var number,x,y;
变量初始化（有var 有变量名字，有赋值）  var number=10; 
（代码的规范）js变量声明都用var ，js中的每一行代码结束都应有分号；，js中区分大小写，js的字符串可以使用单引号|双引号。

### 变量名的注意问题：

变量名的命名规范，要遵循驼峰命名法

> 1、变量的名字要有意义，
> 2、变量名一般以字母，$，_开头，中间或者后面可以用$， 字母，数字
> 3、变量名一般都是小写
> 4、变量名如果是多个单词，第一个单词的首字母是小写的，后面的所有单词的首字母都是大写的，这种命名方式称为：驼峰命名法 
> var bigNumber=10；
> 5、不能使用关键字

### 变量的交换方式：

#### 1、使用第三方的变量进行交换，

```javascript
var num1=10;
var num2=20;
var temp;
temp=num1;
num1=num2;
num2=temp;
```

#### 2、一般适用于数字交换

```javascript
var num1=10;
var num2=20;
num1=num1+num2;
num2=num1-num2;
num1=num1-num2;
```

#### 3、扩展（无需了解）

```javascript
var num1=10;
var num2=20;
num1=num1^num2;
num2=num1^num2;
num1=num1^num2;
```

### 注释：

单行注释：// 一般用于单行代码的上方;
多行注释：/**/ 一般用于函数或整段代码的上方

## 数字类型

### js中的 原始数据类型：

> number：数字类型（整数和小数）
> string：字符串类型（用单引号或双引号括起来）
> boolean：布尔类型（true（真1），false（假0））
> null：空类型，值为null，一个对象指向为空，赋值为null时，是赋值到指向为空的对象。
> undefined：未定义，值为undefined（变量声明了未赋值，函数没有明确的返回值），undefined和一个数字进行计算，得到NaN（not a number）
> object：对象

### 获取数据类型：

typeof 变量名
typeof(变量名)

### 进制表示：

```javascript
var num=10;//十进制
var num1=012;//八进制
var num2=0x123;//十六进制
```

### 数字类型的范围：

```javascript
console.log(Number.MAX_VALUE);//最大值（再往上加显示的还是该值）
console.log(Number.MIN_VALUE);//最小值
Infinity 无穷大
-Infinity 无穷小
```

### 注意：

> 1、不要用小数去验证小数，在内存存储的小数精度有限0.1 + 0.2 != 0.3
> 2、不要用NAN去验证NAN，NAN（not a num）与任何值都不相等，包括它本身，可以用isNAN(变量名)验证

## 字符串类型

字符串可以用成对的单引号或双引号括起来
获取字符串的长度：变量名.length
js中的转义符：/b 换行 /t制表符。。。
字符串的拼接：str1+str2 ，注意只要有一个是字符串，其他的是数字，结果也是拼接

```javascript
var str1="10";
var str2=5;
console.log(str1*str2);
/*
*结果是50，除+外，js会把与数字相运算的字符串转换成数字，这种方式叫隐式转换
*/
```

## 布尔类型

true(1)和false(0)，区分大小写

## undefine和null

undefined声明未赋值
null要设置必须手动设置

## 类型转换

### 其他类型转数字类型：

#### 1、parseInt();//转整数

```javascript
console.log(parseInt("10"));//10
console.log(parseInt("10asda"));//10
console.log(parseInt("a10"));//NaN
console.log(parseInt("1asd0"));//1
console.log(parseInt("10.98"));//10
console.log(parseInt("10.98asda"));//10
```

#### 2、parseFloat();//转小数

```javascript
console.log(parseFloat("10"));//10
console.log(parseFloat("10asda"));//10
console.log(parseFloat("a10"));//NaN
console.log(parseFloat("1asd0"));//1
console.log(parseFloat("10.98"));//10.98
console.log(parseFloat("10.98asda"));//10.98
```

#### 3、Number();//转数字

```javascript
console.log(Number("10"));//10
console.log(Number("10asda"));//NaN
console.log(Number("a10"));//NaN
console.log(Number("1asd0"));//NaN
console.log(Number("10.98"));//10.98
console.log(Number("10.98asda"));//NaN
```

### 其他类型转字符串类型：

#### 1、.toString();

```javascript
var num=10;
console.log(num.toString());//字符串类型
//如果变量有意义，调用.toString()转换
```

#### 2、String();

```javascript
var num=10;
console.log(String(num));
//如果变量没有意义（undefined,null），调用Strig()转换
```

### 其他类型转布尔类型：

#### 1、Boolean(值)；

```javascript
console.log(Boolean(1));//true
console.log(Boolean(0));//false
console.log(Boolean(11));//true
console.log(Boolean(-1));//true
console.log(Boolean("哈哈"));//true
console.log(Boolean(""));//false
console.log(Boolean("null"));//false
console.log(Boolean("undefined"));//false
```

## 运算符

### 算数运算符：

有： +  -  /  %

### 一元运算符：

这个运算符只需要一个操作数就可以运算的符号：++  --，如果不参与运算，++/--在前在后没差别。

#### 1、++ ：

```javascript
var num=10;
var sum=num++ +10;
console.log(sum);//20
console.log(num);//11
//如果++在后面：如上式参与运算，先参与运算，运算结束后自身再加1。在后后加
var num=10;
var sum=++num+10;
console.log(sum);//21
console.log(num);//11
//如果++在前面：如上式参与运算，先自身加1，再参与运算。在前先加
var i=1;
var j=i++ + i++;//=>1 + 2++=3
console.log(j);//3
console.log(i);//3
//++对自身的加是在整个运算中的，而不是等运算结束后在对自身加
```

#### 2、--：

```javascript
var num=10;
var sum=num-- +10;
console.log(sum);//20
console.log(num);//9
//如果--在后面：如上式参与运算，先参与运算，运算结束后自身再减1
var num=10;
var sum=--num+10;
console.log(sum);//19
console.log(num);//9
//如果--在前面：如上式参与运算，先自身减1，再参与运算 
```

### 二元运算符：

### 三元运算符：

转到三元表达式语句

### 复合运算符：

+=  -=  *=  /=  %=

### 关系运算符：

<   >  >=  <=  ==(不严格的相等，值一样，类型可不一样'5'==5)  ===(严格的相等，值和类型同时相等)  !=  !==
关系运算表达式的结果是布尔类型

### 逻辑运算符：

&&---逻辑与   ||---逻辑或  !---逻辑非

### 运算符的优先级：

> 1.（）最高
> 2.一元运算符 ++  -- ！
> 3.算数运算符   * /     + -
> 4.关系运算符  >  >=  <  <=
> 5.相等运算符  ==  !=  ===  !==
> 6.逻辑运算符  先&& 后||
> 7.赋值运算符

字面量：把一个值直接赋值给一个变量

## 流程控制

代码的执行过程

### 流程控制的方式

#### 1、顺序结构：

从上到下，从左到右执行的顺序

#### 2、分支结构：

##### if语句：

主要是判断

```javascript
/*
语法：
if(表达式){
	代码块
}
执行过程：先判断表达式的结果是true还是false，如果是true，则执行代码块
*/
if(8>6){
    console.log(8);
}//8
```

##### if-else语句：

两个分支，只能执行一个分支

```javascript
/*
if(表达式){
	代码1
}else{
	代码2
}
执行过程：如果表达式的结果是true，则执行代码1，否则执行代码2
*/
var num1=10;
var num2=20;
if(num1>num2){
    console.log(num1);
}else{
    console.log(num2);
}
```

##### if-else if-else if...语句：

最终也是执行一个；与switch-case相比，一般是对范围的判断

```javascript
/*
语法：
if(表达式1){
	代码1
}else if(表达式2){
	代码2
}else if(代码3){
	代码3
}else{
	代码4
}
执行过程：先判断表达式1的结果，如果为true，则执行代码1；如果为false，判断表达式2，如果为true，则执行代码2；。。。
*/
var score=Number(prompt("请您输入成绩"));
if(!isNaN(score)){//如果是数字
    if(score>90&&score<=100){
    	console.log("A级");
	}else if(score>80){
    	console.log("B级");
	}else if(score>70){
    	console.log("C级");
    }else if(score>=60){
        console.log("D级");
    }else{
        console.log("E级");
	}
}else{
    console.log("输入有误");
}
```

##### switch-case语句：

分支语句，多分支语句；与if-else if else if...相比，一般是对具体值得判断

```javascript
/*
语法：
switch(表达式){
    case 值1:
    	代码1;
    	break;
    case 值2:
    	代码2;
    	break;
    case 值3:
    	代码3;
    	break;
    case 值4:
    	代码4;
    	break;
    ...
    default:
    	代码5;
    	break;//可以省略
}
执行过程：获取表达式的值，和值1比较，如果一样，则执行代码1，遇到break则跳出整个语句，后面的代码不执行；
...
*/
var level="c";
switch(level){
    case "a":console.log("90~100");break;
    case "b":console.log("80~90");break;
    case "c":console.log("70~80");break;
    case "d":console.log("60~70");break;
    case "e":console.log("0~60");break;      
}//70~80
```

注意的问题：

> default后面的break是可以省略的
> default也是可以省略的
> switch-case 语句中和case后面的值比较的时候是严格模式===
> break是可以省略的，但是后面的代码会执行直到遇到break
>
> 表达式判断与case 的值一样后，如果没有break，后面的case后的代码会继续执行

##### 三元表达式语句：

```javascript
/*
运算符号：？ ：
语法：
var 变量=表达式1？表达式2:表达式3;
执行过程：表达式1的结果是true则执行表达式2，然后把结果给变量，如果表达式1的结果是false，则执行表达式3，把结果给变量。
*/
var x=10;
var y=20;
var result=x>y?x:y;
console.log(result);//20
```

#### 3、循环结构

##### while循环：

特点：先判断后循环，有可能一次循环体都不执行。

```javascript
/*
while循环语法：
计数器
var 变量=0;
while（循环的条件）{
	循环体;
	计数器++;
}
执行过程：先判断循环的条件，如果是true，则执行循环体，计数器加1；再判断循环的条件，重复上述步骤；直到循环的条件为false。
*/
var i=0;
while(i<10){
    console.log("hello world"+(i+1));
    i++;
}
```

##### do-while循环：

特点：先执行后判断，循环体至少执行一次。

```js
/*
do-while循环语法：
do{
	循环体;
	计数器++；
}while(条件);
执行过程：先执行一次循环体，然后判断条件是否成立，为true，则继续执行；重复直到判断条件为false。
*/
var i=1;
var sum=0;
do{
    if(i%3==0){
        sum+=i;
    }
    i++;
}while(i<=100);
console.log(sum);
```

##### for循环：

```javascript
/*
for循环语法：
for(表达式1;表达式2;表达式3){
    循环体;
}
执行过程：先执行表达式1，然后判断表达式2，为true，则执行循环体，执行表达式3，再判断表达式2，重复以上步骤，直到表达式2为false。
*/
for(var i=0;i<10;i++){
    console.log("hello world");
}
```

##### 后期还有for-in循环：

## 关键字

### break：

如果在循环中使用，遇到break，则立即跳出当前所在的循环。

### continue：

在循环中，遇到continue；直接开始下一次循环。	

## 数组

一组有序的数据。

### 数组的作用：

可以一次性存储多个数据。

### 数组的语法：

#### 1、通过构造函数创建数组:

```javascript
var 数组名=new Array();
var array=new Array();//定义了一个空数组。
//数组的名字如果直接输出，那么直接就把数组中的数据显示出来；如果没有数据，就看不到数据。
var array=new Array(5);//长度为5
var array=new Array(10,20,30,40,50);//长度为5，数据为[10,20,30,40,50]
```

#### 2、通过字面量的方式：

```javascript
var 数组名=[];//空数组
var array=[];
var array=[10];//值为10，长度为1
```

#### 小结：

无论是构造函数的方式还是字面量的方式，定义了有长度的数组，默认值是undefined；
数组元素：数组中每个数据都可以叫数组的元素；
数组长度：就是数组的元素的个数；
数组索引（下标）：用来存储或访问数组中的数据的，索引从0开始；

如何设置数组中某个位置的值：
数组名[下标]=值；
array[3]=100;

如何获取数组中某个位置的值：
var result=数组名[下标]；
console.log(result)；

数组存储的类型可以不一样：

```javascript
var arr=[10,"hello world",true,null,undefined,new Object()]
```

数组的长度可以改变：

```javascript
var arr=[];
arr[1]=10;
console.log(arr.length);
```

```javascript
//去掉数组中的0，并放到一个新数组中去
var arr=[10,20,0,30,0,40,50,0];
newArr=[];
for(i=0;i<arr.length;i++){
    if(arr[i]!=0){
        newArr[newArr.length]=arr[i];//把新数组的长度作为下标使用，数组的长度是可以改变的
    }
}
//冒泡排序
var arr=[10,6,5,1,51,66,55,151,51,54];
for(var i=0;i<arr.length-1;i++){
    for(var j=0;j<arr.length-1-i;j++){
        if(arr[j]>arr[j+1]){
            var temp=arr[j];
            arr[j]=arr[j+1];
            arr[j+1]=arr[j];
        }
    }
}
console.log(arr);
```

## 函数

把一坨重复的代码封装，在需要的时候直接调用

### 函数的作用：

代码的重用

### 函数的语法：

```javascript
//函数的定义
function 函数名(){
    函数体;
}
//函数的调用
函数名();
```

### 函数的参数：

在函数定义的时候，函数名字后面的小括号的变量就是参数，目的是在函数调用时，对用户传进来的值进行操作。

形参：函数在定义时小括号里的变量叫形参
实参：函数在调用时小括号里传入的值叫实参，实参可以是变量也可以是值
形参的个数和实参的个数可以不一致

### 函数的返回值：

函数的返回值：在函数内部有return关键字，并且在关键字后面有内容，这个内容被返回了
当函数调用之后，需要这个函数的返回值，可以定义变量接收
如果一个函数没有返回值，但在调用的时候定义变量接收返回值，那么结果就是undefined；有return但没有明确的返回值，结果也一样
没有明确返回值：无return或者return 后为空
return下面的代码是不会执行的
return在函数的循环中相当于break

函数里面可以调用其他函数	

```javascript
function sum(x,y){//形参
    var sum=x+y;
    return sum；
}
var num1=parseInt(prompt("请输入第一个数："));
var num2=parseInt(prompt("请输入第二个数："));
var result=sum(num1,num2);//实参
console.log(result); 
console.log(sum);//显示sum函数的代码
```

### arguments对象：

定义一个函数，如果不确定用户是否传入参数或不确定参数的个数

```javascript
//定义
function f1(){
    //arguments相当于一个数组，但是是伪数组
    var sum=0;
    for(var i=0;i<arguments.length;i++){
        sum+=arguments[i];
    }
    return sum;
}
console.log(f1(10,20,30));
```

### 函数的其他定义方式：

命名函数：函数有名字
匿名函数：函数没有名字，匿名函数不能直接调用

#### 函数表达式：

把一个函数赋值给一个变量，形成函数表达式
变量存储的是一个函数，可以通过加"( )"直接调用

```javascript
/*
var 变量=匿名函数；
var f1=function (){
	函数体
};
*/
var f4=function (){
    console.log("hello world");
};//必须加;号，此时是赋值操作!!!!!!!!!!!!!!!
f4();
//两个方式的对比
//函数声明================================
function f1(){
    console.log("hello world");
}
f1();//hello,函数重名了，不管在哪调用都会是后面新定义的函数(http://example.com/"doubleName")
function f1(){
    console.log("hello");
}
f1();//hello
//函数表达式=============================
var f2=function (){
    console.log("hello world");
};
f2();//hello world,相当于变量的重新赋值，后面的不会影响前面的
f2=function (){
    console.log("hello");
};
f2();//hello
//总结：函数表达式会比函数声明更安全一些
```

#### 函数自调用：

没有名字，声明的同时，直接调用
一次性的---都是匿名函数，不会有重复

```javascript
(function (){
    console.log("hello world");
})();
```

### 函数数据类型：

是function类型

```javascript
function f1(){
	console.log("hello world");
}
console.log(typeof f1);
```

### 回调函数：

函数可以作为参数使用，如果一个函数作为参数，这个函数就叫回调函数
只要看到一个函数作为参数使用，它就是回调函数

```javascript
function f1(fn){
	fn();//fn是一个函数
}
function f2(){
	console.log("hello world");
}
f1(f2);//hello world
```

### 函数作为返回值使用

```javascript
function f1(){
	return function (){
		console.log("this is a function");
	}
}
var f2=f1();//此时f2内存储的是一个匿名函数
```

## 作用域

### 全局变量：

> 声明的变量是使用var声明的，可以在页面的任何位置使用
> 除了函数内部定义的，其他位置定义的变量都是全局变量
> 全局变量，如果页面不关闭，内存就不会释放，消耗内存！

### 局部变量:

在函数内部定义的变量，外面不能使用

### 全局作用域：

全局变量的使用范围

### 局部作用域：

局部变量的使用范围

### 块级作用域：

一对大括号就是一块，在块内定义的变量，只能在这个区域中使用；
但是在js中，没有块级作用域，在块外也能使用。//函数除外

### 扩展：

#### 隐式全局变量：

声明的变量不使用var

```javascript
function f1() {
    number =100;//此时是隐式全局变量，可以被外部引用
}
f1();
console.log(number);//100
//全局变量是不能被删除的，隐式全局变量可以被删除
var num1=10;
num2=20;
delete num1;//删除了num1
delete num2;
console.log(typeof num1);//number
console.log(typeof num2);//undefined，此时要用typeof，否则直接输num2会报错
//总结：声明变量使用var是不会被删除的，没有var的可以被删除
//变量的重新赋值不是隐式全局变量！！！！！！！！！！
```

### 作用域链：

```javascript
var num=10;//0级作用域：在script标签下，函数外定义的变量
function f1() {
    var num=20;//1级作用域
    function f2(){
        var num=30;//2级作用域
        function f3(){
           var num=50;//3级作用域
            console.log(num);//50；若3级作用域没重新赋值，则往上找直到0级作用域
        }
        f3();
    }
    f2();
}
//变量的值：关系找最亲的，往上找，直到0级作用域
```

### 预解析

在执行代码之前，提前解析代码
把**变量的声明/函数的声明提前**了--------提前到当前（函数）所在的作用域的最上面！！！！

```javascript
console.log(num);//undefined
var num=10;
/*相当于
var num;
console.log(num);
num=10;
可以解释前面的
*/
//高级例子
f1();//undefined,===================>|var num=20;
var num=20;//                        |function f1(){
function f1(){//                     |	var num;
    console.log(num);//              |	console.log(num);
    var num=10;//                    |	num=10;                     
}//                                  |}
//                                   |f1();

f1();//undefined,===================>|var num;
var num=20;//                        |function f1(){
function f1(){//                     |	console.log(num); 
    console.log(num);//              |}
}//                                  |f1(); 
//                                   |num=20;
//总结：需提前解析？-->按执行顺序提前到当前域的前面，即变量使用之前
f1();
console.log(c);//9
console.log(b);//9
console.log(a);//报错
function f1(){
    var a=b=c=9;
    cosole.log(a);//9
    cosole.log(b);//9
    cosole.log(c);//9
}
/*等价于下面的
function f1(){
    var a;
    a=9;//局部变量
    b=9；//隐式全局变量
    c=9;//隐式全局变量
    cosole.log(a);//9
    cosole.log(b);//9
    cosole.log(c);//9
}*/
f1();
console.log(c);//9
console.log(b);//9
console.log(a);//报错，因为除了a不是全局变量，b、c都是
//易错点
f1();//报错
var f1=function (){
    console.log(a);
    var a=10;
}
/*
var f1;
f1();//不能调用！
*/
```

<a href="#函数表达式：">函数表达式</a>

#### 小结：

预解析中，变量的提升只会在当前的在作用域中提升，提前到当前的作用域的最上面
函数中的变量只会提前到函数的作用域的前面，不会超出函数
预解析会分段（多对的script标签中函数重名，互不影响）
变量的声明提前，不会提升赋值；函数的声明提前，不会提升调用
变量名和函数名重名了，且有预解析时，变量名提升在最上面

## 编程思想

### 面向过程：

每件事的具体过程都要知道，注重过程

### 面向对象：

根据需求找对象，所有的事都由对象来做，注重结果	

面向对象的特性：封装，继承，多态（抽象性）
js不是面向对象的语言，但是可以模拟面向对象的思想
js是一门基于对象的语言
有属性和方法，特指的某个事物
一组无序属性的集合的键值对，属性的值可以是任意的类型

## 对象

### 创建对象的三种方式：

#### 1、调用系统的构造函数创建对象

```javascript
//var 变量名=new Object();   Objiect是系统的构造函数
var obj=new Onject();//实例化对象
//添加属性    对象名.名字=值；
obj.name="eureka";
console.log(obj.name);
//添加方法    对象.名字=函数;
obj.say=function (){//匿名函数
    console.log("hello world,my name is"+obj.name); //可以使用this代替当前对象，即：this.name  
};//分号不能丢！
obj.say();
//一次性创造多个对象，把创建的对象的代码封装在一个函数内(工厂模式)
function creatObject(name,age){
    var obj=new Object();
    obj.name=name;//前面的name是对象的属性，后面的是传进来的变量
    obj.age=age;
    obj.say=function (){
        console.log("hello world");
    };
    return obj;//记得返回
}
var per1=creatObject("eureka",18);
```

#### 2、自定义构造函数创建对象（结合第一种和需求通过工厂模式创建对象）

```javascript
//构造函数和函数的区别：首字母大写！
function Person(name,age){
    this.name=name;
    this.age=age;
    this.say=function (){
        console.log("hello world,my name is"+this.name);
    };
}
var per1=Person("eureka",18);
console.log(per1 instanceof Person);//判断对象类型
//创建对象时，系统做了哪些事
> 1、在内存中开辟空间，存储创建新的对象
> 2、把this设置为当前的对象
> 3、设置对象的属性和方法的值
> 4、把this这个对象返回
```

#### 3、字面量的方式创建对象

```javascript
var obj= {};//空对象
obj.name="eureka";
//优化后
var obj={
    name:"eureka",
    age:18,
    say:function (){
        console.log("hello world");
    }//不加分号
};
console.log(obj.height);//undefined;点语法是不需要事先声明，没有的属性只会返回undefined
//缺陷：一次性的对象
```

### 判断是否对象类型

```javascript
//变量 instanceof 类型的名字
console.log(obj instanceof Object);//true  or   false
```

### 设置属性和方法的另一写法

```javascript
obj.name="eureka";//obj["name"]="eureka";中括号内必须为双引号；
obj["say"]();//调用函数的小括号不能省略
//对象中确实有这个属性，则 对象.属性  和 对象["属性"]都可以
//对象没有这个属性，则只能使用对象["属性"]
```

### JSON

JSON格式的数据一般都是成对的，是键值对，无论是键，还是值，都是用双引号括起来的；它也是一个对象
json的数据实际上就是格式化后的一组字符串的数据

### 遍历对象的内容

```javascript
//不能用for循环，无序
var json={
    "name":"eureka",
    "age":"18",
    "sex":"boy"
};
for(var key in json){//key是一个变量，存储json所有属性的名字
    console.log(json[key]);//这里不能用点语法，否则undefined
}//for in 语句
```

### 简单类型和复杂类型

原始数据类型：number,string,boolean,undefined,null,object

基本类型（简单类型），值类型：number,string,boolean
值类型在栈中存储
值类型传递的是值

```javascript
var num1=50;//全局变量
var num2=60;//全局变量
function f1(num,num1){//50，60
    num=100;//50->100,局部变量
    num1=100;//60->100，局部变量
    num2=100;//隐式全局变量
    console.log(num);//100
	console.log(num1);//100
    console.log(num2);//100
}
f1(num1,num2);//传50，60
console.log(num1);//50
console.log(num2);//100
console.log(num);//报错
```

复杂类型（引用类型）：object
引用类型的对象在堆中存储，地址在栈中存储
引用类型传递的是地址（引用）

```javascript
function Person(name,age,alary) {
    this.name=name;
    this.age=age;
    this.salary=salary;
}
function f1(person){
    person.name="jack";
    person=new Person("mike",18,10);//改变地址，不再指向tom这个对象，指向新的对象            
}
var p=new Person("tom",1,1000);//p->0x120  |   0x120->tom对象
console.log(p.name);//tom
f1(p);//传p这个对象的地址当参数,f1(0x120)
console.log(p.name);//jack
```

空类型：undefined,null

### 三种对象

#### 1、内置对象

js系统自带的对象

实例对象：通过构造函数创建出来，实例化的对象
静态对象：不需要创建，方法（静态方法）可以直接调用
实例方法：必须通过实例对象调用
静态方法：必须通过大写的对象调用

##### Math

Math是一个对象，不是函数，不能实例化，直接调用属性（静态）、方法（静态）即可

```javascript
Math.abs(null);//0----空，默认为0
Math.ceil(12.09);//13----向上取整
Math.floor(12.9);//12----向下取整
Math.max(10,5,6,4,45);//45----最大值
Math.min(10,5,6,4,45);//4----最小值
Math.pow(3,2);//9---3的2次方
Math.sqrt(9);//3----9的开平方根
Math.random();//随机数----[0,1),靠*、+扩大范围
Math.Pi;//圆周率
```

##### Date

需创建Date的实例对象

```javascript
var dt=new Date.now();//数字型，以毫秒为单位
var dt=new Date("1970-01-01");//1970,01,01 | "1970/01/01" |能将毫秒数转成日期对象
var dt=new Date();//当前时间---服务器的当前时间
dt.getFullYear();//获取年
dt.getMonth()+1;//月份从0开始，应加1
dt.getDate();//获取日期
dt.getHours();//获取小时
dt.getMinutes();//获取分钟
dt.getSeconds();//获取秒
dt.getDay();//获取星期，0代表星期日
dt.toDateString();//Wed Sep 13 2017
dt.toLocaleDateString();//2017/9/13
dt.toTimeString();//11:03:14 GMT+0800(中国标准时间)
dt.toLocaleTimeString();//上午11:03:04
dt.valueOf();//相当于var dt=new Date.now()
var dt=+new Date();//一种特殊写法，只适用Date，在浏览器不适用html5时的写法
```

##### String

String 是一个用于字符串或一个字符序列（字符组成的数组）的构造函数
js中没有字符类型，即单个字母

字符串特性：不可变性，字符串的值是不可变的

```javascript
//此时当做对象看待
var str="hello world";
str.length;//字符串的长度
str.charAt(index);//index=[0,str.length-1]  返回指定索引位置的字符，超出索引为空
String.fromCharCode(数字值,[可以是多个参数]);//返回ASCII码对应的值,静态方法，使用 对象.方法
str.concat(string2[,string3...]);//将多个字符串与原字符串连接合并，返回新的字符串
str.indexOf(string[,给定位置的索引]);//返回这个字符串的索引，-1为not found,给定的索引可以超出str的长度
str.startswith('http://');//true|false,以什么开头
str.endsWith('http://');//true|false,以什么结尾
str.lastIndexOf(string[,给定位置的索引]);//从后往前找，索引
str.replace(替换字符,被替换字符);//将str里的字符替换成别的，返回新的字符串
str.split(""[,3]);//将str以空格分割[3次]，返回数组
str.slice(5[,10]);//截取从索引5开始到[10的前一个]结束[5,10)，返回截取的字符串；允许负数，可以从后往前数
str.substr(5[,5]);//截取从序列5开始，截取到后面长度为5的字符串
str.substring(5[,10]);//截取从序列为5开始，到序列为10前[5,10)，取两者最小值为start，负数自动转为0，并为start
str.toLocalLowerCase();//转为小写，效果相同的有，没多大差别str.toLowerCase();
str.toLocalUpperCase();//转为大写，效果相同的有，没多大差别str.toUpperCase();
str.trim();//切掉字符串两端的空格
```

##### Array

```javascript
//构造函数
var arr=new Array();
//字面量
var arr=[];
Array.isArray(obj);//判断obj是否是数组
arr instanceof Array;//同上
Array.from(arr);//将arr转成新数组
arr.concat(arr1[,arr2]);//拼接arr和arr1两个数组
arr.every(function(ele,index,array){});//ele:元素值，index:元素的索引，array：arr原数组本身，以上参数看需求添加，返回布尔值；用于判断arr的元素是否都符合函数的条件，都有则true
arr.filter(function (){});//返回符合函数条件的新数组，函数的参数同上
arr.map(function(){});//数组中每个元素都要执行这个函数，返回执行后的新数组，里面可以是方法，不一定是函数
arr.push(100);//在末尾添加元素100，返回数组长度
arr.pop();//删除最后一个元素，返回值为该元素
arr.shift();//删除第一个元素
arr.unshift();//在第一个前面添加元素
arr.forEach(function (){});//参数同上，遍历数组，相当于for循环，无返回
arr.indexOf(元素值);//有则返回索引，无则-1
arr.join(",");//将arr以","拼接成字符串返回
arr.reverse();//反转数组
arr.sort();//数组排序；不稳定，可以添加比较函数
/*
function (a,b){ //a---arr[j];b---arr[j]
	if(a>b){
		return 1;
	}else if(a==b){
		return 0;
	}else{
		return -1;
	}
}//固定写法，sort就稳定了
*/
arr.slice(3,4);//从索引3开始，到4前截取新的数组；[3,4)
arr.splice(开始位置,删除个数,用于替换的元素);//用于删除元素或替换，或添加(删除个数为0，即添加)
arr.length=0;//清空数组
```

#### 2、自定义对象

自己定义的构造函数创建的对象

#### 3、浏览器对象

### 基本包装类型

普通变量不能直接调用属性和方法
对象可以直接调用属性和方法

基本包装类型：本身是基本类型，但在执行代码的过程中，如果这种类型的变量调用了属性和方法，那么这种类型就不再是基本类型，而是基本包装类型，这个变量成了基本包装类型对象

```javascript
var flag=new Boolean(false);
var result=flag&&true;//true,如果是一个对象&&true---->true
var result=true&&flag;//对象,如果是true&&一个对象---->对象
//----------------------
var num=10;//基本类型
var num=Number("10");//类型转换
var num=New Number("10");//基本包装类型
```

## DOM

### JavaScript分三个部分

ECMAScript标准：JS的基本语法
DOM：Document Object Model ---->文档对象模型----操作页面元素---顶级对象：document
BOM：Browser Object Model ---->浏览器对象模型----操作的是浏览器---顶级对象：window,页面的内容都属于window,当调用window下的属性和方法时，可以省略window，window.name输出为空

文档：一个页面可以看成一个文档
元素（element）：页面中的所有的标签都是元素，元素可以看成是对象
节点（node）：页面中所有的内容都是节点：标签，属性，文本(文字、换行、回车)

### 元素

#### 获取元素

```javascript
//<input type="button" value="按钮" id="btn"/>
//input 要放在document.getElementById()之前！！！动态类型
//==============获取元素==================
var btnObj=document.getElementById("btn");//根据id获取元素，返回一个元素对象
//为当前的这个按钮元素（对象），注册点击事件，添加事件处理函数（匿名函数）
btnObj.onclick=function (){
    alart("hello world");
};
document.getElementsByTagName("p");//返回所有p标签组成的伪数组，需用for遍历每个数组
//============下面的有的浏览器不支持/IE低版本的===============
document.getElementsByName("name1");//获取name="name1"的所有表单，返回伪数组
//基本标签没有name值，表单标签才有
document.getElementsByClassName('cls');//获取class="cls"的所有元素，返回伪数组，属于H5
document.querySelector("#btn");//根据选择器的方式获取元素，获取id="btn"的元素，返回一个
document.querySelectorAll(".cls");//根据选择器获取所有class=".cls"的元素，返回多个
//=============this==================
btnObj.onclick=function (){
    this.value="hello";
};//在某个元素的事件中，在本事件中的this就是当其的元素对象
var inputs = document.getElementsByTagName('input');
    for (var i = 0; i < inputs.length; i++) {
      inputs[i].onclick = function() {//在点击之后执行
        for (var j = 0; j < inputs.length; j++) {
          inputs[j].value = "hello";
        }
        this.value = "hello world";//这个地方不能用inputs[i],for循环是在页面加载之后就执行完毕，此时i=inputs.length!!!!
      }
    }
//================属性及值唯一时=====================
<input type="button" value="默认" id="btn" />
<input type="radio" name="" value="2" name="sex" />男
<input type="radio" name="" value="2" name="sex" id="rad1" />女
<input type="radio" name="" value="2" name="sex" />保密
/*<script>
document.getElementById("btn").onclick=function (){
	document.getElementById("rad1").checked=true;//默认选中
};
表单标签中，如果属性和值只有一个，并且是该属性本身，那么，DOM操作时，这个属性值是布尔类型就可以了
</script>*/
//=============多单词写法======================
<div id="dv" style="backgorund-color :red"><//div>
/*<script>
<div id="dv" style="backgorund-color :red"></div>
document.getElementById("dv").style.backgroundColor="yellow";
凡是css的属性是多个单词的写法，DOM操作时，可将-去掉，后面的单词首字母大写即可
</script>*/
//==============className=================
.cls {width:100px;heigth:100px}
//<div id="dv"></div>
document.getElementById("dv").className="cls";//在js代码中，设置元素的类样式，不能用class关键字，应用className
// =========classList==========
document.getElementById("dv").classList.add('cls');
document.getElementById("dv").classList.remove('cls');
document.getElementById("dv").classList.toggle('cls');
//当只有一个参数时：切换 class value; 即如果类存在，则删除它并返回false，如果不存在，则添加它并返回true。
//当存在第二个参数时：如果第二个参数的计算结果为true，则添加指定的类值，如果计算结果为false，则删除它
/* 与className类似 
使用“classList”，可以添加或删除一个类，而不会影响该元素可能具有的任何其他类。但是如果你指定“className”，它将在添加新类时消灭任何现有的类（或者如果你指定一个空字符串，它将消除所有类）
配“className”可以方便确定元素上不会使用其他类，通常只使用“classList”方法。
而且“classList”也有方便的“切换”和“替换”方法。
*/
//==============对元素操作================
/*阻止超链接的默认跳转：return false
<a href="javascript:void(0);"></a>
如果是addEventListener等的兼容代码，在函数内部return false无效，
需在兼容函数后加document.getElementById('aa').setAttribute("onclick","return false;");
原因：相当于下面代码
obj.onclick=function(){
        addEventListener();上面的只有跳出第一层，应该在最外一层加
      }
*/
//鼠标点击：获取的元素.onclick
//鼠标经过：获取的元素.onmouseover
//鼠标离开：获取的元素.onmouseout
//搜索框获取焦点：获取的元素.onfocus
//搜索框失去焦点：获取的元素.onblur
//设置成对的标签中的文本内容：获取的元素.innertText//IE8的标准
//设置成对的标签中的文本内容：获取的元素.textContent//W3C的标准，IE8不支持
//设置文本内容、设置html标签：获取的对象.innerHtml//推荐使用这个设置文本，相对前两个没有兼容问题；也可以设置带标签的内容，且有标签效果。获取时也会带标签获取
//=============兼容代码=============
//浏览器不支持，返回undefined
function setInnerText(element,text){
    if(typeof element.textContent=="undefined"){//不支持
        element.innerText=text;
    }else{
        element.textContent=text;
    }
}
//=========标签中自定义属性===============
//<li score="10">hello world</li>
this.getAttribute("score");//获取自定义属性的值，直接.会得到undefined
lis[i].setAttribute("score",(i+1)*10);//批量设置标签的自定义属性，直接.在标签中看不到，缓存在DOM中
this.removeAttribute("score");//移除自定义属性
this.removeAttribute("class");//也可移除自带属性；相比于把值设空，本法更为干净，属性名都删除了
//========排他功能===================
for(var i=0;i<x.length;i++){
    x[i].onclick=function {
        for(var j=0;j<y.length;j++){
    	x[j].removeAttribute("class");
        }
    this.className="current";
    }
}
```

### 节点

> nodeType：节点类型==1---标签，2---属性，3---文本（文本，换行，回车）
> nodeName：节点名字==标签节点---大写的标签名，属性节点---小写的属性名，文本节点---#text
> nodeValue：节点值==标签节点---null，属性节点---属性值，文本节点---文本内容

```javascript
var ulObj=documenet.getElementById("uu");
ulObj.parentNode;//父级节点
ulObj.parentElement;//父级元素
ulObj.childNodes;//子节点
ulObj.children;//子元素可以
//=======以下IE8只支持第一种，而且获取的变成元素，不是节点，第二中会变成undefined===========
ulObj.firstChild;//第一个子节点
ulObj.firstElementChild;//第一个子元素
ulObj.lastChild;//最后一个子节点
ulObj.lastElementChild;//最后一个子元素
ulObj.previousSibling;//某元素的前一个兄弟节点
ulObj.previousElementSibling;//某元素的前一个兄弟元素
ulObj.nextSibling;//某元素的后一个兄弟节点
ulObj.nextElementSibling;//某元素的后一个兄弟元素

ulObj.getArributeNode("id");//获取属性为id的属性
//获取的节点可以.nodeType....
```

### 元素创建

为了提高用户的体验

元素创建的三种方式：

```javascript
//1、docunment.write("标签的代码及内容");========创建的是一个字符串
docunment.write("<p>这是一个p</p>");
//缺陷：如果在页面加载完毕后，此时通过此法，页面（head、body）上的其他内容都被清除；但在加载时使用此法不会清除
//2、对象.innerHTML="标签代码及内容";==========创建的是一个字符串
var divObj=document.getElementById("dv");
divObj.innerHTML="<p>这是一个p</p>";
//谁要创建子元素，就在谁后面.;不能放在body里，相当重新赋值，会清除原始数据
//3、document.creatElement("标签名字");==========创建的是一个对象
var pObjs=document.createElement("p");//创建元素，标签名
var ulObj = document.createElement("ul");//如果要获取这个ul对象，直接调用ulObj就行，这个变量存储在内存中，只要页面不关闭，就能调用
pObjs.innerText="这是一个p";//p标签增加内容
divObj.appendChild(pObj);//将创建好的元素追加到父元素中！！，创建好后要追加！这里可以无限在后面追加
divObj.insertBefore(pObj,divObj.firstElementChild);//把新的元素添加到第一个子元素之前
divObj.replaceChild(pObj,divObj.firstElementChild);//与第一个子元素替换
divObj.removeChild(divObj.firstElementChild);//删除第一个子元素
//解决appendChild无限追加的bug-------有则删除，无则创建----------------
//<input type="button" name="" value="创建按钮" id="btn" />
//<div id="dv"></div>
var divObj = document.getElementById('dv');
    document.getElementById('btn').onclick = function() {
      if (document.getElementById('btn2')) {//有则删除
        divObj.removeChild(document.getElementById('btn2'));
      }
      var inputObj = document.createElement("input");
      inputObj.type = "button";
      inputObj.value = "按钮";
      inputObj.id="btn2";
      divObj.insertBefore(inputObj,divObj.firstElementChild);
    };
```

#### Tips：

> 如果是循环内添加事件，推荐使用命名函数，节省内存空间
> 如果是在循环的外面添加事件，推荐使用匿名函数

### 为同一个元素绑定多个事件

```javascript
document.getElementById("dv").onclick=function (){};//只能绑定一个事件
//=================================
document.getElementById("dv").addEventListener("click",function(){},false);
//对象.addEventListener("事件类型",事件处理函数,false);谷歌火狐支持，IE8不支持
//参数1：事件的类型---事件的名字，没有on，string类型
//参数2：事件处理函数---函数（命名函数、匿名函数）
//参数3：布尔类型，false----事件冒泡阶段，true----事件捕获阶段
//==================================
document.getElementById("dv").attachEvent("onclick",function(){});
//对象.attachEvent("有on的事件类型",事件处理函数);谷歌火狐不支持，IE8支持
//参数1：事件的类型---事件的名字，有on！
//参数2：事件处理函数---函数（命名函数、匿名函数）
```

### 为元素解绑事件

```javascript
document.getElementById("btn").onclick=null;//为id为btn的按钮事件解绑
/*
tips：
用什么方式绑定事件，就应该用对应的方式解绑事件
1、解绑事件：
对象.on事件名字=事件处理函数----->绑定事件
对象.on事件名字=null;---->解绑
2、解绑事件
对象.addEventListener("没有on的事件类型"，命名函数，false);-----绑定事件；不能用匿名函数，不是同一个函数
对象.removeEventListener("没有on的事件类型"，函数名字，false);-----解绑事件
3、解绑事件
对象.attachEvent("有on的事件类型"，命名函数);----绑定事件
对象.detachEvent("有on的事件类型"，函数名字);----解绑事件事件
*/
```

### 事件冒泡

多个元素嵌套，有层次关系，这些元素都注册了相同事件，如果里面的元素触发了，外面的元素的该事件自动的触发了

```html
<div id="dv1">
    <div id="dv2">
      <div id="dv3"></div>
    </div>
  </div>
<script>
	document.getElementById("dv1").onclick=function(){
		console.log("hello world");
};
    document.getElementById("dv2").onclick=function(e){//IE8没有这个参数，udefined
		console.log("hello world");
        window.event.cancelBubble=true;//在这层停止，dv1不触发 IE、谷歌支持，火狐不支持
        e.stopPropagation();//IE8不支持 ，谷歌、火狐支持
};
	document.getElementById("dv3").onclick=function(){
		console.log("hello world");
};
</script>
<!--点击dv3的盒子，dv2、dv1的点击事件会自动触发，添加 以上代码 会在那一层停止-->
<!--window.event和e都是事件参数对象，事件参数e在IE8中不存在，此时用window.event-->
<!--addEventListener(没有on的事件类型,事件处理函数,控制事件阶段的)
事件有三个阶段：通过e.eventPhase这个属性可以知道事件的阶段
1->事件捕获阶段：从外向内
2->事件目标阶段：最开始选择的那个
3->事件冒泡阶段：从里向外
一般默认都是冒泡阶段，很少捕获阶段
-->
```

为同一元素绑定多个事件，指向相同事件处理函数

```html
<input type="button" name="" value="按钮" id="btn">
  <script type="text/javascript">
    var mouseObj = document.getElementById('btn');
    mouseObj.onclick = f1;
    mouseObj.onmouseover = f1;
    mouseObj.onmouseout = f1;
    function f1(e) {//谷歌、火狐才有e
      switch (e.type) {//没有on的事件名
        case "click":
          console.log("hello world");
          break;
        case "mouseover":
          this.style.backgroundColor = "red";
          break;
        case "mouseout":
          this.style.backgroundColor = "green";
          break;4
      }
    }
  </script>
```

## BOM

```javascript
window.onload=function(){};//页面加载完毕，这个事件就会触发----标签、属性、文本、外部引入的js文件，window可以省略
window.onunload=function(){};//扩展事件----页面关闭后才触发的事件，IE8才有
window.onbeforeunload=function(){};//扩展事件----页面关闭之前触发的事件，IE8才有
```

### location对象

获取或者设置浏览器地址栏的URL，window可以省略

```javascript
//属性
location.hash;//得到地址栏上#及后面的内容
location.host;//主机名及端口号
location.hostname;//主机名
location.pathname;//文件的路径----相对路径
location.port;//端口号
location.portocol;//协议
location.search;//s?搜索的内容
location.href="http://www.baidu.com";//要调转的地址
//方法
location.assign("http://www.baidu.com");//要调转的地址
location.reload();//重新加载
location.replace("http://www.baidu.com");//替换地址，没有历史记录
```

### history对象

```javascript
history.back();//后退
history.forward();//前进
history.go();//取决于内容，正数前进，负数后退
```

### navigator对象

```javascript
navigator.userAgent;//判断用户浏览器类型
navigator.platform;//判断系统平台类型
```

### 定时器

```javascript
setInterval();//常用的，反复执行
/*
参数1：函数
参数2：时间，毫秒为单位
执行过程：页面加载完毕后，每过x毫秒，执行一次函数
返回值：定时器的Id
cleanInterval(timeId);
参数：要清理的定时器的Id
*/
setTimeout();//一次性定时器，只执行一次，但是还是占内存空间、	
/*
参数、返回值同上
clearTimeout(timeId);
用于定时器，释放空间
*/
```

### Tips:

```javascript
//如果要获取div的left值(无论是在标签还是属性)，可以使用   
document.getElementById.offsetLeft;//数字类型，没有px，而不能用my$("dv").style.left 
//获取任意一个元素的任意一个样式属性的值 兼容代码,offsetLeft 要脱标left的值才有效
function getStyle(element, attr) {//返回字符串类型
        return window.getComputedStyle?window.getComputedStyle(element, null)[attr]:element.currentStyle[attr]||0;
      }
    }
//移动盒子动画案例：
function moveElement(element, json) { //元素 json{属性名字:目标位置}
      clearInterval(element.timeId); //先清理定时器,保证每次都是同一个定时器
      element.timeId = setInterval(function() { //对象的属性唯一，不会重复创建定时器
        var flag = true; //假设默认全部到达位置
        for (var attr in json) {
          var current = parseInt(getStyle(element, attr));
          var target = json[attr];
          //动态步距，最后每一步走1像素，直到目标位置停下
          var step = (target - current) / 10;
          //0.1取1,-0.1取-1。应往大了取才会到
          step = step > 0 ? Math.ceil(step) : Math.floor(step);
          current += step;
          //到最后每步走一像素，肯定不会超出
          element.style[attr] = current + "px";
          if (current != target) {
            flag = false; //只要有一个没到达目标就false
          }
        }
        if (flag) {//所有元素到达指定位置，清理定时器
          clearInterval(element.timeId); //清理定时器
        }
      }, 20);
    }
//克隆元素
var new=ulObj.children[0].cloneNode(true);//克隆ulObj中的第一个子元素，true---包括属性，false---不包括属性
ulObj.appendChild(new);//添加到ulObj的子元素后面
```

#### offset系列

如果样式的代码是在style标签中设置，外面是获取不到的 | <style>
如果样式的代码是在style属性中设置外面是可以获取到的 | style=" "

offsetWidth：获取元素的宽
offsetHeight：获取元素的高（自身的高+自身的border）
offsetLeft：获取元素的距离左边位置的值
offsetTop：获取元素的距离上面位置的值

没有脱离文档流：
offsetLeft：父级元素margin+父级元素padding+父级元素border+自己的margin
脱离文档流：
offsetLeft：只跟自己的left和margin有关系

#### 可直接通过document获取的

```javascript
document.body;//获取body，body标签
document.title;//获取title,title内的内容
document.documentElement;//获取html，html标签
```

#### 元素跟随鼠标移动

```html
<img src="./images/1.jpg" alt="" id="img">
  <script type="text/javascript">
    document.onmousemove = function(e) {
      var imgObj = document.getElementById('img');
      imgObj.style.left = e.clientX + "px";//可视区域的X坐标
      imgObj.style.top = e.clientY + "px";
    }
</script>
```

#### scroll系列

scrollWidth：元素中内容的实际的宽（没有边框），如果没有内容（或者内容不足盒子高度）就是元素的宽
scrollHeight：元素中内容的实际的高（没有边框），如果没有内容（或者内容不足盒子高度）就是元素的高
scrollLeft：向左滚动（卷曲）出去的距离
scrollTop：向上滚动（卷曲）出去的距离

```javascript
//实时获取滚动条向上滚动的距离
document.getElementById('dv').onscroll=function () {//获取id为dv的div的滚动事件
    console.log(this.scrollTop);
};
//封装获取scroll值的兼容代码
function getScroll() {
    return {						      left:window.pageXOffset||document.documentElement.scrollLeft||document.body.scrollLeft||0;    top:window.pageYOffset||document.documentElement.scrollTop||document.body.scrollTop||0;
  };
}
```

```javascript
//获取任意一个元素的任意一个样式属性的值 兼容代码,offsetLeft 要脱标left的值才有效
    function getStyle(element, attr) {//返回字符串类型
      if (window.getComputedStyle) {
        return window.getComputedStyle(element, null)[attr];//谷歌、火狐支持
      } else {
        return element.currentStyle[attr];//IE8支持
      }
    }//上面的可以简写成3元表达式
```

#### 变速动画函数

```javascript
//获取任意一个元素的任意一个样式属性的值
    function getStyle(element, attr) {
      if (window.getComputedStyle) {
        return window.getComputedStyle(element, null)[attr];
      } else {
        return element.currentStyle[attr];
      }
    }
//动画函数
    function moveElement(element, json, fn) { //元素 json{属性名字:目标位置} fn回调函数
      clearInterval(element.timeId); //先清理定时器,保证每次都是同一个定时器
      element.timeId = setInterval(function() { //对象的属性唯一，不会重复创建定时器
        var flag = true; //假设默认全部到达位置
        for (var attr in json) { // 循环遍历json中的每个属性
          if (attr == "opacity") {
            //将返回的数字字符串*100转成数字，透明度放大100倍
            var current = getStyle(element, attr) * 100;
            //透明度放大100倍
            var target = json[attr] * 100;
            //动态步距，最后每一步走1像素，直到到达目标位置停下
            var step = (target - current) / 10;
            //0.1取1,-0.1取-1。应往绝对值大了取才会到
            step = step > 0 ? Math.ceil(step) : Math.floor(step);
            current += step;
            //缩小为原来大小
            element.style[attr] = current / 100;
          } else if (attr == "zIndex") { //改变层级
            element.style[attr] = json[attr];
          } else { //普通属性
            //将返回的字符串转变为数字
            var current = parseInt(getStyle(element, attr));
            //目标位置
            var target = json[attr];
            //动态步距，最后每一步走1像素，直到到达目标位置停下
            var step = (target - current) / 10;
            //0.1取1,-0.1取-1。应往绝对值大了取才会到
            step = step > 0 ? Math.ceil(step) : Math.floor(step);
            current += step;
            //到最后每步走一像素，肯定不会超出
            element.style[attr] = current + "px";
          }
          //是否到达目标位置
          if (current != target) {
            flag = false; //只要有一个没到达目标就false
          }
        }
        if (flag) { //所有属性都到达目标
          clearInterval(element.timeId); //清理定时器
          if (fn) { //如果回调函数存在，则调用
            fn();
          }
        }
      }, 20);
    }
```

#### client系列

可视区域

clientWidth:可视区域的宽（没有边框），边框内部宽度
clientHeight:可视区域的高（没有边框），边框内部高度
clientLeft:左边边框的宽度
clientTop: 上面边框的宽度
clientX:可视区域的横坐标
clientY:可视区域的纵坐标

```javascript
document.onmousemove = function(e) {
      e = window.event || e;
      document.getElementById('im').style.left = e.clientX + "px";
      document.getElementById('im').style.top = e.clientY + "px";
    };
```

#### 阻止超链接跳转

```html
<a id="sk" href="javascript:void(0);"></a>
<!--第1种-->
<script>
document.getElementById("ak").onclick=function (e) {
    alert("hello");
    return false;//第2种
    e.preventDefault();//第3种
};
</script>
```

```javascript
onmousedown;// 鼠标按住
onmouseup;// 鼠标松开
```

```javascript
window.getSelection ? window.getSelection().removeAllRanges() :document.selection.empty();//鼠标按住滑动不选取内容
```

#### 隐藏元素

```javascript
//不占位
my$("dv").style.display="none";
//占位
my$("dv").style.visibility="hidden";
//占位
my$("dv").style.opacity=0;
//占位
my$("dv").style.height="0px";
my$("dv").style.border="0px solid red";
```

## 面向对象

### 构造函数与实例对象之间的关系

> 1、实例对象是通过构造函数来创建的----创建的过程叫实例化
> 2、如何判断对象是不是这个数据类型？
> ​	1）通过构造器的方式  实例对象.构造器(constructor)==构造函数名字
> ​	2）对象 instanceof 构造函数名字（推荐）

### 原型

通过原型来添加方法，解决数据共享，节省内存空间

```javascript
function Person(name, age) {
      this.name = name;
      this.age = age;
    }
    Person.prototype.eat = function() {//原型
      console.log("hello world");
    };
//简单的原型写法
Person.prototype = {
    constructor:Person,//需要手动添加构造器
    height:"188",
    study:function () {
        console.log("hello");
    }
};
```

> 实例对象中有个属性，`__proto__`，也是对象，叫原型，不是标准的属性，供浏览器使用
> 构造函数中有个属性，prototype，也是对象，叫原型，是标准属性，供程序员使用
>
> 原型--->`__proto__`或者是prototype，都是原型对象
>
> 原型的作用：数据共享，节省内存空间
>
> 实例对象使用的属性和方法，现在实例中查找，再到实例对象的`__proto__`指向的原型对象prototype中找，直到找不到返回undefined。有则返回
>
>要对实例对象添加属性或者方法可以通过原型添加

把局部变量给window就变成全局变量

```javascript
(function (window){
    var num=10;
    window.num=num;
})(window);
```

bind()函数

在函数后添加，改变函数内的this为指定的对象

```javascript
var that=null;
function Person(name){
    that=this;
    this.name=name;
}
function Animate(){
   console.log(this.name);//此时this为Person的实例对象
}.bind(that)
```

原型链：

是一种关系，实例对象和原型对象之间的关系，关系是通过原型（`__ proto __`)来联系的

原型的指向是可以改变的，通过改变构造函数的原型（prototype）

只要是实例对象就有`__proto__`，只要是`__proto__`，就指向某个构造函数的原型

原型最终指向null
Person的实例对象的`__proto__`指向Person的prototype，Person的prototype的`__proto__`指向Object的prototype，Object的prototype的`__proto__`指向null

### 继承

 首先继承是一种关系，类（class）与类之间的关系，JS中没有类，但是可以通过构造函数模拟类，然后通过原型来实现继承
继承也是为了数据共享，js中的继承也是个为了实现数据共享

原型的作用：
1、数据共享、节省内存空间
2、为了实现继承

```javascript
function Person(age){
    this.age=age;
}
function Student(score){
    this.score=score;
}
//原型继承
Student.prototype=new Person(10);//继承了人的所有属性、方法
Student.prototype.study=function (){
    console.log("studing!");
};//应该放在改变指向之后！
//缺陷，age等属性值只能通过重新赋值改变
//更新----借用构造函数：函数名字.call(this,...);
function Student(age,score){
    Person.call(this,age);//继承了Person的属性及方法
    this.score=score;
}
//缺陷：父级对象（Person）的方法不能继承
//更新----组合继承：原型继承+借用构造函数继承
function Student(age,score){
    Person.call(this,age);//继承了Person的属性及方法
    this.score=score;
}
Student.prototype=new Person();//不传值！
//还有拷贝继承
//直接把对象需要的属性和方法循环遍历复制到另一个对象
```

## 高阶函数

### 函数的声明和函数表达式的区别

```javascript
if (true) {
      function f1() {
        console.log("hello");
      }
    } else {
      console.log("hello world");
    }
f1();//在if-else中，IE8会变成hello world！
//换成函数表达是就不会了
f1=function(){};
```

### this的指向

```javascript
//普通函数的this
function f1(){
    console.log(this);
}
f1();//window--->window.f1()
//定时器的this
setInterval(function(){
    console.log(this);
},1000)//window--->window.setInterval()
//构造函数中的this---->实例对象
//对象.方法中的this---->实例对象
//原型对象中的方法的this---->实例对象
//=========================
//当严格模式下
"use strict";//严格模式
f1();//undefined
window.f1();//window
```

函数是对象，对象不一定是函数
对象中有`__proto__`，函数中有prototype
所有的函数都是Funtion的构造函数创建出来的实例对象

### apply和call

调用的时候，可以改变this的指向

```javascript
function f1(x, y) {
    console.log((x + y) + "====>" + this);
    return "这是函数的返回值";
}
f1(10, 20);
var obj = {
    age: 10,
};
f1.apply(obj, [10, 20]);//此时的this是obj
f1.call(obj, 10, 20);//此时的this是obj
//使用方法：参数的传递方式不一样,作用一样
//函数名字.apply(对象,[参数1,参数2,...]);
//方法名字.apply(对象,[参数1,参数2,...]);
//函数名字.call(对象,参数1,参数2,...);
//方法名字.call(对象,参数1,参数2,...);
```

### bind方法

复制的意思，参数可以在复制时传进去，也可以在复制之后调用时再传

```javascript
function Person(age) {
    this.age = age;
}
Person.prototype.say = function() {
    console.log("hello:===>" + this.age);
};

function Student(age) {
    this.age = age;
}
var per = new Person(10);
var stu = new Student(20);
var ff = per.say.bind(stu);
ff();
//使用语法
//函数名字.bind(对象,参数1,参数2...);---->返回值是复制之后的这个函数
//方法名字.bind(对象,参数1,参数2...);---->返回值是复制之后的这个方法
```

### 函数中的几个成员

函数中有name属性--->函数名
函数中有arguments属性--->实参的个数
函数中有length属性--->函数定义时形参的个数
函数中有caller属性--->调用该函数者

### 判断是否是某个类型

1、typeof 变量

2、变量 instanceof 类型

3、Object.prototype.toString.call(要判断的实例对象);

## 闭包

>闭包的概念:函数A中,有一个函数B(或对象),函数B(或对象)中可以访问函数A中定义的变量或者是数据,此时形成了闭包(这句话暂时不严谨)
>
>闭包的模式:函数模式的闭包,对象模式的闭包
>
>闭包的作用:缓存数据,延长作用域链
>
>闭包的优点和缺点:缓存数据

总结：如果想缓存数据，就把这个数据放在外层函数和里层函数之间

局部变量是在函数中，函数使用结束后，局部变量就会被自动的释放
闭包后，里面的局部变量的使用作用域链就会被延长

```javascript
function f1() {
    var num=parseInt(Math.random()*10+1);
    return function () {
        console.log(num);
    }
}
var f2=f1();
f2();
f2();
f2();//每次获取的都是相同的随机数
```

## 沙箱

就是一个环境，也叫黑盒，在这个环境中模拟外面真实的开发环境，完成需求，效果和外面的真实环境是一样的。

避免命名冲突

```javascript
(function (){
    
}());//相当于一个沙箱，里面的局部变量在这个函数相当于全局变量
//全局变量只有在页面关闭之后才会释放，所以多使用局部变量
//好处：将一段功能相似的代码封装在沙箱（自调用函数）中，可以避免变量名冲突，又可以节省内存
```

## 递归

函数中调用自己，此时就是递归，递归一定要有结束的条件

递归效率很低

```javascript
function getFi(x) {
    if (x==1||x==2) {
        return 1;
    }
    return getFi(x-1)+getFi(x-2);
}
console.log(getFi(12));
```

## 浅拷贝

把一个对象中的所有内容复制一份给另一个对象，直接复制或者说就是把一个对象的地址给了另一个对象，他们的指向相同 slice(0) ,[...arr2] = arr1,...属于浅拷贝

`注意：`如果 子元素 含有复杂对象 深拷贝 会 引用它的地址，而不是创建新的子元素

```javascript
// eg.
// 如果 [[0,0],[1,1],[2,2]] 这种 子元素属于复杂对象的 ，浅拷贝只会复制第一层 
// 里面的 [0,0] 、[1,1]、[2,2] 是引用原来的数组的地址，改变 [0,0] ==>[1,0] 原来的数组也会改变
```

## 深拷贝

把一个对象中的所有属性和方法，一个个找到，并且在另一个对象中开辟相应的空间，一个个的存储到另一个对象中

`注意：`如果 子元素 含有复杂对象 深拷贝 会 创建新的子元素 ，而不是引用它的地址

## 正则表达式

作用：匹配字符串的
组成：有元字符或者是限定符组成的一个式子

### 元字符：

> .	表示除\n以外的任意一个字符
> []	表示一个范围：[0-9a-z] 或者把元字符的意义去掉：[.]
> |	表示或者
> ()	分组，提升优先级
>
> ()*	前面的表达式出现了0次或多次 {0,}
> ()+	前面的表达式出现1次或多次 {1,}
> ？	前面的表达式出现0次或1次;或者阻止贪婪模式 {0,1}
> ^	以...开始，或者取非 ^[0-9]以数字开头  [ ^0-9]非数字
> $	以...结束
>
> \d	[0-9]
> \D	非数字
> \s	空白符
> \S	非空白符
> \w	非特殊符号[0-9a-zA-Z_]
> \W	特殊符号
> \b	单词边界

邮箱

```javascript
^[0-9a-zA-Z_.-]+[@][0-90-9a-zA-Z_.-]+([.][a-zA-z]+){1.2}$//严格模式
```

### 创建正则表达式

```javascript
//1、通过构造函数创建对象
var str="hello world";
var reg=new RegExp(/\d{5}/);
var falg=reg.test(str);//返回true或false
reg.exec(str);//与下面的match类似，不过调用一次只能输出一个.若reg里添加g，就是全局;要多次输出需多次调用console.log(reg.exec(str));
str.match(reg);//返回str里面符合reg条件的字符串,只匹配一次
str.match(/\d{5}/g);//g表示全局，匹配所有,返回符合条件的数组
var arr=str.match(/(\d{4})[-](\d{2})[-](\d{2})/g);
console.log(RegExp.$3);//获取第3组，每个小括号为一组(数左括号是第几个)，通过正则表达式对象.$
str.replace(/1/g,"2");//将所有1替换成2
str.replace(/h/gi,"s");//将所有h,并忽略大小写，替换成s
//2、字面量的方式
var reg=/\d{5}/;
var falg=reg.test(str);
```

### 中文与编码转换

控制台

```javascript
escape("中文");
unescape("%u4E2D%u6587");
//中文范围：[\u4e00-\u9fa5]
```

## 数组与伪数组

真数组的长度是可变的，可以使用数组中的方法，真数组的`__proto__`指向的是Array的prototype
伪数组的长度是不可变的，不可以使用数组中的方法

```javascript
var arr=[0,1,2,3];//真数组
var arr={
    0:0,
    1:1,
    2:2,
    3,3,
    length:4
}//伪数组
```
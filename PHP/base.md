# Apache

## httpd.conf详解

httpd.conf是apache的主配置文件

### 1、ServerRoot

apache安装位置
ServerRoot "D:/wamp/apache2.4"

### 2、Listen 

apache监听端口号

### 3、ServerAdmin

管理员邮箱

### 4、ServerName

域名

### 5、DocumentRoot

用于设置站点的根目录

### 6、Diretory配置段

主要用于对站点根目录的特性的设置

```php
ServerName www.one.com
DocumentRoot "D:/wamp/apache2.4/htdocs"
<Directory "D:/wamp/apache2.4/htdocs">
	DirectoryIndex #用于设置默认首页
    Options Indexes FollowSymLinks MultiViews #是否列出目录结构，当请求文件不存在时，列出目录结构,加#号注释为否
    AllowOverride All或None #是否开启外部配置文件  关闭：不写或者后面写none
    Order #用于配置此目录的访问权限
    语法一：Order deny,allow #如果没有明确的拒绝则允许
    deny from ip地址
    allow from all
    语法二：Order allow,deny #如果没有明确的允许则全部拒绝
    allow from ip地址
    deny from all
    Require All Granted #所有的请求都需授权
</Directory>
```

## httpd.exe作用

使用终端打开

> httpd.exe -k start/stop/restart  apache服务的启动/停止/重启
> httpd.exe -t 配置文件的语法检查

## 虚拟主机配置

使用一个apache软件，配置多个主机（域名）

1、在主配置文件中开启扩展配置文件(Include conf/extra/httpd-vhosts.conf)，去掉#

2、配置conf/extra/httpd-vhosts.conf文件，跟在主配置文件的写法的一样

放在<VirtualHost *:80> </VirtualHost>里

3、把主配置文件原来的配置注释掉

## 外部配置文件

除主配置文件conf/httpd.conf和扩展配置conf/extra外的配置文件

创建.htaccess放在站点根目录下，优先级比前两个高，并且不用重启apache

配置跟前两个一样

### 自定义错误提示页面

语法:
ErrorDocument 错误代码(404) /错误提示文件

# MySQL

## 安装过程

> cd C:\Develop\mysql\bin
>
> mysqld --initialize --user=mysql --console
> 初始化
>
> mysqld --install MySQL
> 安装名字为MySQL的服务
>
> net start MySQL
> 启动服务
>
> mysql -u root -p
>
> set password for root@localhost = password('123456');
> 更改密码

# PHP

## apache加载PHP功能模块

在apache主配置文件中，加载php模块
LoadModule php5_module "D:/wamp/php5.6/php5apache2_4.dll"

在apache主配置文件中，设置php文件扩展名
AddType Application/x-httpd-php .php

在apache主配置文件中，设置php配置文件
php.ini配置文件的设置不影响php运行，但是会影响php的一些扩展功能，php.ini 由php.ini-development或php.ini-production更名获得
PHPIniDIR "D:\wamp\php5.6\php.ini"

## 语法规则

主配置文件中设置了php文件的扩展名.php，所以所有的php文件的扩展名都是.php

php文件必须通过域名访问，文件中（包括路径）不能包括中文

php每一条语句后必须有';'

php中变量名区分大小写，其余函数名、方法名、类名都不区分大小写

## PHP标记

1、标准的
<?php ...?>

2、script格式

```php
<script language="php">
</script>
```

3、短格式(默认没有开启)
在php.ini中short_open_tag = on/off（开启短格式的标签）    
<? ... ?>

4、asp格式
在php.ini中asp_tag = on/off（开启asp格式的标签）
<% ... %>

## 注释

单行注释：

```php
//注释内容
```

多行注释：

```php
/*多行注释*/
```

## 变量

### 声明语法

$变量名=值; 
要使用也需带有$

### 删除变量

unset($变量名)； 

### 可变变量

```php
<?php 
    $v="age";
	$age=20;
	echo $v;//age
	echo $$v;//$age--->20
?>
```

### 预订变量

```php
$_GET	用于接受前台表单使用get方式提交的数据
$_POST	用于接受前台表单使用get方式提交的数据
$_REQUEST	用于接受前台表单使用get或post方式提交的数据
$_SERVER	记录服务器端与客户端的相关信息
$_COOKIE	一种会话技术
$_SESSION	一种会话技术
$_FILES		用于记录用户上传的文件信息
$GLOBAL		用于记录全局变量
```

### 内存结构

#### 1、栈区

保存的是变量名（术语称之为引用）
特点：对于cpu来说，读写速度是最快的

#### 2、数据段

数据全局区：存储的是简单的数据，例如：整形、浮点型、布尔值
数据静态区：存储静态变量和常量

#### 3、堆区

存储的是"复杂"的数据，字符串、数组、对象

#### 4、代码段

存储的是源代码对应的机器指令

#### 5、输出缓存

只要是遇到输出命令，例如：echo、print、这些指令都会将所要输出数据放在输出缓存中

### 数据存储方式：

```php
$v1=10;
$v2=$v1;//找到v1存储的内容10，复制一份给v2，在数据区申请空间存v2的值
```

### php嵌入到html中

php仅处理<?php?>标签内的代码，js、html、css对php来说都是字符串

### php中变量的传值方式

#### 1、赋值传值

使用一个变量a为另一个变量b赋值时，传递的是变量a的值

```php
<?php
    $v1=10;
	$v2=$v1;
	$v2=20;
	echo $v1;//10
?>
```

#### 2、引用传值

使用一个变量a为另一个变量b赋值时，传递的是变量a的地址

```php
<?php
    $v1=10;
	$v2=&$v1;//传递的是v1的地址，&是地址符
	$v2=20;
	echo $v1;//20
?>//v1与v2指向同一个地址
```

#### 提示

js中不允许人为的更改传值方式，但是PHP中可以使用地址符'&'，来将赋值传值更改为引用传值（单向的），常量默认区分大小写，一般建议在命名时使用全大写形式

## 常量

常量一旦定义就不允许更改其值，也不允许删除，常量的值只能是基本数据类型(标量数据类型)

### 语法

1、define('常量名',值);

2、const 常量名=值;

### 区别

define语法可以在分支结构（if语句）中定义常量，const不行
define定义的常量可以自定义是否区分大小写;define('常量名',值,true);//不区分

### 判断常量是否存在及获取常量

```php
defined('常量名');//返回true或false
get_defined_constants();//获取所有常量，返回数组格式
```

### 魔术常量

```php
__FILE__	获取当前文件完整路径及文件名
__DIR__		获取当前文件路径
__LINE__	此代码所处的行号
__FUNCTION__	获取当前函数的函数名
__METHOD__		获取当前方法的方法名
__CLASS__		获取当前类的类名
__NAMESPACE__	获取当前空间的空间名
```

## 数据类型

1、标量(scalar)数据类型
int整型、float浮点、boolean布尔、string字符串

2、复合数据类型
array数组、object对象

3、特殊数据类型
null、resource资源类型、

### 整型

线性的整数
十进制、八进制（以0开头）、十六进制（以0x开头）
在显示时会自动转化为十进制

### 浮点类型

带有小数点的数

```php
$v1=3.5;//法1
$v2=1.25E-3;//法2
//注意小数间计算 得到的结果精度不精确
```

### 字符串类型

使用单引号或双引号括起来的0个或多个字符

#### 1、使用单引号定义的字符串

```能够被转义的字符有'\\'  \'```
单引号定义的字符串中的变量不能被解析

#### 2、使用双引号定义的字符串

```能够被转义的字符有 \"  \t  \r  \n  \\  \$```
双引号定义的字符串中的变量会被解析（变量名应用"{}"括起来，范围解析限定符）

#### 3、heredoc

也是使用双引号定义字符串的，定义大段的字符串

```php
//heredoc是变量名，可随意定义
$heredoc = <<<abc   
	...   
abc;//abc可换成任意字符，但必须相同，结束标记必须定格
```

### 布尔类型

只有true和false

### 数组类型

#### 1、索引数组

数组元素的下标是数值

#### 2、关联数值

数组元素的下标是字符串 

```php
echo $_SERVER['REMOTE_ADDR'];//不加引号会解析成常量
$V1="<h3>$_SERVER[REMOTE_ADDR]</h3>";//在字符串内不需再加引号，不会当成常量看待
$v2="<h3>{$_SERVER['REMOTE_ADDR']}</h3>";
//相当于字符串的拼接;
//"<h3>.$_SERVER['REMOTE_ADDR'].</h3>";php用点拼接字符串
```

### null类型

只有一个值，null

### 资源类型

resoure类型：也是一个特殊的变量，没有办法直接定义一个资源，必须使用php提供的获取资源的函数

### 数据类型的转换

#### 1、自动转换

```php
$v1=100;
$v2='100元';
echo $v1*$v2;//10000
//*号是算术运算符，会将字符串自动转换为数字类型
```

#### 2、强制转换

```php
(integer)变量	将其他数据类型转换为整型
(float)变量	将其他数据类型转换为浮点型
(array)变量	将其他数据类型转换为数组
(object)变量	将其他数据类型转换为对象
(string)变量	将其他数据类型转换为字符串
(boolean)变量	将其他数据类型转换为布尔值  '0'-->false  0.0-->false 未定义的变量-->false
```

### 数据类型的判断

is_int(v);    is_string(v);   is_bool(v);   is_scalar(v);标量   is_resource(v);....

isset(v)  判断变量是否设置值（判断是否为null）只有null或该变量不存在才false
empty(v)  判断变量v的值是否为"空"===>等效于false的值，如果是空则返回true
empty($dict['foo'])===!isset($dict['foo'])||$dict['foo']==false

## 运算符

### 字符串运算符

```.	对字符串进行拼接```
```.=	对字符串进行拼接```

如果拼接一个数值，数值与.之间要有空格，否则会解析成小数
```$str='$i的值为：'. 10```没有空格会被当作0.10

### 比较运算符

==	用于判断两个数的值是否相等 '10'==10 ---->true
===	同时判断变量的值与类型是否相等 '10'===10 --->false，类型不同

### 逻辑运算符

#### 逻辑与短路

&&	如果第一个操作数是false，后面的就不会操作

```php
$n =10;
false && ++$n;
echo $n;//10;++不执行
```

#### 逻辑或短路

||	如果第一个为true，后面的就不会操作

### 强制转换布尔

```php
$v1=10;
!!$v1;//true
```

and	逻辑与
运算规则与&&相同，唯一的区别是&&的优先级高于=；而and的优先级低于=

or	逻辑或
运算规则与||相同，唯一的区别是||的优先级高于=；而or的优先级低于=

### 条件运算符

三元运算符
表达式？表达式A：表达式B；

### 运算符的优先级

单、算、关、逻、条、赋、逗

### 错误控制运算符

#### 1、 

@	错误抑制符

作用是不显示错误信息

```php
$link=@mysqli_connect('127.0.0.1','root','123');
//在语句之前加了@之后即使有错误，也不会出现提示
```

#### 2、

php.ini中设置隐藏错误

全局不显示
display_errors=on/off

#### 3、

脚本级的错误控制仅限于当前的php脚本文件

ini_set()	主要用于在php脚本中来设置php.ini中的配置项
ini_set(配置项名,值)

```php
<?php
ini_set('display_errors','off');
//ini_get('配置项名');获取配置项
```

### 位运算符

&	按位与 两个操作数的二进制形式对应的位进行与运算

|	按位或

^	按位异或	相同为0,不同为1
可以用来互换两个数的数据

```php
<?php
    $v1=10;//01010
	$v2=20;//10100
		   //11110
	$v1=$v1 ^ $v2;
	$v2=$v1 ^ $v2;
	$v1=$v1 ^ $v2;
```

~	按位非

<<	左移
左侧溢出忽略，右侧补0，移一位×2

```>>	右移```
右侧溢出忽略，左侧补0，移一位/2

## 进制转换函数

decbin(v)；	十进制转二进制
dechex(v);	十进制转十六进制
decoct(v);	十进制转八进制

## php输出语句

echo	只能输出标量数据类型，将任何东西转换为字符串输出，true转为1,false转为空；数组不能转换--->报错
print();	同echo，区别：print只能输出一个，echo能输出多个用','分隔，print不是函数可以省略"()"
print_r();	可以输出标量及复合数据类型，但布尔值会被转换为1或空
var_dump();	主要是程序员进行代码调试，可以输出十分详细的信息（类型及值、字符串|数组长度）
sprintf();	用于格式化输出

```php
/*
sprintf(格式化字符串,数值1,数值2,...);
%b	二进制
%d	十进制
%o	八进制
%f	浮点
%x	十六进制
*/
echo sprintf('255的八进制为：%o',255);
```

## 字符集

ASCII	一个字符占据1个字节，127个英文
GBK	一个字符占据2个字节，中文
UTF-8	一个字符占据3个字节，世界词典

## 流程控制

### 分支结构

#### if语句

如果语句体只有一条语句，此语句的{}可以省略

```php
if(表达式A){
    语句体A
}elseif(表达式B){
    语句体B
}
```

#### switch语句

```php
switch(变量){
    case 值1:
		语句体1;
    	break;//break可以省略，但会继续执行下面的语句，直到结束或遇到break
    default:
		缺省语句体;
}
```

### 循环结构

#### for循环

```php
for(循环的控制变量初始化;表达式;循环控制变量的更改){
    //循环体
}
```

#### while循环

```php
//循环的控制变量初始化
while(表达式){//直到表达式成立
    //循环体
    //循环控制变量的更改
}
```

#### do while循环

```php
do{
    //循环体
}while(表达式)//直到表达式不成立
```

### 循环的结束与退出

#### continue

```php
continue [n];
/*n为整数，默认为1
n主要是用在循环嵌套的情况下
结束当前循环结构的本次循环，继续上n层循环结构的下一次循环
*/
```

#### break

```php
break [n];
/*n为整数，默认为1
n主要是用在循环嵌套的情况下
直接结束上n层循环结构循环
*/
```

### 流程控制语句的标签语法

```php+HTML
<tbody>
    <?php for($i=0;$i<4;$i++){?><!--将一个for语法放在两个php标签中-->
    	<tr>
            <td><?php echo $data[$i][0]?></td>
            <td><?php echo $data[$i][1]?></td>
            <td><?php echo $data[$i][2]?></td>
            <td><?php echo $data[$i][3]?></td>
    	</tr>
	<?php }?><!--}前一定要有空格-->
</tbody>
```

```php
标准语法：
<?php for($i=0;$i<4;$i++):?>//{换成:
<?php endfor?>//}换成endfor
简化语法：
<?php for($i=0;$i<4;$i++){?>
<?php }?>
//if while 都可以
```

### 文件载入

引入其他的html或php文件

#### require

```php
require(文件名);
require_once(文件名);
```

#### include

```php
include(文件名);
include_once(文件名);
```

在同一文件内，函数可以先调用后声明，变量不行；在编译的过程就已经存在于代码段
引入的文件如果有函数，记得先引入再调用

目的：
1、在php文件中获取数据，在html文件中显示数据，php引入html文件
2、在当前文件（php）中想使用另一文件（php）中的功能性代码

#### 引入路径的问题

在项目中，对于html文件，是不允许用户直接请求的，而是指向一个php文件，让php文件来引用这个html文件。
当一个php文件引入一个html文档时，html文件本身也会引入一些其他的文件，如：图片文件、css文件、js文件。这时会引发路径更改问题。**路径要以当前php文件所在的路径为起点**。被引用的html中应，如下写法：

```html
<body>
    <img src="./public/123.jpg" /><!--当前php所在的文件夹的相对路径-->
    <img src="http://www.localhost/code/public/123.jpg" /><!--以域名开头的绝对路径-->
</body><!--注意不能以盘符开头，会在用户的文件夹找-->
<!--include语句本身可以使用有关盘符（D、E、F）绝对路径-->
```

#### include与require的区别

include在引入文件时，被引入文件不存在则会报错，但程序还会继续向下执行
require在引入文件时，被引入文件不存在则中断程序

经验：
require一般用于引入php文件，因为php里面一般是功能性代码
include一般用于引入html文件

include_once、require_once每次在引入文件时，都会检查之前有没被引入过，有引入就不会再引入,只执行一次；而require和include则没有

```__FILE__、__DIR__```不会因为被引入而更改，它们是常量

## 错误处理

### 错误的触发

#### 系统错误的触发

程序员无法干涉系统错误，系统编译不通过自动触发

#### 自定义的错误

trigger_error();

```php
trigger_error(msg,type);
//msg:错误描述信息
//type:自定义错误的代码(E_USER_ERROR、E_USER_WARNING、E_USER_NOTICE)
if(!is_array($listArr)){
    trigger_error('参数类型不正确',E_USER_NOTICE);
}
//Notice：参数类型不正确 in ...
//使用错误处理机制的好处：可以将错误记录起来，默认会被记录到apache的/logs/error.log文件
```

## 函数

### 可变函数

将函数名赋值给变量

```php
function nxn($name){//形参有$
    echo "hello world $name";
}
$f=nxn;//根据赋值的不同，下面调用时的函数也不同
$f();
```

### 匿名函数

```php
function (){
    echo __FUNCTION__;//{closure}闭包
};//匿名函数必须以分号结尾
```

php匿名函数无法自调用！
主要用于赋值给一个变量，还可以用于某个函数的参数（作为回调函数）

### 函数的参数

#### 形参的默认值

```php
function showInfo($i=100){
    echo $i;
}
```

#### 形参的引用传值

```php
function showInfo(&$v1,&$v2){//不加&，是正常的赋值传值
    $v1=100;
    $v2=200;
    echo $v1,$v2;//100,200
}
$a=10;
$b=20;
showInfo($a,$b);
echo $a,$b;//100,200
//引用传值，即将$a,$b的地址传递给形参，形参在函数内部更改也会影响外部的值
```

### 相关参数（系统函数）

func_get_args();

用于获取实参，并以数组的形式返回

func_get_arg(index);

用于获取index下标指定的实参

func_num_args();

用于获取实参的个数

```php
function getNum(){
    echo '实参的个数为：',func_num_args();
    echo '第一个实参为：',func_get_arg(0);
    echo '所有参数为：',print_r(func_get_args());
}
getNum(10,20,30);
//另一种函数未知实参的个数
function getNum(...$args){//用于将实参以数组的形式保存在这个变量中
    print_r($args);
}
```

## 作用域

### 全局作用域与全局变量

在函数外部定义的变量，其作用域就是全局作用域，变量就是全局变量
php中，函数内部不能访问全局变量！

### 局部作用域与局部作用域

函数内部定义的变量，其作用域就是局部作用域，这个变量就是局部变量
php中，函数外也不能访问局部变量

php中的作用域，外部只能访问外部的，内部只能访问内部的

#### global关键字

1、通过传址的方式
通过形参加&引用传值的方式使内外部互通

```php
$i=10;
function showInfo(&$p){
	$p+=10;
    echo $p;
}
showInfo($i);//20
echo $i;//20
```

2、$GLOBALS

全局变量会自动保存在$GLOBALS中
```$_GET、$_POST、$GL OBALS ...超全局变量```

```php
$name='tom';
function showInfo(){
    echo $GLOBALS['name'];//通过GLOBALS可以实现内部访问外部的全局变量
}
showInfo();
```

3、global关键字

```php
//global $变量名;
$i=10;
function showInfo(){
    global $i;//用于函数内，不能用于赋值，只能声明；用于获取外部全局变量或将内部局部变量变为全局变量
    echo $i;
}
showInfo();
/*
在函数内部创建一个与函数外部同名的变量的引用；如果外部没有这样的同名变量，会在外部创建一个同名的变量
global只能获取全局变量，一个函数内的无法global另一个函数内的变量
*/
```

## 静态变量

在函数内部使用static的变量
静态变量在函数多次被调用时，只会被初始化一次，并且静态变量的值并不会随着函数执行后被销毁
在函数的下一次调用时，仍然可以访问其值

```php
function showInfo(){
    static $i=1;//变量i的值存储在数据段静态区，函数执行结束后，i不会被销毁；由于static存在，只会初始化一次
    $i++;
    echo $i;
}
showInfo();//2
showInfo();//3
showInfo();//4
```

## 系统函数

### 日期时间函数

time();
用于获取当前时间的时间戳，单位秒；时间戳就是从时间原点到现在的一个秒数
默认格林威治时间

设置时区
1  date_default_timezone_set('PRC');
2  配置文件php.ini   date.timezone=PRC

mocrotime(); 毫秒（秒为单位）+秒；不重要

data();
用于格式化时间

```php
data(format[,time]);
echo data('Y-m-d H:i:s');//遇到以下字符自动转换，可以Y年m月之类的格式
//Y--4位年份;m--月份，两位;d--日期;H--24小时制;i--分钟;s--秒
```

mktime();
获取指定时间的时间戳

```php
mktime(时，分，秒，月，日，年);
```

strtotiem();
将有格式的时间字符串转换为时间戳

## 字符串

### 字符串的长度

1、strlen(变量);//计算的是字节数，对于汉字等多字节字符不友好

2、多字节字符的支持

在php.ini中打开 extension=php_mbstring.dll，打开之后就可以

```php
$str='hello你好';
//php中对于宽字符集的处理都是在前面加mb_的函数
//但是需要加载php_mbstring.dll
//在php的安装目录创建一个php.ini
//将extension_dir="绝对路径"
//extension=php_mbstring.dll解开注释
//修改apache配置文件加载php.ini默认位置
//PHPIniDir "绝对路径"
mb_strlen($str,'utf-8');//7
mb_string(变量);
```

### 查找并截取函数

strstr(str,substr);
用于在字符串str中查询子字符串substr首次出现的位置，并截取到最后一个字符

strrchr(str,substr);
用于在字符串str中查询子字符串substr最后一次出现的位置，并截取到最后一个字符

### 查找

strpos(str,substr);
用于在字符串str中查询子字符串substr首次出现的位置

strrpos(str,substr);
用于在字符串str中查询子字符串substr最后一次出现的位置

### 分割

explode(分割符,str);
将指定的分割符分割字符串str，并返回有每一部分组成的数组

### 替换

str_replace(search,rep,str);
在字符串str中，查找search部分，并替换成rep，返回新字符串

### 大小写转换

strtoupper($str);

strtolower($str);

### 去除指定字符

trim(str[,substr]);
用于将字符串str两侧的字符串substr去除，如果substr省略表示去除空格

ltrim(str[,substr]);针对左
rtrim(str[,substr]);针对右

### pathinfo

pathinfo(path[,option]);

path是一个含文件路径的字符串，返回数组
option用于获取路径inxi中指定的部分：PATHINFO_DIRNAME、PATHINFO_BASENAME、PATHINFP_EXTENSION、PATHINFO_FILENAME
用于获取一个文件的路径信息（文件名、文件夹、文件名、扩展名）

### md5

md5(str);
对于str字符串进行加密，得到32位长度的字符串

### htmlspecialchars

htmlspecialchars(str);
将字符串中的大于号小于号转换为相应的字符实体 
``` < &lt;   > &gt;```

```php
$img="<img src='123.jpg' />";
echo $img;//输出图片
$img=htmlspecialchars($img);
echo $img;//输出<img src='123.jpg' />
$img=htmlspecialchars_decode($img);//转换回来
```

## 数组

### 数组的分类

1、索引数组

2、关联数组

### 数组的创建

1、索引数组的创建

```php
//显示创建
$arr = array(10,20,30,40,50);
$arr = [10,20,30,40];//低版本php此法不行
//隐式创建
$arr = array();
$arr[0] = 10;
$arr[2] = 30;
$arr[1] = 20;//php中数组的下标可以不连续
```

2、关联数组的创建

```php
$arr = array(键名=>键值,键名=>键值,...);
$arr = [键名=>键值,键名=>键值,...];//低版本php此法不行
$arr = array();
$arr['id']=10;
//php中，数组元素由键名（下标）和键值组成
```

### 数组长度

count($arr);
用于获取数组的长度

### 数组的指针

```php
current($arr);	用于当前指针所指向的元素的键值
key($arr);	用于当前指针所指向的元素的键名
next($arr);	用于数组的指针下移
prev($arr);	用于数组的指针上移
reset($arr);	用于将数组的指针重置（归位,默认位于第1个元素）
end($arr);	用于将数组的指针移到最后一个元素
```

### 数组的遍历

1、for循环
是使用循环控制变量来模拟下标的方式来遍历数据，只能遍历连续或有规律的下标（键名）

2、foreach

```php
foreach($arr as [$key =>]$value){//$key、$value是一个变量变量名可以自定义
    //循环体
}//通过指针遍历
```

while-each-list遍历

each($arr);
each(['id'=>'a','age'=>20]);
================['0'=>'a','id'=>'a','1'=>20,'age'=>20]
将数组arr的一个元素，放入一个由索引和键名两种格式组成的新数组

list();
list($k,&v)=[0=>'hello',1=>'world'];
将hello赋值给k；world赋值给v

### 数组操作常用的函数

#### 获取数组元素的键名和键值

array_keys();
获取数组元素所有键名，返回数组

array_values();
获取数组元素的所有键值，返回数组

#### 判断键名与键值是否存在

array_key_exists(key,array);
判断某个键名是否存在数组中，存在返回true

in_array(value,array);
判断某个键值是否存在与数组中，存在返回true

isset($arr['key']);
不仅可以用来判断是否存在键,还可以用来判断变量是否存在

### 数组的追加

array_push();

$arr[] = 'new value';

### 数组的合并

array_merge(array1,array2,...);

### 数组的排序

sort();
按数组的键值进行升序排序

rsort();
按数组的键值进行降序排序

### extract

用于解压数组，关联元素转换为以键名为变量名的变量，转换不成功就不转换（数字不能做变量名）

### 排序算法

#### 冒泡排序

```php
$arr=[20,215,0,5,151,522,1];
$len=count($arr);
for($i=1;$i<$len;$i++){
    for($j=0;$j<count($arr)-$i;$j++){
        if($arr[$j]>$arr[$j+1]){
            $tmp=$arr[$j];
        	$arr[$j]=$arr[$j+1];
        	$arr[$j+1]=$tmp;
        }
    }
}
```

#### 插入排序

```php
$arr=[20,215,0,5,151,522,1];
$len=count($arr);
for($i=1;$i<$len;$i++){
    $target=$arr[$i];
    for($j=$i-1;$j>=0;$j--){
        if($arr[$j]>$arr[$j+1]){
        	$arr[$j+1]=$arr[$j];
        	$arr[$j]=$target;
        }
    }
}
```

### 查找算法

#### 顺序查找法

按顺序一个个比较

#### 二分查找法

前提：数组一定要有序且元素不能重复

```php
function findNum($search,$arr){
    $l=0;
	$r=count($arr)-1;
	while($l<$r){
   		$m=ceil(($l+$r)/2);
    	if($arr[$m]==$search){
        	return $m;
        }else if($search>$arr[$m]){
            $l=$m+1;
        }else if($search<$arr[$m]){
            $r=$m-1;
        }
    return -1;
	}
}
```


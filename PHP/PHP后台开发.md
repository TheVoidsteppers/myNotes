数据渲染 表单处理 状态保持

## 表单处理

> 1. 接收并校验
> 2. 持久化
> 3. 响应

```php+HTML
<!--
1. 必须有 form 标签
2. form 必须指定 action 和 method
不设置 action 默认是当前页面 （必须设置，因为兼容）
不设置 method 默认是 get
3. 表单元素（表单域）必须有 name (如果希望被提交的情况)
4. 表单中必须有一个提交按钮
-->
<form action="11-foo.php" method="post">
<!--
一般为了便于维护action指向自己,考虑到鲁棒性要好
action="<?php echo $_SERVER['PHP_SELF']; ?>?id=<?php echo $user['id']; ?>"
post方式提交表单附带url参数的方式
如果表单中有文件域,必须设置一下几点
1 method="post" enctype="multipart/form-data"
2 <input type="file" name="img">  php用$_FILES获取
3 $_FILES['img'] =$avatar;//上传文件的name是img
4 $avatar['error']==UPLOAD_ERR_OK  代表上传成功
5 move_uploaded_file($avatar['tmp_name'],'./uploads' . $avatar['name']);保存文件,可以根据返回值判断是否保存成功；保存数据的文件记录的路径一定要以绝对路径保存,文件操作用相对路径
----
autocomplete="off" 关闭显示填充历史记录
-->
    <table border="1">
        <tr>
            <td>用户名</td>
            <td><input type="text" name="username"></td>
        </tr>

        <tr>
            <td>性别</td>
            <td><label><input type="radio" name="gander" value="male">男</label></td>
            <td><label><input type="radio" name="gander" value="female">女</label></td>
            <!--
			提交时,以name为键名,value为键值(不写默认为on)；要设置不同的value才能辨别
			label标签当点击文字也能选中
			-->
        </tr>
        <tr>
            <td>密码</td>
            <td><input type="text" name="password"></td>
        </tr>
        <tr>
            <td><input type="checkbox" name="fun" value="baskball">篮球</td>
            <td><input type="checkbox" name="fun" value="football">足球</td>
            <td><input type="checkbox" name="fun" value="earth">地球</td>
            <!--复选框
			要有name,服务器才能获取值；没有选中也不会提交数据,选中会提交on,可以设置value值更改
			多选时,由于name相同,会有覆盖的问题:fun[](是一个空数组)；或者取不同的name
			-->
        </tr>
        <tr>
            <td></td>
            <!-- accept设置上传文件类型 也可以.tct,.doc....这种写法 -->
            <!-- multiple可以上传多个文件,name必须为数组-->
            <td><input type="file" name="images[]" accept="image/*" multiple></td>
        </tr>
        <tr>
            <td></td>
            <!-- input: type内填submit image -->
            <!-- button 默认type为submit-->
            <td><button>登录</button></td>
        </tr>
    </table>
</form>
```

php.ini中post_max_size配置 让服务端可以接受更大的请求体体积
php.ini中upload_max_filesize配置 让服务端支持更大的单个上传文件

## header函数

```php+HTML
<!--heade函数专门设置HTTP响应头,':'前不能加空格-->
<?php header('Content-Type: text/css; charset=utf-8'); ?>
<?php header('Content-Type: application/octet-stream'); ?><!--设置为浏览器无法解析的类型,用于用户下载该文件-->
<?php header('Content-Disposition: attachment; filename=abc.txt'); ?><!--与上面的一起使用,设置下载的文件名-->
<!--设置文件类型,默认为html-->
<?php header('Location: register.php'); ?>
<!--跳转到register.php文件,重定向-->
```

## JSON

```php
//HTTP只能传递字符串，数组等类型数据不能传递，服务端要返回结构化数据一般转为json格式的字符串给客户端

//json是数据的表述手段,不是存储手段
//1 json 中 属性名称必须用双引号包裹
//2 json 中 字符串必须用双引号包裹
//3 json 中 不允许使用注释
//4 json 没有undefined
<?php
    //获取的是json格式的字符串，也属于json
    $contents=file_get_contents('storage.json');
	//反序列化:将json格式的字符串转化为对象；反序列化时,默认转换为php中stdClass类型的对象
	//如果要转为关联数组时,加上true
	$data=json_decode($contents,true);
```

## PHP操作MySQL

```php
// 查询数据的查询语句
// 1. 建立与数据库服务器之间的连接
$connection = mysqli_connect('127.0.0.1', 'root', '123456', 'demo2');
// 1. 必须在查询数据之前
// 2. 必须传入连接对象和编码
mysqli_set_charset($connection, 'utf8');
//或 mysqli_query($connection, 'set names utf8;');
if (!$connection) {
  // 连接数据库失败
  exit('<h1>连接数据库失败</h1>');
}

// 基于刚刚创建的连接对象执行一次查询操作
//id是唯一的，但是where是全表检索，加上limit 1使数据库找到就返回
$query = mysqli_query($connection, "select * from users where id={$id} limit 1;");
if (!$query) {
  exit('<h1>查询失败</h1>');
}

// 遍历结果集
while ($row = mysqli_fetch_assoc($query)) {
  var_dump($row);
}

// 释放查询结果集
mysqli_free_result($query);

// 炸桥 关闭连接
mysqli_close($connection);
```

```php
// 增删改数据的查询语句
// 1. 建立与数据库服务器之间的连接
$connection = mysqli_connect('127.0.0.1', 'root', '123456', 'demo2');
if (!$connection) {
  // 连接数据库失败
  exit('<h1>连接数据库失败</h1>');
}

// 基于刚刚创建的连接对象执行一次查询操作
$query = mysqli_query($connection, 'delete from users where id = 5;');
if (!$query) {
  exit('<h1>查询失败</h1>');
}

// 如何拿到受影响行
// 传入的一定是连接对象
$rows = mysqli_affected_rows($connection);
var_dump($rows);

// 炸桥 关闭连接
mysqli_close($connection);
```

### 当从数据库获取中文为"?"时

```php
//从MySQL数据库获取到的中文为"?"时设置字符集

$connection = mysqli_connect('127.0.0.1', 'root', '123456', 'demo2');

// 1. 必须在查询数据之前
// 2. 必须传入连接对象和编码
mysqli_set_charset($connection, 'utf8');
// 或mysqli_query($connection, 'set names utf8;');

$query = mysqli_query($connection, 'select * from users;');
```

## Http

### 状态码

| 状态码 |              类别              |          原因短语          |
| :----: | :----------------------------: | :------------------------: |
|  1XX   |  Informational(信息性状态码)   |     接受的请求正在处理     |
|  2XX   |      Success(成功状态码)       |      请求正常处理完毕      |
|  3XX   |   Redirection(重定向状态码)    | 需要进行附加操作以完成请求 |
|  4XX   | Client Error(客户端错误状态码) |     服务器无法处理请求     |
|  5XX   | Server Error(服务端错误状态码) |     服务器处理请求出错     |

### Cookie

```php
// 只传递一个参数是删除
// 原理：设置过期时间为一个过去时间
setcookie('key');

// 传递两个参数是设置 cookie
setcookie('key1', 'value1');

// 传递第三个参数是设置过期时间
// 不传递就是 会话级别的 Cookie （关闭浏览器就自动删除）
setcookie('key2', 'value2', time() + 1 * 24 * 60 * 60);
//path=>设置cookie作用路径范围 /users设置只能在users目录下路径才能访问
setcookie('key3', 'value3', time() + 1 * 24 * 60 * 60, '/users');
//domain 设置cookie的作用域名范围，所有...的子域都能访问到 第二个空字符串
//secure false 代表不是只能https才能访问 true代表只有https才可以用
//httponly true只在发生http时使用，js中访问不到（document.cookie)
setcookie('key4', 'value4', time() + 1 * 24 * 60 * 60, '', '', false, true);

//获取cookie
var_dump($_COOKIE);
```

### Cookie进阶版

session：基于cookie基础之上的手段

```php
//相当于给用户的cookie是钥匙，数据存在服务器内，用户通过cookie访问获得实际数据
//找一个当前访问者的箱子，如果没有就创建新箱子，并且把箱子的钥匙（session_id）交给用户（cookie）
session_start();
$_SESSION['num'] = $num;//存
$_SESSION['num'];//取，不管是存还是取都要有session_start();
unset($_SESSION['num']);//清空
```

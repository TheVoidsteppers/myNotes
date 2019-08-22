# AJAX

AJAX是浏览器提供的一套API，可以通过JavaScript调用，从而实现通过代码控制请求与响应，实现网络编程
核心提供的类型：XMLHttpRequest
AJAX是浏览器内置的，无需引用

```javascript
var xhr = new XMLHttpRequest();
console.log(xhr.readyState);
// 0 => 初始化 请求代理对象

xhr.open('POST', 'time.php');//设置请求行，http版本自动设置
console.log(xhr.readyState);
// 1 => open 方法已经调用，建立一个与服务端特定端口的连接

xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');//设置一个请求头，多个的话再次调用该方法

xhr.send('key1=value1&kay2=value2');//send内填请求体，若以urlencoded格式设置，前面的请求头必须设置 Content-Type application/x-www-form-urlencoded
// 因为客户端永远不知道服务端何时才能返回我们需要的响应
// 所以 AJAX API 采用事件的机制（通知的感觉）
xhr.addEventListener('readystatechange', function () {
    switch (this.readyState) {
        case 2:
            // 2 => 已经接受到了响应报文的响应头，可以拿到响应头
            // console.log(this.getAllResponseHeaders());
            console.log(this.getResponseHeader('server'));
            // 但是还没有拿到响应体
            console.log(this.responseText);
            break;
        case 3:
            // 3 => 正在下载响应报文的响应体，有可能响应体为空，也有可能不完整
            // 在这里处理响应体不保险（不可靠）
            console.log(this.responseText);
            break;
        case 4:
            // 4 => 一切 OK （整个响应报文已经完整下载下来了）
            //xhr.onlaod()=function(){};//等价于readyState==4时的状态，html5支持
            console.log(this.responseText);
            break;
    }
})
```



| readyState |     状态描述     |                         说明                         |
| :--------: | :--------------: | :--------------------------------------------------: |
|     0      |      UNSENT      |        代理(XHR)被创建,但尚未调用 open() 方法        |
|     1      |      OPENED      |           open() 方法已经被调用,建立了连接           |
|     2      | HEADERS_RECEIVED | send() 方法已经被调用,并且已经可以获取状态行和响应头 |
|     3      |     LOADING      | 响应体下载中, responseText 属性可能已经包含部分数据  |
|     4      |       DONE       |       响应体下载完成,可以直接使用 responseText       |

AJAX操作一般是为了向服务器拿数据，因为http中约定报文内容只能是字符串（或二进制数据e.g：文件），为了表示结构化的数据，一般转为json
！！！返回json时，必须设置响应头  header('Content-Type', 'application/json');！！！
对于返回数据的地址，一般称之为API（接口）

## 模板引擎

art-template

```html
<!--script标签的特点
1、innerHTML永远不会显示在界面上
2、如果type!="text/javascript" 内部的内容不会作为JavaScript执行
建议写成 type="text/x-art-template" x后面跟上模板引擎的名字-->
<table id="demo"></table>
<script id="tmpl" type="text/x-art-template">
	{{each comments}}
    <!-- each 内部 $value 拿到的是当前被遍历的那个元素 -->
    <tr>
      <td>{{$value.author}}</td>
      <td>{{$value.content}}</td>
      <td>{{$value.created}}</td>
    </tr>
    {{/each}}
</script>
<script src="template-web.js"></script>
<script>
	var xhr = new XMLHttpRequest()
    xhr.open('GET', 'test.php')
    xhr.send()
    xhr.onreadystatechange = function () {
      if (this.readyState !== 4) return
      var res = JSON.parse(this.responseText)
      // 模板所需数据   res.data得到的是json数据转化得到的[{...},{...},...]
      var context = { comments: res.data }
      // 借助模板引擎的API 渲染数据
      var html = template('tmpl', context)
      document.getElementById('demo').innerHTML = html
 }
  </script>  
```

## JQuery

```javascript
//底层接口
$.ajax({
      url: 'time.php',
      type: 'post',
      beforeSend: function (xhr) {
        // 在所有发送请求的操作（open, send）之前执行
        console.log('beforeSend', xhr)
      },
      // 用于提交到服务端的参数，
      // 如果是 GET 请求就通过 url 传递
      // 如果是 POST 请求就通过请求体传递
      // data设置的是请求体的请求参数
      data: { id: 1, name: '张三' },
      // dataType用于设置响应体的类型 注意 跟 data 参数没关系！！！
      dataType: 'json',
      success: function (res) {
        // res => 拿到的只是响应体
        // res 会自动根据服务端响应的 content-type 自动转换为对象 这是 jquery 内部实现的！原生AJAX需自己转换
        // 一旦设置的 dataType 选项，就不再关心 服务端 响应的 Content-Type 了
        // 客户端会主观认为服务端返回的就是 JSON 格式的字符串
        console.log(res)
      }
      error: function (xhr) {
        // 隐藏 loading
        // 只有请求不正常（状态码不为200）才会执行
        console.log('error', xhr)
      },
      complete: function (xhr) {
        // 不管是成功还是失败都是完成，都会执行这个 complete 函数
        console.log('complete', xhr)
      }
    })
```

```javascript
//高度封装
//没有请求头，{ id: 1 }可省略
$.get('json.php', { id: 1 }, function (res) {
    console.log(res)
})
$.post('json.php', { id: 1 }, function (res) {
    console.log(res)
})
//对于服务端没设置响应头Content-Type:application/json时，采用以下方法
$.getJSON('json.php', { id: 1 }, function (res) {
    console.log(res)
})
```

## 全局事件处理

```javascript
$(document)
    .ajaxStart(function () {
    // 只要有 ajax 请求发生 就会执行
    $('.loading').fadeIn()
    // 显示加载提示
    console.log('注意即将要开始请求了')
})
    .ajaxStop(function () {
    // 只要有 ajax 请求结束 就会执行
    $('.loading').fadeOut()
    // 结束提示
    console.log('请求结束了')
})
```

## 同源策略

```javascript
//只有同源（协议、域名、端口）的地址才可以相互通过AJAX的方式请求
//同源策略：不同源地址之间，默认不能相互发送AJAX请求
//不同源地址之间如果需要相互请求，必须服务端和客户端配合才能完成

//请求不同源地址：跨域请求
//img link script iframe
//1、img 可以发送不同源地址之间的请求 无法拿到响应结果
var img = new Image()
img.src='...'
//2、link:链入一个文档，通过rel属性声明链入的文档与当前文档之间的关系
//rel='stylesheet'时，表示链入的是样式表文档，会自动下载该文件作为样式表
//与img相同
var link = document.creatElement('link')
link.rel = 'stylesheet'
link.src = '...'
document.body.appendChild(link)
//3、script
//借助可以发送不同源地址之间的请求的特性 但无法拿到响应结果
//但是可以借助于与服务器的配合 能够作为JS执行
var script = document.creatElement('script')
script.src = '...'
document.body.appendChild(script)//开始发送请求，注意是异步操作，需要时间获取数据，无法直接获得服务器发送过来的数据
//客户端定义函数
function foo(res){
    console.log(res)
}
```

```php
$json =json_encode(array('time'=>time()));
echo "foo({$json})";//服务器调用客户端设置的函数，并将数据作为参数返回
```

### jsonp

```javascript
//原理：通过script标签请求一个服务端的PHP文件 这个标签返回的内容是js，作用是调用客户端事先定义好的一个函数，从而获取服务端发送的数据
//解决客户端设置 需为不同请求（多个）回调函数名 问题
function jsonp (url, params, callback) {
    var funcName = 'jsonp_' + Date.now() + Math.random().toString().substr(2, 5)
   
    if (typeof params === 'object') {
        var tempArr = []
        for (var key in params) {
            var value = params[key]
            tempArr.push(key + '=' + value)
        }
        params = tempArr.join('&')
    }

    var script = document.createElement('script')
    script.src = url + '?' + params + '&callback=' + funcName
    document.body.appendChild(script)

    window[funcName] = function (data) {
        callback(data)

        delete window[funcName]
        document.body.removeChild(script)
    }
}
```

```php
//服务端设置
$conn = mysqli_connect('localhost', 'root', '123456', 'demo');
$query = mysqli_query($conn, 'select * from users');
while ($row = mysqli_fetch_assoc($query)) {
  $data[] = $row;
}

if (empty($_GET['callback'])) {
  header('Content-Type: application/json');
  echo json_encode($data);
  exit();
}

// 如果客户端采用的是 script 标记对我发送的请求
// 一定要返回一段 JavaScript
header('Content-Type: application/javascript');
$result = json_encode($data);
$callback_name = $_GET['callback'];
echo "typeof {$callback_name} === 'function' && {$callback_name}({$result})";
```

```javascript
//ajax设置jsonp
$.ajax({
    url: 'http://localhost/jsonp/server.php',
    dataType: 'jsonp',//只需设置响应体类型
    success: function (res) {
        console.log(res)
    }
})
```

```php
//AJAX跨域（CORS)
//只需在服务器设置允许
// 允许跨域请求
header('Access-Control-Allow-Origin: *');
```

## 异步上传

```javascript
// 异步上传图片==============================
// console.log(file.files[0]) // 获取文件
var formData = new FormData();
formData.append('image',file.files[0]);
// http://localhost/bxiang/admin.php/classroom/upload_video
var url = "../../../../admin.php/classroom/upload_video";
axios({
    method: 'post',
    url: url,
    data: formData,
    headers:{'Content-Type':'multipart/form-data'}
}).then(function(res){
    console.log(res.data.data)
});
// 异步上传图片==============================
```


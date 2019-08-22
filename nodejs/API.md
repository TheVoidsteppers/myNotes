# require

```javascript
/* require 是一个方法
 它的作用就是用来加载模块的
 在 Node 中，模块有三种：
    具名的核心模块，例如 fs、http
    用户自己编写的文件模块
      相对路径必须加 ./
      可以省略后缀名
      相对路径中的 ./ 不能省略，否则报错
    在 Node 中，没有全局作用域，只有模块作用域
      外部访问不到内部
      内部也访问不到外部
      默认都是封闭的
    既然是模块作用域，那如何让模块与模块之间进行通信
    有时候，我们加载文件模块的目的不是为了简简单单的执行里面的代码，更重要是为了使用里面的某个成员*/

/* require 方法有两个作用：
    1. 加载文件模块并执行里面的代码
    2. 拿到被加载文件模块导出的接口对象
   
    在每个文件模块中都提供了一个对象：exports
    exports 默认是一个空对象
    你要做的就是把所有需要被外部访问的成员挂载到这个 exports 对象中*/

```

# fs

>  fs 是 file-system 的简写，就是文件系统的意思，在 Node 中如果想要进行文件操作，就必须引入 fs 这个核心模块，在 fs 这个核心模块中，就提供了所有的文件操作相关的 API

## fs.readFile()

```javascript
/*读取文件
    第一个参数就是要读取的文件路径
    第二个参数是一个回调函数  
        成功
          data 数据
          error null
        失败
          data undefined没有数据
         error 错误对象*/
fs.readFile('./data/a.txt', function (error, data) {
  // <Buffer 68 65 6c 6c 6f 20 6e 6f 64 65 6a 73 0d 0a>
  // 文件中存储的其实都是二进制数据 0 1
  // 这里为什么看到的不是 0 和 1 呢？原因是二进制转为 16 进制了
  // 所以我们可以通过 toString 方法把其转为我们能认识的字符

  // 在这里就可以通过判断 error 来确认是否有错误发生
  if (error) {
    console.log('读取文件失败了')
  } else {
    console.log(data.toString())//用以下方法代替
  }
})
fs.readFile('./data/a.txt', 'utf8' function (error, data) {// 如果是string，自动转换为 utf8 而不是二进制
    
}
```

## fs.readdir()

```javascript
var fs = require('fs')
//以数组的方式返回该目录下的所有文件/文件夹
fs.readdir('D:/Movie/www', function (err, files) {
  if (err) {
    return console.log('目录不存在')
  }
  console.log(files)
})
```

# http

```javascript
var http = require('http')
var server = http.createServer()

// request 请求事件处理函数，需要接收两个参数：
//    Request 请求对象
//        请求对象可以用来获取客户端的一些请求信息，例如请求路径
//    Response 响应对象
//        响应对象可以用来给客户端发送响应消息
server.on('request', function (request, response) {
  console.log('收到客户端的请求了，请求路径是：' + request.url)

  // response 对象有一个方法：write 可以用来给客户端发送响应数据
  // write 可以使用多次，但是最后一定要使用 end 来结束响应，否则客户端会一直等待
  response.write('hello')

  // 告诉客户端，我的话说完了，你可以呈递给用户了
  //  response.write()的方式比较麻烦，推荐使用更简单的方式，直接 end 的同时发送响应数据
  //  response.end()可以接收 二进制 字符串 
  //  一次请求对应一次响应 response.end()后面的代码不执行
  response.end()
  // 在服务端默认发送的数据，其实是 utf8 编码的内容
  // 但是浏览器不知道你是 utf8 编码的内容
  // 浏览器在不知道服务器响应内容的编码的情况下会按照当前操作系统的默认编码去解析
  // 中文操作系统默认是 gbk
  // 解决方法就是正确的告诉浏览器我给你发送的内容是什么编码的
  // 在 http 协议中，Content-Type 就是用来告知对方我给你发送的数据内容是什么类型
  // res.setHeader('Content-Type', 'text/plain; charset=utf-8')
  response.end('hello 世界')
})

server.listen(3000, function () {
  console.log('服务器启动成功了，可以通过 http://127.0.0.1:3000/ 来进行访问')
})
```

# url

- 将 类似 /pinglun?name=jack&message=hello 这种请求转换为

```javascript
Url {
    protocol: null,
    slashes: null,
    auth: null,
    host: null,
    port: null,
    hostname: null,
    hash: null,
    search: '?name=jack&message=hello',
    query: 'name=jack&message=hello',
    pathname: '/pinglun',
    path: '/pinglun?name=jack&message=hello',
    href: '/pinglun?name=jack&message=hello' }
```

```javascript
var url = require('url')

var obj = url.parse('/pinglun?name=jack&message=hello', true)
//  第二个参数默认为false 
//  设为true时 query：{ { name: 'jack', message: 'hello' }}

// /pinglun?name=jack&message=hello
// 对于这种表单提交的请求路径，由于其中具有用户动态填写的内容
// 所以你不可能通过去判断完整的 url 路径来处理这个请求

// 结论：对于我们来讲，其实只需要判定，如果你的请求路径是 /pinglun 的时候，那我就认为你提交表单的请求过来了
```

# path

## path.basename()

```javascript
// 获取文件名
path.basename('c:/a/b/c/index.js')// index.js
path.basename('c:/a/b/c/index.js','.js')// index
```

## path.dirname()

```javascript
// 获取路径部分
path.dirname('c:/a/b/c/index.js')// c:/a/b/c
```

## path.extname()

```javascript
// 获取扩展名
path.extname('c:/a/b/c/index.js')// .js
```

## path.parse()

```javascript
// 将路径解析成一个对象
path.parse('c:/a/b/c/index.js')
/*
{ root: 'c:/',
  dir: 'c:/a/b/c',
  base: 'index.js',
  ext: '.js',
  name: 'index' }*/
```

## path.isAbsolute()

```javascript
// 判断是否是绝对路径
path.isAbsolute('c:/a/b/c/index.js')// true
```

## path.join()

```javascript
// 路径拼接
path.join('c:/a', 'b')// c:/a/b 
```


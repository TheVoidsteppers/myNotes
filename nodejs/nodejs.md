## Node.js 是什么

- JavaScript 运行时
- 既不是语言，也不是框架，它是一个平台

- Node.js 中的 JavaScript
  + 没有 BOM、DOM
  + EcmaScript 基本的 JavaScript 语言部分
  + 在 Node 中为 JavaScript 提供了一些服务器级别的 API
    * 文件操作的能力
    * http 服务的能力

《深入浅出Node.js》

## Node 中的 JavaScript

- EcmaScript
  + 变量
  + 方法
  + 数据类型
  + 内置对象
  + Array
  + Object
  + Date
  + Math

- 模块系统
  + 在 Node 中没有全局作用域的概念
  + 在 Node 中，只能通过 require 方法来加载执行多个 JavaScript 脚本文件
  + require 加载只能是执行其中的代码，文件与文件之间由于是模块作用域，所以不会有污染的问题
    * 模块完全是封闭的
    * 外部无法访问内部
    * 内部也无法访问外部
  + 模块作用域固然带来了一些好处，可以加载执行多个文件，可以完全避免变量命名冲突污染的问题
  + 但是某些情况下，模块与模块是需要进行通信的
  + 在每个模块中，都提供了一个对象：`exports`
  + 该对象默认是一个空对象
  + 你要做的就是把需要被外部访问使用的成员手动的挂载到 `exports` 接口对象中
  + 然后谁来 `require` 这个模块，谁就可以得到模块内部的 `exports` 接口对象
  + 还有其它的一些规则，具体后面讲，以及如何在项目中去使用这种编程方式，会通过后面的案例来处理

- 核心模块
  + 核心模块是由 Node 提供的一个个的具名的模块，它们都有自己特殊的名称标识，例如
    * fs 文件操作模块
    * http 网络服务构建模块
    * os 操作系统信息模块
    * path 路径处理模块
    * 。。。。
  + 所有核心模块在使用的时候都必须手动的先使用 `require` 方法来加载，然后才可以使用，例如：
    * `var fs = require('fs')`


## http相关

- 端口号
  + ip 地址定位计算机
  + 端口号定位具体的应用程序
- Content-Type
  + 服务器最好把每次响应的数据是什么内容类型都告诉客户端，而且要正确的告诉
  + 不同的资源对应的 Content-Type 是不一样，具体参照：http://tool.oschina.net/commons
  + 对于文本类型的数据，最好都加上编码，目的是为了防止中文解析乱码问题
- 通过网络发送文件
  + 发送的并不是文件，本质上来讲发送是文件的内容
  + 当浏览器收到服务器响应内容之后，就会根据你的 Content-Type 进行对应的解析处理


## 代码风格-无分号

- [JavaScript Standard Style](https://standardjs.com/)
- Airbnb JavaScript Style
- 《编写可维护的 JavaScript》

```javascript
//当你采用了无分号的代码风格的时候，只需要注意以下情况就不会有上面的问题了：
//    当一行代码是以：
//        ( [ ` 
//    开头的时候，则在前面补上一个分号用以避免一些语法解析错误。
//    所以你会发现在一些第三方的代码中能看到一上来就以一个 ; 开头。
//    ` 是 EcmaScript 6 中新增的一种字符串包裹方式，叫做：模板字符串
//  EcmaScript 6 的 ` 字符串中，可以使用 ${} 来引用变量
```

## nodemon

解决修改完代码自动重启

第三方命令行工具`nodemon`解决频繁修改代码重启服务器问题

nodemon是基于Node.js开发的第三方命令行工具

```shell
npm install --global nodemon
#使用 会监视文件变化，当文件方生变化，自动帮你重启服务器
nodemon app.js
```

## 服务端渲染

即在服务端使用模板引擎

```javascript
var http = require('http')
var fs = require('fs')
var template = require('art-template')

var server = http.createServer()
var wwwDir = 'D:/Movie/www'

server.on('request', function (req, res) {
  var url = req.url
  fs.readFile('./template-apache.html', function (err, data) {
    if (err) {
      return res.end('404 Not Found.')
    }
    // 1. 如何得到 wwwDir 目录列表中的文件名和目录名
    //    fs.readdir
    // 2. 如何将得到的文件名和目录名替换到 template.html 中
    //    2.1 在 template.html 中需要替换的位置预留一个特殊的标记（就像以前使用模板引擎的标记一样）
    //    2.2 根据 files 生成需要的 HTML 内容
    // 只要你做了这两件事儿，那这个问题就解决了
    fs.readdir(wwwDir, function (err, files) {
      if (err) {
        return res.end('Can not find www dir.')
      }

      // 这里只需要使用模板引擎解析替换 data 中的模板字符串就可以了
      // 数据就是 files
      // 然后去你的 template.html 文件中编写你的模板语法就可以了
      var htmlStr = template.render(data.toString(), {
        title: '哈哈',
        files: files
      })

      // 3. 发送解析替换过后的响应数据
      res.end(htmlStr)
    })
  })
})
server.listen(3000, function () {
  console.log('running...')
})
```

### 服务端渲染和客户端渲染的区别

> 客户端渲染不利于 SEO 搜索引擎优化
> 服务端渲染是可以被爬虫抓取到的，客户端异步渲染是很难被爬虫抓取到的
> 所以你会发现真正的网站既不是纯异步也不是纯服务端渲染出来的
> 而是两者结合来做的
> 例如京东的商品列表就采用的是服务端渲染，目的了为了 SEO 搜索引擎优化
> 而它的商品评论列表为了用户体验，而且也不需要 SEO 优化，所以采用是客户端渲染

## 处理客户端响应

```javascript
// app application 应用程序
// 把当前模块所有的依赖项都声明再文件模块最上面
// 为了让目录结构保持统一清晰，所以我们约定，把所有的 HTML 文件都放到 views（视图） 目录中
// 我们为了方便的统一处理这些静态资源，所以我们约定把所有的静态资源（images、css、js及模块）都存放在 public 目录中
var http = require('http')
var fs = require('fs')
var url = require('url')
var template = require('art-template')
// 创建服务器的简写方式
http
    .createServer(function (req, res) { //该函数会直接被注册为 server 的 request 请求事件处理函数
})
    .listen(3000, function () {
    console.log('running...')
})
```

### 对用户路径请求处理

```javascript
http
    .createServer(function (req, res) { 
    // 使用 url.parse 方法将路径解析为一个方便操作的对象，第二个参数为 true 表示直接将查询字符串转为一个对象（通过 query 属性来访问）
    var parseObj = url.parse(req.url, true)

    // 单独获取不包含查询字符串的路径部分（该路径不包含 ? 之后的内容）
    var pathname = parseObj.pathname
    })
```

### 控制用户访问

```javascript
// 哪些资源能被用户访问，哪些资源不能被用户访问，我现在可以通过代码来进行非常灵活的控制
// /public 整个 public 目录中的资源都允许被访问
if (pathname.indexOf('/public/') === 0) {
    // 例 /public/css/main.css
    // 统一处理：
    //    如果请求路径是以 /public/ 开头的，则我认为你要获取 public 中的某个资源
    //    所以我们就直接可以把请求路径当作文件路径来直接进行读取
    fs.readFile('.' + pathname, function (err, data) {
        if (err) {
            return res.end('404 Not Found.')
        }
        res.end(data)
    })
```

### 服务器让客户端重定向

```javascript
// 如何通过服务器让客户端重定向？
//    1. 状态码设置为 302 临时重定向
//        statusCode
//    2. 在响应头中通过 Location 告诉客户端往哪儿重定向
//        setHeader
// 如果客户端发现收到服务器的响应的状态码是 302 就会自动去响应头中找 Location ，然后对该地址发起新的请求
// 所以你就能看到客户端自动跳转了
res.statusCode = 302
res.setHeader('Location', '/')
res.end()
```

## 模块系统

使用Node编写应用程序主要是在使用：

- EcmaScript语言
- 核心模块
- 第三方模块
- 自定义模块

### 什么是模块化

- 文件作用域
- 通信规则
  + 加载 require
  + 导出

### CommonJS模块规范

在 Node 中的 JavaScript 有一个概念：模块系统

- 模块作用域
- 使用 require 方法加载模块
- 使用 exports 接口对象用来导出模块中的成员

#### 加载 `require`

语法

```javascript
var 变量名 = require('模块')
```

两个作用：

- 执行被加载模块中的代码
- 得到被加载模块中的 `exports`导出接口对象

#### 导出`exports`

- Node中是模块作用域，默认文件中所有成员只在当前文件模块有效
- 对于希望可以被其他模块访问的成员：就需要把这些公开成员都挂载到`exports`接口对象中

导出多个成员：

```javascript
exports.num = 123
```

导出单个成员(函数、字符串)：

如果一个模块需要直接导出某个成员，而非挂载，这时必须使用下面的方式

```javascript
function add(){}
module.exports = add 
module.exports = 'hello'//只会返回一个，后者会覆盖前者
```

#### 原理

```javascript
// 在 Node 中，每个模块内部都有一个自己的 module 对象
// 该 module 对象中，有一个成员叫：exports 也是一个对象
// 也就是说如果你需要对外导出成员，只需要把导出的成员挂载到 module.exports 中
var module = {
    exports: {
        foo: 'bar',
        add: function
    }
}

// 我们发现，每次导出接口成员的时候都通过 module.exports.xxx = xxx 的方式很麻烦，点儿的太多了
// 所以，Node 为了简化你的操作，专门提供了一个变量：exports 等于 module.exports
var exports = module.exports
// 两者一致，那就说明，我可以使用任意一方来导出内部成员

// 当一个模块需要导出单个成员的时候
// 直接给 exports 赋值是不管用的
exports = {}
//给 exports 赋值会断开和 module.exports 之间的引用 复杂数据是引用
//但最后 return 的是 module.exports
//所以exports再怎么赋值也不会影响到return的结果

// 同理，给 module.exports 重新赋值也会断开
// 这里导致 exports !== module.exports
module.exports = {
    foo: 'bar'
}
//之后exports再怎么赋值也不会影响到return的结果

// 谁来 require 我，谁就得到 module.exports
// 默认在代码的最后有一句：
// 一定要记住，最后 return 的是 module.exports
// 不是 exports！
// 所以你给 exports 重新赋值不管用，
return module.exports
```

#### require 方法加载规则

- 优先从缓存加载

  如果要加载的b模块已经在加载a模块时加载了，那么就不会重复加载

- 判断模块标识

  + 核心模块

    * 核心模块的本质也是文件
    * 核心模块文件已经被编译到了二进制文件中了，我们只需要按照名字来加载就可以了

  + 第三方模块

    * 凡是第三方模块都必须通过 npm 来下载

    * 使用的时候就可以通过 require('包名') 的方式来进行加载才可以使用

    * 不可能有任何一个第三方包和核心模块的名字是一样的

    * 查找规则

      - 先找到当前文件所处目录中的 node_modules 目录

        + node_modules/art-template --> node_modules/art-template/package.json 文件 --> node_modules/art-template/package.json 文件中的 main 属性 --> main 属性中就记录了 art-template 的入口模块

        + 如果 package.json 文件不存在或者 main 指定的入口模块也没有,则 node 会自动找该目录下的 index.js
      - 如果以上所有任何一个条件都不成立，则会进入上一级目录中的 node_modules 目录查找，直到磁盘根目录

  + 自定义模块

## npm

node package manager

```javascript
//一个项目有且只有一个 node_modules，放在项目根目录中
npm install <pkg> --save//安装第三方包时最好加上--save 会在package.json 中生成dependencies 保存第三方包依赖信息
```


### 常用命令

- `npm install <pkg> ` 安装包

  + install 可简写成 i

  + 可添加--save保存依赖信息可简写成-S

- `npm uninstall <pkg>` 删除包
  + 可添加--save把依赖信息一起去除

### 解决npm被墙问题

http://npm.taobao.org/ npm淘宝镜像

- 安装淘宝的cnpm

```shell
#在任意目录执行都可以 --global表示安装到全局
npm install --global cnpm
#以后安装包时把npm替换成cnpm就行
```

- 安装时添加地址使用淘宝服务器下载

```shell
#临时使用
npm install <pkg> --registry https://registry.npm.taobao.org install express
#持久使用
npm config set registry https://registry.npm.taobao.org
#查看配置信息
npm config list
```

## packgae.json

建议每一个项目都要有一个 `package.json` 文件（包描述文件，就像产品的说明书）

这个文件可以通过 `npm init` 的方式来初始化出来

当项目的node_modules文件夹丢失，可以通过 `npm install` 会根据package.json自动下载依赖

建议执行 `npm install <pkg> --save` 用来保存依赖项信息 

npm 5.0 之后使用 `npm install` 会自动加入 `dependencies` 的依赖项，不需加 `--save`，还会自动创建 `package-lock.json` 

`package-lock.json` 这个文件的作用：记录依赖信息；锁定版本号，防止自动升级

## Node 中的非模块成员

在每个模块中，除了 `require` 、 `exports` 等模块相关 API 之外，还有两个特殊的成员：

-  `__dirname`  获取 当前文件 模块所属目录的绝对路径
-  `__filename` 获取 当前文件 的绝对路径

在文件操作中，使用相对路径是不可靠的，因为在 Node 中文件操作的路径被设计为相对于执行 node 命令所处的路径，可以将相对路径替换为绝对路径 `path.join(__dirname,'...')`

推荐：所有跟文件操作相关的都使用 动态的绝对路径

> 模块中路径标识不受执行 node 命令所处路径影响（引用自定义模块使用绝对路径不受影响）

## Express

Node.js的Web开发框架 -- 封装的http

### init

#### 安装

```shell
npm install --save express
```

#### hello world

```javascript
const express = require('express')
const app = express()
app.get('/',(req,res) => res.send('Hello World!'))
app.listen(3000,() => console.log('Example app listening on port 3000'))
```

#### 基本路由

路由器

- 请求方法
- 请求路径
- 请求处理函数

get：

```javascript
app.get('/', function (req, res) {
  res.send('Hello World!')
})
```

post：

```javascript
app.post('/', function (req, res) {
  res.send('Got a POST request')
})
```

#### 静态服务

```javascript
// 开放指定目录 ./public/ 下的资源

// 当省略第一个参数时 可以通过省略 /public 的方式来访问 .../js/a.js
app.use(express.static('./public/'))

// 当以 /public/ 开头时，去./public/目录中查找对应资源 .../public/js/a.js
app.use('/public/', express.static('./public/'))
// 当以 /a/ 开头时，去./public/目录中查找对应资源 .../a/js/a.js
app.use('/a/', express.static('./public/'))

// 当以 /public/ 开头时，以绝对路径查找 /public
app.use('/public', express.static(path.join(__dirname, './public')))
```

### 在Express配置art-template模板引擎

> 参考文档：[art-template docs](https://aui.github.io/art-template/zh-cn/docs/)

安装：

```shell
npm install --save art-template
npm install --save express-art-template
```

配置：

```javascript
// 第一个参数，表示，当渲染以 .html 结尾的文件的时候，使用 art-template 模板引擎
app.engine('html', require('express-art-template'))
```

使用：

```javascript
// Express 为 Response 相应对象提供了一个方法：render
// render 方法默认是不可以使用，但是如果配置了模板引擎就可以使用了

app.get('/', function (req, res) {
    //express 默认会去项目中的 views 目录查找index.html
    //Express 有一个约定：开发人员把所有的视图文件(html文件)都放到 views 目录中
    res.render('index.html', {
        comments: comments
    })
})
```

如果希望修改默认的`views` 视图渲染存储目录可以：

```javascript
// 第一个参数是默认的 表示修改views为xxx
app.set('views', 目录路径)
```

### 在 Express 获取表单 GET 请求体数据

Express 内置了一个API，可以直接通过 `req.query` 来获取

```javascript
req.query
```

### 在 Express 获取表单 POST 请求体数据

在Express中没有内置获取表单POST请求体的API，需要使用第三方包：`body-parser`

安装：

```shell
npm install --save body-parser
```

配置：

```javascript
var express = require('express')
//0.引包
var bodyParser = require('body-parser')

var app = express()

// 配置 body-parser 中间件
// 只要加入这个配置，则在 req 请求对象上会多出来一个属性：body
// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }))
// parse application/json
app.use(bodyParser.json())
```

使用：

```javascript
app.use(function (req, res) {
	// 可以通过 req.body 来获取 POST 请求体数据
    res.end(JSON.stringify(req.body, null, 2))
})
```

### 在 Express 中包装路由

router.js:

```javascript
// 专门用来包装路由的 
var express = require('express')

// 1. 创建一个路由容器
var router = express.Router()

// 2. 把路由都挂载到 router 路由容器中 替代app.get()
router.get('/students/new', function (req, res) {
  res.render('new.html')
})
// 3. 把 router 导出
module.exports = router
```

app.js:

```javascript
// 4. 把路由容器挂载到 app 服务中 ---在入口文件中
var router = require('./router')
app.use(router)
```

### Session

在 Express 中默认不支持 Session 和 Cookie

但是我们可以使用第三方中间件：express-session 来解决 

下载：

```shell
npm i express-session
```

配置：

```javascript
var express = require('express')
var session = require('express-session')

var app = express()

// 一定要在app.use(router)之前
// 该插件会为 req 请求对象添加一个成员：req.session 默认是一个对象
app.use(session({
    // 配置加密字符串，它会在原有加密基础之上和这个字符串拼接起来去加密
    // 目的：为了增加安全性，防止客户端恶意伪造
    secret: 'keyboard cat',
    resave: false,
    saveUninitialized: true // 无论是否使用 Session ，默认直接分配一把钥匙给客户端
}))
```

使用：

```javascript
// 添加 Session 数据
req.session.foo = 'bar'
// 获取 Session 数据
req.session.foo
// 删除
req.session.foo = null
delete req.session.foo
```

tips：默认 Session 数据是内存存储的，服务器一旦重启就会丢失 ，真正的生产环境会把 Session 进行持久化存储

### Express 中间件

> 参考文档：[express middleware](http://expressjs.com/en/guide/using-middleware.html)

#### 应用程序级别中间件

使用和函数将[应用](http://expressjs.com/en/4x/api.html#app)程序级中间件绑定到[app对象](http://expressjs.com/en/4x/api.html#app)的实例

```javascript
// 没有装载路径的中间件功能。每次应用程序收到请求时都会执行该功能。
var app = express()

// 不关心请求路径和请求方法的中间件,任何请求都会进入这个中间件
app.use(function (req, res, next) {
    console.log('Time:', Date.now())
    next()
    // 如果请求进入中间件之后，没有调用 next 则代码会停在当前中间件
    // next 是一个方法，用来调用下一个中间件的
    // 调用 next 方法也是要匹配的（不是调用紧挨着的那个）
})

// /user/:id路径上安装的中间件功能。对/user/:id路径上的任何类型的HTTP请求执行该功能。
app.use('/user/:id', function (req, res, next) {
    console.log('Request Type:', req.method)
    next()
})

// 严格匹配请求方法和请求路径的中间件 app.get app.post
// 路由及其处理函数（中间件系统）。该函数处理对/user/:id路径的GET请求
app.get('/user/:id', function (req, res, next) {
    res.send('USER')
})

// 如果没有能匹配的中间件，则 Express 会默认输出：Cannot GET 路径
```

#### 路由级别中间件

路由器级中间件的工作方式与应用程序级中间件的工作方式相同，只不过它被绑定到一个实例`express.Router()`。

```javascript
var router = express.Router()

// 与应用级别的相同，只不过把 app 换成 router
router.use(function (req, res, next) {
    console.log('Time:', Date.now())
    next()
})

// 其他省略
```

#### 错误处理中间件

错误处理中间件总是需要*四个*参数。您必须提供四个参数以将其标识为错误处理中间件函数。

```javascript
app.get('/', function (req, res, next) {
    fs.readFile('.d/sa./d.sa/.dsa', function (err, data) {
        if (err) {
            // 当调用 next 的时候，如果传递了参数，则直接往后找到 错误处理中间件
            // 当发生错误的时候，我们可以调用 next 传递错误对象
            // 然后就会被全局错误处理中间件匹配到并处理之
            next(err)
        }
    })
})

// 错误处理中间件
// (err, req, res, next) 顺序不能乱，个数不能少
app.use(function (err, req, res, next) {
    console.error(err.stack)
    res.status(500).send('Something broke!')
})
```

#### 内置中间件

Express 内置中间件的功能：

```javascript
/* 例如
express.static 提供静态资产，如HTML文件，图像等
express.json 使用JSON有效负载解析传入的请求
express.urlencoded 使用URL编码的有效负载解析传入的请求
*/
```

#### 第三方中间件

使用第三方中间件为Express应用程序添加功能。

安装Node.js模块以获得所需的功能，然后在应用程序级别或路由器级别将其加载到您的应用程序中。

以下示例说明了安装和加载cookie解析中间件功能`cookie-parser`。

```shell
npm install cookie-parser
```

```javascript
var express = require('express')
var app = express()
var cookieParser = require('cookie-parser')

// load the cookie-parsing middleware
app.use(cookieParser())
```

## MongoDB

[菜鸟教程](http://www.runoob.com/mongodb/mongodb-tutorial.html)

### 关系型数据库和非关系型数据库

- 所有关系型数据库都需要 `sql` 语言来操作
- 所有的关系型数据库在操作之前都需要设计表结构
- 而且数据表还支持约束
  + 唯一的
  + 主键
  + 默认值
  + 非空

- 非关系型数据库非常的灵活
- MongoDB是最像关系型数据库的非关系型数据库
  + 数据库-》数据库
  + 数据表-》集合（数组）
  + 表记录-》（文档对象)

- MongoDB不需要设计表结构

### 启动和关闭

启动：

```shell
# mongodb 默认使用 mongod 命令 以所处盘符根目录下的 /data/db 作为自己的数据存储目录
# 所以在第一次执行时应手动创建一个 /data/db
mongodb
```

如果想要修改默认数据存储目录

```shell
mongod --dbpath=数据存储目录
```

关闭：

`ctrl + c`

### 连接和退出

连接：

```shell
# 默认连接本机的 MongoDB 服务
mongo
```

退出：

```shell
exit
```

### 基本命令：

```shell
# 查看所有数据库
show dbs
# 切换到指定数据库（没有则新建）
use 数据库名
# 查看当前操作的数据库
db
# 插入数据

```

## Node 操作 MongoDB 数据库

- 使用官方的 `mongodb` 包操作

  > https://github.com/mongodb/node-mongodb-native

- 使用第三方 `mongoose` 来操作 MongoDB 数据库
- 第三方包：`mongoose` 基于 MongoDB 官方的 `mongodb` 包再一次做了封装

### mongoose

- 官网：https://mongoosejs.com/
- 官方指南：https://mongoosejs.com/docs/guide.html
- 官方 API 文档：https://mongoosejs.com/docs/api.html

#### init

安装：

```shell
npm i mongose
```

hello world:

```javascript
var mongoose = require('mongoose');

// 连接 MongoDB 数据库
mongoose.connect('mongodb://localhost/test', { useMongoClient: true });
mongoose.Promise = global.Promise;

// 创建一个模型 就是在设计数据库
// MongoDB 是动态的，非常灵活，只需要在代码中设计你的数据库就可以了
// mongoose 这个包就可以让你的设计编写过程变的非常的简单
var Cat = mongoose.model('Cat', { name: String });

// 实例化一个 Cat
var kitty = new Cat({ name: '喵喵' + i });

// 持久化保存 kitty 实例
kitty.save(function (err) {
    if (err) {
        console.log(err);
    } else {
        console.log('meow');
    }
});
```

### 官方指南

设计 Schema 发布 Model ：

```javascript
var mongoose = require('mongoose')
// 架构
var Schema = mongoose.Schema

// 1. 连接数据库
// 指定连接的数据库不需要存在，当你插入第一条数据之后就会自动被创建出来
mongoose.connect('mongodb://localhost/itcast')

// 2. 设计文档结构（表结构）
// 字段名称就是表结构中的属性名称
var userSchema = new Schema({
    username: {
        type: String,
        required: true // 必须有
    },
    password: {
        type: String,
        required: true
    },
    email: {
        type: String
    }
})

// 3. 将文档结构发布为模型
//    mongoose.model 方法就是用来将一个架构发布为 model
//    第一个参数：传入一个大写名词单数字符串用来表示你的数据库名称
//                 mongoose 会自动将大写名词的字符串生成 小写复数 的集合名称
//                 例如这里的 User 最终会变为 users 集合名称
//    第二个参数：架构 Schema  
//    返回值：模型构造函数
var User = mongoose.model('User', userSchema)

// 4. 当我们有了模型构造函数之后，就可以使用这个构造函数对 users 集合中的数据（增删改查）
```

#### 增加

```javascript
var admin = new User({
    username: 'zs',
    password: '123456',
    email: 'admin@admin.com'
})

admin.save(function (err, ret) {
    if (err) {
        console.log('保存失败')
    } else {
        console.log('保存成功')
    }
})
```

#### 查询

查询所有：

```javascript
User.find(function (err, ret) {
    if (err) {
        console.log('查询失败')
    } else {
        console.log(ret)
    }
})
```

按条件查询所有：返回多个对象组成的 数组

```javascript
User.find({
    username: 'zs'
}, function (err, ret) {
    if (err) {
        console.log('查询失败')
    } else {
        console.log(ret)
    }
})
```

查询单个：返回一个 对象

```javascript
User.findOne({
    username: 'zs'
}, function (err, ret) {
    if (err) {
        console.log('查询失败')
    } else {
        console.log(ret)
    }
})
```

#### 删除

```javascript
User.remove({
    username: 'zs'
}, function (err, ret) {
    if (err) {
        console.log('删除失败')
    } else {
        console.log('删除成功')
    }
})
```

#### 更新

```javascript
User.findByIdAndUpdate('5a001b23d219eb00c8581184', {
    password: '123'
}, function (err, ret) {
    if (err) {
        console.log('更新失败')
    } else {
        console.log('更新成功')
    }
})
```

## Node 操作 MySQL 数据库

安装：

```shell
npm install mysql --saves
```

配置：

```javascript
var mysql = require('mysql');

// 1. 创建连接
var connection = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: 'root',
  database: 'db' 
});

// 2. 连接数据库 
connection.connect();

// 3. 执行数据操作 所有的数据库操作都在参数1写
connection.query('SELECT * FROM `users`', function (error, results, fields) {
  if (error) throw error;
  console.log('The solution is: ', results);
});

// 4. 关闭连接 
connection.end();
```

## 异步编程

### 回调函数

  + 异步编程
  + 如果需要得到一个函数内部异步操作的结果，这是时候必须通过回调函数来获取
  + 在调用的位置传递一个函数进来
  + 在封装的函数内部调用传递进来的函数



服务端重定向只针对同步请求，异步请求无效

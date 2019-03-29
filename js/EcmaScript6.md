> 参考书籍：[EcmaScript6 入门](http://es6.ruanyifeng.com/)

## 变量声明的扩展：let 和 const

### var：

- 作用：变量声明
- var的缺陷：变量提升，可以重复声明
- 作用域：全局作用域，函数作用域
- 没有块级作用域 
- 在浏览器中默认属于 window

### let：

- 作用：变量声明
- 块级作用域（被"{}"扩起来的）
- 没有变量提升
- 必须先声明后使用
- 不允许重复声明

### const：

- 作用：常量声明
- 常量：一旦声明不可改变
  + 只读：普通数据类型不能修改
  + 复杂数据类型
    * 可以修改
    * 不能重新赋值

- 声明同时必须赋值
- 块级作用域
- 没有常量提升
- 不可重复声明

## 字符串的扩展：

### includes

```javascript
'hello world'.includes('hello')// true，包含 hello
```

### startsWith

```javascript
'hello world'.startsWith('hello')// true，以 hello 开头
```

### endsWith

```javascript
'hello world'.endsWith('d')// true，以 d 结尾
```

### repeat

```javascript
'hello world'.repeat(2)// 'hello worldhello world' 重复2次
```

### 模板字符串、

```javascript
// 可以换行 变量拼接很方便
var user= {
    name:'jack',
    age:18
}
var greeting = `
大家好，我叫${user.name}，
我今年${user.age}岁了
`
```

## 解构赋值

本质属于 模式匹配

如果解构不成功，变量的值就是 undefined

### 数组的解构赋值

```javascript
var arr = ['hello', 'world']
// var str1 = arr[0]
// var str2 = arr[1]
// 数组解构赋值是按照 索引 顺序来解构的
// 可以设定解构不成功的默认值
var [str1, str2='hi'] = arr
```

### 对象的解构赋值

```javascript
var user = {
    name:'jack',
    age:18
}
// 对象解构赋值是按照 键名 来解构的
// 支持设置解构不成功的默认值
var {name,age,gender = '男'} = user
```

### 字符串的解构赋值

```javascript
var [a,b,c,d,e] = 'hello'
// a = h ,b = e,....
```

### 函数参数的解构赋值

```javascript
var nums = [1,2]
function add([x,y]){
    // var [x,y] = nums
    return x + y
}
add(nums)
function ajax({
    url,
    type = get
}){
    console.log(url,type)
}
ajax({
    url:'/hello',
    type:'post'
})
```

### 用途

#### 交换变量的值

```javascript
var x = 1
var y = 2
[x, y] = [y, x]
```

#### 输入模块的指定方法

```javascript
var {readFile,writeFile} = require('fs')
readFile('./index.html','utf8',function(err,data){
    ...
})
```

## 数值的扩展

### Number

Number.isFinite()

Number.isNaN()

Number.parseInt()

```javascript
// 将全局的 parseInt() 函数，转为 Number 的方法 使语法更为模块化
Number.parseInt('123')// 123
```

Number.parseFloat()

Number.isInterger()

### Math

## 函数的扩展

### 函数参数的默认值

- 需要一个参数有默认值
- 最好把该参数作为尾参数
- 因为只有尾参数才可以省略

### 函数参数支持解构

### 函数参数的 rest 参数

- rest 参数后面不能跟形参了
- 接收所有剩余参数放到一个 数组 中
- 建议以后不使用 arguments 参数（伪数组），用 args 代替

```javascript
function add(...args){
    console.log(args)
}
add(1,2,3,4,5) // [1,2,3,4,5]
```

### 箭头函数

- 函数声明不能使用箭头函数
- 函数表达式可以使用箭头函数
- 匿名函数可以使用箭头函数(箭头函数多用于匿名函数)
- 语法
  + 一个参数可以省略()
  + 没有参数不能省略()
  + 函数体只有一句代码可以省略()
  + 如果省略了{}则不能显示return

```javascript
var add = function (x,y) {
    return x + y
}
var add (x, y) => { return x + y}
// 简写
var add x => x
// 用于匿名函数 简化代码
var arr = ['a', 'b' ,'c']
arr.forEach(item => console.log(item))
```


- 箭头函数不能当做构造函数，不能new
- 箭头函数内部不要使用 arguments 对象，有问题
- 箭头函数内部使用 rest 参数代替 arguments
- 箭头函数默认自动 bind(this)
- 由于没有自己的 this ，所以call、apply、bind无效

```javascript
// 箭头函数自动绑定外部的this bind(this)
function foo() {
    setTimeout(() => {
        console.log('id:', this.id);// 不是 windows 而是 { id:42 }这个对象
    }, 100);
}

var id = 21;
foo.call({ id: 42 });// id: 42
// 例2
function Person(){
    this.name = 'jack',
    this.age = 18
}
Person.prototype.greeting = function (){
    window.setTimeout(() => {
        console.log(this)
    },1000)
}
var p1 = new Person()
p1.greeting()// p1 这个实例对象

// 箭头函数无法通过 call、apply、bind 手动改变 this 指向
// 它内部的 this 都是取决于外部环境
var fn = () => console.log(this)
var foo = {}
fn()// window 对象
fn.call(foo)// window 对象
```

## Promise对象

> 参考书籍：[ECMAScript 6 入门](http://es6.ruanyifeng.com/)

callback hell

异步编程无法保证执行顺序 ---> 回调嵌套 ---> 嵌套过深，代码可读性差

EcmaScript 6 中新增了一个 API：`Promise`

Promise 是一个构造函数，在Promise中存放一个异步任务(pending) ---> resolve | reject

基本语法：

```javascript
var fs = require('fs')

function pReadFile(filePath) {
    return new Promise(function (resolve, reject) {
        fs.readFile(filePath, 'utf8', function (err, data) {
            if (err) {
                reject(err)
            } else {
                resolve(data)
            }
        })
    })
}
// then(fun1,fun2) fun1 就是上面的resolve，fun2就是上面的reject(可以省略)
pReadFile('./data/a.txt')
    .then(function (data) {// 当读取成功的时候
    console.log(data)
    // 当前函数中 return 的结果就可以在后面的 then 中 function 接收到
    //      没有 return 后面收到的就是 undefined
    // 我们可以 return 一个 Promise 对象
    // 当 return 一个 Promise 对象的时候，后续的 then 中的 方法的第一个参数会作为第二个异步操作的 resolve
    return pReadFile('./data/b.txt')
})
    .then(function (data) {
    console.log(data)
    return pReadFile('./data/c.txt')
})
    .then(function (data) {
    console.log(data)
})
```


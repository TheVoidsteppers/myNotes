## 开发动态网站应用程序流程

### 1、需求分析

那些功能，那些业务XMind

### 2、原型设计

axure rp | 墨刀：快速原型设计工具

### 3、UI设计

"草稿"转换为设计稿，并提供相应产物（设计稿、静态页面）PS | AI | Sketch

### 4、技术选型

数据库技术栈、后端技术栈、前端技术栈

### 5、数据库设计

根据业务需要设计数据库的结构

### 6、项目架构设计

搭框架，创建基本的项目结构、编写基础的公共模块代码和组织代码

### 7、业务迭代开发

基于项目架构完成各项业务功能

### 8、集中测试

将所有功能模块整合到一起，集中测试业务功能是否有BUG

### 9、部署上线

部署到服务器上

## 管理后台登录

```php+HTML
<?php// 在每个后台页面都需校验数据当前访问用户的 箱子（session）有没有登录的登录标识
session_start();

if (empty($_SESSION['current_login_user'])) {
    // 没有当前登录用户信息，意味着没有登录
    header('Location: /admin/login.php');
}
?>
<!--用户名和密码分开校验-->
<?php
if ($user['password'] !== md5($password)) {//密码一般加密存储，md5加密
    // 密码不正确
    $GLOBALS['message'] = '邮箱与密码不匹配';
    return;
}
//登录成功后，给session以便记录用户的登录状态
$_SESSION['current_login_user'] = $user;
?>
```

### Tips：

```php
//从MySQL数据库获取到的中文为"?"时设置字符集
$connection = mysqli_connect('127.0.0.1', 'root', '123456', 'demo2');
// 1. 必须在查询数据之前
// 2. 必须传入连接对象和编码
mysqli_set_charset($connection, 'utf8');
// 或mysqli_query($connection, 'set names utf8;');

$query = mysqli_query($connection, 'select * from users;');
```

```php
//定义函数名时要注意函数名不能和内置函数重名（内置1000+函数），一般在定义的函数名前面加上项目名作为前缀
//判断函数是否定义
//JS typeof fn ==='function'
//PHP function_exists('get_current_user');
```

```php
//被引入的文件要注意的问题
//1、以引入者为当前文件路径考虑
require_once '../functions.php';
//2、以物理路径 拼接
require_once dirname(__FILE__) . '/../../functions.php';
```

`数据转图表 1、chartjs 2、echarts百度`

```php
//sql注入 传入?id=1 or 1 = 1 
$id=$_GET['id'];
baixiu_execute('delete from categories where id ='.$id);
//==>delete from categories where id = 1 or 1 = 1 ==>delete from categories
//解决办法
$id=(int)$_GET['id'];
```

```php
//数据库可以联合查询
$posts = baixiu_fetch_assoc_all('select
  posts.id,
  posts.title,
  users.nickname as user_name,
  categories.name as category_name,
  posts.created,
  posts.status
from posts
inner join categories on posts.category_id = categories.id
inner join users on posts.user_id =users.id
where 1=1 and posts.status ='public'
order by posts.created desc
limit 0,20;');
//inner join 关联数据表 on 原表值=关联表值
//order by 根据xx排序 desc降序 asc升序
//limit 0,20 越过0条取20条
```

```php
//跳转回原来的页面
//http中的referer用于标识当前请求的来源
header('Location: ' . $_SERVER['HTTP_REFERER']);
```

### 富文本编辑器

```php
//UEditor百度研发
//CKEditor
//TinyMCE
//SimpleMDE  Markdown编辑器
//浏览器API
document.body.contentEditable=true|false;//可编辑
document.execCommand();//加粗、倾斜...命令
```

### ajax 分页功能拓展插件

```javascript
// twbs-pagination
function loadPageData(page){
    $('tbody').fadeOut()
    $.getJSON('/admin/api/comments.php',{page:page},function(res){
        $('.pagination').twbsPagination({
            totalPages:res['total_page'],
            visiablePages:5,
            //设置第一次初始化时不触发onPageClick函数，默认触发
            initiateStartPageClick:false,
            onPageClick:function(e,page){
                loadPageData(page)
            }
        })
        var html = $('#comments_tmpl').render({comments:res['comments']})
        $('tbody').html(html).fadeIn()
    })
}
```

```javascript
//css  loading样式
loading.io
loading.awesomes.cn
```

```html
<tr  data-id="{{:id}}">
<td class="text-center">
        <a href="javascript:;" class="btn btn-danger btn-xs btn-delete">删除</a>
</td>
</tr>
<script>
//事件委托 因为创建元素在元素绑定事件之前
// 有一个bug  点击删除自动重新加载
//原因是a标签没href=""含义是重新加载页面，应该href="javascript:;"
$('tbody').on('click','.btn-delete', function () {
    var $tr = $(this).parent().parent()
    var id = $tr.data('id')
    $.get('/admin/api/comment-delete.php', {
        id: id
    }, function (res) {
        if (!res) return
        loadPageData(currentPage)
    })
})
</script>
```

### 异步文件上传

```html
<!--更改checkbox、文件域 样式利用兄弟元素和label-->
<style>
    .form-image {
        position: relative;
        background-color: #fff;
    } 
</style>
<label class="form-image">
    <input id="logo" type="file">
    <img src="/static/assets/img/logo.png">
    <i class="mask fa fa-upload"></i><!--bootstrap的css-->
    <input type="hidden" name="avatar"><!--不显示 专门用于表单提交键值-->
</label>
<script>
    $('#logo').on('change',function(){
        var $this=$(this)
        var files=$this.prop('files')
        if(!files.length) return
        var file=files[0]
        //FormDate 是HTML5中新增的 专门配合AJAX操作 用于在客户端与服务端传递二进制数据
        var data = new FormDate()
        data.append('avatar',file)
        var xhr = new XMLHttpRequest()
        xhr.open('POST','/admin/api/upload.php')
        xhr.send(data)
        xhr.onload=function(){
            $this.siblings('img').attr('src',this.responseText)
            $this.siblings('input').val(this.responseText)
        }
    })
</script>
```
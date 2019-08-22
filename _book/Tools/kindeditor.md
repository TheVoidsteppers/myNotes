## 引包

```html
<link rel="stylesheet" href="../themes/default/default.css" />
<script src="../public/plugins/kindeditor/kindeditor-all-min.js"></script>
<script src="../public/plugins/kindeditor/lang/zh-CN.js"></script>
```

## html

```html
<textarea id="editor_id" name="content" style="width:700px;height:300px;visibility:hidden;display: block;"></textarea>
```

## 初始化

```js
// 设置默认字体大小
KindEditor.options.cssData = 'body { font-size: 15px; }'
// 初始化
KindEditor.ready(function(K) {
    window.editor = K.create('#editor_id',{
        width : "70%", //编辑器的宽度为70%
        height : "536px", //编辑器的高度为100px
        filterMode : false, //不会过滤HTML代码
        resizeType : 1, //编辑器只能调整高度
        items : ['fontname', 'fontsize', '|', 'forecolor', 'hilitecolor', 'bold', 'italic', 'underline','removeformat', '|', 'justifyleft', 'justifycenter', 'justifyright', 'insertorderedlist','insertunorderedlist', '|', 'emoticons', 'image', 'link']
    })
})
```

## 获取内容

```javascript
// 取得HTML内容
html = editor.html();

// 同步数据后可以直接取得textarea的value
editor.sync() // 必写

html = document.getElementById('editor_id').value; // 原生API
html = K('#editor_id').val(); // KindEditor Node API
html = $('#editor_id').val(); // jQuery

// 设置HTML内容
editor.html('HTML内容');
```

## 结合 vue 使用

```js
// 如果是 在路由内 使用 activated
// KindEditor 初始化后 editor.html('HTML内容') 不成功，可以使用 KindEditor.remove('#editor_id') 移除再初始化
mounted() {
    // 初始化富文本
    window.editor = KindEditor.create('#editor_id',{
        width : "70%", //编辑器的宽度为70%
        height : "536px", //编辑器的高度为100px
        filterMode : false, //不会过滤HTML代码
        resizeType : 1 //编辑器只能调整高度
    })
}
```

## 图片上传

```javascript
KindEditor.ready(function(K) {
    window.editor = K.create('#editor_id',{
        //将编辑器上传的图片保存到七牛云
        allowImageUpload:true,//允许上传图片
        allowFileManager:true, //允许对上传图片进行管理
        uploadJson : '../../../../admin.php/classroom/qn_image',// 上传图片接口
        filePostName: 'imgFile',// name属性默认值
        allowImageRemote: false //禁止网络图片上传
    })
})
```

## 图片过大时自动缩放

```javascript
// K.create 的配置参数
afterUpload : function(url, data, name) {
    //上传文件后执行的回调函数，必须为3个参数
    if(name=="image" || name=="multiimage"){ //单个和批量上传图片时
        var img = new Image()
        img.src = url
        img.onload = function(){ //图片必须加载完成才能获取尺寸
            if(img.width>750) window.editor.html(window.editor.html().replace('<img src="' + url + '"','<img src="' + url + '" width="750"'))
        }
    }
}
```


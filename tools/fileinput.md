```html
<input id="upload" class="file" type="file" multiple data-min-file-count="1" name="upload_logo">
<!--data-min-file-count=”1”是指文件上传最低数量-->
```

```javascript
//  初始化
$("#upload").fileinput({
    uploadUrl: "upload", //上传到后台处理的方法
    uploadAsync: false, //设置同步，异步 （同步）
    language: 'zh', //设置语言
    allowedFileExtensions : [ 'jpg','jpeg','png', 'gif' ],
    allowedFileTypes: ['image', 'video', 'flash'],
    overwriteInitial: false, //不覆盖已存在的图片
    //下面几个就是初始化预览图片的配置    
    initialPreviewAsData: true,
    initialPreviewFileType: 'image',
    initialPreview:path , //要显示的图片的路径
    initialPreviewConfig:con,
    maxFileSize : 1000,//上传文件最大的尺寸
    maxFilesNum : 1,//上传最大的文件数量
    initialCaption: "请上传商家logo",//文本框初始话value
})

// 获取上传的返回值 可以连 链式编程 连在上面后面
$('#upload').on('fileuploaded', function(event, data, previewId, index) {
    var form = data.form, files = data.files, extra = data.extra,
        response = data.response, reader = data.reader;
    console.log(response);//打印出返回的json
    console.log(response.paths);//打印出路径
})
```
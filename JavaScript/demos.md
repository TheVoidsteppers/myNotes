## textarea 自动调整高度


```html
<textarea id="textarea" style="resize: none;"></textarea>
```

```javascript
$('#textarea').on('input',function () {
    // 最小高度
    var minRows = 2;
    // 最大高度，超过则出现滚动条
    var maxRows = 5;
    if (this.scrollTop == 0) {
        this.scrollTop = 1
    }
    while (this.scrollTop == 0) {
        if (this.rows > minRows){
            this.rows--
        }else{
            break
        }
        this.scrollTop = 1
        if (this.rows < maxRows) {
            this.style.overflowY = "hidden"
        }                 
        if(this.scrollTop > 0){
            this.rows++;
            break;
        }
    }
    while(this.scrollTop > 0){
        if (this.rows < maxRows) {
            this.rows++
            if (this.scrollTop == 0){
                this.scrollTop = 1
            }
        }else{
            this.style.overflowY = "auto";
            break
        }
    } 
})  
```

## 注册 被 vue 动态渲染(生成) 及 后面生成的 DOM 元素

```javascript
// 解决 ：点击展开 DOM 树之后 才可以绑定上事件 
$(document).on('click', '.selectCate', function() {
	console.log('123')
})
```

## jquery 点击元素以外任意地方隐藏该元素

```javascript
// 第一先实现点击任何地方都隐藏该元素(假设id="bar")

$(document).click(function(){
	$("#bar").hide();
});
// 那么bar也属于document，点击bar也会让自己隐藏，显然这不是想要的，这时候要阻止冒泡事件，即document的事件对bar无效
$("#bar").click(function(event){
	event.stopPropagation();
});
```

## FormData

```html
<!-- 批量上传 -->
<form id="uploadForm" enctype="multipart/form-data" style="position: relative;">
    <button class="btn btn-primary" @click.prevent="">选择文件</button>
    <input id="file" type="file" name="file" style="position: absolute;top: 5px;left: 5px;opacity: 0;" />
    <button id="upload" type="button" class="btn btn-primary" @click.prevent="postMultiGift">上传</button>
</form>
```

```javascript
new Vue({
    methods:{
        postMultiGift(){
        let formData = new FormData()
        formData.append('file',$('#file')[0].files[0])
        axios({
          method: 'post',
          url: './batch_activity_order',
          data: formData,
          headers:{'Content-Type':'multipart/form-data'}
        }).then(function(response){
          const res = response.data
          // console.log(res)
        })
      }
    }
})
```

## ios关闭软键盘页面不回弹

```javascript
$('input,textarea').on('blur',function(){
	window.scroll(0,0)
    // 或者
    let scrollHeight = document.documentElement.scrollTop || document.body.scrollTop || 0
    window.scrollTo(0, Math.max(scrollHeight - 1, 0))
})
$('select').on('change',function(){
	window.scroll(0,0)
})
```

## iOS 微信 视频自动播放 | 视频不全屏

```html
<video src="<?=$row['video_url']?>" controls="controls" autoplay width="100%" @ended="videoEnd" webkit-playsinline="true" playsinline x5-video-player-type="h5" x-webkit-airplay="true"></video>
<!-- 不全屏加上 
    安卓设置属性：
    x5-playsinline="true"
    实测，不可加以下属性设置，否则还是会跳出黑底全屏播放
    x5-video-player-type='h5' x5-video-player-fullscreen='true'
    IOS设置属性：
    webkit-playsinline="true" playsinline="true" x-webkit-airplay="true" 
即可-->

<!-- 必须加载微信api资源 -->
<script src="http://res.wx.qq.com/open/js/jweixin-1.0.0.js"></script>
<script>
// 一般情况下，这样就可以自动播放了，但是一些奇葩iPhone机不可以
$('.swiper-slide video')[0].play()
// iOS 必须在微信Weixin JSAPI的WeixinJSBridgeReady才能生效
document.addEventListener("WeixinJSBridgeReady", function () {
  $('.swiper-slide video')[0].play()
}, false)
</script>
```

## 导出 excel

### JavaScript生成 CSV 文件 

```
data:[<mediatype>][;base64],<data>
```

- `data:`是前缀
- `mediatype`: 文件的MIME类型，比如`image/jpge`对应JPGE文件，默认值为`text/plain;charset=US-ASCII`
- `base64`: 文件内容是否base64格式的
- `data`: 文件的正文内容

```javascript
// \uFEFF 解决中文乱码问题
// download 自定义文件名 H5
var a = document.createElement('a');
a.href=`data:text/csv;charset=utf-8,\uFEFF${encodeURI('姓名,年龄\nMofei,18')}`;
a.download="Mofei的CSV.csv";
a.click();
```


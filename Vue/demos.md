## 递归渲染

```javascript
// 递归组件
;(function () {
  const template= `
  <li class="tree-menu">
    <div>
      <span @click="selectCate(id)">{{title}}</span>
      <span v-if="tree" class="glyphicon glyphicon-chevron-down" @click="toggleChildren"></span>
    </div>
    <ul style="padding-left: inherit;">
      <tree-menu
        v-if="showChildren"
        v-for="tre in tree"
        :id="tre.id"
        :title="tre.title"
        :tree="tre.tree"
      >
      </tree-menu>
    </ul>
  </li>
 `
  window.TreeMenu = {
    name: 'tree-menu',
    template,
    props: [ 'title', 'tree','id' ],
    data(){
      return {
        showChildren:false
      }
    },
    methods: {
      toggleChildren() {
        this.showChildren = !this.showChildren;
      },
      selectCate(id){
		console.log(id)
      }
    }
  }
})()
```

```html
<ul class="cateList" v-if="isshow">
	<tree-menu v-for="cate in cateList" :id="cate.id" :title="cate.title" :tree="cate.tree"></tree-menu>
</ul>

<script>
new Vue({
    data:{
       cateList:[{
			id: "221",
			pid: "0",
			title: "服饰箱包",
             tree: {
                id: "207",
                pid: "221",
                title: "箱包
             }
            }]
    },
    components: {
    TreeMenu
  }
})
</script>
```

## 上传 多张图片

```html
<div style="margin-bottom: 20px" class="kcUPNr">
    <span class="required">*</span>
    <h6 style="display: inline-block;"><strong>背景图片</strong></h6>
    <span class="tips">
        <i class="tips2">未选择文件</i>仅支持jpg、jpeg、png格式的图片
    </span>
    <div class="bgImgList">
        <ul class="clearfix">
            <li v-for="(image,index) in bgPreImg">
                <img :src="image" width="65" height="65" />
                <a href="javascript:;" @click.prevent='delImage(index)'>
                    <span class="glyphicon glyphicon-remove"></span>
                </a>
            </li>
            <li>
                <img src="<?=ZT_ADMIN_IMG?>ADDbTN.png" width="65" height="65" @click.prevent="addPic">
                <input type="file" @change="onFileChange" multiple style="display: none;">
            </li>
        </ul>
    </div>
</div>
<div v-if="bgPreImg.length > 0">
    <button class="btn btn-danger" @click="removeImage">移除全部图片</button>
    <button class="btn btn-primary" @click='uploadImage'>上传</button>
</div>
```

```css
.bgImgList ul{padding: 0;margin: 0;}
.bgImgList li{float: left;margin-right: 15px;position: relative;padding:5px;background-color: #ccc;}
.bgImgList li:last-child{padding: initial;background-color:initial;}
.bgImgList li a{position: absolute;top: -8px;right: -8px;}
```

```javascript
const activityEdit = new Vue({
  el: '#activityEdit',
  data: {
    id:'',
    bgPreImg:[],
    bgImgFiles:[],
    bg_pic:[]
  },
  methods:{
    init(){
      // 获取 活动分类
      sendPostAxios(activityTypeUrl,{},(response)=>{
        const res = response.data
        this.activityType = res.data
      })
    },
    addPic(event){
      $(event.currentTarget).next('input[type=file]').trigger('click')
      return false
    },
    onFileChange(e) {
      let files = e.target.files || e.dataTransfer.files
      if (!files.length)return
      this.bgImgFiles = files
      this.createImage(files)
    },
    createImage(file) {
      if(typeof FileReader==='undefined'){
          layer.msg('您的浏览器不支持图片上传，请升级您的浏览器')
          return false
      }
      const leng = file.length
      for(var i=0;i<leng;i++){
        var reader = new FileReader()
        reader.readAsDataURL(file[i])
        reader.onload =(e) => {
          this.bgPreImg.push(e.target.result)
        }
      }
    },
    delImage:function(index){
      if (this.bg_pic.length > 0) {
        this.bg_pic.splice(index,1)
      }
      this.bgPreImg.splice(index,1)
    },
    removeImage: function(e) {
      this.bgPreImg = []
      this.bg_pic = []
    },
    uploadImage: function() {
      let sucNum = 0
      let retNum = 0
      for (let i = 0; i < this.bgImgFiles.length; i++) {
        let formData = new FormData()
        formData.append('activity_img',this.bgImgFiles[i])
        axios({
          method: 'post',
          url: uploadImgUrl,
          data: formData,
          headers:{'Content-Type':'multipart/form-data'}
        }).then((response)=>{
          const res = response.data
          console.log(res)
          retNum ++
          if (res.status == 0) {
            this.bg_pic.push(res.data)
            sucNum ++
          }
          if (retNum < this.bgImgFiles.length){
            return false
          }
          if(sucNum == this.bgImgFiles.length){
            layer.msg('上传成功')
          }else{
            this.bg_pic = []
            layer.msg('上传失败，请重新上传')
          }
        })
      }
    }
  },
  mounted() {
    this.init()
    this.id = location.search.substr(4)
    if (this.id == '') {
      return false
    }
    sendPostAxios(activityInfoUrl,{id:this.id},(response)=> {
      const res = response.data
      // 背景图片
      this.bg_pic = res.data.bg_pic.split(',')
      this.bgPreImg = res.data.bg_pic.split(',')
    })
  }
})
```


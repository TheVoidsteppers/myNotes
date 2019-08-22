## 确认框

```html
<style>
    .my-skin .layui-layer-btn a:first-child{border: 1px solid #1ab394;color:#1ab394;background-color: #fff;border-radius: 5px;}
    .my-skin .layui-layer-btn a:last-child{background-color: #1ab394;border: 1px solid #1ab394;border-radius: 5px;color: #FFF;}
    .my-skin .layui-layer-btn{text-align: center;padding: 0 20px 25px;}
</style>
<script>
    layer.confirm("是否修改审核状态", {
        title : "提示",
        skin : "my-skin",
        btn : [ '返回', '确定' ]
    },(index) =>{
        // 返回的回调函数
        layer.close(index)
    }, (index) =>{
        // 确定的回调函数
        if (this.checkitem.length <= 0) {
            layer.msg('未选中任何商品')
            layer.close(index)
            return false
        }
        const params = {
            apply_id:this.checkitem,
            audit:flag
        }
        sendPostAxios(updateAuditUrl,params,(response)=> {
            const res = response.data
            layer.msg(res.message)
            this.init()
        })
    })
</script>
```

## 输入框

```javascript
const remarkIndex = layer.open({
    id:1,
    type: 1,
    title:'请输入备注(选填)',
    skin:'layui-layer-rim',
    area:['450px', '265px'],
    content: `
<textarea class="remark" style="margin: 10px;width: 400px;height: 75px;resize: none;"></textarea>
`,
    btn:['确定','取消'],
    btn1:  (index,layero) =>{
        const remark = $(layero).find('.remark').val().trim()||''
        if (this.checkitem.length <= 0 && typeof id === 'undefined') {
            layer.msg('未选中任何商品')
            layer.close(remarkIndex)
            return false
        }
        const params = {
            audit:flag,
            remark:remark
        }
        if (typeof id === 'undefined') {
            params['apply_id'] = this.checkitem
        }else{
            params['apply_id'] = id
        }
        sendPostAxios(updateAuditUrl,params,(response)=> {
            const res = response.data
            layer.msg(res.message)
            layer.close(remarkIndex)
            this.init()
        })
    },
    btn2: (index,layero) =>{
        layer.close(remarkIndex);
    }
})
```

## end - 层销毁后触发的回调

```javascript
end: function(){} 
```


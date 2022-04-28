## 引入

```html
<script src="/static/js/jquery-2.2.3.min.js"></script>
<script src="/static/js/jquery.twbsPagination.min.js"></script>
```

## 使用

```html
<!--分页-->
<span style="font-size:14px;">
    <div class="text-center">
        <ul id="pagination" class="pagination-sm"></ul>
    </div>
</span>
```

## 初始化

```javascript
//分页
let totalPages = Math.ceil(res.data.count/20)
totalPages = totalPages<=0?1:totalPages
let visiblePages = totalPages<5?totalPages:5
$('#pagination').twbsPagination({
    //total总记录数，就是多少条数据  pages总页数
    totalPages: totalPages,
    visiblePages: visiblePages,
    first:'首页',
    last:'末页',
    prev:'上一页',
    next:'下一页',
    initiateStartPageClick:false,  // 不自动执行 onPageClick                             
    onPageClick: function (event, page) {
        // console.log(event, page);
	}
})
// 重载
$('#pagination').empty()
$('#pagination').removeData("twbs-pagination")
$('#pagination').unbind('page')
```

## 配置参数
{% raw %}
|         选项名称         |                  描述                  | 类型 |       默认值        |
| :----------------------: | :------------------------------------: | :--: | :-----------------: |
|       `totalPages`       |                  页数                  |  数  |          1          |
|       `startPage`        |          开始时显示的当前页面          |  数  |          1          |
|      `visiblePages`      |              最大可见页面              |  数  |         五          |
| `initiateStartPageClick` |  当插件初始化时，单击开始页面上的Fire  | 布尔 |       `true`        |
|    `hideOnlyOnePage`     | 如果它有一个页面，则会隐藏所有控制按钮 | 布尔 |       `false`       |
|          `href`          |         生成查询字符串或生成＃         | 布尔 |       `false`       |
|      `pageVariable`      |           将替换为页码的模板           |  串  |    `'{{page}}'`     |
|   `totalPagesVariable`   |             将被总页数替换             |  串  | `'{{total_pages}}'` |
|          `page`          |         它可用于自定义页码标签         |  串  |       `null`        |
|         `first`          |         “第一个”按钮的文本标签         |  串  |      `'First'`      |
|          `prev`          |           “上一页”按钮的标签           |  串  |    `'Previous'`     |
|          `next`          |           “下一步”按钮的标签           |  串  |      `'Next'`       |
|          `last`          |            “最后”按钮的标签            |  串  |      `'Last'`       |
|          `loop`          |           旋转木马风格的分页           | 布尔 |       `false`       |
|      `onPageClick`       |              单击事件回调              | 功能 |       `null`        |
|    `paginationClass`     |            分页组件的根样式            |  串  |   `'pagination'`    |
|       `nextClass`        |          “下一步”按钮的CSS类           |  串  | `'page-item next'`  |
|       `prevClass`        |           “上一个”按钮的类别           |  串  | `'page-item prev'`  |
|       `lastClass`        |            “最后”按钮的类别            |  串  | `'page-item last'`  |
|       `firstClass`       |           “第一个”按钮的类别           |  串  | `'page-item first'` |
|       `pageClass`        |             页面按钮的类别             |  串  |    `'page-item'`    |
|      `activeClass`       |             活动按钮的类别             |  串  |     `'active'`      |
|     `disabledClass`      |             禁用按钮的类别             |  串  |    `'disabled'`     |
|      `anchorClass`       |         CSS类用于按钮内的锚点          |  串  |    `'page-link'`    |
{% endraw %}

## 动态总页数

您可以通过两个简单的步骤来完成。调用`destroy`方法，然后使用新选项初始化它。

```javascript
var $pagination = $('selector');
var defaultOpts = {
    totalPages: 20
};
$pagination.twbsPagination(defaultOpts);
$.ajax({
    ...,
    success: function (data) {
        let totalPages = Math.ceil((res.data.data.count-0)/20)
        let visiblePages = totalPages<=5?totalPages:5
        if (visiblePages <= 0) {
            visiblePages = 1
        }
        if (totalPages <= 0) {
            totalPages = 1
        }

        if($('#pagination').data("twbs-pagination")){
            $('#pagination').twbsPagination('destroy')
        }

        $('#pagination').twbsPagination({
            totalPages: totalPages,
            visiblePages: visiblePages,
            hideOnlyOnePage: true,
            first: '首页',
            last: '末页',
            prev: '上一页',
            next: '下一页',
            startPage : goodsListVue['params']['page'],
            initiateStartPageClick: false,
            onPageClick: function(event, page) {
                goodsListVue['params']['page'] = page
                let params = goodsListVue['params']
                sendGetAxios(goodsListUrl,params, (response) => {
                    const res = response.data
                    console.log(res)
                })
            }
        })
	}
})
```


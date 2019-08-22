[高德ip定位Doc](<https://lbs.amap.com/api/webservice/guide/api/ipconfig/>)

```html
<script src="https://pv.sohu.com/cityjson?ie=utf-8"></script>
<script>
    window.source_ip = returnCitySN.cip
    const getGeoUrl = 'https://restapi.amap.com/v3/ip'
    axios({
        method:'get',
        url: getGeoUrl,
        params: {
            ip: returnCitySN.cip,
            key:'f374bc695faab863a3150011f9c45c85',
            output:'JSON'
        }
    }).then(function (response) {
        const res = response.data
        window.source_city = res.city
    })
</script>
```

| 参数名 | 含义             | 规则说明                                                     | 是否必须 | 缺省值 |
| :----- | :--------------- | :----------------------------------------------------------- | :------- | :----- |
| key    | 请求服务权限标识 | 用户在高德地图官网[申请Web服务API类型KEY](https://lbs.amap.com/dev/) | 必填     | 无     |
| ip     | ip地址           | 需要搜索的IP地址（仅支持国内）若用户不填写IP，则取客户http之中的请求来进行定位 | 可选     | 无     |
| sig    | 签名             | 选择数字签名认证的付费用户必填                               | 可选     | 无     |
| output | 返回格式         | 可选值：JSON,XML                                             | 可选     | JSON   |
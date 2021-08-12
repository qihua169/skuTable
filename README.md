# SkuTable 组件

## 简介

基于 Layui 的 SkuTable 组件。根据配置动态生成 sku 表。



## 效果

![Snipaste_2021-08-04_17-38-53.png](https://i.loli.net/2021/08/04/jkaxlJYsnywNUMZ.png)



![Video_20210805152802 00_00_00-00_00_30.gif](https://i.loli.net/2021/08/05/sl6UiPfXvF9rQaJ.gif)



## 示例

> 注意：带 fairy* 的 class 属性值固定写法不可更改！

```html
<!doctype html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>动态SKU表</title>
    <!-- 引入 layui.css -->
    <link rel="stylesheet" type="text/css" href="./layui/css/layui.css"/>
</head>
<body>

<div class="layui-container">
    <form action="" class="layui-form fairy-form">
        <!-- sku参数表 -->
        <div class="layui-form-item">
            <label class="layui-form-label">规格：</label>
            <div class="layui-input-block">
                <div class="fairy-spec-table"></div>
            </div>
        </div>

        <!-- 动态sku表 -->
        <div class="layui-form-item">
            <label class="layui-form-label">SKU表：</label>
            <div class="layui-input-block">
                <div class="fairy-sku-table"></div>
            </div>
        </div>

        <div class="layui-form-item">
            <div class="layui-input-block">
                <button class="layui-btn" lay-submit lay-filter="submit">立即提交</button>
                <button type="reset" class="layui-btn layui-btn-primary">重置</button>
            </div>
        </div>
    </form>
</div>
<!-- 引入 layui.js -->
<script src="./layui/layui.js"></script>
<!-- 引入 skuTable.js -->
<script src="./skuTable.js"></script>
<script>
    layui.config({
        base: './'
    }).use(['form', 'skuTable'], function () {
        var form = layui.form, skuTable = layui.skuTable;

        //注意！！！ 注意！！！ 注意！！！
        //如果配置了相关接口请求的参数，请置本示例于服务器中预览，不然会有浏览器跨域问题
        //示例中的json文件仅做格式返回参考，若多次执行添加规格后再为新增后的规格添加规格值，会发现所有新增的规格都增加了该规格值。注意！此处并非是bug，原因是因为示例中返回的新增规格值id是重复的，而在正常接口请求每次返回的新增规则id是不一样的
        var obj = skuTable.render({
            //sku表相同属性值是否合并行
            rowspan: true,
            //上传接口地址
            //接口要求返回格式为 {"code": 200, "data": {"url": "xxx"}, "msg": ""}
            uploadUrl: './json/upload.json',
            //添加规格接口地址，如果为空则表示不允许增加规格
            //接口要求返回格式为 {"code": 200, "data": {"id": "xxx"}, "msg": ""}
            specCreateUrl: './json/specCreate.json',
            //添加规格值接口地址，如果为空则表示不允许增加规格值
            //接口要求返回格式为 {"code": 200, "data": {"id": "xxx"}, "msg": ""}
            specValueCreateUrl: './json/specValueCreate.json',
            //sku表格配置参数
            skuTableConfig: {
                thead: [
                    {title: '图片', icon: ''},
                    {title: '销售价(元)', icon: 'layui-icon-cols'},
                    {title: '市场价(元)', icon: 'layui-icon-cols'},
                    {title: '成本价(元)', icon: 'layui-icon-cols'},
                    {title: '库存', icon: 'layui-icon-cols'},
                    {title: '状态', icon: ''},
                ],
                tbody: [
                    {type: 'image', field: 'picture', value: '', verify: '', reqtext: ''},
                    {type: 'input', field: 'price', value: '0', verify: 'required|number', reqtext: '销售价不能为空'},
                    {type: 'input', field: 'market_price', value: '0', verify: 'required|number', reqtext: '市场价不能为空'},
                    {type: 'input', field: 'cost_price', value: '0', verify: 'required|number', reqtext: '成本价不能为空'},
                    {type: 'input', field: 'stock', value: '0', verify: 'required|number', reqtext: '库存不能为空'},
                    {type: 'select', field: 'status', option: [{key: '启用', value: '1'}, {key: '禁用', value: '0'}], verify: 'required', reqtext: '状态不能为空'},
                ]
            },
            //规格数据, 一般从接口获取
            specData: [
                {
                    id: "1",
                    title: "颜色",
                    child: [
                        {id: "1", title: "红", checked: true},
                        {id: "2", title: "黄", checked: false},
                        {id: "3", title: "蓝", checked: false}
                    ]
                }, {
                    id: "2",
                    title: "尺码",
                    child: [
                        {id: "4", title: "S", checked: true},
                        {id: "5", title: "M", checked: true},
                        {id: "6", title: "L", checked: false},
                        {id: "7", title: "XL", checked: false}
                    ]
                }, {
                    id: "3",
                    title: "款式",
                    child: [
                        {id: "8", title: "男款", checked: true},
                        {id: "9", title: "女款", checked: true}
                    ]
                }
            ],
            //sku数据
            //新增的时候为空对象
            //编辑的时候可以从后台接收，会自动填充sku表，可以去掉注释看效果
            // skuData: {
            //     "skus[1-4-8][picture]": "https://cdn.layui.com/upload/2019_5/168_1559291577683_9348.png",
            //     "skus[1-4-8][price]": "100",
            //     "skus[1-4-8][market_price]": "200",
            //     "skus[1-4-8][cost_price]": "50",
            //     "skus[1-4-8][stock]": "18",
            //     "skus[1-4-8][status]": "0",
            //     "skus[1-4-9][picture]": "",
            //     "skus[1-4-9][price]": "0",
            //     "skus[1-4-9][market_price]": "0",
            //     "skus[1-4-9][cost_price]": "0",
            //     "skus[1-4-9][stock]": "0",
            //     "skus[1-4-9][status]": "1",
            //     "skus[1-5-8][picture]": "",
            //     "skus[1-5-8][price]": "0",
            //     "skus[1-5-8][market_price]": "0",
            //     "skus[1-5-8][cost_price]": "0",
            //     "skus[1-5-8][stock]": "0",
            //     "skus[1-5-8][status]": "1",
            //     "skus[1-5-9][picture]": "",
            //     "skus[1-5-9][price]": "0",
            //     "skus[1-5-9][market_price]": "0",
            //     "skus[1-5-9][cost_price]": "0",
            //     "skus[1-5-9][stock]": "0",
            //     "skus[1-5-9][status]": "1"
            // }
        });

        form.on('submit(submit)', function (data) {
            //获取规格数据
            console.log(obj.getSpecData());
            //获取表单数据
            console.log(data.field);

            var state = Object.keys(data.field).some(function (item, index, array) {
                return item.startsWith('skus');
            });
            state ? layer.alert(JSON.stringify(data.field), {title: '提交的数据'}) : layer.msg('sku表数据不能为空', {icon: 5, anim: 6});
            return false;
        });
    });
</script>
</body>
</html>
```


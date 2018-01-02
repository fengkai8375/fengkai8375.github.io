---
layout: post
title: "Bootstrap创建表头不动内容滚动且对齐的表格"
date: 2018-01-08 09:00:00 +0800 
categories: JavaScript
tag:
 - JavaScript
 - Bootstrap
---
* content
{:toc}

在Bootstrap创建表头不动内容滚动且对齐的表格。

要求：

1. 表头不动
2. 表格内容超出最大高度的部分自动滚动
3. 表头和内容的单元格对齐！

直接贴代码：

<!-- more -->

> 由于用的是Bootstrap，所以需要先引入相关的CSS和JS，我这里省略了。

```
<div id="data_area">
    <table class="scrolltable">
        <thead style="display:block;overflow-y: scroll;border-bottom:1px solid #eee;">
        <tr>
            <th><input class="check-all" type="checkbox"/></th>
            <th>ID</th>
            <th>用户名</th>
            <th>操作</th>
        </tr>
        </thead>
        <tbody style="display:block; max-height:800px;overflow-y: scroll;">
        <tr>
            <td>
                <input class="ids" type="checkbox" name="id[]" value=""/>
            </td>
            <td>ID</td><td>用户名</td><td>操作</td>
        </tr>
        <tr>
            <td>
                <input class="ids" type="checkbox" name="id[]" value=""/>
            </td>
            <td>ID</td><td>用户名</td><td>操作</td>
        </tr>
        <tr>
            <td>
                <input class="ids" type="checkbox" name="id[]" value=""/>
            </td>
            <td>ID</td><td>用户名</td><td>操作</td>
        </tr>
        <tr>
            <td>
                <input class="ids" type="checkbox" name="id[]" value=""/>
            </td>
            <td>ID</td><td>用户名</td><td>操作</td>
        </tr>
        </tbody>
    </table>
</div>
<script type="text/javascript" >
    $(function(){
        var _width=$('#data_area').width();
        var children_len = $('#data_area thead tr').children().length;
        other_width = _width* (0.9 / (children_len -2));
        for(var i=1;i<children_len;i++){
            $('#data_area thead tr').children().eq(i).width(other_width);
        }
        $('#data_area tbody tr').each(function(){
            for(var k=1;k<children_len;k++){
                $(this).children().eq(k).width(other_width);
            }
        });
    })
</script>
```

关键在于`JavaScript`部分，在页面加载完成后动态调整每个`td/th`的宽度。在本示例中，头两个`td/th`因含`checkbox`，所以合占10%的宽度，其余`td/th`平分剩下的90%。可以根据自己的需要做细节上的调整。


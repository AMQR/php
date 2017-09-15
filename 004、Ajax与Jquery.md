# Ajax与JQuery


Ajax做网络请求非常方便，但是代码比较累赘，所以如果有封装起来会更加好用。

## JQuery封装的Ajax Api


JQuery为我们封装了便捷的Ajax Api，大概如下


方法 |	描述
--  | ---
$.ajax()	| 执行异步 AJAX 请求
$.ajaxPrefilter() |  在每个请求发送之前且被 $.ajax() 处理之前，处理自定义 Ajax 选项或修改已存在选项
$.ajaxSetup()| 	为将来的 AJAX 请求设置默认值
$.ajaxTransport()	| 创建处理 Ajax 数据实际传送的对象
$.get()	 |  使用 AJAX 的 HTTP GET 请求从服务器加载数据
$.getJSON()| 	使用 HTTP GET 请求从服务器加载 JSON 编码的数据
$.getScript()| 	使用 AJAX 的 HTTP GET 请求从服务器加载并执行 JavaScript
$.param()| 	创建数组或对象的序列化表示形式（可用于 AJAX 请求的 URL 查询字符串）
$.post()| 	使用 AJAX 的 HTTP POST 请求从服务器加载数据
ajaxComplete()| 	规定 AJAX 请求完成时运行的函数
ajaxError()| 	规定 AJAX 请求失败时运行的函数
ajaxSend()| 	规定 AJAX 请求发送之前运行的函数
ajaxStart()| 	规定第一个 AJAX 请求开始时运行的函数
ajaxStop()| 	规定所有的 AJAX 请求完成时运行的函数
ajaxSuccess()| 	规定 AJAX 请求成功完成时运行的函数
load()| 	从服务器加载数据，并把返回的数据放置到指定的元素中
serialize()	| 编码表单元素集为字符串以便提交
serializeArray()	| 编码表单元素集为 names 和 values 的数组


`具体使用参考`  [jQuery AJAX 方法](http://www.runoob.com/jquery/jquery-ref-ajax.html)

.
.
## JQuery的ajax方法


### 最简单的使用
使用 AJAX 请求改变 `<div>` 元素的文本
```

$("button").click(function(){
    $.ajax({url:"demo_test.txt",success:function(result){
        $("#div1").html(result);
    }});
});
```

### 实例

上图吧
![](/assets/jq_aj.gif)


west_aj/html
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JSON</title>
    <style>
        table {
            width: 700px;
            border-collapse: collapse;
        }

        td {
            height: 40px;
            text-align: center;
            border: 1px solid #CCC;
        }
    </style>
</head>
<body>
<table>
</table>
<div class="btn"><input type="button" value="jQuery Ajax方法"></div>

<!-- 引入jQuery -->
<script src='./js/jquery-1.11.1.min.js'></script>

<!-- 运行脚本 -->
<script>
    //var btn = document.querySelector('.btn');

    $('.btn').on('click', function () {
        // 调用jQuery中的ajax方法
        $.ajax({
            type: 'post',
            url: 'west.php',
            timeout: 3000,
            dataType: 'json',
            // async: false,
            data: {name: 'haha', age: 10},
            beforeSend: function () {
                alert('beforeSend');
                // 会终止请求
                return true;
            },
            success: function (info) {
                alert('success');
                // info 实际就是服务端返回的数据
                console.log(info);

                var html = '';

                for (var i = 0; i < info.length; i++) {
                    html += '<tr>';
                    html += '<td>' + info[i].name + '</td>';
                    html += '<td>' + info[i].nickname + '</td>';
                    html += '<td>' + info[i].arms + '</td>';
                    html += '<td>' + info[i].num + '</td>';
                    html += '<td>' + info[i].sex + '</td>';
                    html += '</tr>';
                }

                $('table').html(html);

            },
            error: function (err, errmsg) {
                alert('error');

                // 错误原因
                console.log(errmsg);
            },
            complete: function () {
                alert('complete');
            }
        });
    });
</script>

</body>
</html>

```










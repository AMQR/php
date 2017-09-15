# [^1]Ajax与JQuery

Ajax做网络请求非常方便，但是代码比较累赘，所以如果有封装起来会更加好用。

## JQuery封装的Ajax Api

JQuery为我们封装了便捷的Ajax Api，大概如下

| 方法 | 描述 |
| --- | --- |
| $.ajax\(\) | 执行异步 AJAX 请求 |
| $.ajaxPrefilter\(\) | 在每个请求发送之前且被 $.ajax\(\) 处理之前，处理自定义 Ajax 选项或修改已存在选项 |
| $.ajaxSetup\(\) | 为将来的 AJAX 请求设置默认值 |
| $.ajaxTransport\(\) | 创建处理 Ajax 数据实际传送的对象 |
| $.get\(\) | 使用 AJAX 的 HTTP GET 请求从服务器加载数据 |
| $.getJSON\(\) | 使用 HTTP GET 请求从服务器加载 JSON 编码的数据 |
| $.getScript\(\) | 使用 AJAX 的 HTTP GET 请求从服务器加载并执行 JavaScript |
| $.param\(\) | 创建数组或对象的序列化表示形式（可用于 AJAX 请求的 URL 查询字符串） |
| $.post\(\) | 使用 AJAX 的 HTTP POST 请求从服务器加载数据 |
| ajaxComplete\(\) | 规定 AJAX 请求完成时运行的函数 |
| ajaxError\(\) | 规定 AJAX 请求失败时运行的函数 |
| ajaxSend\(\) | 规定 AJAX 请求发送之前运行的函数 |
| ajaxStart\(\) | 规定第一个 AJAX 请求开始时运行的函数 |
| ajaxStop\(\) | 规定所有的 AJAX 请求完成时运行的函数 |
| ajaxSuccess\(\) | 规定 AJAX 请求成功完成时运行的函数 |
| load\(\) | 从服务器加载数据，并把返回的数据放置到指定的元素中 |
| serialize\(\) | 编码表单元素集为字符串以便提交 |
| serializeArray\(\) | 编码表单元素集为 names 和 values 的数组 |

`具体使用参考`  [jQuery AJAX 方法](http://www.runoob.com/jquery/jquery-ref-ajax.html)

.  
.

## JQuery的ajax方法

### 1、最简单的使用ajaxs适用



使用 AJAX 请求改变 `<div>` 元素的文本

```
$("button").click(function(){
    $.ajax({url:"demo_test.txt",success:function(result){
        $("#div1").html(result);
    }});
});
```

### 2、详细实例

上图吧  
![](/assets/jq_aj.gif)

west\_aj/html

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

.  
.  
west.json

```
[
    {
        "name": "猴子",
        "nickname": "孙悟空",
        "arms": "金箍棒",
        "num": 1,
        "sex": "男"
    },
    {
        "name": "猪",
        "nickname": "朱悟能",
        "arms": "九齿钉耙",
        "num": 2,
        "sex": "男"
    },
    {
        "name": "胡子",
        "nickname": "沙悟净",
        "arms": "没想好",
        "num": 3,
        "sex": "男"
    },
    {
        "name": "龙马",
        "nickname": "没取上法号",
        "arms": "提子吧",
        "num": 4,
        "sex": "男"
    }
]
```

.  
.  
west.php

```
<?php
    // 指定文档类型  application/json
    // 告诉浏览器这个文件要当成一个json来处理
    header('Content-Type:application/json; charset=utf-8');
    $result = file_get_contents('west.json');
    //var_dump($result);  // 字符串
    echo $result;
?>
```

.  
.
### 3、`$.getJSON() 以GET方式请求json数据`

`$.getJSON`
`相当于$.ajax({}) dataType默认值为json `

```
        $('.btn').on('click', function () { 
            // 以GET方式请求json数据 
      // 相当于$.ajax({}) dataType默认值为json 
            $.getJSON('stars.php', function (json) { 
                console.log(json); 
            }); 

            // $.ajax({ 
            //     type: '', 
            //     dataType: 'json' 
            // }) 
        }); 
```


`$.getScript` 
相当于`$.ajax({}) dataType默认值为script `

### 4、`$.getScript() 以GET方式请求javascript脚本`

```
        $('.btn').on('click', function () { 
            // $.getScript() 以GET方式请求javascript脚本 
            // 相当于$.ajax({}) dataType默认值为script 
            $.getScript('getScript.js', function () { 
                alert(a); 
            }); 

            // $.ajax({ 
            //     dataType: 'script' 
            // }) 
        }); 
```

.
.
getScript.js

```
var a = 10;
var b = 20;
var c = 30;
```
.
.
###  5、$('selector').load('xxx.html') 以GET方式加载html片段

```
<body> 
    <div class="wrapper"> 
        <header class="header"></header> 
    </div> 
    <button class="btn">测试jQuery load方法</button> 
    <!-- 引入jQuery --> 
    <script src="./js/jquery.min.js"></script> 
    <script> 

        $('.btn').on('click', function () { 

            // 加载html片段 
            $('.header').load('template.html'); 

        }); 

    </script> 
</body> 
```
.
.

template.html
```
<!-- html片段，一般用于公共/重复的模板 --> 
<nav>哪个网站没有头</nav>
```

.
.
### serialize() 序列化表单（即格式化key=val&key=val）

```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
<script src="http://cdn.static.runoob.com/libs/jquery/1.10.2/jquery.min.js">
</script>
<script>
$(document).ready(function(){
	$("button").click(function(){
		$("div").text($("form").serialize());
	});
});
</script>
</head>
<body>

<form action="">
第一个名称: <input type="text" name="FirstName" value="Mickey" /><br>
最后一个名称: <input type="text" name="LastName" value="Mouse" /><br>
</form>
<button>序列化表单值</button>
<div></div>

</body>
</html>
```

拼接结果：FirstName=Mickey&LastName=Mouse
.
.
### ajaxSetup({}) 全局配置Ajax请求

jQuery在XMLHttpRequest内置对象的基础之上结合现实开发中的需求封装了很多方法，并且可以灵活配置参数实现不同的业务需求，然而有些回调方法XMLHttpRequest对象并没有，比如beforeSend和complete回调方法，这些方法是jQuery进行的封装，其目的就是为了提升开发效率。

jQuery把一个ajax请求分成了若干个阶段，不是所有请求都回调了所有方法，我们可以将这些回调做一个全局配置。

.
.
**01.HTML**
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>jQuer Ajax 全局事件</title>
	<style>
		body {
			margin: 0;
			padding: 0;
			background-color: #F7F7F7;
		}

		table {
			width: 900px;
			border: 1px solid #CCC;
			border-collapse:collapse;
			margin: 50px auto;
		}

		caption {
			font-size: 24px;
			margin-bottom: 10px;
		}

		td, th {
			height: 40px;
			text-align: center;
			border: 1px solid #CCC;
		}

		button {
			display: block;
			margin: 10px auto;
		}

		.loading {
			display: none;
		}

	</style>
</head>
<body>
	<table>
		<caption>jQuery Ajax 全局事件</caption>
		<tr>
			<th>操作</th>
			<th>ajaxStart</th>
			<th>ajaxSend</th>
			<th>ajaxSuccess</th>
			<th>ajaxError</th>
			<th>ajaxComplete</th>
			<th>ajaxStop</th>
			<th>Loading状态</th>
		</tr>
		<tr>
			<td>
				<button class="btn1">点此获取</button>
				<button class="btn2">点此获取</button>
			</td>
			<td><span class="start">被执行了...</span></td>
			<td><span class="send">被执行了...</span></td>
			<td><span class="success">被执行了...</span></td>
			<td><span class="error">被执行了...</span></td>
			<td><span class="complete">被执行了...</span></td>
			<td><span class="stop">被执行了...</span></td>
			<td><img class="loading" src="./images/loading.gif" alt=""></td>
		</tr>
	</table>
	<script src="./js/jquery.min.js"></script>
	<script>
		// Ajax请求
		$('.btn1').on('click', function () {

			$.ajax({
				type: 'post',
				url: '01.php',
				data: {},
				success: function () {},
				error: function () {}
			});

		});

		$('.btn2').on('click', function () {

			$.ajax({
				type: 'post',
				url: '02.php',
				dataType: 'json',
				data: {},
				success: function () {},
				error: function () {}
			});

		});

		// 全局事件
		// 只要有Ajax请求，全局事件就会触发
		$('.start').ajaxStart(function () {
			$(this).css('color', 'red');
			console.log('start');
		});

		// 发送
		$('.send').ajaxSend(function () {
			$(this).css('color', 'red');
			console.log('send');
		});

		// 成功
		$('.success').ajaxSuccess(function () {
			$(this).css('color', 'red');
			console.log('success');
		});

		// 失败
		$('.error').ajaxError(function () {
			$(this).css('color', 'red');
			console.log('error');
		});

		// 完成(任可一个请求完成即触发)
		$('.complete').ajaxComplete(function () {
			$(this).css('color', 'red');
			console.log('complete');
		});

		// 所有请求都完成
		$('.stop').ajaxStop(function () {
			$(this).css('color', 'red');
			console.log('stop');
		});


		// send显示loading状态
		// complete隐藏loading状态
		$('.loading').ajaxSend(function () {
			$(this).show();
		});

		$('.loading').ajaxComplete(function () {
			$(this).hide();
		});
	</script>
</body>
</html>
```
.
.
**01.php**
```
<?php
	echo 'hello ajax';
	sleep(5);
?>
```
.
.


**02.php**
```
<?php

	echo 'hello ajax';
	// sleep(1);
?>```


[ajaxSetup demo 链接](https://github.com/AMQR/php/blob/master/assets/SETUP.zip)
.
.






end

> 本文目的，结合php服务器，进行数据解析。

很常见的一个过程，大概如下：

* 1、html网页上点击某个按钮，发起网络请求，连接后台php
* 2、php网页查询数据库，我们这里用一个json文件模拟数据库结果，然后返回结果
* 3、html网页拿到返回的文本，将文本转成Json，最后处理Json，将结果展示在页面上


# 一、xml原生解析
上图
![](/assets/parseXml.gif)

涉及到三个文件

## `west_for_x.xml`

```
<?xml version="1.0" encoding="utf-8"?>
<root>
	<person>
		<name type="star">猴子xml</name>
		<nickname>孙悟空</nickname>
		<arms>金箍棒</arms>
		<num>1</num>
		<sex>男</sex>
	</person>
	<person>
		<name type="star">猪xml</name>
		<nickname>朱悟能</nickname>
		<arms>九齿钉耙</arms>
		<num>2</num>
		<sex>男</sex>
	</person>
	<person>
		<name type="star">胡子xml</name>
		<nickname>沙悟净</nickname>
		<arms>没想好</arms>
		<num>3</num>
		<sex>男</sex>
	</person>
	<person>
		<name type="star">龙马xml</name>
		<nickname>没取上法号</nickname>
		<arms>提子吧</arms>
		<num>4</num>
		<sex>男</sex>
	</person>

</root>


```
.
.
## west_for_x.html
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
	<div class="btn"><input type="button" value="陪阿唐去天竺的小伙伴(xml)"></div>

	<!-- 运行脚本 -->
	<script>
		var btn = document.querySelector('.btn');

		btn.onclick = function () {

			// 发起请求
			var xhr = new XMLHttpRequest;
			xhr.open('get', 'west_for_x.php');
			xhr.send(null); // get方式send为null

			// 响应
			xhr.onreadystatechange = function () {
				if(xhr.readyState == 4 && xhr.status == 200) {
					// 当返回的内容为xml格式时
					var result = xhr.responseXML;
					console.log(result);

					// 通过DOM解析
					var persons = result.getElementsByTagName('person');

					var html = '';

					//  遍历生成HTML字符串
					for(var i=0; i<persons.length; i++) {
							var name = persons[i].getElementsByTagName('name')[0];
							var nickname = persons[i].getElementsByTagName('nickname')[0];
							var arms = persons[i].getElementsByTagName('arms')[0];
							var num = persons[i].getElementsByTagName('num')[0];
							var sex = persons[i].getElementsByTagName('sex')[0];

							html += '<tr>';
							html += '<td>' + name.innerHTML + '</td>';
							html += '<td>' + nickname.innerHTML + '</td>';
							html += '<td>' + arms.innerHTML + '</td>';
							html += '<td>' + num.innerHTML + '</td>';
							html += '<td>' + sex.innerHTML + '</td>';
							html += '</tr>';
					}
					//console.log(html);

					// 通过innerHTML的方式写入布局
					document.querySelector('table').innerHTML = html;
				}
			}
		}
	</script>

</body>
</html>


```
.
.
## west_for_x.php
```
<?php

	// 指定文档类型  text/xml
	// 由PHP处理程序，生成一个xml文档
	header('Content-Type: text/xml; charset=utf-8');
	// 获取xml文档
	$result = file_get_contents('west_for_x.xml');

	echo $result;
?>


```

.
.

# 二、json解析

先看效果吧



![](/assets/parseJson.gif)


涉及到如下三个文件

## west.json

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
## west.html

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
	<div class="btn"><input type="button" value="陪阿唐去天竺的小伙伴"></div>

	<!-- 运行脚本 -->
	<script>
		var btn = document.querySelector('.btn');

		btn.onclick = function () {

			// 发起请求
			var xhr = new XMLHttpRequest;
			xhr.open('get', 'west.php');
			xhr.send(null); // get方式send为null

			// 响应
			xhr.onreadystatechange = function () {
				if(xhr.readyState == 4 && xhr.status == 200) {
					var result = JSON.parse(xhr.responseText); //  // 将文本转换成Json
					console.log(result);

					var html = '';

					// 解析拼凑字符串，比较麻烦比较类
					for (var i = 0; i < result.length; i++) {
							html += '<tr>';
							html += '<td>' + result[i].name +'</td>';
							html += '<td>' + result[i].nickname +'</td>';
							html += '<td>' + result[i].arms +'</td>';
							html += '<td>' + result[i].num +'</td>';
							html += '<td>' + result[i].sex +'</td>';
							html += '</tr>';
					}
					// 通过innerHTML的方式写入布局
					document.querySelector('table').innerHTML = html;
				}
			}
		}
	</script>

</body>
</html>


```
.
.
## west.php
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


原生Json解析至此结束
.
.
.
.
.


## 对比

1、首先php的header的设置肯定是不同的
2、返回结果的xhr.responseText和xhr.responseXML也是不一样的
3、剩下就是各自的解析语法啦


本文到此结束，我们看到原生解析语法复杂，手动拼凑非常容易出错，相关的下一篇我们会说一下 `引擎模板` ，利用引擎模板我们可以很好地解决这个问题。

模板引擎，其本质是利用正则表达式，替换模板当中预先定义好的标签。
主要作用：解决拼接字符串不方便的问题。
我们想将页面展现在页面上，但是我们又不想去拼接字符串，就可以考虑使用模板引擎。


end。








> 本文目的，结合php服务器，进行数据解析。

很常见的一个过程，大概如下：

* 1、html网页上点击某个按钮，发起网络请求，连接后台php
* 2、php网页查询数据库，我们这里用一个json文件模拟数据库结果，然后返回结果
* 3、html网页拿到返回的文本，将文本转成Json，最后处理Json，将结果展示在页面上


# 一、xml原生解析

# 二、json解析

先看效果吧



![](/assets/parseJson.gif)


涉及到如下三个文件

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


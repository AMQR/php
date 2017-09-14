# XMLHttpRequest

XMLHttpRequest是浏览器内建对象，用于在`后台与服务器通信`\(交换数据\) ，由此我们便可实现对网页的部分更新，而不是刷新整个页面。

我们先来看一个简单由于XMLHttpRequest完成的完整的请求过程。

```
<!DOCTYPE html> 
<html lang="en"> 
<head> 
    <meta charset="UTF-8"> 
    <title>Document</title> 
    <style> 
        h3 { 
            font-size: 28px; 
            text-align: center; 
        } 
    </style> 
</head> 
<body> 
    <h3>XMLHttpRequest</h3> 
    <p id="result"></p> 
    <script> 

        // 实例化一个对象，并返回一个对象实例 
        // 并且这个对象实例提供了一些方法 
        var xhr = new XMLHttpRequest; 

        // 设置请求行，遵照HTTP协议需要有一个 
        // 请求方式、请求URL 
        xhr.open('post', '8-1.php'); 

        // 设置请求头，遵照HTTP协议 
        // 请求头是键值对的形式 
        xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded'); 

        // 设置请求主体，遵照HTTP协议 
        // 满足key=val&key=val格式 
        xhr.send('name=itcast&age=10'); 

        // 监听并处理响应结果 
        // 由于网络等原因，响应并不能马上完成 
        // 所以将请求及响应划分成了若干状态0~4 
        // 处在不同的阶段会有不同的状态码 
        // 然后提供了一个事件来监听状态的变化 
        // 但是只有状态等于4时候最有意义 
        xhr.onreadystatechange = function () { 
            if(xhr.readyState == 4) { 
                // 接收服务器返回的内容，即响应主体 
                var result = xhr.responseText; 

                // 通过DOM操作将数据添加到页面中去 
                document.getElementById('result').innerHTML = result; 
            } 
        } 


        // 还有一些程序等待执行 
    </script> 
</body> 
</html>
```

## 一、请求和响应

一个网络请求，可以分成两部分，请求和响应。

### 一.1 请求

我们知道，一个请求可以分为  `请求行` `请求头`  `请求体`  三部分

**设置请求头**

```
xhr.open('post', '8-1.php');
```

设置请求头

```
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
```

设置请求体

```
xhr.send('name=zhangsan&age=10');
```

### 一.1 响应

HTTP响应是由服务端发出的，作为客户端更应关心的是响应的结果。

HTTP响应3个组成部分与XMLHttpRequest方法或属性的对应关系。

由于服务器做出响应需要时间（比如网速慢等原因），所以我们需要监听服务器响应的状态，然后才能进行处理。

`onreadystatechange`是Javascript的事件的一种，其意义在于监听XMLHttpRequest的状态

```
        // 监听并处理响应结果 
        // 由于网络等原因，响应并不能马上完成 
        // 所以将请求及响应划分成了若干状态0~4 
        // 处在不同的阶段会有不同的状态码 
        // 然后提供了一个事件来监听状态的变化 
        // 但是只有状态等于4时候最有意义 
        xhr.onreadystatechange = function () { 
            if(xhr.readyState == 4) { 
                // 接收服务器返回的内容，即响应主体 
                var result = xhr.responseText; 

                // 通过DOM操作将数据添加到页面中去 
                document.getElementById('result').innerHTML = result; 
            } 
        }
```

**1、获取状态行 （状态码和状态信息）**

```
xhr.status; // 获取状态码
xhr.statusText; // 状态信息
```

**2、获取响应头**

```
// 获取指定头信息xhr.getResponseHeader('Content-Type');
​// 获取所有响应头信息xhr.getAllResponseHeaders();
```

**3、响应主体**

```
xhr.responseText();
xhr.responseXML();
```

我们需要检测并判断响应头的MIME类型后确定使用request.responseText或者request.responseXML

## 二、api详解

* xhr.open\(\) 发起请求，可以是get、post方式

  * `同步和异步`   open方法还可以传入boolean值，默认为true，代表异步，如果传入fase则为同步

* xhr.setRequestHeader\(\) 设置请求头

* xhr.send\(\) 发送请求主体。

  * `get方式使用xhr.send(null)`

* xhr.onreadystatechange = function \(\) {} 监听响应状态

> xhr.readyState = 0时，UNSENT open尚未调用
>
> xhr.readyState = 1时，OPENED open已调用
>
> xhr.readyState = 2时，HEADERS\_RECEIVED 接收到头信息
>
> xhr.readyState = 3时，LOADING 接收到响应主体
>
> xhr.readyState = 4时，DONE 响应完成

响应严格的话应该进入双重判断

readyState 和 status 同时判断

```
if(xhr.readyState == 4 && xhr.status == 200) {
}
```

### 三、注意点

#### get

1、GET没有请求主体，使用xhr.send\(null\)

2、GET可以通过在请求URL上添加请求参数

3、GET效率更好（应用多）

4、GET大小限制约4K，POST则没有限制

#### post

5、POST可以通过xhr.send\('name=zhangsan&age=10'\)

6、POST需要设置请求头

xhr.setRequestHeader\('Content-Type', 'application/x-www-form-urlencoded'\);

  
p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; font: 26.0px 'Helvetica Neue'; color: \#000000}  
span.s1 {font: 26.0px '.PingFang SC Light'}  


\#\#\#\# 完成



end


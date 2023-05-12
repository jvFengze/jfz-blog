---
title: JS高级语法
index_img: ./img/JS.webp
date: 2023-2-14 12:36
categories: JS
---

# JS高级语法

## Exception

### 异常

在ES3之前JavaScript代码执行的过程中，一旦出现错误，整个JavaScript代码都会停止执行，这样就显的代码非常的不健壮。

在Java或C#等一些高级语言中，都提供了异常处理机制，可以处理出现的异常，而不会停止整个应用程序。

从ES3开始，JavaScript也提供了类似的异常处理机制，从而让JavaScript代码变的更健壮，即使执行的过程中出现了异常，也可以让程序具有了一部分的异常恢复能力。

> 当错误发生时，JavaScript 提供了错误信息的内置 error 对象。Error具有下面一些主要属性：
>
> - description: 错误描述 (仅IE可用).
> - fileName: 出错的文件名 (仅Mozilla可用).
> - lineNumber: 出错的行数 (仅Mozilla可用).
> - message: 错误信息 (在IE下同description)
> - name: 错误类型.
> - number: 错误代码 (仅IE可用).
> - stack: 像Java中的Stack Trace一样的错误堆栈信息 (仅Mozilla可用).

error 对象提供两个有用的属性：`name` 和 `message` 。

| 属性    | 描述                             |
| ------- | -------------------------------- |
| name    | 设置或返回错误名                 |
| message | 设置或返回错误消息（一条字符串） |

error 的 name 属性可返回六个不同的值：

| 错误名         | 描述                          |
| -------------- | ----------------------------- |
| EvalError      | 已在 eval() 函数中发生的错误  |
| RangeError     | 已发生超出数字范围的错误      |
| ReferenceError | 已发生非法引用                |
| SyntaxError    | 已发生语法错误                |
| TypeError      | 已发生类型错误                |
| URIError       | 在 encodeURI() 中已发生的错误 |

### 异常捕捉

ES3开始引入了 try-catch 语句，是 JavaScript 中处理异常的标准方式。

```javascript
try{ 
//可能发生异常的代码 
}catch(error){ 
//发生错误执行的代码 
}finally {
    // 无论是否出错都会执行的代码
}
```

> 在JavaScript中，如果添加了 finally 语句，则 catch 语句可以省略。但是如果没有 catch 语句，则一旦发生错误就无法捕获这个错误，所以在执行完 finally 中的语句后，程序就会立即停止了。所以，在实际使用中，最好一直带着 catch 语句。如果你写了 catch 语句，则finally 语句也是可以省略的。

### 异常抛出

在大部分的代码执行过程中，都是出现错误的时候，由浏览器(javascript引擎)抛出异常，然后程序或者停止执行或被try…catch 捕获。

然而有时候我们在检测到一些不合理的情况发生的时候也可以主动抛出错误，请使用 throw 关键字抛出来主动抛出异常。

> 1. thow后面就是我们要抛出的异常对象，在以前的时候都是出现错误的时候浏览器抛出异常对象，只是现在是我们自己主动抛出的异常对象。
> 2. 只要有异常对象抛出，不管是浏览器抛出的，还是代码主动抛出，都会让程序停止执行。如果想让程序继续执行，则有也可以用try…catch来捕获。
> 3. 每一个错误类型都可以传入一个参数，表示实际的错误信息。
> 4. 我们可以在适当的时候抛出任何我们想抛出的异常类型。`throw new SyntaxError("语法错误...");`

**主动抛出自定义异常**

如果要自定义错误类型，只需要继承任何一个自定义错误类型都可以，一般直接继承Error即可。

```
MyError.prototype = new Error();
throw new MyError("注意：这是自定义错误类型")
```

## JSON

JSON：JavaScript Object Notation（JavaScript 对象标记法），它是一种存储和交换数据的语法。

当数据在浏览器与服务器之间进行交换时，这些数据只能是文本，JSON 属于文本并且我们能够把任何 JavaScript 对象转换为 JSON，然后将 JSON 发送到服务器。我们也能把从服务器接收到的任何 JSON 转换为 JavaScript 对象。以这样的方式，我们能够把数据作为 JavaScript 对象来处理，无需复杂的解析和转译。

### 语法

在json中，每一个数据项，都是由一个键值对组成的，但是键必须是字符串，且由双引号包围，而值必须是以下数据类型之一：

- 字符串（在 JSON 中，字符串值必须由双引号编写）
- 数字
- 对象（JSON 对象）
- 数组
- 布尔
- null

不可以是以下的类型

- 函数
- 日期
- undefined

### JSON字符串转JS对象

JSON.parse()：可以将以JSON字符串转换为JS对象，它需要一个JSON字符串作为参数，会将该字符串转换为JS对象并返回

> 注意 ：JSON这个对象在IE7及以下的浏览器中不支持，所以在这些浏览器中调用时会报错

### JS对象转JSON字符串

JSON.stringify()：可以将一个JS对象转换为JSON字符串，需要一个js对象作为参数，会返回一个JSON字符串

> 注意 ：JSON这个对象在IE7及以下的浏览器中不支持，所以在这些浏览器中调用时会报错

## AJAX

传统的web交互是用户触发一个http请求服务器，然后服务器收到之后，在做出响应到用户，并且返回一个新的页面，每当服务器处理客户端提交的请求时，客户都只能空闲等待，并且哪怕只是一次很小的交互、只需从服务器端得到很简单的一个数据，都要返回一个完整的HTML页，而用户每次都要浪费时间和带宽去重新读取整个页面。这个做法浪费了许多带宽，由于每次应用的交互都需要向服务器发送请求，应用的响应时间就依赖于服务器的响应时间，这导致了用户界面的响应比本地应用慢得多。

AJAX 的出现,刚好解决了传统方法的缺陷，AJAX 是一种用于创建快速动态网页的技术，通过在后台与服务器进行少量数据交换，AJAX 可以使网页实现异步更新（浏览器五个线程），这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

### AJAX的XMLHttpRequest对象

**AJAX 的核心是 XMLHttpRequest 对象**

XMLHttpRequest 对象用于幕后同服务器交换数据，这意味着可以更新网页的部分，而不需要重新加载整个页面。

```js
//创建 XMLHttpRequest 的语法
variable = new XMLHttpRequest();
//老版本的 Internet Explorer（IE5 和 IE6）使用 ActiveX 对象
variable = new ActiveXObject("Microsoft.XMLHTTP");
=====================================
//应对所有浏览器
var xhttp;
if (window.XMLHttpRequest) {
    xhttp = new XMLHttpRequest();
} else {
    // code for IE6, IE5
    xhttp = new ActiveXObject("Microsoft.XMLHTTP");
}
```

> 出于安全原因，现代浏览器不允许跨域访问，这意味着尝试加载的网页和 XML 文件都必须位于相同服务器上。

### XMLHttpRequest对象方法

| 方法                                          | 描述                                                         |
| --------------------------------------------- | ------------------------------------------------------------ |
| new XMLHttpRequest()                          | 创建新的 XMLHttpRequest 对象                                 |
| abort()                                       | 取消当前请求                                                 |
| getAllResponseHeaders()                       | 返回头部信息                                                 |
| getResponseHeader()                           | 返回特定的头部信息                                           |
| open(*method*, *url*, *async*, *user*, *psw*) | 规定请求method：请求类型 GET 或 POST url：文件位置 async：true（异步）或 false（同步） user：可选的用户名称 psw：可选的密码 |
| send()                                        | 将请求发送到服务器，用于 GET 请求                            |
| send(*string*)                                | 将请求发送到服务器，用于 POST 请求                           |
| setRequestHeader()                            | 向要发送的报头添加标签/值对                                  |

### XMLHttpRequest对象属性

| 属性               | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| onreadystatechange | 定义当 readyState 属性发生变化时被调用的函数                 |
| readyState         | 保存 XMLHttpRequest 的状态。 0：请求未初始化 1：服务器连接已建立 2：请求已收到 3：正在处理请求 4：请求已完成且响应已就绪 |
| responseText       | 以字符串返回响应数据                                         |
| responseXML        | 以 XML 数据返回响应数据                                      |
| status             | 返回请求的状态号 200: "OK" 403: "Forbidden" 404: "Not Found" 等 |
| statusText         | 返回状态文本（比如 “OK” 或 “Not Found”）                     |

### AJAX的get请求

工程结构

users.json

```json
[
  {"name":"孙悟空","age":18,"gender":"男"},
  {"name":"猪八戒","age":19,"gender":"男"},
  {"name":"唐僧","age":20,"gender":"男"},
  {"name":"沙和尚","age":21,"gender":"男"}
]
```

index.html

```js
//步骤一：创建异步对象
var ajax = new XMLHttpRequest();
//步骤二：设置请求的url参数，参数一是请求的类型，参数二是请求的url
ajax.open("get", "users.json");
//步骤三：发送请求
ajax.send();
//步骤四：注册事件 onreadystatechange 状态改变就会调用
ajax.onreadystatechange = function () {
    if (ajax.readyState == 4 && ajax.status == 200) {
        //步骤五：如果能够进到这个判断，说明数据完美的回来了，并且请求的页面是存在的
        console.log(ajax.responseText);//输入响应的内容
    }
};
```

### AJAX的POST请求

工程结构：

user.json

```js
[
  {"name":"孙悟空","age":18,"gender":"男"},
  {"name":"猪八戒","age":19,"gender":"男"},
  {"name":"唐僧","age":20,"gender":"男"},
  {"name":"沙和尚","age":21,"gender":"男"}
]
```

index.html

```js
//步骤一：创建异步对象
var ajax = new XMLHttpRequest();
//步骤二：设置请求的类型及url，注意：post请求一定要添加请求头才行不然会报错
ajax.open("post", "users.json");
ajax.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
//步骤三：发送请求
ajax.send();
//步骤四：注册事件 onreadystatechange 状态改变就会调用
ajax.onreadystatechange = function () {
    //步骤五：如果能够进到这个判断，说明数据完美的回来了，并且请求的页面是存在的
    if (ajax.readyState == 4 && ajax.status == 200) {
        console.log(ajax.responseText);//输入响应的内容
    }
};
```

整合请求

```js
var Ajax = {
    get: function (url, fn) {
        var xhr = new XMLHttpRequest();
        xhr.open('GET', url, true);
        xhr.onreadystatechange = function () {
            if (xhr.readyState == 4 && xhr.status == 200 || xhr.status == 304) {
                fn.call(this, xhr.responseText);
            }
        };
        xhr.send();
    },
    post: function (url, data, fn) {
        var xhr = new XMLHttpRequest();
        xhr.open("POST", url, true);
        xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
        xhr.onreadystatechange = function () {
            if (xhr.readyState == 4 && (xhr.status == 200 || xhr.status == 304)) {
                fn.call(this, xhr.responseText);
            }
        };
        xhr.send(data);
    }
};

// 演示GET请求
Ajax.get("users.json", function (response) {
    console.log(response);
});

// 演示POST请求
Ajax.post("users.json", "", function (response) {
    console.log(response);
});
```

## Cookie 

Cookie 是一些数据，存储于你电脑上的文本文件中，当 web 服务器向浏览器发送 web 页面时，在连接关闭后，服务端不会记录用户的信息，Cookie 的作用就是用于解决 “如何记录客户端的用户信息”：

- 当用户访问 web 页面时，它的名字可以记录在 cookie 中。
- 在用户下一次访问该页面时，可以在 cookie 中读取用户访问记录。

Cookie以名/值对的形式存储

```js
username = jfz
```

当浏览器从服务器上请求 web 页面时， 属于该页面的 cookie 会被添加到该请求中，服务端通过这种方式来获取用户的信息。

### 创建

```js
document.cookie = "username = jfz";
//添加过期时间，默认关闭浏览器时删除cookie
//使用path参数告诉浏览器cookie的路径，默认情况下cookie属于当前界面
document.cookie = "usermame = jfz;expirce = Sun, 26 Feb 2023 12:00:00 GMT; path = /"
```

### 读取

document.cookie 将以字符串的方式返回所有的 cookie，类型格式： cookie1=value; cookie2=value; cookie3=value;

```js
var cookies = document.cookie;
```

### 修改

使用 document.cookie 将旧的 cookie 将被覆盖就是修改。

### 删除

只需要设置 expires 参数为以前的时间

### Cookie值设置函数

```js
/**
 * Cookie值设置函数
 * @param cname     cookie名称
 * @param cvalue    cookie值
 * @param exdays    过期天数
 */
function setCookie(cname, cvalue, exdays) {
    var d = new Date();
    d.setTime(d.getTime() + (exdays * 24 * 60 * 60 * 1000));
    var expires = "expires=" + d.toGMTString();
    document.cookie = cname + "=" + cvalue + "; " + expires;
}
```

### Cookie值获取函数

```javascript
/**
 * Cookie值获取函数
 * @param cname     cookie名称
 * @returns {string}
 */
function getCookie(cname) {
    var name = cname + "=";
    var ca = document.cookie.split(';');
    for (var i = 0; i < ca.length; i++) {
        var c = ca[i].trim();
        if (c.indexOf(name) == 0) return c.substring(name.length, c.length);
    }
    return "";
}
```

## WebStorage

WebStorage是HTML5中本地存储的解决方案之一

Cookie的缺陷：

- 数据大小：作为存储容器，Cookie的大小限制在4KB左右
- 安全性问题：在HTTP请求中的Cookie是明文传递的（HTTPS不是）
- 网络负担：Cookie会被附加在每个HTTP请求中，在HttpRequest和HttpResponse的header中都是要被传输的，所以无形中增加了一些不必要的流量损失

**Web Storage又分为两种： sessionStorage 和localStorage** ，这两个是Storage的一个实例。essionStorage将数据保存在session中，浏览器关闭也就没了；而localStorage则一直将数据保存在客户端本地，*两者使用的API都相同*。

**localStorage和sessionStorage只能存储字符串类型**，对于复杂的对象可以使用ECMAScript提供的JSON对象的stringify和parse来处理，低版本IE可以使用[json2.js](https://github.com/douglascrockford/JSON-js/blob/master/json2.js)

### LocalStorage方法

localStorage在本地永久性存储数据，除非显式将其删除或清空

常见方法

- 保存单个数据：localStorage.setItem(key,value);
- 读取单个数据：localStorage.getItem(key);
- 删除单个数据：localStorage.removeItem(key);
- 删除所有数据：localStorage.clear();
- 获取某个索引的key：localStorage.key(index);

### sessionstorage方法

它的生命周期为当前窗口或标签页，一旦窗口或标签页被永久关闭了，那么所有通过sessionStorage存储的数据也就被清空了

sessionStorage中的数据可以跨越页面刷新而存在，如果浏览器支持，浏览器崩溃并重启之后依然可以使用（Firefox和Weblit都支持，IE则不行）

sessionStorage对象绑定于某个服务器会话，所以当文件在本地运行的时候是不可用的。存储在sessionStorage中的数据只能由最初给对象存储数据的页面访问到，所以对多页面应用有限制

常见方法：

- 保存单个数据：sessionStorage.setItem(key,value);
- 读取单个数据：sessionStorage.getItem(key);
- 删除单个数据：sessionStorage.removeItem(key);
- 删除所有数据：sessionStorage.clear();
- 获取某个索引的key：sessionStorage.key(index);

## Closure

- 如何产生闭包
  - 当一个嵌套的内部(子)函数引用了嵌套的外部(父)函数的变量(函数)时，就产生了闭包
- 什么才是闭包
  - 理解一：闭包是嵌套的内部函数(绝大部分人认为)
  - 理解二：包含被引用变量(函数)的对象(极少部分人认为)
- 闭包的作用
  - 它的最大用处有两个，一个是可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中

闭包的优点

1. 可以减少全局变量的定义，避免全局变量的污染
2. 能够读取函数内部的变量
3. 在内存中维护一个变量，可以用做缓存

缺点

1. 造成内存泄露

   闭包会使函数中的变量一直保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。

   解决方法——使用完变量后，手动将它赋值为null；

2. 闭包可能在父函数外部，改变父函数内部变量的值。

3. 造成性能损失

   由于闭包涉及跨作用域的访问，所以会导致性能损失。

   解决方法——通过把跨作用域变量存储在局部变量中，然后直接访问局部变量，来减轻对执行速度的影响

### 闭包的生命周期

生命周期：

1. 产生：在嵌套内部函数定义执行完时就产生了(不是在调用)
2. 死亡：在嵌套的内部函数成为垃圾对象时就死亡了

### 闭包应用

定义js模块

- 具有特定功能的js文件
- 将所有的数据和功能都封装在一个函数内部(私有的)
- 只向外暴露一个包含n个方法的对象或函数
- 模块的使用者，只需要通过模块暴露的对象调用方法来实现对应的功能

eg：

myModule.js

```js
function myModule() {
    //私有数据
    var msg = 'Hello, World';

    //操作数据的函数
    function doSomething() {
        console.log('doSomething() ' + msg.toUpperCase());
    }

    function doOtherthing() {
        console.log('doOtherthing() ' + msg.toLowerCase());
    }

    //向外暴露对象(给外部使用的方法)
    return {
        doSomething: doSomething,
        doOtherthing: doOtherthing
    }
}
```

index.html

```js
var module = myModule();
module.doSomething();
module.doOtherthing();
```


---
title: BOM
index_img: ./img/BOM.webp
date: 2023-2-13 12:36
categories: JS
---

# BOM

浏览器对象模型（**B**rowser **O**bject **M**odel (BOM)）使 JavaScript 有能力与浏览器"对话"，尚无正式标准。

浏览器对象模型（BOM）可以使我们通过JS来操作浏览器，在BOM中为我们提供了一组对象，用来完成对浏览器的操作，常见的BOM对象分为window对象和window子对象：

- Window：代表的是整个浏览器的窗口，是JS访问浏览器窗口的一个接口，同时window也是网页中的全局对象，声明的所有的全局变量，全局方法函数最终都是window对象的属性或者方法。
- Navigator：代表的当前浏览器的信息，通过该对象可以来识别不同的浏览器
- Location：代表当前浏览器的地址栏信息，通过Location可以获取地址栏信息，将URL解析为独立片段，或者操作浏览器跳转页面
- History：代表浏览器的历史记录，可以通过该对象来操作浏览器的历史记录，由于隐私原因，该对象不能获取到具体的历史记录，只能操作浏览器向前或向后翻页，而且该操作只在当次访问时有效
- Screen：代表用户的屏幕的信息，通过该对象可以获取到用户的显示器的相关的信息

这些BOM对象在浏览器中都是作为window对象的属性保存的，可以通过window对象来使用，也可以直接使用。

## window对象

### **弹出框**

**警告框**

```js
window.alert("11")
```

**确认框**

```javascript
//window.confirm("")
//如果用户单击“确定”，该框返回 true。如果用户单击“取消”，该框返回 false。
var r = confirm("请按按钮")
if(r == true){
    x = "确认";
}else{
    x = "取消";
}
```

**提示框**

```javascript
window.prompt("标题","默认文字");
//当提示框弹出时，用户将不得不输入值后单击“确定”或点击“取消”来继续进行。
//如果用户单击“确定”，该框返回输入值。如果用户单击“取消”，该框返回 NULL。
```

> 以上方法可以不带window前缀

### 定时事件

window 对象允许以指定的时间间隔执行代码，这些时间间隔称为定时事件（ Timing Events）。

**延时器**

```javascript
var timeId = window.setTimeout(function, milliseconds);
```

- 第一个参数是要执行的函数。
- 第二个参数指示执行之前的毫秒数。

**定时器**

```javascript
var timeId = window.setInterval(function, milliseconds);
```

- 第一个参数是要执行的函数。
- 第二个参数每个执行之间的时间间隔的长度。

清除两个对象

```javascript
window.clearTimeout(timeId);　
window.clearInterval(timeId);
```

> 不清理可能造成内存泄漏

### 常用窗口属性

两个属性可用用于确定浏览器窗口的尺寸

浏览器窗口（浏览器视口）不包括工具栏和滚动条。

- window.innerHeight - 浏览器窗口的内高度（以像素计）
- window.innerWidth - 浏览器窗口的内宽度（以像素计）

对于 Internet Explorer 8, 7, 6, 5：

- document.documentElement.clientHeight

- document.documentElement.clientWidth

  或

- document.body.clientHeight

- document.body.clientWidth

### 其他窗口方法

**打开新的窗口**

```javascript
window.open(URL,name,specs,replace);
```

![image-20201022094734710](https://img-blog.csdnimg.cn/img_convert/786bc5b7eacc1a8aba149c0741484aee.png)

**关闭当前窗口**

```
window.close();
```

**移动当前窗口**

```
window.moveTo(x,y);
```

**调整当前窗口**

```
window.resizeTo(width,height);
```

## Navigator对象

Navigator代表的当前浏览器的信息，通过该对象可以来识别不同的浏览器，由于历史原因，Navigator对象中的大部分属性都已经不能帮助我们识别浏览器了，一般我们只会使用userAgent来判断浏览器的信息，userAgent是一个字符串，这个字符串中包含有用来描述浏览器信息的内容，不同的浏览器会有不同的userAgent。在IE11中已经将微软和IE相关的标识都已经去除了，所以我们基本已经不能通过UserAgent来识别一个浏览器是否是IE了，如果通过UserAgent不能判断，还可以通过一些浏览器中特有的对象，来判断浏览器的信息，比如：ActiveXObject。

## Location对象

Location对象中封装了浏览器的地址栏的信息，如果直接打印location，则可以获取到地址栏的信息

**常用属性**

```javascript
console.log(location);          //输出location对象
console.log(location.href);     //输出当前地址的全路径地址
console.log(location.origin);   //输出当前地址的来源
console.log(location.protocol); //输出当前地址的协议
console.log(location.hostname); //输出当前地址的主机名
console.log(location.host);     //输出当前地址的主机
console.log(location.port);     //输出当前地址的端口号
console.log(location.pathname); //输出当前地址的路径部分
console.log(location.search);   //输出当前地址的?后边的参数部分
```

**方法**

assign()：用来跳转到其它的页面，作用和直接修改location一样

reload()：用于重新加载当前页面，作用和刷新按钮一样，如果在方法中传递一个true，作为参数，则会强制清空缓存刷新页面

replace()：可以使用一个新的页面替换当前页面，调用完毕也会跳转页面，它不会生成历史记录，不能使用回退按钮回退

## History对象

History对象可以用来操作浏览器向前或向后翻页

**方法**

back()：可以回退到上一个页面，作用和浏览器的回退按钮一样

forward()：可以跳转到下一个页面，作用和浏览器的前进按钮一样

go()：可以用来跳转到指定的页面，它需要一个整数作为参数

- 1：表示向前跳转一个页面，相当于forward()
- 2：表示向前跳转两个页面
- -1：表示向后跳转一个页面，相当于back()
- -2：表示向后跳转两个页面

## Screen对象

Screen 对象包含有关客户端显示屏幕的信息。

每个 Window 对象的 screen 属性都引用一个 Screen 对象。Screen 对象中存放着有关显示浏览器屏幕的信息。JavaScript 程序将利用这些信息来优化它们的输出，以达到用户的显示要求。例如，一个程序可以根据显示器的尺寸选择使用大图像还是使用小图像，它还可以根据显示器的颜色深度选择使用 16 位色还是使用 8 位色的图形。另外，JavaScript 程序还能根据有关屏幕尺寸的信息将新的浏览器窗口定位在屏幕中间。

**对象属性**

| 属性                                                         | 描述                                         |
| ------------------------------------------------------------ | -------------------------------------------- |
| [availHeight](https://www.w3school.com.cn/jsref/prop_screen_availheight.asp) | 返回显示屏幕的高度 (除 Windows 任务栏之外)。 |
| [availWidth](https://www.w3school.com.cn/jsref/prop_screen_availwidth.asp) | 返回显示屏幕的宽度 (除 Windows 任务栏之外)。 |
| [bufferDepth](https://www.w3school.com.cn/jsref/prop_screen_bufferdepth.asp) | 设置或返回调色板的比特深度。                 |
| [colorDepth](https://www.w3school.com.cn/jsref/prop_screen_colordepth.asp) | 返回目标设备或缓冲器上的调色板的比特深度。   |
| [deviceXDPI](https://www.w3school.com.cn/jsref/prop_screen_devicexdpi.asp) | 返回显示屏幕的每英寸水平点数。               |
| [deviceYDPI](https://www.w3school.com.cn/jsref/prop_screen_deviceydpi.asp) | 返回显示屏幕的每英寸垂直点数。               |
| [fontSmoothingEnabled](https://www.w3school.com.cn/jsref/prop_screen_fontsmoothingenabled.asp) | 返回用户是否在显示控制面板中启用了字体平滑。 |
| [height](https://www.w3school.com.cn/jsref/prop_screen_height.asp) | 返回显示屏幕的高度。                         |
| [logicalXDPI](https://www.w3school.com.cn/jsref/prop_screen_logicalxdpi.asp) | 返回显示屏幕每英寸的水平方向的常规点数。     |
| [logicalYDPI](https://www.w3school.com.cn/jsref/prop_screen_logicalydpi.asp) | 返回显示屏幕每英寸的垂直方向的常规点数。     |
| [pixelDepth](https://www.w3school.com.cn/jsref/prop_screen_pixeldepth.asp) | 返回显示屏幕的颜色分辨率（比特每像素）。     |
| [updateInterval](https://www.w3school.com.cn/jsref/prop_screen_updateinterval.asp) | 设置或返回屏幕的刷新率。                     |
| [width](https://www.w3school.com.cn/jsref/prop_screen_width.asp) | 返回显示器屏幕的宽度。                       |
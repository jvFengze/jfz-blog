---
title: JS对象
index_img: ./img/js.webp
date: 2023-2-11 12:20
categories: JS
---

# JS对象

## 数组对象

数组也是对象的一种，数组是一种用于表达有顺序关系的值的集合的语言结构，也就是**同类数据元素的有序集合**。

数组的存储性能比普通对象要好，在开发中我们经常使用数组来存储一些数据。但是在JavaScript中是支持数组可以是不同的元素，这跟JavaScript的弱类型有关，此处不用纠结，我们大多数时候都是相同类型元素的集合。数组内的各个值被称作元素，每一个元素都可以通过索引（下标）来快速读取，索引是从零开始的整数。

使用typeof检查一个数组对象时，会返回object。

### 创建数组

#### 使用对象创建

~~~JavaScript
//同类型有序数组
var arr = new Array();
arr[0] = 1;
arr[1] = 2;
arr[2] = 3;
//不同类型有序数组
var arr = new Array();
arr[0] = 1;
arr[1] = "2";
arr[2] = 3;
arr[3] = "4";
~~~

#### 使用字面量创建

~~~JavaScript
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
var arr = [1, "2", 3, "4", 5, "6", 7, "8", 9];
~~~

### 遍历数组

~~~JavaScript
for (var i = 0; i < arr.length; i++) {
    console.log(arr[i]);
}
~~~

### 数组属性

**constructor属性演示：返回创建数组对象的原型函数**

**length属性演示：设置或返回数组元素的个数**

### 数组方法

- **push()方法：该方法可以向数组的末尾添加一个或多个元素，并返回数组的新的长度**

- **pop()方法：该方法可以删除数组的最后一个元素，并将被删除的元素作为返回值返回**

- **unshift()方法：该方法向数组开头添加一个或多个元素，并返回新的数组长度**

- **shift()方法：该方法可以删除数组的第一个元素，并将被删除的元素作为返回值返回**

- **forEach()方法：该方法可以用来遍历数组**

  forEach()方法需要一个函数作为参数，像这种函数，由我们创建但是不由我们调用的，我们称为回调函数。数组中有几个元素函数就会执行几次，每次执行时，浏览器会将遍历到的元素，以实参的形式传递进来，我们可以来定义形参，来读取这些内容，浏览器会在回调函数中传递三个参数：

  - 第一个参数：就是当前正在遍历的元素
  - 第二个参数：就是当前正在遍历的元素的索引
  - 第三个参数：就是正在遍历的数组

  > 注意：这个方法只支持IE8以上的浏览器，IE8及以下的浏览器均不支持该方法，所以如果需要兼容IE8，则不要使用forEach()，还是使用for循环来遍历数组。

  ~~~JavaScript
  var arr = ["孙悟空", "猪八戒", "沙和尚"];
  arr.forEach(function (value, index, obj) {
      console.log(value + " #### " + index + " #### " + obj);
  });
  ~~~

- **slice()方法：该方法可以用来从数组提取指定元素，该方法不会改变元素数组，而是将截取到的元素封装到一个新数组中返回**

  > 注意：索引可以传递一个负值，如果传递一个负值，则从后往前计算，-1代表倒数第一个，-2代表倒数第二个。

- **splice()方法：该方法可以用于删除数组中的指定元素，该方法会影响到原数组，会将指定元素从原数组中删除，并将被删除的元素作为返回值返回**

  - 第一个参数：表示开始位置的索引
  - 第二个参数：表示要删除的元素数量
  - 第三个参数及以后参数：可以传递一些新的元素，这些元素将会自动插入到开始位置索引前边

- **concat()方法：该方法可以连接两个或多个数组，并将新的数组返回，该方法不会对原数组产生影响**

- **join()方法：该方法可以将数组转换为一个字符串，该方法不会对原数组产生影响，而是将转换后的字符串作为结果返回，在join()中可以指定一个字符串作为参数，这个字符串将会成为数组中元素的连接符，如果不指定连接符，则默认使用，作为连接符**

- **reverse()方法：该方法用来反转数组（前边的去后边，后边的去前边），该方法会直接修改原数组**

- **sort()方法：该方法可以用来对数组中的元素进行排序，也会影响原数组，默认会按照Unicode编码进行排序**

  > 注意：即使对于纯数字的数组，使用sort()排序时，也会按照Unicode编码来排序，所以对数字进排序时，可能会得到错误的结果。

  我们可以自己来指定排序的规则，我们可以在sort()添加一个回调函数，来指定排序规则，回调函数中需要定义两个形参，浏览器将会分别使用数组中的元素作为实参去调用回调函数，使用哪个元素调用不确定，但是肯定的是在数组中a一定在b前边，浏览器会根据回调函数的返回值来决定元素的顺序，如下：

  - 如果返回一个大于0的值，则元素会交换位置
  - 如果返回一个小于0的值，则元素位置不变
  - 如果返回一个等于0的值，则认为两个元素相等，也不交换位置

  经过上边的规则，我们可以总结下：

  - 如果需要升序排列，则返回 a-b
  - 如果需要降序排列，则返回 b-a

  ~~~JavaScript
  var arr = [1, 3, 2, 11, 5, 6];
  arr.sort(function (a, b) {
      return a - b;
  });
  console.log(arr);
  //=====================
  [1,2,3,5,6,11]
  ~~~

## 函数对象

### call()和apply()

call()和apply()这两个方法都是函数对象的方法，需要通过函数对象来调用，当对函数调用call()和apply()都会调用函数执行，在调用call()和apply()可以将一个对象指定为第一个参数，此时这个对象将会成为函数执行时的this，call()方法可以将实参在对象之后依次传递，apply()方法需要将实参封装到一个数组中统一传递，如下演示：

**call()方法：**

~~~javascript
function fun(a, b) {
    console.log("a = " + a);
    console.log("b = " + b);
    console.log("fun = " + this);
}

var obj = {
    name: "obj",
    sayName: functi4on () {
        console.log(this.name);
    }
};

fun(2, 3);
console.log("===============");
fun.call(obj, 2, 3);
~~~

> 注意：默认fun()函数调用，this指向的是window对象，你可以使用call()调用函数，在调用的时候传入一个对象，这个对象就是this所指向的对象，也就是说，可以自己指定this的指向，然后从第二个参数开始，实参将会依次传递

![](../img/call.png)

**apply()方法演示：**

~~~javascript
function fun(a, b) {
    console.log("a = " + a);
    console.log("b = " + b);
    console.log("fun = " + this);
}

var obj = {
    name: "obj",
    sayName: function () {
        console.log(this.name);
    }
};

fun(2, 3);
console.log("===============");
fun.apply(obj, [2, 3]);
~~~

![](../img/call.png)

> 注意：默认fun()函数调用，this指向的是window对象，你可以使用apply()调用函数，在调用的时候传入一个对象，这个对象就是this所指向的对象，也就是说，可以自己指定this的指向，然后从第二个参数开始，需要制定一个实参数组进行参数传递

### arguments参数

在调用函数时，浏览器每次都会传递进两个隐含的参数：

1. 函数的上下文对象： **this**
2. 封装实参的对象： **arguments**

arguments是一个类数组对象，它也可以通过索引来操作数据，也可以获取长度，在调用函数时，我们所传递的实参都会在arguments中保存，比如：arguments.length 可以用来获取实参的长度，我们即使不定义形参，也可以通过arguments来使用实参，只不过比较麻烦

```js
function fun(a, b) {
    // 通过下标获取第一个参数
    console.log(arguments[0]);
    // 通过下标获取第二个参数
    console.log(arguments[1]);
    // 获取实参的个数
    console.log(arguments.length);
    // 它的函数对象
    console.log(arguments.callee);
    console.log(arguments.callee == fun);
}

fun("Hello", "World");
```

![](../img/arguments.png)

## Data对象

在JavaScript中使用Date对象来表示一个时间，如果直接使用构造函数创建一个Date对象，则会封装为当前代码执行的时间。

~~~JavaScript
var date = new Date();
console.log(date);

console.log(date.getFullYear());//获取当前日期对象的年份(四位数字年份)
console.log(date.getMonth());//获取当前日期对象的月份(0 ~ 11)
console.log(date.getDate());//获取当前日期对象的日数(1 ~ 31)
console.log(date.getHours());//获取当前日期对象的小时(0 ~ 23)
console.log(date.getMinutes());//获取当前日期对象的分钟(0 ~ 59)
console.log(date.getSeconds());//获取当前日期对象的秒钟(0 ~ 59)
console.log(date.getMilliseconds());//获取当前日期对象的毫秒(0 ~ 999)
~~~

## Math对象

Math和其它的对象不同，它不是一个构造函数，它属于一个工具类不用创建对象，它里边封装了数学运算相关的属性和方法。

eg：

~~~JavaScript
/*固定值*/
console.log("PI = " + Math.PI);
console.log("E  = " + Math.E);
console.log("===============");
/*正数*/
console.log(Math.abs(1));        //可以用来计算一个数的绝对值
console.log(Math.ceil(1.1));     //可以对一个数进行向上取整，小数位只有有值就自动进1
console.log(Math.floor(1.99));   //可以对一个数进行向下取整，小数部分会被舍掉
console.log(Math.round(1.4));    //可以对一个数进行四舍五入取整
console.log("===============");
/*负数*/
console.log(Math.abs(-1));       //可以用来计算一个数的绝对值
console.log(Math.ceil(-1.1));    //可以对一个数进行向上取整，小数部分会被舍掉
console.log(Math.floor(-1.99));  //可以对一个数进行向下取整，小数位只有有值就自动进1
console.log(Math.round(-1.4));   //可以对一个数进行四舍五入取整
console.log("===============");
/*随机数*/
//Math.random()：可以用来生成一个0-1之间的随机数
//生成一个0-x之间的随机数：Math.round(Math.random()*x)
//生成一个x-y之间的随机数：Math.round(Math.random()*(y-x)+x)
console.log(Math.round(Math.random() * 10));            //生成一个0-10之间的随机数
console.log(Math.round(Math.random() * (10 - 1) + 1));  //生成一个1-10之间的随机数
console.log("===============");
/*数学运算*/
console.log(Math.pow(12, 3));   //Math.pow(x,y)：返回x的y次幂
console.log(Math.sqrt(4));      //Math.sqrt(x) ：返回x的平方根
~~~

## String对象

在JS中为我们提供了三个包装类，通过这三个包装类可以将基本数据类型的数据转换为对象

- String()：可以将基本数据类型字符串转换为String对象
- Number()：可以将基本数据类型的数字转换为Number对象
- Boolean()：可以将基本数据类型的布尔值转换为Boolean对象

> 我们在实际应用中不会使用基本数据类型的对象，如果使用基本数据类型的对象，在做一些比较时可能会带来一些不可预期的结果

###  字符串属性

**constructor属性演示：返回创建字符串对象的原型函数**

**length属性演示：可以用来获取字符串的长度**

### 字符串方法

- **charAt()方法：该方法可以根据索引获取指定位置的字符**
- **charCodeAt()方法：该方法获取指定位置字符的字符编码（Unicode编码）**
- **concat()方法：该方法可以用来连接两个或多个字符串**
- **indexof()方法：该方法可以检索一个字符串中是否含有指定内容，如果字符串中含有该内容，则会返回其第一次出现的索引，如果没有找到指定的内容，则返回-1，可以指定一个第二个参数，指定开始查找的位置**
- **lastIndexOf()方法：该方法的用法和indexOf()一样，不同的是indexOf是从前往后找，而lastIndexOf是从后往前找，也可以指定开始查找的位置**
- **slice()方法：可以从字符串中截取指定的内容，不会影响原字符串，而是将截取到内容返回**
- **substring()方法：可以用来截取一个字符串，它和slice()类似**
  - 第一个参数：开始截取位置的索引（包括开始位置）
  - 第二个参数：结束位置的索引（不包括结束位置），如果省略第二个参数，则会截取到后边所有的

> 注意：不同的是这个方法不能接受负值作为参数，如果传递了一个负值，则默认使用0，而且它还自动调整参数的位置，如果第二个参数小于第一个，则自动交换

- **substr()方法演示：该方法用来截取字符串**
  - 第一个参数：截取开始位置的索引
  - 第二个参数：截取的长度
- **split()方法演示：该方法可以将一个字符串拆分为一个数组，需要一个字符串作为参数，将会根据该字符串去拆分数组**
- **toUpperCase()方法演示：将一个字符串转换为大写并返回**
- **toLowerCase()方法演示：将一个字符串转换为小写并返回**

## RegExp对象

正则表达式用于定义一些字符串的规则，计算机可以根据正则表达式，来检查一个字符串是否符合规则，获取将字符串中符合规则的内容提取出来。

使用typeof检查正则对象，会返回object

### 创建正则对象

**使用对象创建**

var 变量名 = new RegExp("正则表达式","匹配模式");

**匹配模式：**

- i：忽略大小写
- g：全局匹配模式
- ig：忽略大小写且全局匹配模式

#### 使用字面量创建

**var 变量名 = /正则表达式/匹配模式;** 

**匹配模式：**

- i：忽略大小写
- g：全局匹配模式
- m：执行多行匹配

> 注意：可以为一个正则表达式设置多个匹配模式，且顺序无所谓

~~~JavaScript
// 这个正则表达式可以来检查一个字符串中是否含有a
var reg = /a/i;
var str = "Abc";
var result = reg.test(str);
console.log(result);
~~~

### 正则进阶

**|表示或者，[]里的内容也表示或者**

**常见组合：**

- [a-z]：任意小写字母
- [A-Z]：任意大写字母
- [A-z]：任意字母
- [0-9]：任意数字

- [^a-z]：除了任意小写字母
- [^A-Z]：除了任意大写字母
- [^A-z]：除了任意字母
- [^0-9]：除了任意数字

### 正则方法

**split()方法演示：该方法可以将一个字符串拆分为一个数组，方法中可以传递一个正则表达式作为参数，这样方法将会根据正则表达式去拆分字符串，这个方法即使不指定全局匹配，也会全都插分**

~~~JavaScript
var str = "1a2b3c4d5e6f7";
var result = str.split(/[A-z]/);
console.log(result);
~~~

![image-20201018213447809](https://img-blog.csdnimg.cn/img_convert/691bd30df6aa462310da2ffecac54eb7.png)

**search()方法演示：该方法可以搜索字符串中是否含有指定内容，如果搜索到指定内容，则会返回第一次出现的索引，如果没有搜索到返回-1，它可以接受一个正则表达式作为参数，然后会根据正则表达式去检索字符串，serach()只会查找第一个，即使设置全局匹配也没用**

~~~JavaScript
var str = "hello abc hello aec afc";
var result = str.search(/a[bef]c/);
console.log(result);
~~~

![img](https://img-blog.csdnimg.cn/img_convert/8ba6eb1153172af98f7b11e8f3b0b0f9.png)

**match()方法演示：该方法可以根据正则表达式，从一个字符串中将符合条件的内容提取出来，默认情况下我们的match()只会找到第一个符合要求的内容，找到以后就停止检索，我们可以设置正则表达式为全局匹配模式，这样就会匹配到所有的内容，可以为一个正则表达式设置多个匹配模式，且顺序无所谓，match()会将匹配到的内容封装到一个数组中返回，即使只查询到一个结果**

~~~JavaScript
var str = "1a2a3a4a5e6f7A8B9C";
var result = str.match(/[a-z]/ig);
console.log(result);
~~~

![image-20201018213447809](https://img-blog.csdnimg.cn/img_convert/691bd30df6aa462310da2ffecac54eb7.png)

**replace()方法演示：该方法可以将字符串中指定内容替换为新的内容，默认只会替换第一个，但是可以设置全局匹配替换全部**

- 第一个参数：被替换的内容，可以接受一个正则表达式作为参数
- 第二个参数：新的内容

~~~JavaScript
var str = "1a2a3a4a5e6f7A8B9C";
var result = str.replace(/[a-z]/gi, "@_@");
console.log(result);
~~~

![image-20201018213653322](https://img-blog.csdnimg.cn/img_convert/588f6ab348d21f0bcd080929ca09d10c.png)

### 正则量词

通过量词可以设置一个内容出现的次数，量词只对它前边的一个内容起作用，如果有多个内容可以使用 `()` 括起来，常见量词如下：

- `{n}` ：正好出现n次
- `{m,}` ：出现m次及以上
- `{m,n}` ：出现m-n次
- `+` ：至少一个，相当于{1,}
- `*` ：0个或多个，相当于{0,}
- `?` ：0个或1个，相当于{0,1}

~~~JavaScript
var str = "abbc";

reg = /(ab){3}/;
console.log(reg.test(str));
console.log("===============");
reg = /b{3}/;
console.log(reg.test(str));
console.log("===============");
reg = /ab{1,3}c/;
console.log(reg.test(str));
console.log("===============");
reg = /ab{3,}c/;
console.log(reg.test(str));
console.log("===============");
reg = /ab+c/;
console.log(reg.test(str));
console.log("===============");
reg = /ab*c/;
console.log(reg.test(str));
console.log("===============");
reg = /ab?c/;
console.log(reg.test(str));
console.log("===============");
~~~

### 正则高阶

如果我们要检查或者说判断是否以某个字符或者字符序列开头或者结尾就会使用`^`和`$`。

- `^` ：表示开头，注意它在`[^字符序列]`表达的意思不一样
- `$` ：表示结尾

如果我们想要检查一个字符串中是否含有`.`和`\`就会使用转义字符

- `\.` ：表示`.`
- `\\` ：表示`\`

> 注意：使用构造函数时，由于它的参数是一个字符串，而`\`是字符串中转义字符，如果要使用`\`则需要使用`\\`来代替

~~~JavaScript
var reg1 = /\./;
var reg2 = /\\/;
var reg3 = new RegExp("\\.");
var reg4 = new RegExp("\\\\");
~~~

- `\w` ：任意字母、数字、_，相当于[A-z0-9_]
- `\W` ：除了字母、数字、_，相当于[^A-z0-9_]
- `\d` ：任意的数字，相当于`[0-9]`
- `\D` ：除了任意的数字，相当于`[^0-9]`
- `\s` ：空格
- `\S` ：除了空格
- `\b` ：单词边界
- `\B` ：除了单词边界

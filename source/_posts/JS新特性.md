---
title: JS新特性
index_img: ./img/ES6.jpg
date: 2023-2-16 12:36
categories: JS
---

# JS新特性

### let关键字

- 不允许重复声明
- 块儿级作用域
- 不存在变量提升
- 不影响作用域链

### const关键字

- 不允许重复声明
- 块儿级作用域
- 声明必须赋初始值
- 值不允许修改
- 标识符一般为大写

> **声明对象类型使用 const，非对象类型声明选择 let**

### 变量解构赋值

```js
//数组的解构赋值
const arr = ["张学友", "刘德华", "黎明", "郭富城"];
let [zhang, liu, li, guo] = arr;

//简单对象的解构赋值
const lin = {
    name: "林志颖",
    tags: ["车手", "歌手", "小旋风", "演员"]
};
let {name, tags} = lin;
```

### 模板字符串

模板字符串（template string）是增强版的字符串，用反引号（`）标识，特点：

- 字符串中可以出现换行符
- 可以使用 ${xxx} 形式输出变量

> **当遇到字符串与变量拼接的情况使用模板字符串**

### 简化对象

ES6 允许在大括号里面，直接写入变量和函数，作为对象的属性和方法，这样的书写更加简洁。

```js
let name = "张三";
let age = 18;
let speak = function () {
    console.log(this.name);
};

//属性和方法简写
let person = {
    name,
    age,
    speak
};

console.log(person.name);
console.log(person.age);
person.speak();
```

### 箭头函数

（）=>{}

- 如果形参只有一个，则小括号可以省略
- 函数体如果只有一条语句，则花括号可以省略，函数的返回值为该条语句的执行结果
- 箭头函数 this 指向声明时所在作用域下 this 的值，箭头函数不会更改 this 指向，用来指定回调函数会非常合适
- 箭头函数不能作为构造函数实例化
- 不能使用 arguments 实参

### rest参数

ES6 引入 rest 参数，用于获取函数的实参，用来代替 arguments 参数。

rest 参数非常适合不定个数参数函数的场景

```js
function add(a,b,...args) {
    console.log(args);
}
add(1, 2, 3, 4, 5);
// rest 参数必须是最后一个形参

//1 2 [3,4,5]
```

### spread扩展运算符

扩展运算符（spread）也是三个点（…），它好比 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列，对数组进行解包。

```js
// 展开数组
let t = ["1", "2", "3"];
function fn() {
    console.log(arguments);
}
fn(...t);//0:"1",1:"2",3:"3"

//展开对象
let a = {a:"1"}
console.log(...a)//a:"1"
```

### symbol

- Symbol 的值是唯一的，用来解决命名冲突的问题
- Symbol 值不能与其它数据进行运算
- Symbol 定义的对象属性不能使用 for…in 循环遍 历 ，但是可以使用 Reflect.ownKeys 来获取对象的所有键名

|                           |                                                              |
| ------------------------- | ------------------------------------------------------------ |
| 内置值                    | 描述                                                         |
| Symbol.hasInstance        | 当其它对象使用 instanceof 运算符，判断是否为该对象的实例时，会调用这个方法 |
| Symbol.isConcatSpreadable | 对象的 Symbol.isConcatSpreadable 属性等于的是一个布尔值，表示该对象用于 Array.prototype.concat()时， 是否可以展开 |
| Symbol.species            | 创建衍生对象时，会使用该属性                                 |
| Symbol.match              | 当执行 str.match(myObject) 时，如果该属性存在，会调用它，返回该方法的返回值 |
| Symbol.replace            | 当该对象被 str.replace(myObject)方法调用时，会返回该方法的返回值 |
| Symbol.search             | 当该对象被 str.search (myObject)方法调用时，会返回该方法的返回值 |
| Symbol.split              | 当该对象被 str.split(myObject)方法调用时，会返回该方法的返回值 |
| Symbol.iterator           | 当对象进行 for…of 循环时，会调用 Symbol.iterator 方法， 返回该对象的默认遍历器 |
| Symbol.toPrimitive        | 当对象被转为原始类型的值时，会调用这个方法，返 回该对象对应的原始类型值 |
| Symbol. toStringTag       | 当对象上面调用 toString 方法时，返回该方法的返 回值          |
| Symbol. unscopables       | 当对象指定了使用 with 关键字时，哪些属性会被 with 环境排除   |

### BIgInt

JS 中的`Number`类型只能安全地表示`-9007199254740991 (-(2^53-1))` 和`9007199254740991(2^53-1)`之间的整数，任何超出此范围的整数值都可能失去精度。

`BigInt`数据类型的目的是比`Number`数据类型支持的范围更大的整数值。在对大整数执行数学运算时，以任意精度表示整数的能力尤为重要。使用`BigInt`，整数溢出将不再是问题。此外，可以安全地使用更加准确时间戳，大整数ID等，而无需使用变通方法。

```js
//定义BigInt只需要在数字的后边加上一个n
let n = 521n;
//普通整数转化为bigint
let n = 123;
console.log(BigInt(n));
//不能使用浮点数进行转换
console.log(BigInt(0.2));
```



### 迭代器

遍历器（Iterator）就是一种机制。它是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 Iterator 接口，就可以完成遍历操作。ES6 创造了一种新的遍历命令 for…of 循环，Iterator 接口主要供 for…of 消费，原生具备 iterator 接口的数据：

- Array
- Arguments
- Set
- Map
- String
- TypedArray
- NodeList

> 需要自定义遍历数据的时候，要想到迭代器

1. 创建一个指针对象，指向当前数据结构的起始位置
2. 第一次调用对象的 next 方法，指针自动指向数据结构的第一个成员
3. 接下来不断调用 next 方法，指针一直往后移动，直到指向最后一个成员
4. 每调用 next 方法返回一个包含 value 和 done 属性的对象

```js
//声明一个数组
const xiyou = ["唐僧", "孙悟空", "猪八戒", "沙僧"];
//使用 for...of 遍历数组
for (let v of xiyou) {
    console.log(v);
}
console.log("===============");

//获取迭代器对象
let iterator = xiyou[Symbol.iterator]();
//调用对象的next方法
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
```

![image-20201025174729990](https://img-blog.csdnimg.cn/img_convert/7261b300de524e308b2f3d6fa2626003.png)

自定义遍历数组

```js
//声明一个对象
const banji = {
    name: "五班",
    stus: [
        "张三",
        "李四",
        "王五",
        "小六"
    ],
    [Symbol.iterator]() {
        //索引变量
        let index = 0;
        let _this = this;
        return {
            next: function () {
                if (index < _this.stus.length) {
                    const result = {value: _this.stus[index], done: false};
                    //下标自增
                    index++;
                    //返回结果
                    return result;
                } else {
                    return {value: undefined, done: true};
                }
            }
        };
    }
}

//遍历这个对象
for (let v of banji) {
    console.log(v);
}
```

### 生成器

**生成器是ES6中新增的一种函数控制、使用的方案，它可以让我们更加灵活的控制函数什么时候继续执行、暂停执行等*，生成器事实上是一种特殊的迭代器***

生成器函数也是一个函数，但是和普通的函数有一些区别

- 生成器函数需要在function的后面加一个符号*
- 生成器函数可以通过yield关键字来控制函数的执行流程
- 生成器函数的返回值是一个Generator（生成器）

```js
function* a() {
    let b = "213";
    console.log(b);
    yield b
    let c = "234";
    console.log(c);
    yield c 
}
// 不执行
a()
// 调用生成器函数, 会返回一个生成器对象
const iterator = a()
iterator.next()// 213
//next方法返回值
console.log(iterator.next());// 234 {value: 234, done: false}
```



生成器函数默认在执行时, **返回的也是一个生成器对象** , **调用函数时函数内部的代码并没有执行**

**如果想要执行函数内部的代码, 需要调用返回的生成器对象的next方法**

**当函数内部代码, 遇到yield时会中断执行**

生成器可以给每个分段传递参数，还可以抛出异常

- **抛出异常后我们可以在生成器函数中捕获异常**；
- 但是在catch语句中不能继续yield新的值了，但是可以在catch语句外使用yield继续中断函数的执行；
- 目前未学习捕获异常语句, 了解即可

```js
function* a(name) {
    let b = "213";
    console.log(b,name);
    const name2 = yield 
    let c = "234";
    console.log(c,name2);
    yield 
}
// 第一次传递参数是通过函数传递
const iterator = a("1");
iterator.next("1") // 213 1
// 第二次及后面开始传递参数, 是通过yield
iterator.next("2") // 234 2
//===============================
//生成器提前结束
//return函数传值后这个生成器函数就会结束，之后调用next不会继续生成值了
console.log(iterator.return("1")); //{value: '1', done: true}
console.log(iterator.next("2")); //{value: undefined, done: true}
// 抛出异常
console.log(generator.throw("name2 throw")) // Uncaught name2 throw
```

### promise

Promise 是 ES6 引入的异步编程的新解决方案，语法上 Promise 是一个构造函数，用来封装异步操作并可以获取其成功或失败的结果。

https://blog.csdn.net/qq_31676483/article/details/123189691

### set

ES6 提供了新的数据结构 Set（集合）,它类似于数组，**但成员的值都是唯一的**,集合实现了 iterator 接口，所以可以使用『扩展运算符』和『for…of…』进行遍历

- size：返回集合的元素个数
- add()：增加一个新元素，返回当前集合
- delete()：删除元素，返回 boolean 值
- has()：检测集合中是否包含某个元素，返回 boolean 值
- clear()：清空集合，返回 undefined

### map

ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合，“键” 的范围不限于字符串，各种类型的值（包括对象）都可以当作键。

Map 也实现了 iterator 接口，所以可以使用『扩展运算符』和『for…of…』进行遍历

- size：返回 Map 的元素个数
- set()：增加一个新元素，返回当前 Map
- get()：返回键名对象的键值
- has()：检测 Map 中是否包含某个元素，返回 boolean 值
- clear()：清空集合，返回 undefined

### class类

ES6 提供了更接近传统语言的写法，引入了 Class（类）这个概念，作为对象的模板。通过 class 关键字，可以定义类。基本上，ES6 的 class 可以看作只是 一个语法糖，它的绝大部分功能，ES5 都可以做到，新的 class 写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已

- class：声明类
- constructor：定义构造函数初始化
- extends：继承父类
- super：调用父级构造方法
- static：定义静态方法和属性

**class私有属性**：私有属性只能在class中访问

```js
class Person {
    //公有属性
    name;
    //私有属性
    #age;
    #weight;

    //构造方法
    constructor(name, age, weight) {
        this.name = name;
        this.#age = age;
        this.#weight = weight;
    }

    //普通方法
    intro() {
        console.log(this.name);
        console.log(this.#age);
        console.log(this.#weight);
    }
}

//实例化
const girl = new Person("小可爱", 18, "45kg");
girl.intro();
```



### 数值扩展

二进制和八进制  

Number.EPSILON --Number.isFinite--Number.isNaN--Number.parseInt--Number.parseFloat--Number.isInteger--Math.trunc--Math.sign

### 数组方法扩展

- `arr.includes`()：此方法用来检测数组中是否包含某个元素，返回布尔类型值

- `flat`() 方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回，说白了就是将多维数组转化为低维数组。

  ```js
  const arr1 = [1, 2, 3, 4, [5, 6]];
  console.log(arr1.flat());//[1,2,3,4,5,6]
  const arr2 = [1, 2, 3, 4, [5, 6, [7, 8, 9]]];
  console.log(arr2.flat());//[1,2,3,4,5,6,[7,8,9]]
  //参数为深度是一个数字
  console.log(arr2.flat(1));//[1,2,3,4,5,6,[7,8,9]]
  console.log(arr2.flat(2));//[1,2,3,4,5,6,7,8,9]
  ```

- `flatMap`() 方法首先使用映射函数映射每个元素，然后将结果压缩成一个新数组。它与 map 连着深度值为1的 flat 几乎相同，但 flatMap 通常在合并成一种方法的效率稍微高一些。

  ```js
  var arr1 = [1, 2, 3, 4];
  console.log(arr1.map(x => x * 2));
  //[2,4,6,8]
  var arr2 = [1, 2, 3, 4];
  console.log(arr2.flatMap(x => x * 2));
  ```

### 对象扩展

- Object.is：比较两个值是否严格相等，与『===』行为基本一致（+0 与 NaN）
- Object.assign：对象的合并，将源对象的所有可枚举属性，复制到目标对象
- __ proto __、setPrototypeOf、 setPrototypeOf可以直接设置对象的原型
- Object.keys()方法返回一个给定对象的所有可枚举键值的数组
- Object.values()方法返回一个给定对象的所有可枚举属性值的数组
- Object.entries()方法返回一个给定对象自身可遍历属性 [key,value] 的数组
- Object.fromEntries()方法是Object.entries()的逆操作，用于将一个键值对数组转为对象。
- Object.getOwnPropertyDescriptors方法返回指定对象所有自身属性的描述对象

### 模块化

模块化是指将一个大的程序文件，拆分成许多小的文件，然后将小文件组合起来

优点

- 防止命名冲突
- 代码复用
- 高维护性

模块化产品

CommonJS => NodeJS、Browserify

AMD => requireJS

CMD => seaJS

**模块化的语法**

模块功能主要由两个命令构成：export 和 import。

- export 命令用于规定模块的对外接口
- import 命令用于输入其它模块提供的功能

模块化的暴露

```js
//方式一：分别暴露
export let school = "华北理工大学";

export function study() {
    console.log("我们要学习！");
}
//=========================
//方式二：统一暴露
let school = "华北理工大学";

function findJob() {
    console.log("我们要找工作！");
}

export {school, findJob};
//=========================
//方式三：默认暴露
export default {
    school: "华北理工大学",
    change: function () {
        console.log("我们要改变自己！");
    }
}
```

模块化的导入

```js
<script type="module">
    // 引入 m1.js 模块内容
    import * as m1 from "./m1.js";
    // 引入 m2.js 模块内容
    import * as m2 from "./m2.js";
    // 引入 m3.js 模块内容
    import * as m3 from "./m3.js";
     
    m1.study();
    m2.findJob();
    m3.default.change();
</script>
//==========================
//解构赋值形式
<script type="module">
    // 引入 m1.js 模块内容
    import {school, study} from "./m1.js";
    // 引入 m2.js 模块内容
    import {school as s, findJob} from "./m2.js";
    // 引入 m3.js 模块内容
    import {default as m3} from "./m3.js";

    console.log(school);
    study();

    console.log(s);
    findJob();

    console.log(m3);
    m3.change();
</script>
```

> **针对默认暴露还可以直接 `import m3 from "./m3.js"`**

### 浅拷贝和深拷贝

简单来说，假设B复制了A，当修改A时，看B是否会发生变化，如果B也跟着变了，说明这是浅拷贝；如果B没变，那就是深拷贝。**深拷贝与浅拷贝的概念只存在于引用数据类型**

#### 深拷贝：

**自带的：**

**Array：slice()、concat()、Array.from()、… 操作符：只能实现一维数组的深拷贝**

```js
var arr1 = [1, 2, 3, 4];
var arr2 = arr1.slice();// slice等方法
arr2[0] = 200;
console.log(arr1);
console.log(arr2);
```

**Object：Object.assign()、… 操作符：只能实现一维对象的深拷贝**

```js
var obj1 = {
    name: "张三",
    age: 20,
    speak: function () {
        console.log("我是" + this.name);
    }
};

var obj2 = Object.assign({}, obj1);
//var obj2 = {
//    ...obj1
//};

// 当修改obj2的属性和方法的时候，obj1相应的属性和方法不会改变
obj2.name = "李四";
console.log(obj1);
console.log(obj2);
```

**JSON.parse(JSON.stringify(obj))：可实现多维对象的深拷贝，但会忽略 `undefined` 、 `任意的函数` 、`Symbol 值`**

```js
var obj1 = {
    name: "张三",
    age: 20,
    birthday: {
        year: 1997,
        month: 12,
        day: 5
    },
    speak: function () {
        console.log("我是" + this.name);
    }
};

var obj2 = JSON.parse(JSON.stringify(obj1));

// 当修改obj2的属性和方法的时候，obj1相应的属性和方法不会改变
obj2.name = "李四";
console.log(obj1);
console.log(obj2);
```

> 注意：进行JSON.stringify()序列化的过程中，undefined、任意的函数以及 symbol 值，在序列化过程中会被忽略（出现在非数组对象的属性值中时）或者被转换成 null（出现在数组中时），由上面可知，JS 提供的自有方法并不能彻底解决Array、Object的深拷贝问题，因此我们应该自己实现。

#### 通用版

```js
var obj1 = {
    name: "张三",
    age: 20,
    birthday: {
        year: 1997,
        month: 12,
        day: 5
    },
    speak: function () {
        console.log("我是" + this.name);
    }
};

var obj2 = deepClone(obj1);

// 当修改obj2的属性和方法的时候，obj1相应的属性和方法不会改变
obj2.name = "李四";
console.log(obj1);
console.log(obj2);

function deepClone(obj, has = new WeakMap()) {
    // 类型检查
    if (obj == null) return obj;
    if (obj instanceof Date) return obj;
    if (obj instanceof RegExp) return obj;
    if (!(typeof obj == "object")) return obj;

    // 构造对象
    const newObj = new obj.constructor;

    // 防止自引用导致的死循环
    if (has.get(obj)) return has.get(obj);
    has.set(obj, newObj);

    // 循环遍历属性及方法
    for (let key in obj) {
        if (obj.hasOwnProperty(key)) {
            newObj[key] = deepClone(obj[key]);
        }
    }

    // 返回对象
    return newObj;
}
```

### async函数

1. 返回的结果不是一个 Promise 类型的对象，返回的结果就是成功 Promise 对象
2. 返回的结果如果是一个 Promise 对象，具体需要看执行resolve方法还是reject方法
3. 抛出错误，返回的结果是一个失败的 Promise

### await 表达式

async 和 await 两种语法结合可以让异步代码像同步代码一样

1. await 必须写在 async 函数中
2. await 右侧的表达式一般为 promise 对象
3. await 返回的是 promise 成功的值
4. await 的 promise 失败了, 就会抛出异常, 需要通过 try…catch 捕获处理

案例演示:封装AJAX请求

```js
// 发送 AJAX 请求, 返回的结果是 Promise 对象
function sendAJAX(url) {
    return new Promise((resolve, reject) => {
        //1. 创建对象
        const x = new XMLHttpRequest();
        //2. 初始化
        x.open('GET', url);
        //3. 发送
        x.send();
        //4. 事件绑定
        x.onreadystatechange = function () {
            if (x.readyState === 4) {
                if (x.status >= 200 && x.status < 300) {
                    resolve(x.response);//成功
                } else {
                    reject(x.status);//失败
                }
            }
        }
    })
}


// async 与 await 测试
async function fun() {
    //发送 AJAX 请求 1
    let joke = await sendAJAX("https://api.apiopen.top/getJoke");
    //发送 AJAX 请求 2
    let tianqi = await sendAJAX('https://www.tianqiapi.com/api/?version=v1&city=%E5%8C%97%E4%BA%AC&appid=23941491&appsecret=TXoD5e8P')

    console.log(joke);
    console.error(tianqi);//为了区别数据，我这里用红色的error输出
}

// 调用函数
fun();
```

### Promise.allSettled

该Promise.allSettled()方法返回一个在所有给定的promise都已经fulfilled或rejected后的promise，并带有一个对象数组，每个对象表示对应的promise结果。当您有多个彼此不依赖的异步任务成功完成时，或者您总是想知道每个promise的结果时，通常使用它。

```js
//声明两个promise对象
const p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve("商品数据 - 1");
    }, 1000);
});

const p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        // resolve("商品数据 - 2");
        reject("出错啦!");
    }, 1000);
});

//调用 allsettled 方法
const result1 = Promise.allSettled([p1, p2]);
console.log(result1);
```

### 可选链操作符

当我们要使用传进来的一个属性值的时候，我们不知道这个属性值到底有没有传，我们可以使用&&运算符一级一级判断，就像这样 `const dbHost = config && config.db && config.db.host`;但是这样会显得很麻烦，所以在ES11 中就提供了可选链操作符，它就简化了代码，变成了这样 `const dbHost = config?.db?.host`; 另一方面，即使用户没有传入这个属性，我们用了也不会报错，而是undefined

```js
function connect(config) {
    // const dbHost = config && config.db && config.db.host;
    const dbHost = config?.db?.host;
    console.log(dbHost);
}

connect({
    db: {
        host: "192.168.1.100",
        username: "root"
    },
    cache: {
        host: "192.168.1.200",
        username: "admin"
    }
})
```

### 动态import

以前我们import 导入模块是在一开始的时候就全部导入了，这样在模块很多的时候，会显得网页速度加载很慢，在ES11中就提供了一种动态import，案例演示如下：

```js
//分别暴露
export let school = "华北理工大学";

export function study() {
    console.log("我们要学习！");
}
```

```js
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title></title>
</head>
<body>

<button id="btn">点击我，加载m1.js模块</button>

<!-- 在这里写JavaScript代码，因为JavaScript是由上到下执行的 -->
<script type="module">
    const btn = document.getElementById("btn");

    btn.onclick = function(){
        import("./m1.js").then(module => {
            module.study();
        });
    };
</script>
</body>
</html>
```

### BigInt类型

`BigInt`数据类型的目的是比`Number`数据类型支持的范围更大的整数值。使用`BigInt`，整数溢出将不再是问题。

此外，可以安全地使用更加准确时间戳，大整数ID等，而无需使用变通方法。 它就是JS 第二个数字数据类型

在JS中，按照IEEE 754-2008标准的定义，所有数字都以双精度64位浮点格式表示。

在此标准下，无法精确表示的非常大的整数将自动四舍五入。确切地说，JS 中的Number类型只能安全地表示`-9007199254740991 (-(2^53-1))` 和`9007199254740991(2^53-1)`之间的整数，任何超出此范围的整数值都可能失去精度。

定义BigInt只需要在数字的后边加上一个n即可

### globalThis

全局属性 `globalThis` 包含全局的 `this` 值，类似于全局对象（global object）






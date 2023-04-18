---
title: JS基础语法
index_img: ./img/js.webp
date: 2023-02-10 10:00
categories: JS
---



# JS基础语法

## 基本数据类型

  JavaScript中一共有7种基本数据类型：

- 字符串型（String）

- 数值型（Number）

- 布尔型（Boolean）

- undefined型（Undefined):在使用 var 声明变量但未对其加以初始化时，这个变量就是undefined。

- null型（Null）

- symbol型（ES6新增）

- BigInt（ES11新增）

**undefined值实际上是由null值衍生出来的，所以如果比较undefined和null是否相等，会返回true。**

**这7种之外的类型都称为Object，所以总的来看JavaScript中共有七种数据类型。**

> **注意**：从语义上看null表示的是一个空的对象，所以使用typeof检查null会返回一个Object。

### **symble**

- Symbol 的值是唯一的，用来解决命名冲突的问题
- Symbol 值不能与其它数据进行运算
- Symbol 定义的对象属性不能使用 `for…in` 循环遍 历 ，但是可以使用 `Reflect.ownKeys` 来获取对象的所有键名

| 内置值                    | 描述                                                         |
| ------------------------- | ------------------------------------------------------------ |
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

### BigNit

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

## 检查变量的数据类型

1. typeof

   - typeof它返回值是一个字符串，该字符串说明运算数的类型。返回结果只有以下几种：`number，string，boolean，object，undfined，function`
   - 无法判断`对象和数组`，还有`null`，因为都返回的是`object`

   > 可以使用typeof判断变量是否存在（比如 if(typeof a!=“undfined”){ xxx }），而不要去使用if(a),因为a不存在（未声明）会报错。

   ```js
   console.log(typeof(1))
   ```

   > typeof 在判断一个 object的数据的时候只能告诉我们这个数据是 object, 而不能细致的具体到是哪一种 object。所以要 **想区分对象、数组、null，单纯使用 typeof 是不行的**。

2. instanceof

   instanceof 是用来 判断数据是否是某个对象的实例，返回一个`布尔值`，即可以判断数据类型。

   ```js
   console.log(1 instanceof Number)
   ```

   > `对于基本类型的数据，instanceof是不能直接判断它的类型的`，因为实例是一个对象或函数创建的，是引用类型，所以需要通过基本类型对应的 包装对象 来判断。所以对于 `null` 和 `undefined` 就检测不了
   >
   > 因为原型链继承的关系，instanceof 会把数组都识别为 Object 对象，所有引用类型的祖先都是 Object 对象

3. Object.prototype.toString.call()

   在判断数据类型时， Object.prototype.toString在工作中也是比较常用而且准确。
   对于Object.prototype.toString() 方法，会返回一个形如 “[object XXX]” 的字符串

   > 使用`const _toStr = Object.prototype.toString`是因为，toString实在是太容易被重写了。如果toString被其他人重写，将会对代码中涉及到的部分造成影响，所以就保存下来防止这种情况发生
   
   ```js
   Object.prototype.toString.call('stjd')
   //"[object String]"
    
   Object.prototype.toString.call(1)
   //"[object Number]"
    
   Object.prototype.toString.call(true)
   //"[object Boolean]"
   
   Object.prototype.toString.call(null)
   //"[object Null]"
   
   Object.prototype.toString.call(undefined)
   //"[object Undefined]"
   Object.prototype.toString.call(function(){})
   // "[object Function]"
   var date = new Date();
   Object.prototype.toString.call(date);
   //”[object Date]”
   Object.prototype.toString.call([2])
   //"[object Array]"
   Object.prototype.toString.call({q:8})
   //"[object Object]"
   var reg = /[hbc]at/gi;
   Object.prototype.toString.call(reg);
   // "[object RegExp]"
   ```

   > Object.prototype.toString.call(1) 和 Object.prototype.toString.call(new Number(1))时，返回的都是"[object Number]"，也就是说，它并不能区分原始类型和复杂类型.
   
   ```js
   //这种方法不能准确判断person是Person类的实例，而只能用instanceof 操作符来进行判断
   function Person(name, age) {
       this.name = name;
       this.age = age;
   }
   var person = new Person("Rose", 18);
   Object.prototype.toString.call(person); 
   //”[object Object]”
   console.log(person instanceof Person);//输出结果为true
   ```
   


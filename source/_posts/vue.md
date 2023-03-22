---
title: VUE2.x
index_img: ./img/vue2.webp
date: 2023-03-1 13:15:36
categories: VUE
---

# VUE简介

Vue是一套用于构建用户界面的渐进式框架。

特点：

1. 遵循 MVVM 模式，借鉴 angular 的模板和数据绑定技术，借鉴 react 的组件化和虚拟 DOM 技术。
2. 适合 移动/PC 开发，代码简洁，体积小，运行效率高。
3. 它本身只关注 UI，可以轻松引入 vue 插件或其它第三库开发项目。

## 特性：

vue两大特性: 数据驱动视图，双向数据绑定

### (1)数据驱动视图

- 数据变化会驱动视图自动更新,当数据发生变化时,vue会监听数据变化,从而自动重新渲染页面结构(无须手动操作DOM)
- 单向的数据绑定(数据变化导致页面变化) 

### (2)双向数据绑定

- 在填写表单时,双向数据绑定可以辅助开发者在不操作DOM 的前提下,自动把用户填写的内容同步到数据源中
- js数据发生变化,会自动渲染到页面上; 页面表单数据发生变化时,被vue自动获取并更新到js中

## MVVM

- MVVM是vue实现数据驱动视图和双向数据绑定的核心原理(底层)
- MVVM指Model(数据源),View(DOM结构)和ViewModel(vue实例)
- 它把每个HTML页面都拆分成了这三个部分 (Model表当前页面渲染时所依赖的数据源, View表示当前页面所渲染的DOM结构, ViewModel表vue的实例,它是MVVM的核心)
- 当数据源发生变化时,会被ViewModel监听到,并自动重新渲染页面结构
- 当表单元素的值发生变化时,也会被VM 监听到并把变化后最新的值自动同步到Model数据源中

![123](http://106.55.171.176:9000/jfz/123.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=sttch%2F20230321%2F%2Fs3%2Faws4_request&X-Amz-Date=20230321T050831Z&X-Amz-Expires=432000&X-Amz-SignedHeaders=host&X-Amz-Signature=21f8bf70d795bd26b7907b10bbbd01844dc12061fa3fc44cb2e957b229cd18d5)

## 基本使用步骤

- 导入 vue.js 的 script 脚本文件
- 在页面中声明一个将要被 vue 所控制的 DOM 区域
- 创建 vm 实例对象（vue 实例对象）

```js
<body>
    <!-- view区域 -->
    <!-- ==================================== -->
    <!-- 2.使用vue控制该div，填充数据 -->
    <div class="box">{{ username }}</div>
    <!-- ==================================== -->
    <!-- 1.导入vue库文件，在window全局就有了Vue构造函数 -->
    <script src="./lib/vue-2.6.12.js"></script>
    <script>
        // new vue构造函数得到的实例对象就是 ViewModel
        // ==================================
        // 3.创建vue实例
        const vm = new Vue({
            el: '.box',   //el指向的选择器就是View的视图区域
            data: {       //data指向的对象就是Model数据源
                username: 'ys'
            }
        })
        // ==================================
    </script>
</body>
```

## 插值表达式Mustache

- {{}}，只是内容占位符，不覆盖原内容
- 只能用在元素内容节点,不能用在属性节点  <p>性别: {{ sex }}</p> 
- 不能识别标签

# VUE基础

## 1.指令

指令(Directives)是vue为开发者提供的模板语法,用于辅助开发者渲染页面的基本结构,vue 中的指令按照不同的用途可以分为6 大类

### 1.内容渲染指令

- v-text: 会覆盖元素默认内容 <p v-text="username"></p>
- v-html : 渲染带标签的内容,并实现标签样式,会覆盖元素默认内容 <div v-html="属性"></div>
- v-once：元素和组件只渲染一次，不会随着数据的改变而改变
- v-pre：用于跳过这个元素和它子元素的编译过程，一般我们的mustache语法会去data里面找对应的变量然后解析，可是如果我们就想让其显示为 `{{message}}` ，这个时候需要加 `v-pre` 属性
- v-cloak：我们data里面的数据可能是从服务器端获取来的，如果网络不好获取的比较慢，浏览器可能会直接显示出未编译的Mustache标签,我们可以给标签添加 v-cloak 来避免这种效果:
  - 加上v-clock 属性,并加上css。
  - vue解析之前有 v-clock 属性时，让其不显示
  - Vue解析之后没有v-clock 属性，再让其显示

### 2.属性绑定指令

- v-bind: (简写英文 :): 为元素的属性动态绑定属性值, 单向绑定(数据源影响DOM变)
- 支持绑定简单的数据值 <img src="" v-bind:placeholder="tips">
- 支持绑定js表达式的运算 <div :title=" 'box-' + index">div盒子</div> 字符串需加' '

### 3.事件绑定指令

- v-on:(简写@): 为DOM元素绑定事件监听 

- 处理函数写在实例对象的methods属性下 <button @click="add()">点击</button> ()可传参

- 原生DOM对象onclick,oninput,onkeyup等原生事件,替换为@click,@input,@keyup

  

  **$event**

  vue 提供的特殊变量,表原生的事件参数对象event, 在DOM事件的[回调函数](https://so.csdn.net/so/search?q=回调函数&spm=1001.2101.3001.7020)中传入参数`$event`，可以获取到该事件的事件对象,可解决事件参数对象event被覆盖的问题(点击按钮变色问题)

  还可以在子组件中通过`$emit`注册事件，将数据作为参数传入，在父组件中通过`$event`接收

  **事件修饰符**

  | 事件修饰符 | 说明                                                       |
  | ---------- | ---------------------------------------------------------- |
  | stop       | 阻止了事件冒泡，相当于调用了event.stopPropagation方法      |
  | prevent    | 阻止了事件的默认行为，相当于调用了event.preventDefault方法 |
  | self       | 只当在 event.target 是当前元素自身时触发处理函数           |
  | once       | 绑定了事件以后只能触发一次，第二次就不会触发               |
  | capture    | 使事件触发从包含这个元素的顶层开始往下触发                 |
  | passive    |                                                            |

  


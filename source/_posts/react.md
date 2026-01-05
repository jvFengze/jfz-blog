# React

## 简介

react起源于Facebook的内部项目，设计思想独特，性能出众，逻辑简单，是一个用于构建用户界面的js库，主要用于构建ui

*react不是一个完整的MVC框架，很多人认为是MVC中的V（view）*

## 特点

- 声明式编程
- 组件化编程
- 高效
- jsx
- 单向数据流
- 虚拟dom

与vue不同的是，react都为js文件，一切的传值等操作都是函数及对象

# 创建一个react项目

react是一个工具库，以组件的形式拆解并组织ui代码，并不负责路由和数据管理，这些功能需要借助第三方工具或自己实现解决方案

## 工具链

- creart react app

直接再终端输入指令创建项目

```terminal
npx creat-react-app my-app
--------------------------
npx(Node Package Execute)是一个临时包执行器，可以直接运行本地或远程的命令，无需预先全局安装
```

- vite
- parcel
- snowpack

## 框架

- Next.js

> 是一个流行的、基于react构建的轻量级框架，该框架自带了路由、样式表和服务端渲染功能

- gatsby
- razzle

# 入门

react应用程序是由*组件*组成的，是ui的组成部分，拥有自己的逻辑和外观

react组件就是js函数，返回由标签语言编写的用户界面

```js
function MyButton() {
  return (
    <button>Click me</button>
  );
}
```

```js
export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton />
    </div>
  );
}
```

> react组件必须以大写字母开头，而HTML标签必须是小写字母！

## 添加样式

再react中使用className指定标签的class

## 显示数据

使用大括号*{}*在代码中嵌入变量并展示给用户，{}会读取js的变量

也可以在括号中加入更复杂的表达式

```jsx
const user = {
  name: 'Hedy Lamarr',
  imageUrl: 'https://i.imgur.com/yXOvdOSs.jpg',
  imageSize: 90,
};

export default function Profile() {
  return (
    <>
      <h1>{user.name}</h1>
      <img
        className="avatar"
        src={user.imageUrl}
        alt={'Photo of ' + user.name}
        style={{
          width: user.imageSize,
          height: user.imageSize
        }}
      />
    </>
  );
}
```

## 条件渲染

react没有特殊语法编写条件语句，使用js代码

```js
let content;
if(isLoggedIn){
	content = <Component />
}else{
	content = <OtherComponent />
}
    
return (
	<>
    	{ content }
    </>
)
--------------------------------------
return (
	<>
    	{isLoggedIn?(<component />):(<OtherComponent />)}
    </>
)
```

## 渲染列表

使用for循环或map返回elementArray来渲染组件列表

```js
const products = [
  { title: 'Cabbage', id: 1 },
  { title: 'Garlic', id: 2 },
  { title: 'Apple', id: 3 },
];
const listItems = products.map(product =>
  <li key={product.id}>
    {product.title}
  </li>
);

return (
  <ul>{listItems}</ul>
);
```

循环渲染时都应该传递一个字符串或数字给*key*属性，用于在其兄弟节点中标识该元素，在diff算法中思考发生了什么

## 响应事件

在组件中生命事件处理函数响应事件

```JS
function MyButton() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
    <button onClick={handleClick}>
      点我
    </button>
  );
}
```

> 不需要加小括号，否则会直接调用函数，只需要把函数传递给事件即可

## 界面更新

当需要组件记住并展示一些信息时，需要在组件中添加state

```js
// 引入state
import {useState} from 'react'

function MyButton () {
	const [count, setCount] = useState(0)
}
```

在*useState*中获取两个东西： 当前的state及更新state的函数 [someState, setSomeState]

当你需要改变state时，调用setSomeState并传递新的值即可改变state

> 当相同的组件被多次渲染时，每个相同组件都会维护自己的state

## Hook

use开头的函数被称为Hook函数，useState就是一个内置Hook

## 组件传值

相同组件的不同实例共享变量可以把变量写在父组件中，使用{}将值传入子组件

```js
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>共同更新的计数器</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}

function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      点了 {count} 次
    </button>
  );
}

```

这种做法叫做状态提升，通过向上移动state实现组件共享数据


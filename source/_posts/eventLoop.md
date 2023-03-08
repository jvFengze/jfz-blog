---
title: js事件循环
date: 2023-2-20 10:53
categories: JS
---

# js事件循环

**在了解事件循环前，我们先了解一些js特性知识**

- JS 引擎是单线程的，直白来说就是一个时间点下 JS 引擎只能去做一件事情。
- JS 做的任务分为同步和异步两种

## 同步与异步

以js来说，同步就是在浏览器执行js代码的时候，将所有同步（也就是大部分的代码）的代码放到一个执行栈中当中，遇到异步代码就把异步代码放到任务队列中，这样就形成了异步操作，**同步与异步的差别就在于这条流水线上各个流程的执行顺序不同**

- 执行栈：栈的顺序是filo(first in last on)，就是说栈里的代码是先进后出的
- 任务队列：这里用到的就是队列，队列的顺序是[fifo](https://so.csdn.net/so/search?q=fifo&spm=1001.2101.3001.7020)(first in first on)，队列里的代码是先进先出

**所有任务可以分成两种，一种是同步任务（synchronous），另一种是异步任务（asynchronous）。同步任务指的是，在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务；异步任务指的是，不进入主线程、而进入"任务队列"（task queue）的任务，只有等主线程任务执行完毕，"任务队列"开始通知主线程，请求执行任务，该任务才会进入主线程执行**

## 事件循环

事件循环过程可以简单描述为：

1. 函数入栈，当 Stack 中执行到异步任务的时候，就将他丢给 WebAPIs ,接着执行同步任务,直到 Stack 为空;
2. 在此期间 WebAPIs 把回调函数放入 task Queue (任务队列)中等待;
3. 当执行栈为空时，Event Loop 把 task Queue中的一个任务放入Stack中,回到第1步。

事件队列是一个存储着待执行任务的队列，其中的任务严格按照时间先后顺序执行，排在队头的任务将会率先执行，而排在队尾的任务会最后执行。事件队列每次仅执行一个任务，在该任务执行完毕之后，再执行下一个任务,一个任务开始后直至结束，不会被其他任务中断。执行栈则是一个类似于函数调用栈的运行容器，当执行栈为空时，JS 引擎便检查事件队列，如果不为空的话，事件队列便将第一个任务压入执行栈中运行。

## 宏任务和微任务

在JavaScript中，异步任务被分为两种，一种宏任务（MacroTask）也叫Task，一种叫微任务Microtask） 称为 Jobs。

事件循环由宏任务和在执行宏任务期间产生的所有微任务组成。当当前执行栈中的事件执行完毕后，js 引擎首先会判断微任务队列中是否有任务可以执行，如果有就将微任务队首的事件压入栈中执行。当微任务队列中的任务都执行完成后再去判断宏任务队列中的任务。每次宏任务执行完毕，都会去判断微任务队列是否产生新任务，若存在就优先执行微任务，否则按序执行宏任务。

- 宏任务

  - 整体代码script
  - setTimeout
  - setInterval
  - setImmediate
  - i/o操作（输入输出，比如读取文件操作，网络请求）
  - ui render
  - 异步Ajax

- 微任务

  - Promise.(then、catch、finally)
  - async/await
  - process.nextTick
  - Object.observe(用来实时检测js中对象的变化)
  - MutationObserver(监听DOM树的变化)

  > **注意：** `setTimeOut`并不是直接的把你的回掉函数放进上述的异步队列中去，而是在定时器的时间到了之后，把回调函数放到执行异步队列中去。如果此时这个队列已经有很多任务了，那就排在他们的后面。这也就解释了为什么`setTimeOut`为什么不能精准的执行的问题了。`setTimeOut`执行需要满足两个条件：
  >
  > 1. 主进程必须是空闲的状态，如果到时间了，主进程不空闲也不会执行你的回调函数
  > 2. 这个回调函数需要等到插入异步队列时前面的异步函数都执行完了，才会执行

举个例子：

```js
Promise.resolve().then(()=>{
  console.log('微任务1')
  setTimeout(()=>{
    console.log('宏任务2')
  },0)
})
setTimeout(()=>{
  console.log('宏任务1')
  Promise.resolve().then(()=>{
    console.log('微任务2')
  })
},0)
// 微任务1
// 宏任务1
// 微任务2
// 宏任务2
```

可以理解为开始执行代码时：

1. 将微任务1压入微任务队列，宏任务1加入宏任务队列
2. 将微任务1压入栈中执行
3. 执行中将碰到的宏任务2加入宏任务队列，执行完毕，执行栈为空
4. 此时微任务队列为空，将宏任务1压入栈中执行
5. 将执行中遇到的微任务2加入微任务队列，执行完毕，执行栈为空
6. 将微任务2压入栈中执行，执行完毕，执行栈为空
7. 此时微任务队列为，将宏任务2压入栈中执行，执行完毕

```js
async function async1() {
	console.log('1');
	await async2();
	console.log('2');
}
async function async2() {
	console.log('3');
    await async3();
    console.log('9');
}
async function async3(){
    console.log('10');
}
console.log('4');              
setTimeout(() => {
	console.log('5');
}, 0);
async1();
new Promise(function (reslove) {
	console.log('6');
	reslove();
}).then(function () {
	console.log('7');
})
console.log('8');
// 执行顺序为：4  1  3  10  6  8  9  7  2  5
```

此时先打印7在打印2

这是因为`await`如同他的语意，就是在等待，等待右侧的表达式完成。此时的`await`会让出线程，阻塞`async`内后续的代码，先去执行`async`外的代码。等外面的同步代码执行完毕，才会执行里面的后续代码。就算`await`的不是`promise`对象，是一个同步函数，也会等这样操作。

> **带`async`关键字的函数会返回一个`promise`对象**，如果里面没有`await`，执行起来等同于普通函数

此时执行完async3()后打印9进入了微任务队列，await async2()后的代码被阻塞，继续执行async1()之外的代码，继续打印6并让promise.then进入微任务队列，等外面代码执行完后再让console.log('2')进入微任务队列，故而先打印7在打印2

```js
async function async1() {
    console.log('async1 start');
    await async2();
    console.log('async1 end');
}

async function async2() {
    console.log('async2 start');
    await async3();
    console.log('async2 end');
}
async function async3() {
    console.log('async3');
    process.nextTick(() => {
        console.log('2')
    })
}
new Promise((resolve, reject) => {
    resolve('ok')
}).then((value) => {
    console.log(value);
})

process.nextTick(() => {
    console.log('1')
})
async1();

//async1();
//async1 start
//async2 start
//async3
//1
//2
//ok
//async2 end
//async1 end
```

Node 规定，`process.nextTick()`，追加在**本轮循环**，而且是所有异步任务里面**最快**执行的，即同步任务一旦执行完成，就开始执行它们。

**微任务队列追加在`process.nextTick`队列的后面**


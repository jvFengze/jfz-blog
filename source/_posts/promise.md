---
title: Promise
date: 2023-2-28 18:23
categories: JS
---

# promise

##  什么是Promise

Promise是异步编程的一种解决方案，解决了回调地狱。它本身是一个构造函数，自身有`all`、`reject`、`resolve`这几个方法，原型上有`then`、`catch`等方法

Promise的构造函数接收一个参数，是函数，并且传入两个参数：`resolve`，`reject`，分别表示异步操作执行成功后的回调函数和异步操作执行失败后的回调函数。其实这里用“成功”和“失败”来描述并不准确，按照标准来讲，`resolve`是将Promise的状态置为`fullfiled`，`reject`是将Promise的状态置为`rejected`。

`promis`对象有两个特点：

- 对象的状态不受外界影响
- 一旦状态改变，就不会再变，任何时候都可以得到这个结果

> `Promise`对象代表一个异步操作，有三种状态：`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败）,**只有异步操作的结果，可以决定当前是哪一种状态**，任何其他操作都无法改变这个状态。`Promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。你再对`Promise`对象添加回调函数，也会立即得到这个结果。
>
> > 这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

新建一个promise对象：

```js
let p = new Promise((resolve,reject)=>{
    console.log("1")
})
```

此时会直接打印1

只是new了一个`promise`对象，并没有调用，传进去的函数就会执行。所以我们用`promise`时一般包在一个函数里，在需要的时候去执行这个函数。

```js
function promiseTest(){
    let p = new Promise((resolve,reject) =>{
		setTimeout(() =>{
            console.log("执行promise")
            resolve('返回的数据')
        }，1000)；
    })；
    return p
}
```

包装好函数之后，`return`出promise对象，接下来就可以用`then`,`catch`方法了

```js
promiseTest().then((resolve) =>{
    console.log(reslove);
})
// 执行promies
// 返回的数据
```

在函数返回上调用`then`方法，then接收一个参数，是函数，并会拿到调用resolve时传的参数

then里面的函数就跟我们平时的回调函数一个意思，能够在promiseTest这个异步任务执行完成之后被执行。简单来讲**，就是能把原来的回调写法分离出来，在异步操作执行完后，用链式调用的方式执行回调函数**。

## 链式操作

实质上，Promise的精髓是“状态”，用维护状态、传递状态的方式来使得回调函数能够及时调用，它比传递callback函数要简单、灵活的多。

```javascript
promiseTest().then((resolve) =>{
    console.log(reslove);
    return promiseTest1();
}).then((resolve) =>{
    console.log(reslove);
})
//=======================
function promiseTest1(){
    let p = new Promise((resolve,reject) =>{
		setTimeout(() =>{
            resolve('返回的数据1')
        }，1000)；
    })；
    return p
}
```

这样能够按顺序输出每个异步回调中的内容,在promiseTest1中传给resolve的数据可以在下面的then方法中拿到。

**在then方法中，也可以直接return数据而不是Promise对象，在后面的then中就可以接收到数据了**

## reject的用法

reject的作用就是把Promise的状态置为rejected，这样我们在then中就能捕捉到，然后执行“失败”情况的回调。

```js
function getNumber(){
    let p = new Promise((res,rej) =>{
        let num = Math.floor(Math.random()*10)
        if(num > 5){
            res(num);
        }
        rej('数字太小了');
    });
    return p
}
getNumber().then((res) =>{
    console.log(res)
},(rej) =>{
    console.log(rej)
})
```

then方法可以接受两个参数，第一个对应resolve的回调，第二个对应reject的回调。所以我们能够分别拿到他们传过来的数据

## catch的用法

它和then的第二个参数一样，用来指定reject的回调

```js
getNumber()
.then(function(data){
    console.log(data);
})
.catch(function(reason){
    console.log(reason);
});
```

它还有另外一个作用：在执行resolve的回调（也就是上面then中的第一个参数）时，如果抛出异常了（代码出错了），那么并不会报错卡死js，而是会进到这个`catch`方法中，并把错误原因传入reason参数中，与`try`/`catch`相同。

```js
getNumber()
.then(function(data){
    console.log(data);
    console.log(abc);//abc未定义
})
.catch(function(reason){
    console.log(reason);
});
// data
//ReferenceError:abc is not defind
```

## all

Promise.all可以将多个Promise实例包装成一个Promise实例。它可以接收一个数组作为参数，数组中的每一项都是一个Promise实例。Promise.all方法的参数不一定是数组，但是必须具有Iterator接口。

> 如果参数中有不是Promise对象实例，就会先调用Promise.reslove方法，将参数转化为Promise对象实例，再进行下一步的处理

1. 只有数组里每个Promise的状态都编程了Fulfilled，Promise.all的状态才会变成Fulefilled，然后将每一个状态发返回的值，组装成一个数组返回给后面的回调函数
2. 只要数组里面有一个Promise的状态是Rejected，Promise.all的状态就会变成Rejected。这个时候第一个被Rejected的实例的结果会传递给后面你的回调
3. 如果作为参数的Promise实例自身定义了catch方法，那么他被Rejected时并不会被Promise.all的catch的方法给接受

### Promise.all队列中异步的执行顺序

```js
function promise1 () {
  return new Promise(resolve => {
    console.log('promise1 start');
    setTimeout(() => {
      console.log('promise1 over');
      resolve();
    }, 100);
  })
}
function promise2 () {
  return new Promise(resolve => {
    console.log('promise2 start');
    setTimeout(() => {
      console.log('promise2 over');
      resolve();
    }, 90);
  })
}

Promise.all([promise1(), promise2()])
// promise1 start
// promise2 start
// promise2 over
// promise1 over
```

如果Promise.all中的方法是串行的，执行顺序应该是`promise1 start` -> `promise1 over` -> `promise2 start` -> `promise2 over`但实际上却是上面的结果，说明Promise.all的执行顺序应该是并行的。（其实说并行是有问题的）

### 实现Promise.all

```js
function promiseAll(promises) {
    return new Promise((resolve,reject) =>{
        let count = 0;
        let result = [];
        if(!(promises instanceof Array)){
            promises = Array.from(promises)
        }
        for(let i = 0; i < promises.length; i++){
            if(!(promises[i] instanceof Promise)){
                promises[i] = Promise.resolve(promises[i])
            }
            promises[i].then((value) =>{
                count++;
                result[i] = value;
                if(count === promises.length){
                    resolve(result)
                }
            }, (reason) =>{
                reject(reason)
            })
        }
    })
}
```

## race

Promise.race方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。

只要传的值之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。

### 实现promise.race

```js
function PromiseAll(arr){
    return new Promise((resolve,reject)=>{
        arr.forEach((item,i) => {
            Promise.resolve(item).then(val=>{
                resolve(val)
            },err=>{
                reject(err)
            })
        });
    })
}
```


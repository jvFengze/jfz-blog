# 坑

------

vue3中`vue.prototype`已弃用，取而代之的是`app.config.globalProperties`，在调用全局组件时，会使用`getCurrentInstance`api获取当前的组件实例，相当于vue2中的this，但直接调用的话获取的对象只适用于生产环境，打包之后不可用，可以做出如下修改：

```js
import { getCurrentInstance } from 'vue'

const {proxy} = getCurrentInstance()

proxy.$components({props})
```

------


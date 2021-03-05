# vue变化侦测原理

## 如何侦测变化

关于变化侦测首先要问一个问题,在js中,如何侦测一个对象的变化.答案一般大家都知道,js中有两种方法可以侦测到变化,`Object.defineProperty`和ES6的`proxy`

vue2.0用的是`Object.defineProperty`,而vue3.0用的是`proxy`

下面重点说vue2.0的`Object.defineProperty`

先看一段简单的代码:

```JavaScript
function defineReactive(data,key,val) {
  dataect.defineProperty(data,key,{
    enumerable: true,
    configurable: true,
    get: function() {
      return val;
    },
    set: function(newVal){
      if(val === newVal) return;
      val = newVal
    }
  });
}
```

上面我们写了一个函数来封装`Object.defineProperty`,这样只需要传递`data`,`key`,`val`就行了.执行后,每当`data`的`key`读取数据,`get`方法就被触发,写数据的时候`set`方法就被触发. 那么,接下来怎么做到数据变化,页面上所有地方都跟着变化呢?接下来看第二问题~

## 怎么观察

现在要问第二个问题,"怎么观察?"

思考一下,我们之所以要观察一个数据,目的是为了当数据的属性发生了变化时,可以通知那些使用了这个`key`的地方.举个例子:

```html
<template>
  <div>{{ key }}</div>
  <p>{{ key }}</p>
</template>
```

上面的模板中有两处使用了`key`,所以当数据发生变化时,要把这两个地方都通知到.

所以上面的问题,回答是: 先收集依赖, 把这些用到`key`的地方先收集起来,然后等属性发生变化时,把收集好的依赖循环触发一遍. 总结起来就是一句话: **getter中收集依赖,setter中触发依赖**

## 依赖收集到哪里?

我们已经有了明确的目标,就是要在`getter`中收集依赖,那么我们的依赖收集到哪里去呢?

思考一下,首先想到的是每个`key`都有一个数组,用来存储当前`key`的依赖,假设依赖是一个函数存储在`window.target`上,先把`defineReactive`方法稍微改造一下:

```JavaScript
function defineReactive (data, key, val) {
  let dep = [] // 新增
  Object.defineProperty(data, key, {
      enumerable: true,
      configurable: true,
      get: function () {
          dep.push(window.target) // 新增
          return val
      },
      set: function (newVal) {
          if(val === newVal){
              return
          }
          
          // 新增
          for (let i = 0; i < dep.length; i++) {
              dep[i](newVal, val)
          }
          val = newVal
      }
  })
}
```

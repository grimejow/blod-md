---
title: Vue.nextTick
tags: vue,原理
slug: storywriter/upgrade_log
grammar_mindmap: true
renderNumberedHeading: true
grammar_code: true
grammar_decorate: true
grammar_mathjax: true
---





## 概念

下次DOM更新循环结束之后执行回调，修改数据后使用nextTick，可以获得更新后的DOM






## 原理

Vue的响应式并不是数据发生变化后立即在视图上更新呈现，而是有一定的规则来进行更新。

Vue的是异步执行DOM更新的，异步执行的机制的如下：

1.所有的同步任务都在主线程上进行。
2.主线程之外还有一个任务队列，应该是为异步任务存在的队列。
3.主线程所有的同步任务执行完之后，任务队列中的异步任务会被推到主线程来执行。
4.主线程会不断循环第三部。

**直白点就是Vue的数据更新后，视图不会立即更新，而是等同一个事件循环中的所有数据更新完毕，才进行视图更新。**

//改变数据
vm.message = 'changed'

//想要立即使用更新后的DOM。这样不行，因为设置message后DOM还没有更新
console.log(vm.$el.textContent) // 并不会得到'changed'

//这样可以，nextTick里面的代码会在DOM更新后执行
Vue.nextTick(function(){
    console.log(vm.$el.textContent) //可以得到'changed'
})

**Vue中的事件循环：**

1.在主线程上进行修改数据这样的同步任务。
2.开启异步任务队列，缓冲所有的数据改变。
3.同步任务执行完毕，开始执行异步任务队列中的任务，更新DOM。
4.dom更新结束，用Vue.nextTick获取更新后的DOM.

**应用场景**

数据驱动DOM更新后，获取更新后的DOM的操作。
